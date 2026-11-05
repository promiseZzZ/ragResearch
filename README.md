本项目参考目录如下：
rag-security-lab/
├── configs/
│   ├── base.yaml                  # 基础模型与切分配置
│   ├── poison_density_5pct.yaml   # 5% 投毒率配置
│   └── poison_density_10pct.yaml  # 10% 投毒率配置
├── data/
│   ├── clean/                     # 严格保证纯净的基准知识库（Ground Truth）
│   │   └── corpus.txt
│   ├── poisoned/                  # 包含注入样本的脏数据目录（按实验版本区分）
│   │   ├── exp1_keyword_stuffing/
│   │   └── exp2_adversarial_jailbreak/
│   └── attack_payloads/           # 恶意 Payload 与目标答案库 (Target Answers)
│       └── triggers.json
├── storage/                       # 向量存储（不同投毒实验必须隔离目录！）
│   ├── clean_db/                  # 纯净对照组数据库
│   └── poisoned_exp1_db/          # 实验 1 专属污染向量库
├── src/
│   ├── attackers/                 # 【新增】投毒生成器核心模块
│   │   ├── __init__.py
│   │   ├── base_attacker.py       # 投毒基类（定义 inject 接口与投毒比例控制）
│   │   ├── keyword_attacker.py    # 关键词/高频词注入攻击
│   │   └── stealthy_attacker.py   # 语义隐蔽型对抗注入
│   ├── defenders/                 # 【新增，可选】安全防御与清洗模块
│   │   ├── __init__.py
│   │   ├── perplexity_filter.py   # 基于困惑度/异常检测的过滤拦截
│   │   └── similarity_filter.py   # 异常向量分布过滤
│   ├── loaders/
│   ├── splitters/
│   ├── retrievers/                # 检索器（测试稠密 vs 混合检索在面对投毒时的脆弱度）
│   ├── models/
│   └── prompts/
├── evaluation/
│   ├── benchmark_qa.json          # 正常评测问答对
│   ├── attack_qa.json             # 攻击测试集（Trigger -> Target Answer 对）
│   └── security_evaluator.py      # 【新增】自动计算 Retrieval ASR 与 Generation ASR
├── experiments/
│   ├── 01_run_clean_baseline.py   # 步骤1：跑通无投毒干净基线
│   ├── 02_poison_injection.py     # 步骤2：执行投毒注入并构建受污染向量库
│   ├── 03_eval_attack_success.py  # 步骤3：量化攻击成功率（ASR）评测
│   └── 04_eval_defense_effect.py  # 步骤4：挂载过滤防御模块并重测 ASR
├── logs/
└── requirements.txt
