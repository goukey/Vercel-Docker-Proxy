# 🐳 Vercel-Docker-Proxy：Docker 仓库镜像代理工具 (Vercel 版)

这个项目是 [CF-Workers-docker.io](https://github.com/cmliu/CF-Workers-docker.io) 的 Vercel 移植版。它利用 Vercel Edge Functions 实现 Docker 官方镜像仓库的代理，解决访问限制和加速访问问题。

> [!WARNING]
> 请勿滥用 Vercel 资源。本项目仅供学习和个人使用。

## 🚀 部署方式

### Vercel 部署

1.  **Fork 或上传代码**：将本项目代码上传到您的 GitHub 仓库。
2.  **导入项目**：在 Vercel Dashboard 中导入该仓库。
3.  **框架预设**：选择 "Other" (或者 Vercel 会自动识别)。
4.  **环境变量** (可选)：在 Settings -> Environment Variables 中添加：
    *   `URL302`: 主页 302 跳转地址 (例如 `https://t.me/YourChannel`)。
    *   `URL`: 主页伪装地址 (设为 `nginx` 则伪装为 nginx 默认页面)。
    *   `UA`: 自定义屏蔽的 User-Agent。
5.  **部署**：点击 Deploy。

## ⚙️ 如何使用？

假设您的 Vercel 项目域名为：`docker-proxy.vercel.app`

### 1. 官方镜像路径前面加域名

```shell
docker pull docker-proxy.vercel.app/stilleshan/frpc:latest
```

```shell
docker pull docker-proxy.vercel.app/library/nginx:stable-alpine3.19-perl
```

### 2. 一键设置镜像加速

修改文件 `/etc/docker/daemon.json`（如果不存在则创建）

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://docker-proxy.vercel.app"]  # 请替换为您自己的 Vercel 域名
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 🔧 变量说明

| 变量名 | 示例 | 必填 | 备注 |
|---|---|---|---|
| URL302 | `https://t.me/CMLiussss` |❌| 主页302跳转 |
| URL | `https://www.baidu.com/` |❌| 主页伪装(设为`nginx`则伪装为nginx默认页面) |
| UA | `netcraft` |❌| 支持多元素, 元素之间使用空格或换行作间隔 |

## 🙏 鸣谢

*   原项目：[cmliu/CF-Workers-docker.io](https://github.com/cmliu/CF-Workers-docker.io)
