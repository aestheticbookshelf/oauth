# oauth

This module lets you add oauth to your `express` server in a simple way. Currently the only supported `strategy` is `lichess`.

## Synopsis

```javascript
const oauth = require('@aestheticbookshelf/oauth')

const firestore = null // provide firestore instance for persistance

oauth.initOauth(app, firestore)

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

After execute the above code in your `express` server, all your `express` requests will have a lichess user that you can access with `req.user` ( or null if not authorized ). Of course you have to register your oauth app with lichess ( [register lichess oauth app](https://lichess.org/account/oauth/app) ) and have the necessary environment variables set with client id and client secret. If your `authUrl` passed to the oauth module was `/auth/lichess` and your server's host is `myawesomeserver.com`, then you have to register a lichess oauth app with `https://myawesomeserver.com/auth/lichess` for homepage url and `https://myawesomeserver.com/auth/lichess/callback` for callback url. You have to specify your server host in env variable `SITE_HOST`. If this is not defined, then a development server is assumed with host `localhost:3000`; for the development server you need to register a separate oauth app, where you have `http://localhost:3000/auth/lichess` for homepage url and `http://localhost:3000/auth/lichess/callback` for callback url ( or alternatively you can define `SITE_HOST` in development too, eg. `SITE_HOST=localhost:8080` ).