# mactidy

macOS 的体检与清理脚本：看清是谁在占 CPU、内存、磁盘，再把不需要的启动项和应用清掉。单个 bash 文件，兼容系统自带的 bash 3.2，不需要安装依赖。

## 能做什么

| 命令 | 作用 | 会改动系统吗 |
|---|---|---|
| `mactidy` | 体检一次：CPU（含温度降频）、内存压力、各磁盘空间，按应用合并 Helper 子进程后排行，并给出建议 | 否 |
| `mactidy -w [秒]` | 持续刷新（默认 5 秒），Ctrl-C 退出 | 否 |
| `mactidy -n 数量` | 应用排行显示条数（默认 10） | 否 |
| `mactidy startup` | 列出启动项（登录项 / 后台代理 / 系统守护进程） | 否 |
| `mactidy startup off\|on 标签` | 关闭 / 恢复某个启动项（`launchctl disable`，随时可 `on` 恢复） | 是，先预览、确认后才改 |
| `mactidy apps [-d 天数]` | 列出应用的上次使用时间，标出超过 N 天（默认 90）没用的 | 否 |
| `mactidy jobs` | 列出后台定时任务：触发规则、状态、崩溃循环、日志新旧 | 否 |
| `mactidy uninstall 应用名` | 卸载应用：列出 `.app` 和 `~/Library` 下的残留，确认后**移到废纸篓**（不直接删除） | 是，先预览、确认后才改 |

## 安装

```bash
curl -fsSLo ~/.local/bin/mactidy https://raw.githubusercontent.com/nicholas-hoult/mactidy/master/mactidy
chmod +x ~/.local/bin/mactidy
```

或者直接克隆仓库后运行 `./mactidy`。

## 说明

- 体检和各种列表都是只读的；会改动系统的只有 `startup off/on` 和 `uninstall`，两者都会先列出要做什么，等你确认。
- `apps` 的「上次使用」是估算值（取 Spotlight 记录与偏好 / 窗口状态文件中较新的时间），只在菜单栏常驻的应用可能被低估。
- `uninstall` 的预览路径已测试；真实卸载请先看清预览列表再确认。

## 许可

MIT 或 Apache-2.0 双协议，任选其一（见 `LICENSE-MIT`、`LICENSE-APACHE`）。
