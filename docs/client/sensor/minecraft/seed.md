---
id: client-sensor-minecraft-seed.md
---

# Minecraft Seed Sensor

Seed はただのコードネーム。バージョンみたいなもの。

## Config

```json
{
    "_class": 256,
    "tickInterval": 5
}
```

| key          | type  | nullable | default value | desc                       |
| ------------ | ----- | -------- | ------------- | -------------------------- |
| \_class      | `int` | no       | `0x0100`      | クラス ID                  |
| tickInterval | `int` | no       | `5`           | データ送信を行う Tick 間隔 |

## Data

```json
{
    "_class": 256,
    "age": <int>,
    "air": <int>,
    "air.max": <int>,
    "dead": <boolean>,
    "exp.lv": <int>,
    "exp.prog": <float>,
    "food.lv": <int>,
    "food.sat": <float>,
    "gliding": <boolean>,
    "hand.main.count": <int>,
    "hand.main.damage": <int>,
    "hand.main.item": "<itemName>",
    "hand.main.count.max": <int>,
    "hand.main.damage.max": <int>,
    "hand.off.count": <int>,
    "hand.off.damage": <int>,
    "hand.off.item": "<itemName>",
    "hand.off.maxCount": <int>,
    "hand.off.maxDamage": <int>,
    "health": <int>,
    "health.max": <int>,
    "onGround": <boolean>,
    "playerName": "<PlayerName>",
    "playerUUID": "<UUID>",
    "sneaking": <boolean>,
    "swim": <boolean>,
    "velo.x": <double>,
    "velo.y": <double>,
    "velo.z": <double>,
    "yaw.body": <double>,
    "yaw.head": <double>
}
```

| key                  | type      | nullable | desc                                           |
| -------------------- | --------- | -------- | ---------------------------------------------- |
| \_class              | `int`     | no       | クラス ID (`0x0100`)                           |
| age                  | `int`     | no       | データ送信を行う Tick 間隔                     |
| air                  | `int`     | no       | 酸素                                           |
| air.max              | `int`     | no       | 最大酸素量                                     |
| dead                 | `boolean` | no       | 死んでいるか                                   |
| exp.lv               | `int`     | no       | 経験値レベル                                   |
| exp.prog             | `float`   | no       | 経験値のレベル内進捗                           |
| food.lv              | `int`     | no       | 空腹度                                         |
| food.sat             | `float`   | no       | 隠し空腹度                                     |
| gliding              | `boolean` | no       | エリトラなど, グライディング中                 |
| hand.main.item       | `int`     | no       | メインの手に持っているアイテムのダメージ量     |
| hand.main.count      | `int`     | no       | メインの手に持っているアイテムの所持量         |
| hand.main.count.max  | `int`     | no       | メインの手に持っているアイテムの最大所持量     |
| hand.main.damage     | `int`     | no       | メインの手に持っているアイテムのダメージ量     |
| hand.main.damage.max | `int`     | no       | メインの手に持っているアイテムの最大ダメージ量 |
| hand.off.item        | `int`     | no       | オフハンドに持っているアイテムのダメージ量     |
| hand.off.count       | `int`     | no       | オフハンドに持っているアイテムの所持量         |
| hand.off.count.max   | `int`     | no       | オフハンドに持っているアイテムの最大所持量     |
| hand.off.damage      | `int`     | no       | オフハンドに持っているアイテムのダメージ量     |
| hand.off.damage.max  | `int`     | no       | オフハンドに持っているアイテムの最大ダメージ量 |
| health               | `float`   | no       | 体力                                           |
| health.max           | `float`   | no       | 最大体力                                       |
| onGround             | `boolean` | no       | 地面に立っているか                             |
| playerName           | `string`  | no       | プレイヤー名                                   |
| playerUUID           | `string`  | no       | プレイヤー UUID                                |
| sneaking             | `boolean` | no       | スニーキング中                                 |
| swim                 | `boolean` | no       | 泳ぎ中                                         |
| velo.x               | `double`  | no       | 速度 X                                         |
| velo.y               | `double`  | no       | 速度 Y                                         |
| velo.z               | `double`  | no       | 速度 Z                                         |
| yaw.body             | `double`  | no       | 体の向き                                       |
| yaw.head             | `double`  | no       | 頭の向き                                       |
