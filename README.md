# Krepagotchi 懒猫微服应用

像素风虚拟宠物游戏 [Krepagotchi](https://github.com/danielbrendel/krepagotchi-game) 的懒猫微服（LazyCat）打包，基于上游官方镜像 `ghcr.io/danielbrendel/krepagotchi-game`，采用镜像模式（不自行构建 Docker 镜像）。

## 结构

| 文件 | 说明 |
| --- | --- |
| `package.yml` | 包元数据（包名 `community.lazycat.app.krepagotchi`） |
| `lzc-manifest.yml` | 运行配置：`web`（游戏）+ `db`（MariaDB 11.4 LTS） |
| `lzc-build.yml` | 构建配置 |
| `icon.png` | 图标（取自上游仓库 `public/img/logo.png`） |
| `screenshots/` | 商店截图（取自上游仓库 `public/img/preview.png`、`background.png`） |
| `.github/lazycat-action.yml` | [lazycat-github-action](https://github.com/ca-x/lazycat-github-action) 配置 |

## 说明

- 原服务的 `app` 命名保留给懒猫系统，已重命名为 `web`。
- 数据库与应用日志/迁移目录持久化到 `/lzcapp/var`。
- 数据库密码、root 密码与 `APP_ACCESSTOKEN` 使用 `stable_secret` 生成稳定随机值。
- `APP_BACKEND` 由 `{{ .S.AppDomain }}` 动生成为应用实际域名。
- 上游 tag 为 `vMAJOR.MINOR` 两段式，Action 配置将其映射为 `MAJOR.MINOR.0` 包版本。
- 仅发布喵喵商店（MiaoMiao 社区商店）；官方平台不发布；镜像经 `registry.lazycat.cloud` 投递。

## 自动化

`lazycat.yml` 工作流（配置文件 PR 时 dry-run 验证、每日定时 + 手动触发）自动：

1. 检查上游 `ghcr.io/danielbrendel/krepagotchi-game` 新 tag；
2. 更新版本与 manifest、复制镜像到懒猫镜像仓库；
3. 构建 LPK、发布 GitHub Release；
4. 发布喵喵商店。

## 所需 Secrets

| Secret | 说明 |
| --- | --- |
| `APPSTORE_URL` | 喵喵商店 API 地址 |
| `APPSTORE_TOKEN` | 喵喵商店发布令牌 |
| `APP_ID` | 可选，喵喵商店应用 ID |
| `PRIVATE_STORE_GROUP_CODES` | 可选，私有分组码 |
