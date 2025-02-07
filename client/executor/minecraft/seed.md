---
id: client-executor-minecraft-seed
---

# Minecraft "Seed" Executor

マインクラフトの MOD による Executor.

同じような MOD エミュレータは増えていくだろうと、とりあえずコードネームとして「Seed」という名前をつけた。

- TOC
{:toc}

## Execute Commands

### MinecraftSeed-SleepNTick

指定した tick 数だけコマンドをスリープします。（キューの実行をブロックします。）

| key  | type     | nullable | default | desc                       |
| ---- | -------- | -------- | ------- | -------------------------- |
| cmd  | `string` | no       |         | `MinecraftSeed-SleepNTick` |
| tick | `int32`  | no       |         | tick 数                     |

```json
{
    "cmd": "MinecraftSeed-SleepNTick",
    "tick": 1
}
```

### MinecraftSeed-EarnExperience

経験値を獲得します。

| key        | type     | nullable | default | desc                           |
| ---------- | -------- | -------- | ------- | ------------------------------ |
| cmd        | `string` | no       |         | `MinecraftSeed-EarnExperience` |
| experience | `int32`  | no       |         | 経験値数                           |

```json
{
    "cmd": "MinecraftSeed-EarnExperience",
    "experience": 1
}
```

### MinecraftSeed-GiveItem

アイテムをインベントリに与えます。

インベントリが満杯の場合、このコマンドは作用しません。ドロップもしません。

| key      | type     | nullable | default | desc                               |
| -------- | -------- | -------- | ------- | ---------------------------------- |
| cmd      | `string` | no       |         | `MinecraftSeed-GiveItem`           |
| itemName | `string` | no       |         | アイテム名。（デバッグ表示でアイテムのツールチップで見られるアレ。） |
| count    | `int`    | no       |         | 個数                                 |

```json
{
    "cmd": "MinecraftSeed-GiveItem",
    "itemName": "minecraft:dirt",
    "count": 1
}
```

### MinecraftSeed-RepairHandTool

手に持っている道具の耐久度を回復します。

| key      | type      | nullable | default | desc                                       |
| -------- | --------- | -------- | ------- | ------------------------------------------ |
| cmd      | `string`  | no       |         | `MinecraftSeed-RepairHandTool`             |
| repair   | `int`     | no       |         | 修復するダメージ数。負の値を指定しても効果は無い。最大耐久値を超えた分は無視される。 |
| mainHand | `boolean` | yes      | `true`  | `true` でメインの手                              |

```json
{
    "cmd": "MinecraftSeed-RepairHandTool",
    "repair": 1,
    "mainHand": true
}
```

### MinecraftSeed-SendChat

プレイヤーからチャットを送ります。

| key       | type      | nullable | default | desc                     |
| --------- | --------- | -------- | ------- | ------------------------ |
| cmd       | `string`  | no       |         | `MinecraftSeed-SendChat` |
| message   | `string`  | no       |         | メッセージ文章                  |
| color.r   | `int`     | yes      | `255`   | 色の R 成分                  |
| color.g   | `int`     | yes      | `255`   | 色の G 成分                  |
| color.b   | `int`     | yes      | `255`   | 色の B 成分                  |
| bold      | `boolean` | yes      | `false` | 太字                       |
| italic    | `boolean` | yes      | `false` | 斜体                       |
| underline | `boolean` | yes      | `false` | 下線                       |
| strike    | `boolean` | yes      | `false` | 打消し線                     |

```json
{
    "cmd": "MinecraftSeed-SendChat",
    "message": "This is chat text.",
    "color.r": 255,
    "color.g": 255,
    "color.b": 255,
    "bold": true,
    "italic": true,
    "underline": true,
    "strike": true
}
```

### MinecraftSeed-SendMessage

サーバーからメッセージを送信します。

| key       | type      | nullable | default | desc                        |
| --------- | --------- | -------- | ------- | --------------------------- |
| cmd       | `string`  | no       |         | `MinecraftSeed-SendMessage` |
| message   | `string`  | no       |         | メッセージ文章                     |
| color.r   | `int`     | yes      | `255`   | 色の R 成分                     |
| color.g   | `int`     | yes      | `255`   | 色の G 成分                     |
| color.b   | `int`     | yes      | `255`   | 色の B 成分                     |
| bold      | `boolean` | yes      | `false` | 太字                          |
| italic    | `boolean` | yes      | `false` | 斜体                          |
| underline | `boolean` | yes      | `false` | 下線                          |
| strike    | `boolean` | yes      | `false` | 打消し線                        |

```json
{
    "cmd": "MinecraftSeed-SendChat",
    "message": "This is chat text.",
    "color.r": 255,
    "color.g": 255,
    "color.b": 255,
    "bold": true,
    "italic": true,
    "underline": true,
    "strike": true
}
```

### MinecraftSeed-SetHealth

体力を設定します。

| key    | type   | nullable | default | desc                            |
| ------ | ------ | -------- | ------- | ------------------------------- |
| cmd    | string | no       |         | `MinecraftSeed-SetHealth`       |
| heal   | float  | yes      |         | 回復量。現在の値から相対値指定。負の値でダメージ。       |
| health | float  | yes      |         | 絶対値指定。こちらを指定した場合 `heal` は無視される。 |

```json
{
    "cmd": "MinecraftSeed-SetHealth",
    "heal": 0.0,
    "health": 20.0
}
```

### MinecraftSeed-SetHunger

満腹度を設定します。

| key            | type     | nullable | default | desc                                                 |
| -------------- | -------- | -------- | ------- | ---------------------------------------------------- |
| cmd            | `string` | no       |         | `MinecraftSeed-SetHunger`                            |
| healFoodLevel  | `int`    | yes      | `null`  | Food Level 回復度(相対値)                                  |
| healSaturation | `float`  | yes      | `null`  | Saturation 回復度(相対値)                                  |
| foodLevel      | `int`    | yes      | `null`  | Food Level の値(絶対値), 指定した場合 `healFoodLevel` は無視されます。  |
| saturation     | `float`  | yes      | `null`  | Saturation の値(絶対値), 指定した場合 `healSaturation` は無視されます。 |

```json
{
    "cmd": "MinecraftSeed-SetHunger",
    "healFoodLevel": 0,
    "healSaturation": 0.0,
    "foodLevel": 10,
    "saturation": 10
}
```
