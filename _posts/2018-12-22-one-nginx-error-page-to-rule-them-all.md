---
title: One NGINX error page to rule them all
---

When I setup an NGINX server I have to setup custom error pages for every error. I want to show the error message on the page and not have a genenic _"Something is wrong"_ error page. If customers complain about an error page it's nice if they can message you the error code.

First of all you need to create an error_page in your server directive:

```
error_page 401 402 403 404 405 500 501 502 503 504 /error.html;

location = /error.html {
  ssi on;
  internal;
  root /var/www/default;
}
```

After this you create a `error.html` file in `/var/www/default/` (you can change this dir of course).

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Your Business</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!--# if expr="$status = 502" -->
      <meta http-equiv="refresh" content="2">
    <!--# endif -->
  </head>
<body>
  <!--# if expr="$status = 502" -->
    <h1>We are updating our website </h1>
    <p>This is only for a few seconds, you will be redirected.</p>
  <!--# else -->
    <h1><!--# echo var="status" --> <!--# echo var="status_text" --></h1>
  <!--# endif -->
</body>
</html>
```

Reload your NGINX config (no need to restart NGINX):

```bash
sudo nginx -t && sudo service nginx reload
```

If you want to have an error status text returned (like "Not found") you can map the `$status` variable. Put these lines somewhere in your http directive (you can leave out the ones you don't need).

```
map $status $status_text {
  400 'Bad Request';
  401 'Unauthorized';
  402 'Payment Required';
  403 'Forbidden';
  404 'Not Found';
  405 'Method Not Allowed';
  406 'Not Acceptable';
  407 'Proxy Authentication Required';
  408 'Request Timeout';
  409 'Conflict';
  410 'Gone';
  411 'Length Required';
  412 'Precondition Failed';
  413 'Payload Too Large';
  414 'URI Too Long';
  415 'Unsupported Media Type';
  416 'Range Not Satisfiable';
  417 'Expectation Failed';
  418 'I\'m a teapot';
  421 'Misdirected Request';
  422 'Unprocessable Entity';
  423 'Locked';
  424 'Failed Dependency';
  426 'Upgrade Required';
  428 'Precondition Required';
  429 'Too Many Requests';
  431 'Request Header Fields Too Large';
  451 'Unavailable For Legal Reasons';
  500 'Internal Server Error';
  501 'Not Implemented';
  502 'Bad Gateway';
  503 'Service Unavailable';
  504 'Gateway Timeout';
  505 'HTTP Version Not Supported';
  506 'Variant Also Negotiates';
  507 'Insufficient Storage';
  508 'Loop Detected';
  510 'Not Extended';
  511 'Network Authentication Required';
  default 'Something is wrong';
}
```
