# discovery-swarm-web
Abstracts away discovery-swarm interaction with WebRTC and a websocket gateway.

```shell
npm install --save discovery-swarm-web
```

## Example:

```js
const DiscoverySwarmWeb = require('discovery-swarm-web')
const swarm = new DiscoverySwarmWeb({
  stream: () => archive.replicate(),
})

swarm.join(archive.discoveryKey)
```

## API

### `new DiscoverySwarmWeb(opts)`

`opts` include:
  - `stream`: The only required field. Return a stream to handle an incoming connection
  - `id`: The ID you want to use to show up in the swarm
  - `signalhub`: Either a URL for a [signalhubws](https://www.npmjs.com/package/signalhubws) server used for WebRTC signaling, or an object that has the same interface as [signalhub](https://www.npmjs.com/package/signalhub). Uses [signalhubws.mauve.moe](wss://signalhubws.mauve.moe) by default. Note that each signalhub server creates a new WebRTC swarm.
  - `discovery`: A `discovery-swarm-web` server URl to connect to. By default it uses [discoveryswarm.mauve.moe](wss://discoveryswarm.mauve.moe). All discovery servers reach out to the same P2P network.

### `swarm.join(key)`

Starts looking for peers for the given `key`. This should be either a string or a Buffer. For Dat, you should use the [discoveryKey](https://github.com/mafintosh/hypercore#feeddiscoverykey) of your hypercore or hyperdrive.

### `swarm.leave(key)`

Stops looking for peers for the given `key`.

### `swarm.close()`, `swarm.destroy()`

Close the swarm: closes all connections to peers, to the discovery server, and to the signalhub.

## CLI

You can start your own gateway for added performance and privacy using the CLI, or if you want to host one for an application you're building. By default it will listen on TCP 3472 (DISC), but that can be changed with the `--port` flag.

```shell
npm install -g discovery-swarm-web

discovery-swarm-web --port 3472
```

## Roadmap:

- [ ] Attempt to connect to a local discovery instance before the internet
- [ ] Generate ID if one is not provided
- [ ] Require "handshake" event (id, topic)
- [ ] Emit the `connection` event
- [ ] Disconnect peers with known ID
- [ ] Don't join discovery for a key unless you get no WebRTC peers (then join after a delay)
- [ ] Make `stream` optional with a default handshake function
- [ ] Have limits for the number of connections made for a given channel (auto-close new connections after reaching it)
