# prove-it
Prove your online identity with cryptography



## Supported Claims

### BlueSky



```js
const postUrl = 'https://bsky.app/profile/qoomon.bsky.social/post/3lh4aqjjy4k27'
const post = postUrl.match(/profile\/(?<profile>[^/]+)\/post\/(?<id>[^/]+)/).groups
const uri = `at://${post.profile}/app.bsky.feed.post/${post.id}`
await fetch(`https://public.api.bsky.app/xrpc/app.bsky.feed.getPostThread?uri=${encodeURIComponent(uri)}`)
  .then(res => res.json())
```
