# Arietta Game Pack Index

[Arietta](https://github.com/PaperKito/Arietta)「游戏包」的默认浏览与下载索引，GitHub Pages 托管：

```
https://arietta-gamepacks.github.io/index/index.json
```

宿主侧「浏览并下载」读这个地址；索引地址在宿主设置里可配置，允许自建多源。索引只存元数据，不存游戏资源；包 zip 一律放各仓库的 GitHub Release（免登录直链下载）。

## index.json 字段

| 字段 | 说明 |
| --- | --- |
| `version` | 索引格式版本，当前 `1` |
| `packs[].id` | 包 id，小写字母/数字/连字符，等于安装目录名 |
| `packs[].names` | 界面语言 tag → 标题 |
| `packs[].author` | 作者名（与包内 `signature.json` 的 `author` 一致） |
| `packs[].publicKey` | 作者公钥（SPKI base64），宿主用于校验 zip 内 `signature.json`，对不上即拒绝安装 |
| `packs[].version` | 包版本 |
| `packs[].api` | provider 协议版本 |
| `packs[].capabilities` | 能力声明，与包内 `game.json` 一致 |
| `packs[].repo` | 包源码仓库 |
| `packs[].download` | zip 直链（指向 Release 资产） |
| `packs[].sha256` | zip 的 sha256 |

## 提交你的包

1. 按《游戏包开发规范》组织包目录，用 `tools/GamePackSign` 签名（`signature.json` 必须在 zip 内）；
2. 包目录 zip 后（`<id>-<version>.zip`，文件在 zip 根，含空目录占位文件）传到你自己的 GitHub Release；
3. PR 修改本仓库 `index.json`：填上表字段，`publicKey` 换成你自己的。

校验链：索引下发 `publicKey` → 下载 zip → 校验 `sha256` → 解包校验 `signature.json` 签名。任何一环对不上，宿主拒绝安装。
