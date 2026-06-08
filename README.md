# Collapse 自动更新 Fork

## 用途

这是一个自用 fork，不是官方发布仓库。

主要改动是通过命令行debug模式启动游戏时可以自动处理游戏更新：

- 游戏已安装且不需要更新：直接启动游戏。
- 游戏已安装但需要更新：先自动更新，更新完成后再启动游戏。
- 游戏未安装、安装损坏、游戏或区域参数无效：不会强制启动。

当前构建未签名，Windows 可能会显示未知发布者或 SmartScreen 提示。

## 自动更新启动用法

短参数写法：

```powershell
.\CollapseLauncher.exe open -g "<game>" -r "<region>" -p
```

完整参数写法：

```powershell
.\CollapseLauncher.exe open --game "<game>" --region "<region>" --play
```

`-p` / `--play` 是自动启动开关。写上它之后，如果当前游戏需要更新，启动器会先更新，更新完成后再启动游戏。

如果不写 `-p` / `--play`，只会打开启动器并切换到指定游戏/区域，不会自动启动游戏。

## 示例

```powershell
.\CollapseLauncher.exe open -g "Genshin Impact" -r "Global" -p
.\CollapseLauncher.exe open -g "Honkai Impact 3rd" -r "Southeast Asia" -p
.\CollapseLauncher.exe open -g "Honkai: Star Rail" -r "Global" -p
.\CollapseLauncher.exe open -g "Zenless Zone Zero" -r "Global" -p
```

## URL 协议写法

可以用 `collapse://open` 触发同样的自动更新启动流程。协议参数里建议把空格写成 `%20`，引号写成 `%22`。

```text
collapse://open%20-g%20%22Genshin%20Impact%22%20-r%20%22Global%22%20-p
collapse://open%20-g%20%22Honkai:%20Star%20Rail%22%20-r%20%22Global%22%20-p
```

上面两条分别等价于：

```powershell
.\CollapseLauncher.exe open -g "Genshin Impact" -r "Global" -p
.\CollapseLauncher.exe open -g "Honkai: Star Rail" -r "Global" -p
```

## 参数取值范围

### `--game` / `-g`

推荐直接使用下面的内部英文名称：

| 游戏 | `--game` 取值 |
| --- | --- |
| 崩坏 3 | `"Honkai Impact 3rd"` |
| 原神 | `"Genshin Impact"` |
| 崩坏：星穹铁道 | `"Honkai: Star Rail"` |
| 绝区零 | `"Zenless Zone Zero"` |

也可以使用游戏序号，范围是 `0` 到 `当前加载游戏数量 - 1`。不推荐使用序号，因为顺序来自启动器运行时 metadata，后续可能变化。

### `--region` / `-r`

推荐直接使用下面的内部英文区域名称：

| `--game` | 常用 `--region` 取值 |
| --- | --- |
| `"Honkai Impact 3rd"` | `"Southeast Asia"`, `"Global"`, `"Mainland China"`, `"TW/HK/MO"`, `"Korea"`, `"Japan"` |
| `"Genshin Impact"` | `"Global"`, `"Mainland China"`, `"Bilibili"`, `"Google Play"` |
| `"Honkai: Star Rail"` | `"Global"`, `"Mainland China"`, `"Bilibili"` |
| `"Zenless Zone Zero"` | `"Global"`, `"Mainland China"`, `"Bilibili"` |

也可以使用区域序号，范围是 `0` 到 `所选游戏区域数量 - 1`。不推荐使用序号，因为顺序来自启动器运行时 metadata，后续可能变化。

如果省略 `--region` / `-r`，启动器会使用该游戏已保存的默认区域。

## 注意事项

- 名称匹配不区分大小写，但推荐照表复制。
- 名称包含空格、冒号或 `/` 时必须加引号。
- 可用游戏和区域最终以启动器运行时加载的 metadata 和插件配置为准。
- 无效游戏或区域不会创建新配置，启动器会保持或回退到当前有效选择。
- `-p` / `--play` 不会强制启动未安装或损坏的游戏。
