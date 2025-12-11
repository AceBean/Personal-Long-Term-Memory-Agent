# Personal Long-Term Memory Agent — 25分钟现场演示(PPT 文稿与讲稿)

演示总时长：25 分钟
目标听众：工程师 / 产品 / 研究人员
风格：技术深度 + 实现细节 + 现场 demo + 结果分析

---

Slide 1 — 标题页
- 标题：Personal Long-Term Memory Agent
- 副标题：设计、实现与效果评估（25 分钟演示）
- 演讲者：{{PRESENTER_NAME}}
- 仓库：AceBean/personal-long-term-memory-agent
- 讲稿提示：简短问候与自我介绍，说明演示目标

---

Slide 2 — 今日议程
- 背景与动机
- 系统架构与技术拆解
- 关键实现挑战与解决方案
- 现场 Demo
- 结果与分析
- Q&A

---

Slide 3 — 背景与动机
- 问题：如何让代理具备长期记忆、并高效检索与更新个人记忆？
- 目标：可靠存储、语义检索、记忆压缩与过期管理
- 价值：提升对话连续性与个性化

---

Slide 4 — 仓库结构 & 快速定位
- README.md / README_ZH.md
- requirements.txt
- src/ 和 src/memory_agent
- 讲稿提示：演示将运行仓库中的 demo（如有），并说明依赖

---

Slide 5 — 高阶系统架构概览
- 元件：输入层、预处理、embedding、向量 DB、检索器、记忆管理、应用层
- 信息流：写入 -> 编码 -> 存储 -> 检索 -> 合并

---

Slide 6 — 数据与预处理
- 数据类型：对话历史、事件、文档
- 预处理：清洗、切分、metadata tagging
- 考量：切分粒度影响 recall 与上下文大小

---

Slide 7 — 向量化与检索
- Embedding：sentence-transformers 或 OpenAI
- 向量 DB：FAISS / Milvus / Weaviate / Pinecone
- 检索：ANN -> top-K -> reranker
- 参数示例：embedding dim, top-K, nprobe/ef

---

Slide 8 — 记忆管理策略
- 冷/热分层
- 聚合与压缩（摘要、merge）
- 衰减与过期（时间、频率、重要性）
- 插入：实时 vs 批量

---

Slide 9 — 系统接口与集成
- 接口：REST/gRPC/SDK/CLI
- 隐私：数据隔离、加密

---

Slide 10 — 关键实现挑战
- 检索精度 vs prompt 长度
- 实时性与吞吐量
- 一致性与并发写入
- 成本与隐私合规

---

Slide 11 — 解决方案概览
- 优先级剪枝与摘要
- 异步批量索引
- 去重策略
- 成本控制与删除 API

---

Slide 12 — 现场 Demo 指南
- 准备：pip install -r requirements.txt；配置 .env
- 演示流程：插入记忆 -> 查询 -> 展示检索结果 -> 压缩/删除
- 回退计划：录制视频

---

Slide 13 — 评估与结果分析
- 指标：Recall@K、MRR、Latency P50/P95/P99、生成质量
- 实验：基线比较、消融研究
- 可视化：Recall@K 曲线、延迟图、存储增长

---

Slide 14 — 示例结果模板
- 占位表格（替换为真实结果）
- 示例结论：embedding tradeoffs、压缩效果

---

Slide 15 — 风险与未来工作
- 限制：隐私、成本、概念漂移
- 方向：增量微调、复杂记忆推理、自动评估

---

Slide 16 — Q&A 与联系方式
- 联系：GitHub: AceBean
- 讲稿提示：优先回答实现细节/部署问题

---

附录：演示命令清单（需替换为仓库实际命令）
- pip install -r requirements.txt
- export OPENAI_API_KEY=xxx
- python -m src.memory_agent.server --port 8080
- python src/tools/insert_memory.py --file examples/event1.json
- python src/tools/query_memory.py --q "我上次说过喜欢哪位作家？"
