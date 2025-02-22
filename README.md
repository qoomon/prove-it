# prove-it
Prove your online identity with cryptography



## Supported Claims

### Domain
```js
// https://dns.google/query?name=XXXXXXXXXXX&type=TXT
// === Proof ===
// Add TXT record with folloing value
// prove-it-domain-verification=PROOF_PROOF_PROOF
// =============
const domain = 'XXXXXXXXXXX'
const proof = await fetch(`https://dns.google/resolve?name=${domain}&type=TXT`)
  .then(res => res.json())
  .then(body => body.Answer.filter(record => record.data.startsWith('proveit-domain-verification=')))
```

### GitHub
#### profile
```js
// https://github.com/XXXXXXXXXXX/XXXXXXXXXXX/blob/HEAD/README.md
// === Proof ===
// Add PROOF_PROOF_PROOF to user profile README.md
// Example:
// ---
// 
// <sup><sub> **prove-it:** PROOF_PROOF_PROOF </sub></sup>
// =============
const username = 'XXXXXXXXXXX'
const proof = await fetch(`https://raw.githubusercontent.com/${username}/${username}/HEAD/README.md`)
    .then(res => res.text())
```
#### gist
```js
// https://gist.github.com/USER_NAME/XXXXXXXXXXX
// === Proof ===
// Create a gist with a 'prove-it.md' file and following content
// PROOF_PROOF_PROOF
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
// https://bsky.app/profile/XXXXXXXXXXX/post/YYYYYYYYYYY
// === Proof ===
// Create a post with folloing content
// prove-it: PROOF_PROOF_PROOF
// =============
const profile = 'XXXXXXXXXXX'
const post = 'YYYYYYYYYYY'
const proof = await fetch(`https://public.api.bsky.app/xrpc/app.bsky.feed.getPostThread?uri=${encodeURIComponent(`at://${profile}/app.bsky.feed.post/${post}`)}`)
  .then(res => res.json())
  .then(body => body.thread.post.record.text)
```

### Reddit
```js
// https://www.reddit.com/user/XXXXXXXXXXX/comments/YYYYYYYYYY/
// === Proof ===
// Add PROOF_PROOF_PROOF to user profile README.md
// ---
// 
// <sup><sub> **prove-it:** PROOF_PROOF_PROOF </sub></sup>
// =============
const user = 'XXXXXXXXXXX'
const post = 'YYYYYYYYYYY'
const proof = await fetch(`https://www.reddit.com/user/${user}/comments/${post}.json`)
  .then(res => res.json())
  .then(body => body[0].data.children[0].data.selftext)
```

### Stack Overflow
```js
// https://stackoverflow.com/users/XXXXXXXXXXX
// === Proof ===
// Add PROOF_PROOF_PROOF to the profile 'about me' section
// Example:
// ---
// 
// <sup><sub> **Prove-it:** PROOF_PROOF_PROOF </sub></sup>
// =============
const user = 'XXXXXXXXXXX'
const proof = await fetch(`https://api.stackexchange.com/2.3/users/${user}?&site=stackoverflow&filter=!AhdF6aF0yuI-5W*ymYcd-`)
    .then(res => res.json())
    .then(body => body.items[0].about_me)
// TODO get username
```
