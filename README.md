# Cookie value extractor plugin for traefik

[![Build Status](https://github.com/bonovoxly/extractcookieregex/workflows/Main/badge.svg?branch=master)](https://github.com/bonovoxly/extractcookieregex/actions)

**Shoutout to <https://github.com/v-electrolux/extractcookie> for the original code...*


Takes specified cookie value (from Cookie header)
and put it in a header you want. You can declare prefix for value,
which will be added to cookie value.
If there is no such cookie, the header value will not be set 

## Configuration

### Fields meaning
- `cookieName`: name of cookie, its value will be extracted. 
   No default, mandatory field
- `headerNameForCookieValue`: name of header, in which will be put cookie value.
  Default is 'Authorization'
- `cookieValuePrefix`: string prefix, that will be added to cookie value.
  Default is 'Bearer '
- `logLevel`: `warn`, `info` or `debug`. Default is `info`

### Static config examples

- cli as local plugin
```
--experimental.localplugins.extractcookieregex=true
--experimental.localplugins.extractcookieregex.modulename=github.com/bonovoxly/extractcookieregex
```

- envs as local plugin
```
TRAEFIK_EXPERIMENTAL_LOCALPLUGINS_extractcookieregex=true
TRAEFIK_EXPERIMENTAL_LOCALPLUGINS_extractcookieregex_MODULENAME=github.com/bonovoxly/extractcookieregex
```

- yaml as local plugin
```yaml
experimental:
  localplugins:
     extractcookieregex:
      modulename: github.com/bonovoxly/extractcookieregex
```

- toml as local plugin
```toml
[experimental.localplugins.extractcookieregex]
    modulename = "github.com/bonovoxly/extractcookieregex"
```

### Dynamic config examples

- docker labels
```
traefik.http.middlewares.extractCookieRegexMiddleware.plugin.extractcookieregex.cookieName=test_access_token
traefik.http.middlewares.extractCookieRegexMiddleware.plugin.extractcookieregex.headerNameForCookieValue=Authorization
traefik.http.middlewares.extractCookieRegexMiddleware.plugin.extractcookieregex.cookieValuePrefix=Bearer 
traefik.http.middlewares.extractCookieRegexMiddleware.plugin.extractcookieregex.logLevel=warn
traefik.http.routers.extractCookieRegexRouter.middlewares=extractCookieRegexMiddleware
```

- yaml
```yml
http:

  routers:
    extractCookieRegexRouter:
      rule: host(`demo.localhost`)
      service: backend
      entryPoints:
        - web
      middlewares:
        - extractCookieRegexMiddleware

  services:
    backend:
      loadBalancer:
        servers:
          - url: 127.0.0.1:5000

  middlewares:
    extractCookieRegexMiddleware:
      plugin:
        extractcookieregex:
          cookieName: test_access_token
          headerNameForCookieValue: Authorization
          cookieValuePrefix: 'Bearer '
          logLevel: warn
```
