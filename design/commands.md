---
id: design-commands
---

# コマンド

クライアントからサーバーへ送信するコマンドです。

すべて `\0` null terminate された json 文字列をやり取りします.

このコマンドの json には、必ず `cmd` キーを含みます。

また、一部のコマンドには送信したコマンドに対するレスポンスがあります。
レスポンスの json には, `response` キーにこの `cmd` の値が格納されます.

要求するとき:

```json
{ "cmd": "<command>" }
```

サーバーからの返答があるとき:

```json
{ "response": "<command>" }
```

* TOC
{:toc}

## hello

**全てのクライアントが必ず 1 回送信**

サーバーに新規接続を知らせます.

このデータを受け取った場合, すべての display, controller モードのクライアントへ `new_client` event データを送信します.

詳しくは, [イベントの TX/new_client](./events.md#new_client) を参照してください.

| key    | type                       | nullable | value                             |
| ------ | -------------------------- | -------- | --------------------------------- |
| user   | `string`                   | no       | [ユーザー名](client.md#クライアントユーザー)     |
| mode   | `uint8`                    | no       | [クライアントモード](client.md#クライアントモード)  |
| label  | `string`                   | yes      | クライアントのラベル。表示向けに使われる情報で、単なるラベルです。 |
| config | `ConfigValueDescription[]` | yes      | このクライアントの Config 情報               |

### ConfigValueDesctiption

`config` は, このクライアントにおいて設定可能なコンフィグ一覧であり, RestAPI やその他通信を用いて設定を更新することができます.

キーはネストすることが無く、`.` で名前空間を表現したフラットなデータ構造になります。

| key      | type              | nullable | desc                           |
| -------- | ----------------- | -------- | ------------------------------ |
| key      | `string`          | no       | json キー                        |
| docs     | `string`          | no       | この Config 項目に対する（簡単な）ドキュメント文字列 |
| type     | `ConfigValueType` | no       | json の値の型                      |
| nullable | `boolean`         | no       | nullable な項目かどうか               |

#### ConfigValueType

| value | desc      |
| ----- | --------- |
| 0     | `boolean` |
| 1     | `double`  |
| 2     | `uint64`  |
| 3     | `string`  |

### Sensor クライアントの送信例

```json
{
    "cmd": "hello",
    "user": "user",
    "mode": 1,
    "config": [
        {
            "key": "testConfig",
            "docs": "テスト用",
            "type": 0, // boolean
            "nullable": false
        }
    ]
}
```

## bye

**全てのクライアントが必ず 1 回送信**

サーバーに切断を要求します.

```json
{ "cmd": "bye" }
```

## request_clients

**全てのクライアントが任意に送信**

サーバーに接続しているすべてのクライアントの情報を取得します.

```json
{ "cmd": "request_clients" }
```

サーバーからの応答は以下の通りです.

```json
{
    "response": "request_clients",
    "clients": [
        {
            "clientID": <clientID>,
            "user": "<user>",
            "label": "<label>",
            "addr": "<address>",
            "mode": <mode>,
            "config": [<ConfigValueDesctiption>]
        }, ...
    ]
}
```

## update_config

**全てのクライアントが任意に送信**

指定したクライアントの config を更新します.

```json
{
    "cmd": "update_config",
    "clientID": <clientID>,
    "config": {
        "<key>": "new value",
        ...
    }
}
```

config キーに, 設定項目の key と, 新規に設定する値を指定します.

すべての設定可能なコンフィグキーを列挙する必要はありません. 列挙しても良いですし, 設定変更をしたい項目だけでも良いです. クライアントに設定可能なコンフィグキー以外の値は無視されます. また型などが異なる場合も無視されます.

サーバーはこのメッセージを受け取った後, 直接対象のクライアントへ転送します。このコマンドは常に成功しますが、実際に値が採用されるかどうかは対象のクライアントによります。

対象のクライアントはサーバーからイベントを受け取ります。詳細は [TX/update_config](./events.md#update_config) を参照してください.

## update_data

**sensor モードのクライアントが, データ更新時に送信**

主に sensor モードのデバイスが送るコマンドです. データを更新します.

このデータは、送信元のクライアントのユーザー名を listen している controller, display に対してそのまま [TX/update_data](./events.md#update_data) で転送されます。
そのため、常に完全データを送信してください。

```json
{
    "cmd": "update_data",
    "data": {
        "<key>": "value",
        ...
    }
}
```

このデータ形式は, クライアントモードによって異なり, あるクライアントモードにおいて固有です.

`data` は `config` と同じく, `.` で名前空間を表現された階層のない構造です.

## execute

**controller モードのクライアントが, 操作を指示するときに送信**

指定したユーザーの execute モードのクライアントに対して, 操作を要求します.

```json
{
    "cmd": "execute",
    "user": "<user>",
    "execute": [<ExecuteCommands>]
}
```

`ExecuteCommands` については, [ExecuteCommands](execute_command.md) を参照してください.

このコマンドを受けたサーバーは, executor, display モードのクライアントに対して `execute` イベントを送信します. [TX/execute](./events.md#execute) の項を参照してください.

## executed

**executor モードのクライアントが, 操作を完了したときに送信**

executor モードのクライアントが送信します.

`execute` イベントによって操作要求をされた executor モードのクライアントが, そのスケジュールにしたがって実際に操作を行ったときに送信されます.

```json
{
    "cmd": "executed",
    "emulated": [<ExecuteCommands>]
}
```

このデータは, （ほぼ）同時に実行された操作を表した [ExecuteCommands](execute_command.md) が含まれます.

このコマンドを受け取ったサーバーは, すべての display モードのクライアントに対して `executed` イベントを送信します.
イベントの詳細は [TX/executed](./events.md#executed) を参照してください。
