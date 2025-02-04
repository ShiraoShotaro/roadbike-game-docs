# Roadbike-Game Documentation

けんこうだいいち。

{:toc}

## ClientID

クライアント ID は, サーバーへ接続してきたクライアントに対して割り当てる ID です. 型は 4bytes unsigned int で, 単純なインクリメントによって管理されます.

## クライアントモード

| mode       | uint32 value | description                                                           |
| ---------- | ------------ | --------------------------------------------------------------------- |
| sensor     | 0            | センサー. ANT+ センサーや HID など.                                   |
| controller | 1            | コントローラ. センサーの値をもとにゲームをコントロールするプログラム. |
| emulator   | 2            | アウトプット. ゲームに対して入力をエミュレートする.                   |
| display    | 3            | ディスプレイ. UI など.                                                |

## TCP 接続方法

以下, 通信方向はサーバー視点とし, TX はサーバーからクライアント, RX はクライアントからサーバーの通信とします.

また, ClientID は現在 4bytes LE unsigned int であるとして説明します.

1. クライアントはサーバーへ Socket 接続を行います.
1. クライアントはその Socket を通じて, 4bytes の `00 00 00 00` を送信します.
1. サーバーはそれを新しいクライアントからの接続として認識し, 以降 TX Socket として利用します.
1. サーバーは TX Socket へ, クライアントに割り当てた ClientID 4bytes を送信します.
1. クライアントはその ClientID 4bytes を受け取ります.
1. クライアントはサーバーへ新たに Socket 接続を行います.
1. クライアントはその Socket に, 割り当てられた ClientID 4bytes を送信します.
1. サーバーは送られてきた ClientID 4bytes をもとに, TX Socket とのペアリングを行います.
1. 正しくペアリングできた場合, サーバーはこの Socket へ ClientID 4bytes をエコー返送し, その後は RX Socket としてデータ受信を待ちます.

## プロトコル

「コマンド+レスポンス」と「イベント」の区分があります。

### コマンド+レスポンス

クライアントからサーバーへ送信するコンテンツを「コマンド」、それに対するサーバーからの返答が「レスポンス」です。レスポンスがないコマンドもあります。

[コマンド一覧へ](commands/index.md)

### イベント

サーバーから送信されるコンテンツが「イベント」です。

[イベント一覧へ](events/index.md)

## クライアント

-   [Sensor](sensor/index.md)
    -   値を publish するクライアント
-   [Controller](controller/index.md)
    -   Sensor の値によってゲームロジックに落とし込むクライアント
-   [Emulator](emulator/index.md)
    -   Controller から指示を受けて実際にゲームを操作するクライアント
-   [Display](display/index.md)
    -   デバッグや UI 表示のために全てのデータを subscribe するクライアント
