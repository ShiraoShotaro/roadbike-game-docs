---
id: client-emulator-minecraft-seed
---

# Minecraft Emulator "Seed"

マインクラフトの MOD によるエミュレータ。

同じような MOD エミュレータは増えていくだろうと、とりあえずコードネームとして「Seed」という名前をつけた。

## Emulate Commands

### minecraft-sleep-ntick

指定した tick 数だけコマンドをスリープします。（実行をブロックします。）

| key  | type   | nullable | desc                          |
| ---- | ------ | -------- | ----------------------------- |
| cmd  | string | no       | `minecraft-seed-sleep-n-tick` |
| tick | int32  | no       | tick 数                       |

### minecraft-heal

指定した量だけ体力を回復します。

| key    | type   | nullable | desc                                                 |
| ------ | ------ | -------- | ---------------------------------------------------- |
| cmd    | string | no       | `minecraft-seed-heal`                                |
| heal   | float  | yes      | 回復量。負の値でダメージ。                           |
| health | float  | yes      | 絶対値指定。こちらを指定した場合 heal は無視される。 |
