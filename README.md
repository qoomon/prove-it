# prove-it
Prove your online identity with cryptography



## Supported Claims

### Domain
Add following `DNS TXT record` to your domain 
```
prove-it-domain-verification=PROOF_PROOF_PROOF
```

```js
const domain = 'XXXXXXXXXXX'

const proof = await fetch(`https://dns.google/resolve?name=${domain}&type=TXT`)
  .then(res => res.json())
  .then(body => body.Answer.filter(record => record.data.startsWith('proveit-domain-verification=')))

const proofUrl = `https://dns.google/query?name=${domain}&type=TXT`
```

### GitHub

#### profile
Add proof to your user profile `README.md` like this
```md
---

<sup><sub> **prove-it:** PROOF_PROOF_PROOF </sub></sup>
```

```js
const user = 'XXXXXXXXXXX'

const proof = await fetch(`https://raw.githubusercontent.com/${user}/${user}/HEAD/README.md`)
    .then(res => res.text())

const proofUrl = `https://github.com/${user}/${user}/blob/HEAD/README.md`
```

#### gist
Create a gist with a `prove-it.md` file and `PROOF_PROOF_PROOF` as content

```js
const gist = 'XXXXXXXXXXX'

const {user, proof} = await fetch(`https://api.github.com/gists/${gist}`)
  .then(res => res.json())
  .then(body => ({
    user: body.owner.login,
    proof: Object.values(body.files)[0].content,
  }))

const proofUrl = `https://gist.github.com/${user}/${gist}`
```

### BlueSky
Create a post with folloing content `prove-it: PROOF_PROOF_PROOF`

```js
const user = 'XXXXXXXXXXX'
const post = 'YYYYYYYYYYY'

const proof = await fetch(`https://public.api.bsky.app/xrpc/app.bsky.feed.getPostThread?uri=${encodeURIComponent(`at://${user}/app.bsky.feed.post/${post}`)}`)
  .then(res => res.json())
  .then(body => body.thread.post.record.text)

const proofUrl = `https://bsky.app/profile/${user}/post/${post}`
```

### Reddit
Create a post with folloing content `prove-it: PROOF_PROOF_PROOF`

```js
const user = 'XXXXXXXXXXX'
const post = 'YYYYYYYYYYY'

const proof = await fetch(`https://www.reddit.com/user/${user}/comments/${post}.json`)
  .then(res => res.json())
  .then(body => body[0].data.children[0].data.selftext)

const proofUrl = `https://www.reddit.com/user/${user}/comments/${post}/`
```

### Stack Overflow
Add `PROOF_PROOF_PROOF` to your profile `about me` section like this
```md
---

<sup><sub> **prove-it:** PROOF_PROOF_PROOF </sub></sup>
```

```js
const userId = 'XXXXXXXXXXX'

const {username, proof} = await fetch(`https://api.stackexchange.com/2.3/users/${userId}?site=stackoverflow&filter=!AhdF6aF0yuI-5W*ymYcd-`)
    .then(res => res.json())
    .then(body => body.items[0].about_me)
    .then(body => ({
      username: body.items[0].display_name,
      proof: body.items[0].about_me,
    }))

const proofUrl = `https://stackoverflow.com/users/${userId}/${username}`
```

### Hacker News
Add `PROOF_PROOF_PROOF` to your profile `about` section like this
```
[ prove-it: PROOF_PROOF_PROOF ]
```

```js
const user = 'XXXXXXXXXXX'

const proof = await fetch(`https://hacker-news.firebaseio.com/v0/user/${user}.json`)
  .then(res => res.json())
  .then(body => body.about)

const proofUrl = `https://news.ycombinator.com/user?id=${user}`
```

### Mastodon
Create a post with folloing content `prove-it: PROOF_PROOF_PROOF`

```js
const server = 'ZZZZZZZZZZZ'
const post = 'YYYYYYYYYYY'

const {user, proof} = await fetch(`https://${server}/api/v1/statuses/${post}`)
  .then(res => res.json())
  .then(body => ({
    user: body.account.username,
    proof: body.content,
  ))

const proofUrl = `https://${server}/@${user}/${post}`
```

### X aka Twitter
Create a post with folloing content `prove-it: PROOF_PROOF_PROOF`

```js
const user = 'XXXXXXXXXXX'
const post = 'YYYYYYYYYYY'

const proof = await fetch('https://cors-tube.vercel.app/?url=' +
  encodeURIComponent('https://publish.twitter.com/oembed?url=' +
  encodeURIComponent(`https://x.com/${user}/status/${post}`)))
  .then(res => res.json())
  .then(body => body.html)

const proofUrl = `https://x.com/${user}/status/${post}`
```
