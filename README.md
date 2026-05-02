```
__________                     __ .__                  .____         .___        
\______   \ ___.__. __ __     |__||__|  ____  ___  ___ |    |      __| _/  ____  
 |       _/<   |  ||  |  \    |  ||  | /    \ \  \/  / |    |     / __ |  /    \ 
 |    |   \ \___  ||  |  /    |  ||  ||   |  \ >    <  |    |___ / /_/ | |   |  \
 |____|_  / / ____||____/ /\__|  ||__||___|  //__/\_ \ |_______ \\____ | |___|  /
        \/  \/            \______|         \/       \/         \/     \/      \/ 
```

# Ryujinx LDN Multiplayer Server

This server drives multiplayer "matchmaking" in Ryujinx's local wireless implementation. Players can create "networks" on the server, which can then be scanned and found by other players as if they were within that network's range. The server entirely manages the network information and player join/leave lifecycle for all games created.

This server is intended for *Ryujinx-to-Ryujinx gameplay.* Any other methods are unsupported and unofficial, and detection of such methods may result in being added to the IP Ban list. 
> We run this server for Ryujinx users. This is not meant to be your NSO replacement.

## License

This software is licensed under the terms of the [MIT license](LICENSE.txt).

## Configuration

This app can be configured using the following environment variables:

| Name                | Description                                                             |   Default value   | Notes                                                                                   |
|:--------------------|-------------------------------------------------------------------------|:-----------------:|-----------------------------------------------------------------------------------------|
| `LDN_HOST`          | The address the server should be listening on.                          |    `"0.0.0.0"`    |                                                                                         |
| `LDN_PORT`          | The port the server should be using.                                    |      `30456`      |                                                                                         |
| `LDN_GAMELIST_PATH` | The path to the file containing a mapping of application ids and names. | `"gamelist.json"` | This can be a relative or an absolute path.                                             |
| `LDN_REDIS_SOCKET`  | The path to the unix socket of the redis server to connect to.          |       `""`        | If this is variable is not empty `LDN_REDIS_HOST` and `LDN_REDIS_PORT` will be ignored. |
| `LDN_REDIS_HOST`    | The address of the redis server to connect to.                          |   `"127.0.0.1"`   |                                                                                         |
| `LDN_REDIS_PORT`    | The port of the redis server to connect to.                             |      `6379`       |                                                                                         |

## Proxy

Ryujinx uses an application layer proxy to simulate communications in a network, which directs traffic created by the C# socket to a proxy server, which routes and sends it to the relevant parties. By default, Ryujinx attempts to create a proxy server on the network owner's system, expose a port to the internet, and tell joiners to connect to it. If this does not work, the proxy is done through the master server itself, using a lot of the same mechanisms.

## Analytics via Redis (JSON objects)

- `ldn`<br />
  This provide global analytics over the whole server.<br />
  E.g.:<br />
  ```json
  {
      "total_game_count": 4,
      "private_game_count": 1,
      "public_game_count": 3,
      "in_progress_count": 0,
      "master_proxy_count": 0,
      "total_player_count": 10,
      "private_player_count": 3,
      "public_player_count": 7
  }
  ```
  
- `games`<br />
  This provide detailed analytics over all games. Only public games are available.<br />
  E.g.:<br />
  ```json
  {
      "d2d0244c2b4544d7a26cd051e0e993e9": {
          "id": "d2d0244c2b4544d7a26cd051e0e993e9",
          "player_count": 3,
          "max_player_count": 4,
          "game_name": "Puyo Puyo Tetris",
          "title_id": "010053d0001be000",
          "mode": "P2P",
          "status": "Joinable",
          "players": [
              "Player1",
              "Player2",
              "Player3"
          ]
      }
  }
  ```
