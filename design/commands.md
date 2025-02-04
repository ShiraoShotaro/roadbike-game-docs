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

## hello

**全てのクライアントが必ず 1 回送信**

サーバーに新規接続を知らせます.

```json
{
    "cmd": "hello",
    "user": "<user>",
    "mode": <ClientMode>,
    "dataClassType": <DataClassType>,
    "requestFulldata": <bool>,
    "config": <config>
}
```

| key             | type     | nullable | value                                                                                |
| --------------- | -------- | -------- | ------------------------------------------------------------------------------------ |
| user            | `string` | no       | [ユーザー名](client.md#クライアントユーザー)                                         |
| mode            | `uint8`  | no       | [クライアントモード](client.md#クライアントモード)                                   |
| dataClassType   | `uint16` | yes      | データクラスタイプ                                                                   |
| requestFulldata | `bool`   | no       | Controller, Display において, 毎回のデータ更新時に完全データを送ってもらうようにする |
| config          | `Config` | yes      | このクライアントの Config 情報                                                       |

`config` は, このクライアントにおいて設定可能なコンフィグ一覧であり, RestAPI やその他通信を用いて設定を更新することができます.
`hello` コマンドでは, その設定キーとデフォルト値 (nullable) をサーバーへ通知します.

このデータを受け取った場合, すべての display, controller モードのクライアントへ `new_client` event データを送信します.

詳しくは, イベントの TX/new_client を参照してください.

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
            "addr": "<address>",
            "mode": <mode>,
            "dataClassType": <DataClassType>
        }, ...
    ]
}
```

## request_config

**全てのクライアントが任意に送信**

指定したクライアント ID の設定可能な項目を取得します.

```json
{"cmd": "request_config", "clientID": <clientID>}
```

サーバーからの応答は以下の通りです.

```json
{
    "response": "request_config",
    "clientID": <clientID>,
    "config": {
        "<key>": "<value>",
        ...
    }
}
```

config はネストされることはなく, `.` で名前空間が表現されることがあります.

```json
{
    "config": {
        "aaa.x": 0,
        "aaa.y": 1,
        "aaa.z": 2,
        "bbb.hoge": "foobar"
    }
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

サーバーはこのメッセージを受け取った後, サーバーにてバリデーションを行い, 有効な値のみを指定のクライアントへ送信します. この時のイベントは TX/update_config を参照してください.

## update_data

**sensor モードのクライアントが, データ更新時に送信**

主に sensor モードのデバイスが送るコマンドです. データを更新します.

このデータは完全データか, 差分データを送信します. サーバーで完全データが常にキャッシュされます. そのため, データキーを削除することはできません.

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

このコマンドをサーバーが受け取った時, サーバーは `update_data` イベントを, すべての display / controller クライアントへ送信します.

## request_fulldata

**controller モードのクライアントが, (主に)起動時に送信**

指定したクライアントの持つデータの完全データを取得します.

```json
{
    "cmd": "request_fulldata",
    "clientID": <clientID>
}
```

display, controller モードのクライアントが, 接続後にデータを同期するときなどに使用します.

サーバーはキャッシュしている完全データを返します.

```json
{
    "response": "request_fulldata",
    "clientID": <clientID>,
    "data": {
        "key": "value",
        ...
    }
}
```

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

`ExecuteCommands` については, ExecuteCommands の章を参照してください.

このコマンドを受けたサーバーは, executor, display モードのクライアントに対して `emulate` イベントを送信します. TX/emulate の項を参照してください.

## emulated

**executor モードのクライアントが, 操作を完了したときに送信**

executor モードのクライアントが送信します.

`execute` イベントによって操作要求をされた executor モードのクライアントが, そのスケジュールにしたがって実際に操作を行ったときに送信されます.

```json
{
    "cmd": "executed",
    "emulated": [<EmulatedCommands>]
}
```

このデータは, （ほぼ）同時に実行された操作を表した `ExecutedCommands` が含まれます.

このコマンドを受け取ったサーバーは, すべての display モードのクライアントに対して `executed` イベントを送信します.
