---
id: client-executor
---

# Executor

Controller からの制御指示を受け取って、Controller に代わって特定の操作をゲームに対して行う。

人間の入力操作を代わりに実行する、という意味で Executor としたけど、ちょっと違うかもしれない。

DirectInput によるキーボード、マウス操作、物理外部デバイスによるキーボード、マウス操作、ゲーム内 mod による操作などを行うクライアント。

## Executors

-   Minecraft
    -   [Minecraft Seed Executor](./minecraft/seed.md)
