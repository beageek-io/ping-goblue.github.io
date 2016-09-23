---
layout: post
title: Use passport facebook strategy with query parameters in the callback
category: javascript
---

Recently, I was having trouble adding query parameters to the callback url when
using [passport facebook
strategy](https://github.com/jaredhanson/passport-facebook). I am going to
share the solution that I figured out.

```javascript
var Passport = require('passport');
var FacebookStrategy = require('passport-facebook').Strategy;
const Router = require("express").Router();
var fbConfig = {
  display: "popup",
  clientID: "YourFbClientId",
  clientSecret: "YourFbClientSecret",
  callbackURL: "http://localhost:8686/auth/facebook/callback",
  profileFields: ['id', 'name', 'gender', 'displayName', 'photos', 'profileUrl', 'email']
}

Passport.use(new FacebookStrategy(fbConfig,
  function(accessToken, refreshToken, profile, callback) {
    return callback(null, accessToken);
  }
));

Router.get("/auth/facebook", function(req, res, next) {
  var callbackURL = fbConfig.callbackURL + "?queryParams=" + req.query.queryParams;
  Passport.authenticate("facebook", { scope : ["email"], callbackURL: callbackURL })(req, res, next);
});

Router.get("/auth/facebook/callback", function(req, res, next) {
  Passport.authenticate("facebook", {
    callbackURL: fbConfig.callbackURL + "?queryParams=" + req.query.queryParams,
    failureRedirect: "/login",
    session: false
  })(req, res, next) },
  function(req, res) {
  console.log(req.query.queryParams);
  //do whatever you want
});

```

Now, you can start to pass query params to `localhost:8686/auth/facebook` to
initiate Facebook auth and the query params will be available in the callbackUrl of Facebook,
`localhost:8686/auth/facebook/callback`

