# Lightning Jukebox

[![npm release](https://img.shields.io/npm/v/lightning-jukebox.svg)](https://www.npmjs.com/package/lightning-jukebox)
[![MIT license](https://img.shields.io/github/license/shesek/lightning-jukebox.svg)](https://github.com/shesek/lightning-jukebox/blob/master/LICENSE)
[![Pull Requests Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)
[![IRC](https://img.shields.io/badge/chat-on%20freenode-brightgreen.svg)](https://webchat.freenode.net/?channels=lightning-charge)

A Lightning powered Jukebox. Pay with Groestlcoin to choose your music from YouTube.

[See it in action on YouTube](https://www.youtube.com/watch?v=AgGYpFJsh24)

Powered by :zap: [Groestlcoin Lightning Charge](https://github.com/groestlcoin/groestlcoin-lightning-charge)

## HOWTO

1. [Setup Lightning Charge](https://github.com/groestlcoin/groestlcoin-lightning-charge/blob/master/README.md#getting-started).

2. Install Lightning Jukebox and start `jukeboxd`:

   ```bash
   $ npm install -g lightning-jukebox

   $ jukeboxd --charge-token mySecretToken --price '0.0001 GRS'
   Jukebox server running on http://localhost:6100
   ```

   You may pick a different theme from [bootswatch](https://bootswatch.com)
   by specifying `--theme [name]` (the default is `darkly`).

3. Navigate to `http://localhost:6100/` on the computer playing the music
   and click `Spawn YouTube player`.
   This will open a new YouTube window in a new tab.
   *Make sure to keep both* the page on `localhost:6100` and the youtube window open.
   You can use the YouTube window to start playing some initial music.

   <img src="https://i.imgur.com/l9PNsdS.png" width="47%"></img>

4. Make the payment page (`http://localhost:6100/pay`) available over the internet or set it up on a local device, like a tablet, near
   the jukebox. The payment page allows users to pay for music selection.

   <img src="https://i.imgur.com/H9kFDQW.png" width="47%"></img>
   <img src="https://i.imgur.com/pTZkZ0H.png" width="47%"></img>

   Once a payment is made, a push notification will be sent to the player window (via websockets),
   which will open the requested song in the spawned youtube window.

   Payments can also be made directly to the jukebox API:

   ```bash
   # with a search string
   $ BOLT11=`curl http://localhost:6100/invoice -d video='are you shpongled full album'`
   $ lightning-cli decodepay $BOLT11
   $ lightning-cli pay $BOLT11

   # with a specific video id
   $ lightning-cli pay `curl http://localhost:6100/invoice \
                        -d video=https://www.youtube.com/watch?v=IDiZG-eAk30`
   ```

## CLI options

```bash
$ jukeboxd --help

  A Lightning powered Jukebox

  Usage
    $ jukeboxd [options]

  Options
    -c, --charge-url <url>      Groestlcoin lightning charge server url [default: http://localhost:9112]
    -t, --charge-token <token>  lightning charge access token [required]

    -P, --price <price>         price to play music [default: 0.0001 GRS]
    -m, --theme <name>          pick theme from bootswatch.com [default: darkly]
    -l, --title <name>          website title [default: Groestlcoin Lightning Jukebox]

    -p, --port <port>           http server port [default: 9115]
    -i, --host <host>           http server listen address [default: 127.0.0.1]
    -h, --help                  output usage information
    -v, --version               output version number

  Example
    $ jukeboxd -t chargeSecretToken -P '0.0005 EUR'
```

## Why a separate YouTube tab instead of embedding the video player?

So that "auto play next" works.

## License

MIT
