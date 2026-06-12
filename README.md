# ASF-QQ

一个通过 WebSocket 接收 QQ 消息，并调用 ASF API 执行 Steam 挂卡、停止挂卡和兑换 Key 的简单示例项目。

## 功能

- 监听 NapCat WebSocket 消息
- 只处理指定 QQ 用户发送的消息
- 调用 ASF `play` 命令开始挂卡
- 调用 ASF `play 0` 命令停止挂卡
- 调用 ASF `redeem` 命令兑换 Key

## 项目结构

```text
ASF-QQ/
├─ main.py
├─ Dockerfile
├─ requirements.txt
└─ README.md
```

## 依赖

- Python 3.12
- websockets 13.1
- requests 2.32.3

## 环境变量

程序通过环境变量读取配置：

- `NAPCAT_WS_URL`: NapCat WebSocket 地址
- `TARGET_USER_ID`: 允许控制机器人的 QQ 用户 ID
- `ASF_API_URL`: ASF API 地址
- `ASF_API_KEY`: ASF API Key

示例：

```bash
NAPCAT_WS_URL=ws://localhost:3001
TARGET_USER_ID=123456789
ASF_API_URL=http://localhost:1242/Api
ASF_API_KEY=your_asf_api_key
```

## 本地运行

安装依赖：

```bash
pip install -r requirements.txt
```

设置环境变量后运行：

```bash
python main.py
```

## Docker 运行

构建镜像：

```bash
docker build -t asf-qq .
```

启动容器：

```bash
docker run -d \
  -e NAPCAT_WS_URL="ws://host.docker.internal:3001" \
  -e TARGET_USER_ID="123456789" \
  -e ASF_API_URL="http://host.docker.internal:1242/Api" \
  -e ASF_API_KEY="your_asf_api_key" \
  --name asf-qq \
  asf-qq
```

## 指令格式

### 开始挂卡

```text
asf play <botname> <gameid>
```

示例：

```text
asf play Bot1 730
```

### 停止挂卡

```text
asf stop <botname>
```

示例：

```text
asf stop Bot1
```

### 兑换 Key

```text
asf <botname> <key>
```

示例：

```text
asf Bot1 ABCDE-FGHIJ-KLMNO
```

## 注意事项

- `TARGET_USER_ID` 以字符串形式比较，请确认值与收到的用户 ID 一致
- 当前代码中的消息结构依赖实际 WebSocket 上报格式，如字段名不同需要调整解析逻辑
- `asf stop` 命令在现有代码里有参数数量判断问题，后续建议修正

## License

仅供学习和个人使用。
