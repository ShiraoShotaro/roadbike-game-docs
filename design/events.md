---
id: design-events
---

# イベント

サーバーからクライアントへ送信するコマンドです。

すべて `\0` null terminate された json 文字列をやり取りします.

このコマンドの json には、必ず `event` キーを含みます。

サーバーからのデータ:

```json
{ "event": "<event>" }
```

サーバーからクライアントへの通信では, 必ず `event` キーが含まれます.

* TOC
{:toc}

## new_client

**全てのクライアントへ送信**

新しいクライアントから `hello` コマンドをサーバーが受け取った後, すべての display, controller モードのクライアントへ以下のデータを送信します.

```json
{
    "event": "new_client",
    "info": {
        "user": "<user>",
        "mode": <mode>,
        "label": "<label>",
        "addr": "<address>",
        "clientID": <clientID>,
        "config": [<ConfigValueDescription>]
    }
}
```

### info について

| key      | type                       | value                                                       |
| -------- | -------------------------- | ----------------------------------------------------------- |
| clientID | `uint32`                   | クライアント固有の ID. ほかのオペレーションでクライアントを指定するときに利用します.               |
| user     | `string`                   | [クライアントユーザ名](client.md#クライアントユーザー)                          |
| mode     | `uint8`                    | [クライアントモード](client.md#クライアントモード)                            |
| label    | `string`                   | クライアントのラベル                                                  |
| addr     | `string`                   | 識別のためのアドレス. このアドレスは表示のためのもので, ほかのオペレーションにおいてこの値を使うことはありません. |
| config   | `ConfigValueDescription[]` | Config Value Descriptions                                   |

## leave_client

**全てのクライアントへ送信**

クライアントとの接続が切断された時に, すべての display, controller モードのクライアントへ以下のデータを送信します.

```json
{
    "event": "leave_client",
    "clientID": <clientID>
}
```

## update_config

**特定のクライアントへ送信**

他クライアントから自分のクライアントへ, 設定の更新の要求が行われたときに送信されるイベントです.

```json
{
    "event": "update_config",
    "config": {
        "key": "value"
    }
}
```

受け取った値でコンフィグを更新します.

## update_data

**controller, display モードのクライアントへ送信**

主に sensor モードのクライアントがデータを更新したときに, サーバーから, すべての display, controller モードのクライアントへ送信されるイベントです.

```json
{
    "event": "update_data",
    "clientID": <clientID>,
    "data": {
        "key": "value",
        ...
    }
}
```

ここで受信できるデータは, クライアントが通知したデータと同じもので, 差分の可能性のあるデータです. 完全データを取得する場合は, 別途 `request_fulldata` コマンドを利用します.

## execute

**executor, display モードのクライアントへ送信**

主に controller モードのクライアントから `execute` コマンドを受信したときに, サーバーから, 指定されたクライアント ID の executor と, すべての display デバイスに対して操作要求を転送します.

```json
{
    "event": "execute",
    "user": "<user>",
    "emulate": [<ExecuteCommand>]
}
```

`ExecuteCommand` については, [ExecuteCommand](./execute_command.md) の章を参照してください. // TODO:

## executed

**display モードのクライアントへ送信**

主に executor モードのクライアントが, 実際に操作を実行した後に `executed` コマンドを発行します.
それを受け取ったサーバーは, この `executed` イベントを, すべての display デバイスに対して送信します.

```json
{
    "event": "executed",
    "clientID": <clientID>,
    "emulated": [<ExecutedCommands>]
}
```
