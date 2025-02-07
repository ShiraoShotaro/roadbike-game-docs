---
id: client-executor
---

# Executor

Controller からの制御指示を受け取って、Controller に代わって特定の操作をゲームに対して行う。

~~人間の入力操作を代わりに実行する、という意味で Executor としたけど、ちょっと違うかもしれない。~~

Executor という名前になりました。

DirectInput によるキーボード、マウス操作、物理外部デバイスによるキーボード、マウス操作、ゲーム内 mod による操作などを行うクライアント。

## Debug Web Panel からのデバッグ方法

Debug Web Panel には、任意のタイミングで任意の Execute コマンドを発行するデバッグ機能があります。

使い方は[こちら](execute_debug.md)を参照してください。

## Executors

- Minecraft
  - [Minecraft Seed Executor](./minecraft/seed.md)
    - Minecraft MOD として作成された、 Minecraft に作用する executor.
