# 配置项详细说明
以下是每个配置块的详细说明，包含：参数作用、如何获取对应的 API Key / 凭据、是否需要配置 Base URL（及默认值）、配置示例及备注。

## 1. LLM PROVIDER (OpenRouter)
**作用**：所有 LLM 调用均通过 OpenRouter 统一转发，无需直接配置各厂商密钥。
**如何获取**：访问 https://openrouter.ai/keys 注册账号并创建 API Key。
**关键参数**：
- OPENROUTER_API_KEY=your_key_here   # 必填
- ### LLM_MODEL 已不再从 .env 读取，默认由 ~/.hermes/config.yaml 中 model.default 决定
- ### 如需临时覆盖，可在命令行使用 `hermes model set <model>` 或修改 config.yaml

**备注**：
- OpenRouter 支持众多模型（GPT、Claude、Gemini等），密钥一次配置即可使用全部。
- 免费额度有限，建议先查看定价：https://openrouter.ai/pricing

## 2. LLM PROVIDER (Google AI Studio / Gemini)
**作用**：直接使用 Google 的 Gemini 模型（通过其 OpenAI 兼容端点）。适用于希望绕过 OpenRouter、直接调用 Gemini 的场景。
**如何获取**：访问 https://aistudio.google.com/app/apikey 创建 API Key。
**关键参数**：
- GOOGLE_API_KEY=your_google_key_here       # 必填（也可用 GEMINI_API_KEY 作为别名）
- GEMINI_BASE_URL=https://generativelanguage.googleapis.com/v1beta/openai  # 官方默认，可选覆盖

**备注**：
- 若只用此提供商，需在 config.yaml 中将 llm.provider 设为 google（或使用 `hermes provider set google`）。
- Gemini 系列模型列表：https://ai.google.dev/gemini-api/docs/models

## 3. LLM PROVIDER (z.ai / GLM)
**作用**：调用 ZhipuAI 的 GLM 系列模型（如 GLM-4-Plus）。
**如何获取**：访问 https://z.ai 或 https://open.bigmodel.cn 注册并获取 API Key。
**关键参数**：
- GLM_API_KEY=your_glm_key_here             # 必填
- GLM_BASE_URL=https://api.z.ai/api/paas/v4 # 默认，可选覆盖

**备注**：
- z.ai 提供国内可访问的高性能中文模型，适合国内用户。
- 文档：https://open.bigmodel.cn/dev/api#glm-4

## 4. LLM PROVIDER (Kimi / Moonshot)
**作用**：调用 Moonshot AI 的 Kimi 系列模型（适用于长文本、代码理解等）。
**如何获取**：访问 https://platform.kimi.ai （Kimi Code 控制台）注册并获取 API Key。注意：Key 通常以 `sk-kimi-` 开头，走 Kimi Code API；若使用旧版平台.moonshot.ai 的 Key，需额外配置 Base URL。
**关键参数**：
- KIMI_API_KEY=your_kimi_key_here           # 必填
- ### 默认 Base URL（适用于 sk-kimi- Key）：
- KIMI_BASE_URL=https://api.kimi.com/coding/v1
- ### 如使用 legacy Key（来自 platform.moonshot.ai），请改为：
- KIMI_BASE_URL=https://api.moonshot.ai/v1
- ### 中国区 Key 使用：
- KIMI_BASE_URL=https://api.moonshot.cn/v1

**备注**：
- Kimi 模型擅长 200k 上下文，适合处理长文档或大规模代码。
- 文档：https://platform.kimi.ai/docs

## 5. LLM PROVIDER (MiniMax)
**作用**：调用 MiniMax 的模型（如抖音豆包系列）。
**如何获取**：全球用户访问 https://www.minimax.io 注册获取 API Key。中国大陆用户使用中国端点。
**关键参数**：
- ### 全球端点
- MINIMAX_API_KEY=your_minimax_key_here
- MINIMAX_BASE_URL=https://api.minimax.io/v1   # 默认，可选覆盖
- ### 中国端点（仅限大陆用户）
- MINIMAX_CN_API_KEY=your_minimax_cn_key_here
- MINIMAX_CN_BASE_URL=https://api.minimaxi.com/v1

**备注**：
- 两套端点互不相通，请根据账号地区选择对应的 Key 和 Base URL。
- 文档：https://api.minimax.io/doc

## 6. LLM PROVIDER (OpenCode Zen)
**作用**：OpenCode Zen 提供付费的 curated 模型（GPT、Claude、Gemini、MiniMax、GLM、Kimi 等），按使用量计费。
**如何获取**：访问 https://opencode.ai/auth 注册并获取 API Key。
**关键参数**：
- OPENCODE_ZEN_API_KEY=your_opencode_zen_key_here
- OPENCODE_ZEN_BASE_URL=https://opencode.ai/zen/v1 

**备注**：
- 适合想要统一账单、多模型路由且不愿自行管理多个密钥的用户。
- 文档：https://opencode.ai/docs

## 7. LLM PROVIDER (OpenCode Go)
**作用**：OpenCode Go 提供 $10/月 订阅制，可访问开源模型（GLM-5、Kimi K2.5、MiniMax M2.5 等）。
**如何获取**：同上，访问 https://opencode.ai/auth 获取 API Key（与 Zen 共用同一账户）。
**关键参数**：
- OPENCODE_GO_API_KEY=your_opencode_go_key_here
- OPENCODE_GO_BASE_URL=https://opencode.ai/zen/go/v1

**备注**：
- 适合预算有限但仍想使用较新开源模型的用户。
- 注意：此 Key 与 Zen 的 Key 是同一个，只需填写一次即可。

## 8. LLM PROVIDER (Hugging Face Inference Providers)
**作用**：通过 Hugging Face 统一端点调用 20+ 开源模型（如 Llama、Mistral 等），走 OpenAI 兼容接口。
**如何获取**：访问 https://huggingface.co/settings/tokens 创建 Token，需勾选 “Make calls to Inference Providers” 权限。
**关键参数**：
- HF_TOKEN=your_hf_token_here
- ### OpenCode Go 也能走 HF 通道（可选覆盖）：
- OPENCODE_GO_BASE_URL=https://opencode.ai/zen/go/v1

**备注**：
- 免费额度：$0.10/月（基本可用），无额外加价。
- 支持模型列表：https://huggingface.co/inference-providers

## 9. LLM PROVIDER (Qwen OAuth)
**作用**：复用本地 Qwen CLI 的登录状态，无需额外 API Key（前提是已运行 `qwen auth qwen-oauth`）。
**如何获取**：无需申请 Key，仅需确保本地已有凭据文件：~/.qwen/oauth_creds.json
**关键参数**：
- ### 可选：覆盖默认 Base URL
- HERMES_QWEN_BASE_URL=https://portal.qwen.ai/v1

**备注**：
- 适合已经在使用 Qwen CLI 且希望省去再申请 Key 的用户。
- 文档：https://qwen.ai

## 10. LLM PROVIDER (Xiaomi MiMo)
**作用**：调用小米米摩（MiMo）系列模型（mimo-v2-pro、mimo-v2-omni、mimo-v2-flash 等）。
**如何获取**：访问 https://platform.xiaomimimo.com 注册并获取 API Key。
**关键参数**：
- XIAOMI_API_KEY=your_xiaomi_key_here
- ### 可选 Base URL 覆盖：
- XIAOMI_BASE_URL=https://api.xiaomimimo.com/v1

**备注**：
- 目前主要面向国内用户，模型在中文理解上表现不错。
- 文档：https://platform.xiaomimimo.com/docs

## 11. TOOL API KEYS
**作用**：为各类网页搜索、内容提取、图像生成等工具提供服务。
- Exa API Key – AI 原生网页搜索与内容提取
  获取途径：https://exa.ai
  参数：EXA_API_KEY=your_exa_key_here
- Parallel API Key – 同上，AI 原生搜索
  获取途径：https://parallel.ai
  参数：PARALLEL_API_KEY=your_parallel_key_here
- Firecrawl API Key – 网页搜索、提取、爬虫
  获取途径：https://firecrawl.dev/
  参数：FIRECRAWL_API_KEY=your_firecrawl_key_here
- FAL.ai API Key – 图像生成（Stable Diffusion、DALL·E 变体等）
  获取途径：https://fal.ai/
  参数：FAL_KEY=your_fal_key_here
- Honcho API Key – 跨会话 AI 原生用户建模（可选）
  获取途径：https://app.honcho.dev
  需要同时在 ~/.honcho/config.json 中设置 enabled=true
  参数：HONCHO_API_KEY=your_honcho_key_here

## 12. TERMINAL TOOL CONFIGURATION
**作用**：配置 Hermes 执行 shell 命令的后端方式及其行为。
**后端类型**（在 ~/.hermes/config.yaml 中设置，.env 仅作覆盖）：
- TERMINAL_ENV=local   # 可选值：local、singularity、docker、modal、ssh
- ### 若在 .env 中设置，会覆盖 config.yaml 的 terminal.backend

**容器镜像**（用于 docker/singularity/modal 后端）：
- TERMINAL_DOCKER_IMAGE=nikolaik/python-nodejs:python3.11-nodejs20
- TERMINAL_SINGULARITY_IMAGE=docker://nikolaik/python-nodejs:python3.11-nodejs20
- TERMINAL_MODAL_IMAGE=nikolaik/python-nodejs:python3.11-nodejs20

**工作目录**（终端命令执行的 CWD）：
- TERMINAL_CWD=.   # 默认值；远程后端请使用容器/VM 内的绝对路径

**超时与生命周期**：
- TERMINAL_TIMEOUT=60           # 单命令超时（秒）
- TERMINAL_LIFETIME_SECONDS=300 # 空闲环境自动清理时间（秒）

**SSH 远程执行**（仅当 TERMINAL_ENV=ssh 时有效）：
- TERMINAL_SSH_HOST=192.168.1.100
- TERMINAL_SSH_USER=agent
- TERMINAL_SSH_PORT=22
- TERMINAL_SSH_KEY=~/.ssh/id_rsa

**SUDO 支持**（所有后端通用）：
- SUDO_PASSWORD=your_password_here   # 明文存储，仅限受信机器使用！
- ### 替代方案：
- ### - SSH 后端：在远程机器配置免密 sudo
- ### - 容器后端：以 root 身份运行容器（无需 sudo）
- ### - 本地后端：编辑 /etc/sudoers 授予特定命令免密权限

## 13. MODAL CLOUD BACKEND（可选）
**作用**：当 TERMINAL_ENV=modal 时使用 Modal 云端运行命令，无需在 .env 中填 API Key。
**如何使用**：
1. 安装 Modal CLI：pip install modal
2. 运行 modal setup 按引导登录（将在浏览器完成 OAuth），凭据会保存在本地。
3. 确认 config.yaml 中 terminal.backend = modal 及 terminal.modal.image 已设。

**备注**：
- Modal 处理身份验证和密钥管理，您只需保证本地已登录。
- 文档：https://modal.com/docs

## 14. BROWSER TOOL CONFIGURATION（Browserbase + Stealth）
**作用**：通过 Browserbase 云端浏览器实现网页自动化（打开网页、填表、截图等），内置反机器人措施。
**如何获取**：访问 https://browserbase.com/ 注册并创建项目，获取 API Key 和 Project ID。
**关键参数**：
- BROWSERBASE_API_KEY=your_browserbase_key_here   # 必填
- BROWSERBASE_PROJECT_ID=your_project_id_here     # 必填，从仪表盘获取

**代理与隐身模式**：
- BROWSERBASE_PROXIES=true   # 默认开启，使用住宅 IP 提高验证码通过率
- BROWSERBASE_ADVANCED_STEALTH=false  # 默认关闭，仅 Scale Plan 可用；开启后使用定制 Chromium 以逃避 bot 检测

**超时设置**：
- BROWSERSESSION_TIMEOUT=300   # 会话非活动超时（秒）
- BROWSER_INACTIVITY_TIMEOUT=120 # 浏览器页面非活动自动关闭（秒）

**备注**：
- 基础 Stealth（指纹随机、自动验证码解答）始终生效；
- Advanced Stealth 需要付费的 Scale Plan。
- 文档：https://docs.browserbase.com

## 15. SESSION LOGGING
**作用**：自动保存完整对话轨迹以便调试、回放或审计。
**路径**：logs/session_YYYYMMDD_HHMMSS_UUID.json
- 无需额外配置，默认开启。
- 若想关闭，需在 config.yaml 中设置 session_logging.enabled=false（.env 中无直接开关）。

## 16. VOICE TRANSCRIPTION & OPENAI TTS
**作用**：提供语音转文字（Whisper）和 OpenAI 文本转语音（TTS）功能。使用独立于 OpenRouter 的 OpenAI 直接调用，因此需单独申请 Key。
**如何获取**：访问 https://platform.openai.com/api-keys 创建 API Key。
**关键参数**：
- VOICE_TOOLS_OPENAI_KEY=your_openai_voice_key_here

**备注**：
- 若不使用语音功能，可留空。
- 本地 STT 默认使用 faster-whisper（无需 Key），详见下面 STT 配置。

## 17. SLACK 集成
**作用**：让 Hermes 能在 Slack 工作区中以 Bot 身份接收消息、回复和执行命令。
**如何获取**：
1. 在 https://api.slack.com/apps 创建一个新 App；
2. 在 “OAuth & Permissions” 中添加以下权限 scope：
   channels:history, groups:history, im:history, mpim:history,
   channels:read, groups:read, im:read, mpim:read,
   chat:write, reactions:read, reactions:write
3. 安装到工作空间，获取：
   - Bot User OAuth Token（以 xoxb- 开头）
   - 若使用 Socket Mode，还需 App-Level Token（以 xapp- 开头）

**关键参数**：
- SLACK_BOT_TOKEN=xoxb-.....          # 必填
- SLACK_APP_TOKEN=xapp-.....          # 仅当使用 Socket Mode 时必填
- SLACK_ALLOWED_USERS=U123,U456       # 可选：逗号分隔的 Slack 用户 ID 白名单；留空表示允许所有已安装工作空间的用户

**备注**：
- 如需使用事件订阅而非 Socket Mode，请在 App 配置中开启 Event Access 并填写 Request URL。
- 文档：https://api.slack.com/bot-users

## 18. TELEGRAM 集成
**作用**：让 Hermes 作为 Telegram Bot 与用户或群聊进行交互。
**如何获取**：
1. 与 @BotFather 对话，发送 /newbot 按引导创建 Bot，获取 Token。
2. （可选）设置隐私模式：在 BotFather 中发送 /setprivacy 并选择 Disable，以让 Bot 能看到群聊中所有消息。

**关键参数**：
- TELEGRAM_BOT_TOKEN=123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11   # 必填
- TELEGRAM_ALLOWED_USERS=123456789,987654321                  # 可选：逗号分隔的 Telegram 用户 ID 白名单；留空表示允许所有人
- TELEGRAM_HOME_CHANNEL=-1001234567890                        # 可选：默认 cron 任务发送目标（频道或群聊 ID）
- TELEGRAM_HOME_CHANNEL_NAME=Hermes Cron Output               # 可选：仅用于显示友好名称

**Webhook 模式**（适用于云部署）：
- TELEGRAM_WEBHOOK_URL=https://my-app.fly.dev/telegram
- TELEGRAM_WEBHOOK_PORT=8443
- TELEGRAM_WEBHOOK_SECRET=your_webhook_secret_here   # 建议在生产环境启用

**备注**：
- 默认使用长轮询（getUpdates）；若想在 VPS、Fly.io 等平台部署，改用 Webhook 模式。
- 文档：https://core.telegram.org/bots/api

## 19. WHATSAPP 集成（可选）
**作用**：使用内置 Baileys 库与 WhatsApp 网络通信（需先扫码配对）。
**如何启用**：
1. 在终端运行 `hermes whatsapp` 按引导扫码登录。
2. 配置白名单（可选）：
   WHATSAPP_ENABLED=true
   WHATSAPP_ALLOWED_USERS=15551234567   # 仅允许此号码与 Bot 交互

**备注**：
- 此功能依赖于设备保持在线且网络稳定。
- 文档：运行 `hermes whatsapp --help` 查看详细说明。

## 20. EMAIL（IMAP/SMTP）集成
**作用**：让 Hermes 能通过电子邮件发送、接收和阅读邮件（如通过 Gmail、Outlook 等）。
**如何获取**（以 Gmail 为例）：
1. 开启两步验证：https://myaccount.google.com/signinoptions/two-step-verification
2. 生成应用专用密码：https://myaccount.google.com/apppasswords
   选择 “Mail” 和您的设备（或选择 “其他”、自定义名称），生成 16 位密码。

**关键参数**：
- EMAIL_ADDRESS=hermes@gmail.com             # 必填：发件/收件邮箱地址
- EMAIL_PASSWORD=your_app_password_here      # 必填：应用专用密码（非登录密码）
- EMAIL_IMAP_HOST=imap.gmail.com             # 默认
- EMAIL_IMAP_PORT=993                        # 默认 SSL
- EMAIL_SMTP_HOST=smtp.gmail.com             # 默认
- EMAIL_SMTP_PORT=587                        # 默认 TLS
- EMAIL_POLL_INTERVAL=15                     # 轮询新邮件间隔（秒）
- EMAIL_ALLOWED_USERS=your@email.com         # 可选：逗号分隔的允许发件人地址白名单
- EMAIL_HOME_ADDRESS=your@email.com          # 可选：默认回送地址

**备注**：
- 若使用其他邮件服务（如 Outlook、Yahoo、自建服务器），请相应修改 HOST、PORT 和 SSL/TLS 设置。
- 文档：参考 himalaya CLI（Hermes 底层所用）：https://github.com/emersion/himalaya

## 21. GATEWAY-WIDE ACCESS CONTROL
**作用**：决定是否对所有传入请求（Telegram、Slack、WhatsApp 等）禁用白名单，开放给任何人。
**关键参数**：
- GATEWAY_ALLOW_ALL_USERS=false   # 默认 false：仅允许已配置白名单的用户
- ### 设为 true 时，任何知道您 Bot Token 的人都能与 Hermes 交互，请慎用！

## 22. RESPONSE PACING（人类延迟）
**作用**：在消息平台（Telegram、Slack 等）上分块发送回复时加入延迟，使输出更像人类打字。
**关键参数**：
- HERMES_HUMAN_DELAY_MODE=off     # 可选：off / natural / custom
- HERMES_HUMAN_DELAY_MIN_MS=800   # 自定义模式下最小延迟（毫秒）
- HERMES_HUMAN_DELAY_MAX_MS=2500  # 自定义模式下最大延迟（毫秒）

**备注**：
- off：不加延迟，一次性发送完整回复。
- natural：使用内置随机分布模拟人类打字节奏。
- custom：在 min~max 范围内均匀随机选择延迟时间。
- 建议在公开场合使用 natural 或 custom 以减少“像机器”感觉。

## 23. DEBUG OPTIONS
**作用**：开启各工具的调试日志，便于排查问题（会产生较多输出，仅在需要时开启）。
**关键参数**：
- WEB_TOOLS_DEBUG=false
- VISION_TOOLS_DEBUG=false
- MOA_TOOLS_DEBUG=false      # Mixture-of-Agents 调试
- IMAGE_TOOLS_DEBUG=false

**备注**：
- 生产环境建议全部设为 false 以避免日志刷屏和潜在的信息泄漏。
- 开启后可在终端或日志文件中看到更详细的内部状态。

## 24. CONTEXT COMPRESSION（自动摘要长对话）
**作用**：当对话长度接近模型上下文限制时，自动将中间片段摘要以释放空间，保持最近和最早的原始内容。
**配置**（实际保存在 ~/.hermes/config.yaml，.env 中仅做覆盖或参考）：
- CONTEXT_COMPRESSION_ENABLED=true        # 默认 true
- CONTEXT_COMPRESSION_THRESHOLD=0.85      # 达到 85% 上下文时开始压缩
- ### 摘要使用的模型（在 config.yaml 中 compression.summary_model 配置，默认：google/gemini-3-flash-preview）

**备注**：
- 摘要内容存储在内存中，不会写入磁盘（除非会话被保存）。
- 若发现摘要丢失关键信息，可调低 THRESHOLD（如 0.7）使压缩更频繁，或改用更强的摘要模型。

## 25. RL TRAINING（Tinker + Atropos）
**作用**：使用 Tinker 与 Atropos 框架对语言模型进行强化学习训练（如奖励模型对齐）。
**如何获取**：
- Tinker API Key：访问 https://tinker-console.thinkingmachines.ai/keys 注册并获取。
- Weights & Biases Key：访问 https://wandb.ai/authorize 获取用于实验记录的 API Key。

**关键参数**：
- TINKER_API_KEY=your_tinker_key_here
- WANDB_API_KEY=your_wandb_key_here
- RL_API_URL=http://localhost:8080   # 默认，若 rl-server 运行在其他地址请修改

**备注**：
- 需要先在本地启动 rl-server（来自 tinker-atropos 包）才能使用。
- 文档：参考 tinker-atropos 项目 README。

## 26. SKILLS HUB（GitHub 集成）
**作用**：让 Hermes 能搜索、安装和发布自定义技能（Skill）到 GitHub。
**如何获取**：访问 https://github.com/settings/tokens 创建 Personal Access Token (PAT)，需勾选以下权限：
- repo（完全控制私有仓库）
- workflow（更新 GitHub Actions）
- admin:org（如需组织级操作）
为避免频率限制，强烈建议使用 PAT 而非匿名访问。

**关键参数**：
- GITHUB_TOKEN=ghp_.....              # 必填
- ### 可选：GitHub App 凭据（用于在 PR 上以机器身份评论等）
- GITHUB_APP_ID=12345
- GITHUB_APP_PRIVATE_KEY_PATH=~/.hermes/github_app_private_key.pem
- GITHUB_APP_INSTALLATION_ID=67890

**Groq API Key**（用于 Whisper STT 的免费备选方案）：
获取途径：https://console.groq.com/keys
参数：GROQ_API_KEY=your_groq_key_here

**备注**：
- 技能存放在 ~/.hermes/skills/ 目录下，可通过 hermes skills list 查看。
- 文档：运行 `hermes skills --help` 或查看内部技能（如 hermes-agent）了解编写规范。

## 27. STT PROVIDER SELECTION（语音转文字）
**作用**：决定 Hermes 使用哪种语音转文字服务（本地优先，若无则回退到云端）。
**如何获取**（云端方案）：
- Groq：访问 https://console.groq.com/keys 获取 API Key。
- OpenAI：同上 OpenAI Key（与 VOICE_TOOLS_OPENAI_KEY 复用）。

**关键参数**：
- ### 在 config.yaml 中设置 stt.provider: local | groq | openai
- ### 以下为可选的环境变量覆盖（若未在 config.yaml 中设置则生效）：
- STT_GROQ_MODEL=whisper-large-v3-turbo   # Groq 使用的模型
- STT_OPENAI_MODEL=whisper-1              # OpenAI 使用的模型
- GROQ_BASE_URL=https://api.groq.com/openai/v1   # 可选自建或代理地址
- STT_OPENAI_BASE_URL=https://api.openai.com/v1

**备注**：
- 本地默认使用 faster-whisper（“base” 模型，约 150 MB），首次运行时自动下载。
- 若想强制使用云端 STT，请在 config.yaml 中明确设置 provider 并填写对应 Key。
- 文档：参考 stt 模块源码或运行 `hermes stt --help`