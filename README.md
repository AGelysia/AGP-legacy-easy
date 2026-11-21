# AGP-legacy-easy

「学长的烂摊子服务器」是一个单实例合作式 CTF 新人训练靶场，模拟一台 2015—2019 年代的社团遗留服务器。权限路径由外部 Web 入口逐步深入至服务账号、系统用户和 root，共包含 18 枚 flag；玩家拿到容器 root 即视为通关。

> **注意：镜像内包含全部 flag 与口令，挂载镜像根文件系统即可读出。请勿将本镜像用于公开报名的计时赛事。**

## 下载

完整部署包约 777 MiB，通过 GitHub Release 分发：

[下载 legacy-lab-opensource.tar](https://github.com/AGelysia/AGP-legacy-easy/releases/latest/download/legacy-lab-opensource.tar)

SHA-256：

```text
24a1df0b08e344638a2ea0e70b597fc689d59e2cd833b1e3e92dff53cd832f7c
```

可使用以下命令下载并校验：

```bash
curl -LO https://github.com/AGelysia/AGP-legacy-easy/releases/latest/download/legacy-lab-opensource.tar
echo "24a1df0b08e344638a2ea0e70b597fc689d59e2cd833b1e3e92dff53cd832f7c  legacy-lab-opensource.tar" | sha256sum -c -
```

## 环境要求

- Docker 25+ 与 Docker Compose v2（镜像为 OCI 格式）
- 2 核 CPU、4 GB 内存、6 GB 可用磁盘
- 无域名依赖，可直接通过 IP 访问

## 部署

```bash
tar xf legacy-lab-opensource.tar
cd legacy-lab-oss
docker load < images.tar.gz
docker compose up -d
```

`docker load` 会载入 `legacy-lab:public` 与 `legacy-flagboard:public` 两个镜像。

## 端口

| 端口 | 服务 | 说明 |
| --- | --- | --- |
| 80 | 主站 | PHP + MySQL，登录、工单、上传、找回密码 |
| 3000 | 子站 | Node 课程评价中心 |
| 2222 | SSH | 系统账号 lab / monitor，root 不能直登 |
| 11451 | 进度墙 | 玩家提交 flag、公开进度页，可在 Compose 配置中删除该服务 |

R2 内存取证题的 1 GB 内存快照与符号表已内置，无需额外下载。

## 使用说明

- **重置（清除状态，秘密不变）**：运行 `docker compose down -v && docker compose up -d`，清除玩家注册、上传和数据库内容；flag 与口令不变。
- **轮换（更换全部秘密）**：当前发布包只含固定镜像，flag 集与镜像绑定，无法一键轮换；有需要可自行修改。
- 进度墙的玩家提交记录保存在 `./data/records.jsonl`，包含昵称、IP 和时间，请勿公开该文件。
- 镜像内 PHP 已包含 2026-10-21 官方安全公告（CVE-2026-109618 / CVE-2026-109619）的发行版回移补丁（8.3.6-0ubuntu0.24.04.13）。

本靶场镜像为免费开源的 demo 版。靶机中的“阿瓜村计算机学院”、内置故事和情景均为虚构，如有与现实近似，纯属巧合。
