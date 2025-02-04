# PedalPower

ペダル型のパワーメータに関する型情報です.

## PedalPowerConfig

| key            | type    | default | desc                                                            |
| -------------- | ------- | ------- | --------------------------------------------------------------- |
| b.highRateMode | boolean | false   | ハイレートモード (8Hz) を有効にします. デフォルトでは 4Hz です. |

## PedalPowerData

| key               | type   | unit    | desc                             |
| ----------------- | ------ | ------- | -------------------------------- |
| \_class           | uint8  |         | 1                                |
| d.power           | double | W       | 瞬間的なパワー                   |
| d.power.rate      | dobule | %       | 瞬間的なパワー, FTP 割合         |
| d.power.pwr       | dobule | %       | 瞬間的なパワー, PowerWeightRatio |
| d.cadence         | double | RPM     | 瞬間的なケイデンス               |
| u64.crankTick.acc | uint64 | 1/2048s | 累計クランク tick                |
| u64.torque.acc    | uint64 | 1/32Nm  | 累計トルク                       |
| d.powerBalance    | double | %       | バランス, 右が +                 |
| d.power.ave1      | double | W       | 1 秒平均パワー                   |
| d.power.ave1.rate | double | %       | 1 秒平均パワー, FTP 割合         |
| d.power.ave1.pwr  | double | %       | 1 秒平均パワー, PowerWeightRatio |
| d.power.ave3      | double | W       | 3 秒平均パワー                   |
| d.power.ave3.rate | double | W       | 3 秒平均パワー, FTP 割合         |
| d.power.ave3.pwr  | double | W       | 3 秒平均パワー, PowerWeightRatio |
| d.power.ave5      | double | W       | 5 秒平均パワー                   |
| d.power.ave5.rate | double | %       | 5 秒平均パワー, FTP 割合         |
| d.power.ave5.pwr  | double | %       | 5 秒平均パワー, PowerWeightRatio |
| d.power.ave10     | double | W       | 10 秒平均パワー                  |
| d.te.left         | double | %       | 左トルク効率                     |
| d.te.right        | double | %       | 右トルク効率                     |
| d.ps.left         | double | %       | 左ペダリングスムースネス         |
| d.ps.right        | double | %       | 左ペダリングスムースネス         |
| d.batt.pedalPower | double | %       | バッテリー残量                   |
