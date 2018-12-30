---
title: Send notifications from Stripe to your own Telegram via Node.js
---

In this blog post I will show you how you can get Telegram notifications from Stripe events. I did set it up for when somebody is a new user (first payment) and for when I get a payment. I use Node.js without any framework. Stripe does not send it's user email address with the webhook payload so I grab that from my own PostgreSQL database.

I assume you have the following environment variables:

- `TELEGRAM_TOKEN`
- `TELEGRAM_CHAT_ID`
- `STRIPE_SECRET`
- `DATABASE_URL`

You can use [dotenv](https://www.npmjs.com/package/dotenv) to load environment variables in your local development.

It needs the following packages:

- [stripe](https://www.npmjs.com/package/stripe)
- [telegraf](https://www.npmjs.com/package/telegraf)
- [pg](https://www.npmjs.com/package/pg) (not really needed if you don't want to show user email)

I also obfuscate the email addresses I send to Telegram, my clients personal information should not end up in their database.

In Stripe you have to enable these webhooks for these events:

- `charge.succeeded`
- `charge.failed`

## Code for the Stripe webhook

```js
// webhook.js
const stripe = require('stripe')(process.env.STRIPE_SECRET)
const { Pool } = require('pg')

const pool = new Pool({ connectionString: process.env.DATABASE_URL })
const { inform, ACTIONS } = require.main.require('./telegram')

const endStripe = res => {
  res.writeHead(201)
  res.end(JSON.stringify({ success: true }));
}

const getJSON = url => {
  return new Promise((resolve, reject) => {
    const req = https.get(url, async (res) => {
      if (res.statusCode !== 200) resolve(false)
      const json = await getJSON(res)
      resolve(json)
    })

    req.on('error', (e) => reject(e))
  })
}

const obfuscate = email => {
  let emailParts = email.split('@')
  emailParts[0] = emailParts[0].slice(0, -3) + '***'
  return emailParts.join('@')
}

module.exports = async (req, res) => {
  res.setHeader('Content-Type', 'application/json')
  res.setHeader('Access-Control-Allow-Origin', '*')

  try {
    if (req.method !== 'POST') throw new Error(`Only POST allowed`)

    // Get all the data
    const { type, data: { object } } = await getJSON(req)

    switch (type) {
      case 'charge.succeeded':
      case 'charge.failed': {
        const { customer, amount } = object
        const usd = amount / 100

        // Get user email from our database (change this for your own needs)
        const { rows } = await pool.query('SELECT email FROM users WHERE stripe_userid = $1', [customer])
        if (!rows || !rows.length) throw new Error(`Stripe user '${customer}' not found in our data to send '${type}' alert`)
        const { email } = rows[0]

        // Inform when new payment failed
        if (type === 'charge.failed') {
          inform(ACTIONS.FAILED_PAYMENT, `<b>$${usd}</b> by ${obfuscate(email)}`)
          break
        }

        // Inform when payment succeeded
        const { data: charges } = await stripe.charges.list({ customer, status: 'succeeded' })
        if (charges.length === 1) inform(ACTIONS.NEW_PAID_USER, `<b>$${usd}</b> by ${obfuscate(email)}`)
        else if (charges.length > 1) inform(ACTIONS.NEW_PAYMENT, `<b>$${usd}</b> by ${obfuscate(email)}`)

        break
      }
      default: {
        throw new Error(`Got invalid event '${type}' for Stripe webhook`)
      }
    }

    return endStripe(res)
  } catch (err) {
    inform(ACTIONS.CONSOLE_ERROR, err.message)
    console.error(err)
    res.writeHead(500)
    res.end(JSON.stringify({ success: false, message: err.message }))
  }
}
```

## Code for the Telegram notifications

```js
// telegram.js
const Telegram = require('telegraf/telegram')

const telegram = new Telegram(process.env.TELEGRAM_TOKEN)

module.exports.ACTIONS = {
  CONSOLE_ERROR: '🚨 console.error',
  NEW_PAID_USER: '💰💰💰 a new PAID user',
  NEW_PAYMENT: '💰 a new payment'
}

module.exports.inform = (action, detail) => {
  try {
    const isArray = Array.isArray(detail)
    const body = isArray ? `- ${detail.join('\n- ')}` : detail
    const log = isArray ? detail.join(', ') : detail
    const message = detail ? `<b>${action}</b>\n${body}` : `<b>${action}</b>`

    // Don't send on development
    if (process.env.NODE_ENV === 'development') return console.log('NOTIFY:', action, log || '')

    telegram.sendMessage(process.env.TELEGRAM_CHAT_ID, message, { parse_mode: 'html' })
    return console.log('NOTIFY:', action, log || '')
  } catch (err) {
    console.error('Something went wrong when trying to send a Telegram notification', err)
  }
}
```
