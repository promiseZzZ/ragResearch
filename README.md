``本项目参考目录如下：
```text
rag-security-lab/
├── configs/                       # 实验配置文件（参数解耦）
│   ├── base.yaml                  # 基础模型底座、上下文窗口与切分配置
│   ├── poison_density_5pct.yaml   # 5% 投毒率参数配置
│   └── poison_density_10pct.yaml  # 10% 投毒率参数配置
├── data/                          # 语料与攻击测试集管理
│   ├── clean/                     # 严格保证纯净的基准知识库（Ground Truth）
│   │   └── corpus.txt
│   ├── poisoned/                  # 注入投毒样本后的脏数据目录（按实验版本隔离）
│   │   ├── exp1_keyword_stuffing/
│   │   └── exp2_adversarial_jailbreak/
│   └── attack_payloads/           # 恶意 Payload 与目标预期输出（Target Answers）
│       └── triggers.json
├── storage/                       # 向量存储（不同实验必须完全隔离）
│   ├── clean_db/                  # 纯净对照组向量数据库
│   └── poisoned_exp1_db/          # 投毒实验专属污染数据库
├── src/                           # 核心业务与算法模块
│   ├── attackers/                 # 投毒生成器核心模块
│   │   ├── __init__.py
│   │   ├── base_attacker.py       # 投毒基类（定义 inject 接口与投毒比例控制）
│   │   ├── keyword_attacker.py    # 关键词/高频词注入攻击
│   │   └── stealthy_attacker.py   # 语义隐蔽型对抗注入
│   ├── defenders/                 # 安全防御与数据清洗模块（可选）
│   │   ├── __init__.py
│   │   ├── perplexity_filter.py   # 基于困惑度/异常检测的过滤拦截
│   │   └── similarity_filter.py   # 异常向量相似度分布过滤
│   ├── loaders/                   # 文档解析与读取模块
│   ├── splitters/                 # 文本分块策略（Chunking）
│   ├── retrievers/                # 向量检索与混合检索实现
│   ├── models/                    # LLM 客户端与 Embedding 接口统一封装
│   └── prompts/                   # 提示词模板集中管理
├── evaluation/                    # 评测基准与安全指标计算
│   ├── benchmark_qa.json          # 正常问答对照评测集
│   ├── attack_qa.json             # 攻击测试集（Trigger -> Target Answer 对）
│   └── security_evaluator.py      # 自动量化 Retrieval ASR 与 Generation ASR
├── experiments/                   # 实验执行入口脚本
│   ├── 01_run_clean_baseline.py   # 阶段 1：运行无污染干净基线
│   ├── 02_poison_injection.py     # 阶段 2：执行投毒注入并构建受污染向量库
│   ├── 03_eval_attack_success.py  # 阶段 3：量化攻击成功率（ASR）与回答劣化度
│   └── 04_eval_defense_effect.py  # 阶段 4：挂载过滤防御模块并重测防护效果
├── logs/                          # 实验运行日志与显存/耗时记录
└── requirements.txt               # 项目依赖清单
