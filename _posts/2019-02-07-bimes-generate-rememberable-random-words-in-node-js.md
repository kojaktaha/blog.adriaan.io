---
title: "Bimes: Generate rememberable random words in Node.js"
---

At my last job we needed a word or string which our customers could remember. It also should be generated automatically. While drawing our ideas on the drawing board we came up with a solution that seemed to work. Why not use a consonant followed by a vowel and repeat this for `n` times.

Af course you need a blacklist of words when generating automatically, especially when you can create actual words. We had both Dutch and English speaking employees, so the blacklist contains those two languages. *If you have more, just edit this blog post.*

```js
// Blacklist containing possible bad words in Dutch and English
const blacklist = ['banus', 'bof', 'boner', 'bug', 'bum', 'coq', 'cum', 'dom', 'dop', 'dope', 'duwen', 'fag', 'fap', 'fuk', 'gay', 'gek', 'homo', 'hor', 'jemig', 'jezus', 'jiz', 'kak', 'kater', 'kike', 'kut', 'lul', 'moron', 'nemen', 'nig', 'niger', 'palen', 'penis', 'pik', 'pot', 'rape', 'rapin', 'satan', 'semen', 'sex', 'tit', 'tyfus', 'zak']

// A selection of letters (conflicting letter like `i` and `l` are ommitted)
const consonants = ['b', 'c', 'd', 'f', 'g', 'h', 'k', 'p', 'q', 'r', 's', 't', 'v', 'w']
const vowels = ['a', 'e', 'i', 'o', 'u']

// Generate the bime
const generateBime = ({ length } = { length: 5 }) => {
  const letters = []
  for (let index = 0; index < length; index++) {
    const list = (index % 2) ? vowels : consonants
    const letter = list[Math.floor(Math.random() * list.length)]
    letters.push(letter)
  }
  const bime = letters.join('')
  for (const badWord of blacklist) if (bime.indexOf(badWord) > -1) return generateBime({ length })
  return bime
}
```
