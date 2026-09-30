# 老板 n8n 录屏需求清单（lesson-10 Udemy · 1920x1080 横屏）
> 用途：把老板真实 n8n 操作画面剪进对应 lecture 垫底，替换/补充机器生成的幻灯片卡。
> 设备要求：1920x1080 录制（OBS 或 ShareX 均可）、关闭无关通知、n8n 界面语言不限（英文更佳）、鼠标高亮开。
> 交片：MP4 丢进 `state\lesson-10-udemy\assets\recordings\`，按 REC-xx 命名即可，我负责剪辑对位。

| # | 用于 Lecture | 需录内容 | 预估时长 | 优先级 |
|---|---|---|---|---|
| REC-01 | L2（导入与节点地图） | n8n.io/workflows/19457 页面 → 复制 JSON → n8n 画布 Import → 展开后整体拖动浏览一遍（含便签分组） | 2–3 min | ★★★ |
| REC-02 | L2 | 不配置任何凭证，点一次 Execute，展示满屏红色报错（讲解「全红是正常的」） | 1 min | ★★ |
| REC-03 | L3（五源拆解） | 依次点开 5 个 Fetch 节点参数面板：URL / query 参数 / onError=continue + retry 设置特写 | 2–3 min | ★★★ |
| REC-04 | L4（凭证接线） | n8n Credentials 页新建 HTTP Query Auth（name=apikey）+ HTTP Header Auth（x-cg-demo-api-key）两个示例，展示名称/值填写位 | 1–2 min | ★★★ |
| REC-05 | L4（裁 QuantGist） | 删除 QuantGist 两支线节点 → Merge numberInputs 5→4 → 运行一次到 Prepare 看合成的 error 占位 | 2 min | ★★ |
| REC-06 | L5（归一化） | 打开 Normalize Twelve Data 节点 Code 编辑器，从上往下滚动展示代码（配我讲稿逐段）+ 用 4 种测试输入各执行一次看 status 输出 | 3–4 min | ★★★ |
| REC-07 | L6（Prepare） | Prepare 节点运行后输出特写：`source_health` 与 `core_source_failures` 字段展开 | 1–2 min | ★★ |
| REC-08 | L8（换 DeepSeek） | 删除假模型节点 → 拖入 DeepSeek Chat Model → 建凭证 → 选 deepseek-chat + temperature 0.3 → 接回 Agent | 2–3 min | ★★★ |
| REC-09 | L8 | 故意填错 FRED key 跑一次，展示 AI 输出 JSON 里 missing_sources 如实报告 | 1–2 min | ★★ |
| REC-10 | L9（Validate） | Validate 绿色通过一次 + 篡改 market_regime='euphoric' 抛错一次（执行列表里红色失败特写） | 2 min | ★★★ |
| REC-11 | L10（投递） | Telegram 真收到早报消息的手机/桌面特写 + Discord 频道收到 embed +（可选）Postgres 表里 SELECT 出一行 | 2–3 min | ★★★ |
| REC-12 | L11（部署） | 激活 Schedule 开关 → Executions 列表展示历史执行 → 环境变量 TZ/GENERIC_TIMEZONE 设置位置 | 1–2 min | ★★ |

合计约 22–31 分钟原始素材，剪辑后预计每 lecture 嵌入 30–90 秒实操画面。
> ⚠️ REC-03/06/08/11 决定 Udemy 审核「实操含量」，优先录；其余可后补。