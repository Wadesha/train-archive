# data · 数据来源与分片仓库

存放**网络核对所用的大数据源的加工结果**。原始文件体积大（如 12306 官方快照约 15MB），故按逻辑分片后存入本仓库，便于追溯与复算，网页本身并不加载这些文件（站点仍由 `index.html` 单文件承载）。

## 目录结构

```
data/
├─ README.md                        # 本说明
└─ 12306-trainlist/
   └─ 2022-08/
      ├─ MANIFEST.json              # 分片清单（来源/快照日期/汇总/每片条数）
      ├─ D.txt  G.txt  C.txt  K.txt  Z.txt  T.txt  S.txt  Y.txt  L.txt
      ├─ NUM.txt                    # 纯数字车次（5619 等）
      └─ other.txt                  # 其它字头
```

## 分片逻辑

- **来源**：12306 官网 `https://kyfw.12306.cn/otn/resources/js/query/train_list.js`
- **快照**：`2022-08-19 — 2022-08-29`（该静态文件的 16 个日期）
- **加工**：聚合全部日期 → 按 `station_train_code` 去重（共 **12,996** 车次）→ 按车次字头分片（`D/G/C/K/Z/T/S/Y/L/纯数字/其它`）
- **每片内容**：一行一条唯一 `车次(起站-止站)`，按字典序排列
- **体量**：原文件约 15MB → 分片后约 **356KB**

## 与网页的关系

网页里的 **车次核验来源标注**（列车详情页「核验来源」）即基于本快照交叉核对得出，见 `SEED_TRAINS` 相关 `source` 字段。三来源综述见仓库主 `README.md` 与本节。

## 其它参考文献

- `sjfkai/grab12306` —— Node 抓取工具，输出 `station_list.json`、车次与时刻表 JSON（与本快照同构）。
- `wj0575/RailRhythm12306` —— Python 工具 + `train_data`（2025-01~03 逐日车次快照）与 `global_data/city_station.json`（城市↔站点），时效更新；此类第三方仓库受其各自许可证约束，仅作参考，本仓库未转载其数据。