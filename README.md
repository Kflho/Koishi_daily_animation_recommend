# Koishi_daily_animation_recommend

基于 [Koishi](https://koishi.chat/) 的机器人部署项目：跑「每日动漫推荐」插件，接 QQ / KOOK / Discord 等聊天平台。默认可单机 SQLite + Web 控制台跑起来。

## 快速开始

需要 Node.js 18 以上和 Yarn 4（仓库自带 `.yarn/releases`，由 `packageManager` 字段固定版本）：

```bash
yarn install
cp .env.example .env
yarn start
```

Windows 的 `cp` 换成 `copy`。启动后控制台默认在 <http://localhost:5140> 并自动打开。

改插件代码想热重载时用 `yarn dev`（esbuild-register，不用先 build）。

## 安装与依赖

- Node.js 18+；Yarn 版本由 `.yarnrc.yml` 的 `yarnPath` 锁定；
- 包管理器走淘宝镜像（`.yarnrc.yml` 的 `npmRegistryServer`），国内直连可装；
- 业务插件 `koishi-plugin-daily-animation-recommend` 从 npm 解析。如果本机存在 `external/daily-animation-recommend`，Workspaces 会优先用这份本地源码。

## 用法

1. 打开控制台，在「插件配置」里启用要用的适配器（QQ / KOOK / Discord / Telegram …），填入对应平台后台签发的 token；
2. `daily-animation-recommend` 默认已启用，是主要业务插件；
3. 控制台里的改动会写回 `koishi.yml`，大部分插件改完即时生效。

## 输出

| 路径 | 内容 |
|---|---|
| `data/koishi.db` | SQLite 库：配置、用户、订阅状态 |
| `data/logs/` | 运行日志 |
| `data/assets/` | 本地上传与缓存资源 |

`data/`、`cache/`、`external/`、`node_modules/`、`.env` 都不进 git。

## 配置

- `.env`：镜像地址（GitHub / Gravatar）。从 `.env.example` 复制后按需修改；
- `koishi.yml`：插件开关与参数，控制台保存时改写；
- **token 只放 `.env` 或控制台，不要写进 `koishi.yml`** —— 这个文件由 git 跟踪；
- 端口在 `koishi.yml` 的 `server.port`（默认 5140，冲突时顺延到 5149）。

## 验证

```bash
yarn start
```

启动成功的判据：

1. 控制台 <http://localhost:5140> 能打开，日志里没有 `error`；
2. 插件页能看到 `daily-animation-recommend` 且状态为启用；
3. 填好适配器 token 后，机器人能在对应平台回消息。

## 开发

插件数据用两份本机小工具准备（Excel → JS 数组、Excel → enazo 题库）。工具不随仓库走，转换出来的数据才提交，说明见 [`docs/dev-tools.md`](docs/dev-tools.md)。

仓库开发和日常使用共用这一份，开发专用文件靠 `.gitignore` 排除，不另开镜像仓库。

## 已知限制

- 默认只开 SQLite；`koishi.yml` 里 Mongo / MySQL / Postgres 的配置都是禁用状态，换库要自己启；
- 适配器默认全部关闭（配置项带 `~` 前缀），不填 token 接不了任何平台；
- `external/` 是本机的 Koishi 插件工作区，不属于仓库，克隆后没有这个目录；
- 生产环境改完代码要重启 `yarn start`；`yarn dev` 只适合本地调试。

## License

AGPL-3.0，见 `package.json`。
