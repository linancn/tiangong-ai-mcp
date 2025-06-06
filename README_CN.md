# TianGong-AI-MCP

[中文](./README.md) | [English](./README_EN.md)

TianGong AI Model Context Protocol (MCP) Server 支持 STDIO 、 SSE（废止）和 Streamable Http三种协议。

## 启动 MCP 服务器

### 客户端 STDIO 服务器

```bash
npm install -g @tiangong-ai/mcp-server

npx dotenv -e .env -- \
npx -p @tiangong-ai/mcp-server tiangong-ai-mcp-stdio
```

### 远程 SSE 服务器

```bash
npm install -g @tiangong-ai/mcp-server
npm install -g supergateway

npx dotenv -e .env -- \
npx -y supergateway \
    --stdio "npx -y -p @tiangong-ai/mcp-server tiangong-ai-mcp-stdio" \
    --port 3001 \
    --ssePath /sse --messagePath /message
```

### 使用 Docker

```bash
# 使用 Dockerfile 构建 MCP 服务器镜像（可选）
docker build -t linancn/tiangong-ai-mcp-server:0.0.13 .

# 拉取 MCP 服务器镜像
docker pull linancn/tiangong-ai-mcp-server:0.0.13

# 使用 Docker 启动 MCP 服务器
docker run -d \
    --name tiangong-ai-mcp-server \
    --publish 9277:9277 \
    --env-file .env \
    linancn/tiangong-ai-mcp-server:0.0.13
```

## 开发

### 环境设置

```bash
# 安装 Node.js
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.2/install.sh | bash
nvm install 22
nvm use

# 安装依赖
npm install

# 更新依赖
npm update && npm ci
```

### 代码格式化

```bash
# 使用代码检查工具格式化代码
npm run lint
```

### 本地测试

#### STDIO 服务器

```bash
# 使用 MCP Inspector 启动 STDIO 服务器
npm run start
```

#### SSE 服务器

```bash
# 打包当前项目
npm run build && npm pack

# 如果需要可以全局安装 supergateway（可选）
npm install -g supergateway

# 启动 SSE 服务器，如配置了参数 --baseUrl ，应设置为有效的 IP 地址或域名
npx dotenv -e .env -- \
npx -y supergateway \
    --stdio "npx -y -p tiangong-ai-mcp-server-0.0.13.tgz tiangong-ai-mcp-stdio" \
    --port 3001 \
    --ssePath /sse \
    --messagePath /message

# 启动 MCP Inspector
npx @modelcontextprotocol/inspector
```

### 发布

```bash
npm login

npm run build && npm publish
```
