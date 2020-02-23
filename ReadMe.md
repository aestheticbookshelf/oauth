# oauth

This module lets you add `oauth` to your `express` server in a simple way. Currently the only supported `strategy` is `lichess`.

## Synopsis

```javascript
const oauth = require('@aestheticbookshelf/oauth')

const firestore = null // provide firestore instance for persistance

const maxAge = 31 * 24 * 60 * 60 * 1000 // cookie max age in ms, optional, default is 1 year

oauth.initOauth(app, firestore, maxAge)

oauth.addLichessStrategy(app, {
    tag: "lichess-common",
    clientID: process.env.LICHESS_CLIENT_ID,
    clientSecret: process.env.LICHESS_CLIENT_SECRET,
    authURL: "/auth/lichess",
    failureRedirect: "/?lichesslogin=failed",
    okRedirect: "/?lichesslogin=ok"
})

oauth.addLichessStrategy(app, {
    tag: "lichess-bot",
    clientID: process.env.LICHESS_BOT_CLIENT_ID,
    clientSecret: process.env.LICHESS_BOT_CLIENT_SECRET,
    authURL: "/auth/lichess/bot",
    scope: "challenge:read challenge:write bot:play",
    failureRedirect: "/?lichessbotlogin=failed",
    okRedirect: "/?lichessbotlogin=ok"
})
```

### User

If you execute the above code in your `express` server, then after visiting the `authURL` on your server ( which takes you to the `oauth` server of the third party ) and approving the `oauth` form, all your `express` requests will have a lichess user that you can access with `req.user` ( or null if not authorized ). The `access token` is available as `req.user.accessToken`.

### Scope

You can pass an optional `scope` string, which should hold a space separated list of the scopes you request.

### Persistance

For making your login persistent you can pass an optional `firestore` instance to `initOauth`. If `null` is passed, an in memory store will be used.

### Registering app

Of course first you have to register your `oauth` app with the `oauth` provider ( see also [register lichess oauth app](https://lichess.org/account/oauth/app) ) and have the necessary environment variables set with `CLIENT_ID` and `CLIENT_SECRET`.

### Urls

If your `authUrl` passed to the `oauth` module was `/auth/lichess` and your server's host is `myawesomeserver.com`, then you have to register a lichess `oauth` app with `https://myawesomeserver.com/auth/lichess` for homepage `url` and `https://myawesomeserver.com/auth/lichess/callback` for callback `url`.

### Host

You have to specify your server's host in `env` variable `SITE_HOST`. If this is not defined, then a development server is assumed with host `localhost:3000`. For the development server you need to register a separate `oauth` app, where you have `http://localhost:3000/auth/lichess` for homepage `url` and `http://localhost:3000/auth/lichess/callback` for callback `url` ( or alternatively you can define `SITE_HOST` in development too, eg. `SITE_HOST=localhost:8080` ).
