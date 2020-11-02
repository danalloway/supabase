---
title: Supabase.js 1.0.0
description: We're releasing a new version of our Supabase client with some awesome new improvements.
author: Paul Copplestone
author_title: Supabase
author_url: https://github.com/kiwicopple
author_image_url: https://avatars2.githubusercontent.com/u/10214025?s=400&u=c6775be2ae667e2acae3ccd347fed62bb3f5b3e7&v=4
authorURL: https://github.com/kiwicopple
# image: /img/supabase-september-2020.png
tags: 
    - supabase
---

Today we're releasing [supabase-js](https://github.com/supabase/supabase-js) version 1.0.0, and it comes with some major Developer Experience improvements.

<!--truncate-->

### New Docs

Before digging into the improvements, we're excited to point out our new [developer docs](http://localhost:3005/docs/client/supabase-client). While they're still a work in progress, here are some things we think you'll like:

- The [Reference Docs](/docs/client/supabase-client) are auto-generated from our Typescript definitions and then enriched with examples. This forces us to document our code and makes it easier to keep everything in sync.
- We added placeholders for the other languages that the community is developing. They have already started with Python, C#, Dart, Rust, and Swift. Expect to see the docs filling up soon!
- We've added sections for all of the open source tools we use, including [Postgres](/docs/postgres/server/about), [PostgREST](/docs/postgrest/server/about), [GoTrue](/docs/gotrue/server/about), and [Realtime](/docs/realtime/server/about). We'll be filling these with lots of valuable information including self-hosting, benchmarks, and simple guides.


### Errors are returned, not thrown

We attribute this improvement to community feedback. This has significantly improved the developer experience. Previously we would throw errors:

```js
try {
  const { body } = supabase.from('todos').select('*')
}
catch (error) {
  console.log(error)
}
```

And now we simply return them:

```js
const { data, error } = supabase.from('todos').select('*')
if (error) console.log(error)

// else, carry on ..
```

After testing this for a while we're very happy with this pattern. Errors are handled next to the offending function. Of course you can always rethrow the error if that's your preference.


### We created `gotrue-js`

Our goal for `supabase-js` is to tie together many sub-libaries. Each sub-library is a standalone implementation for a single external system. This is one of the ways we support existing open source tools.

To maintain this philosophy, we created [`gotrue-js`](https://github.com/supabase/goture-js), a library for Netlify's GoTrue auth server. This libary includes a number of new additions, including third-party logins.

Previously:

```js
const {
  body: { user },
} = await supabase.auth.signup(
  'someone@email.com',
  'password'
)
```

Now:

```js
const { user, error } = await supabase.auth.signUp({
  email: 'someone@email.com',
  password: 'password'
})
```

### Enhancements and fixes

- Native Typescript. All of our libraries are now natively built with Typescript: [`supabase-js`](https://github.com/supabase/supabase-js), [`postgrest-js`](https://github.com/supabase/postgrest-js), [`gotrue-js`](https://github.com/supabase/gotrue-js), and [`realtime-js`](https://github.com/supabase/realtime-js).
- Better realtime scalability: we only generate one socket connection per Supabase client. Previously we would create a connection for every subscription.
- We've added support for OAuth providers.
- 60% of minor bugs outstanding for `supabase-js` have been [solved](https://github.com/supabase/supabase-js/pull/50).
- You can use `select()` instead of `select(*)`

### Breaking changes

We've bumped the major version because there are a number of breaking changes. We've detailed these in the [release notes](https://github.com/supabase/supabase-js/releases/tag/v1.0.0), but here are a few to be aware of:

- `signup()` is now `signUp()` and `email` / `password` is passed as an object
- `logout()` is now `signOut()`
- `login()` is now `signIn()`
- `ova()` and `ovr()` are now just `ov()`
- `body` is now `data`

Previously:

```js
const { body } = supabase.from('todos').select('*')
```

Now:

```js
const { data } = supabase.from('todos').select()
```


### Upgrading

There will be a number of tasks that you want to do when you upgrade:

1. Install the new version: `npm install @supabase/supabase-js@latest`
2. Update all your `body` constants to `data`
3. Update all your `supabase.auth` functions with the new [Auth interface](/docs/client/auth-signup)

### Get started

- Start using Supabase today: [app.supabase.io](https://app.supabase.io/)
- Make sure to [star us on GitHub](https://github.com/supabase/supabase)
- Follow us [on Twitter](https://twitter.com/supabase_io)
- Become a [sponsor](https://github.com/sponsors/supabase)