# prove-it
Prove your online identity with cryptography



## Supported Claims

### Domain
```js
const domain = 'qoo.monster'
const proof = await fetch(`https://dns.google/resolve?name=${domain}&type=TXT`)
  .then(res => res.json())
  .then(body => body.Answer.filter(record => record.data.startsWith('proveit-domain-verification=')))
```

### GitHub
```js
const gistUrl = 'https://gist.github.com/qoomon/6c04f280e4c0a8e0dc492246240f3830'
const gist = gistUrl.match(/\/(?<owner>[^/]+)\/(?<id>[^/]+)$/).groups
const proof = await fetch(`https://api.github.com/gists/${gist.id}`)
  .then(res => res.json())
  .then(body => {
    if(body.owner.login === gist.owner) return body 
    else throw new Error('invalid owner: ' + body.owner.login)
  })
  .then(body => Object.values(body.files)[0].content)

```

### BlueSky
```js
const postUrl = 'https://bsky.app/profile/qoomon.bsky.social/post/3lh4aqjjy4k27'
const post = postUrl.match(/profile\/(?<profile>[^/]+)\/post\/(?<id>[^/]+)$/).groups
const uri = `at://${post.profile}/app.bsky.feed.post/${post.id}`
const proof = await fetch(`https://public.api.bsky.app/xrpc/app.bsky.feed.getPostThread?uri=${encodeURIComponent(uri)}`)
  .then(res => res.json())
  .then(body => body.thread.post.record.text)
```

### Reddit
```js
const postUrl = 'https://www.reddit.com/user/qoomon/comments/1if3bc5/keyoxide_proof/'
const proof = await fetch(`${postUrl.replace(/\/$/, '')}.json`)
  .then(res => res.json())
  .then(body => body[0].data.children[0].data.selftext)
```

### Stack Overflow
```js
const userUrl = 'https://stackoverflow.com/users/5376635/qoomon'
const user = userUrl.match(/users\/(?<id>[^/]+)\/(?<name>[^/]+)$/).groups
const proof = await fetch(`https://api.stackexchange.com/2.3/users/${user.id}?site=stackoverflow&filter=!AH)b5JqVyImf`)
    .then(res => res.json())
    .then(body => body.items[0].about_me)
```
