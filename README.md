# prove-it
Prove your online identity with cryptography



## Supported Claims

### Domain
```js
// https://dns.google/query?name=XXXXXXXXXXX&type=TXT
// === Proof ===
// TXT record: prove-it-domain-verification=XXXXXXXXXXX
// =============
const domain = 'qoo.monster'
const proof = await fetch(`https://dns.google/resolve?name=${domain}&type=TXT`)
  .then(res => res.json())
  .then(body => body.Answer.filter(record => record.data.startsWith('proveit-domain-verification=')))
```

### GitHub
#### profile
```js
// https://github.com/XXXXXXXXXXX/XXXXXXXXXXX/blob/HEAD/README.md
// === Proof ===
// ---
// 
// <sup><sub> **prove-it:** XXXXXXXXXXX </sub></sup>
// =============
const username = 'qoomon'
const proof = await fetch(`https://raw.githubusercontent.com/${username}/${username}/HEAD/README.md`)
    .then(res => res.text())
```
#### gist
```js
// https://gist.github.com/qoomon/6c04f280e4c0a8e0dc492246240f3830
// === Proof ===
// **Prove-it:** aspe:keyoxide.org:X2TJUL5QWM6CYZRFO6LLKFJ72Q
// =============
const gistId = 'XXXXXXXXXXX'
const {username, proof} = await fetch(`https://api.github.com/gists/${gistId}`)
  .then(res => res.json())
  .then(body => ({
    username: body.owner.login,
    data: Object.values(body.files)[0].content,
  }))
```

### BlueSky
```js
// https://bsky.app/profile/qoomon.bsky.social/post/XXXXXXXXXXX
const profile = 'qoomon.bsky.social'
const post = 'XXXXXXXXXXX'
const proof = await fetch(`https://public.api.bsky.app/xrpc/app.bsky.feed.getPostThread?uri=${encodeURIComponent(`at://${profile}/app.bsky.feed.post/${post}`)}`)
  .then(res => res.json())
  .then(body => body.thread.post.record.text)
```

### Reddit
```js
// https://www.reddit.com/user/qoomon
// === Proof ===
// ---
// 
// <sup><sub> **prove-it:** XXXXXXXXXXX </sub></sup>
// =============

const postUrl = 'https://www.reddit.com/user/qoomon/comments/1if3bc5/keyoxide_proof/'
const proof = await fetch(`${postUrl.replace(/\/$/, '')}.json`)
  .then(res => res.json())
  .then(body => body[0].data.children[0].data.selftext)
```

### Stack Overflow
```js
// https://stackoverflow.com/users/5376635/qoomon
// === Proof ===
// ---
// 
// <sup><sub> **Prove-it:** aspe:keyoxide.org:X2TJUL5QWM6CYZRFO6LLKFJ72Q </sub></sup>
// =============
const user = '5376635'
const proof = await fetch(`https://api.stackexchange.com/2.3/users/${user}?site=stackoverflow&filter=!AH)b5JqVyImf`)
    .then(res => res.json())
    .then(body => body.items[0].about_me)
```
