# DATA_SCHEMA.md

# 中国婚恋关系知识库——数据结构规范

## 1. 文件说明

本文件用于规范中国婚恋关系知识库的数据结构、字段名称、分类方式及数据记录标准。

项目采用统一的数据格式，对恋爱、婚姻、家庭、离婚及社会现象等内容进行结构化整理。

建设目标：

- 统一不同类型资料的记录格式。
- 方便关键词检索与分类管理。
- 支持 CSV、JSONL 等结构化数据存储。
- 为后续知识图谱及 AI RAG 检索系统提供数据基础。

## 2. 核心分类体系

所有知识内容按照以下六个一级分类进行归档。

| 分类代码 | 中文分类 | 说明 |
|---|---|---|
| unmarried | 未婚篇 | 恋爱、择偶、订婚及婚前关系 |
| married | 已婚篇 | 夫妻关系、家庭关系及婚内问题 |
| divorce | 离婚篇 | 离婚程序、财产分割及子女抚养 |
| social | 社会现象篇 | 中国婚恋市场及社会观念研究 |
| legal | 法律法规 | 婚姻家庭相关法律及政策 |
| research | 研究报告 | 学术论文、调查报告及专题研究 |

## 3. 数据类型

项目初期建立四类核心数据文件。

| 文件名 | 数据类型 | 用途 |
|---|---|---|
| keywords.csv | CSV | 婚恋问题关键词数据库 |
| cases.jsonl | JSONL | 公开案例数据库 |
| laws.csv | CSV | 法律法规数据库 |
| reports.csv | CSV | 学术研究与社会调查数据库 |

## 4. 关键词数据库：keywords.csv

关键词数据库是整个项目的核心。

每一条记录代表一个具体的婚恋问题、概念或研究主题。

### 字段定义

| 字段 | 类型 | 是否必填 | 说明 |
|---|---|---|---|
| id | string | 是 | 唯一编号 |
| title | string | 是 | 问题或主题名称 |
| category | string | 是 | 一级分类代码 |
| subcategory | string | 是 | 二级分类 |
| keywords | string | 是 | 相关关键词，使用分号分隔 |
| description | string | 是 | 内容简介 |
| risk_type | string | 否 | 风险类型 |
| legal_related | boolean | 是 | 是否涉及法律问题 |
| source_type | string | 是 | 资料来源类型 |
| source_url | string | 否 | 原始资料链接 |
| publish_date | date | 否 | 原始资料发布时间 |
| update_date | date | 是 | 本条记录更新时间 |
| reliability | string | 是 | 信息可信度等级 |
| tags | string | 否 | 补充标签，使用分号分隔 |

### 示例数据

```csv
id,title,category,subcategory,keywords,description,risk_type,legal_related,source_type,source_url,publish_date,update_date,reliability,tags
KW0001,彩礼纠纷,unmarried,彩礼与订婚,彩礼;订婚;婚约财产,整理彩礼给付及返还相关问题,legal_dispute,true,official,,,,A,婚前关系;财产
KW0002,婚前财产,unmarried,婚前财产,婚前财产;个人财产;财产约定,整理婚前财产认定及相关法律问题,financial_risk,true,official,,,,A,法律;财产
KW0003,婚恋观念变化,social,青年婚恋观,晚婚;不婚;婚育观念,研究中国青年婚恋观念及其变化,social_research,false,academic,,,,B,社会调查;青年
```

## 5. 公开案例数据库：cases.jsonl

案例数据库采用 JSONL 格式，每行保存一条独立记录。

### 字段定义

| 字段 | 类型 | 说明 |
|---|---|---|
| id | string | 案例唯一编号 |
| title | string | 案例标题 |
| category | string | 一级分类 |
| case_type | string | 案例类型 |
| summary | string | 案例摘要 |
| key_issues | array | 涉及的核心问题 |
| legal_basis | array | 相关法律依据 |
| court | string | 审理法院，适用时填写 |
| judgment_date | date | 裁判日期，适用时填写 |
| source_url | string | 公开来源链接 |
| source_type | string | 来源类型 |
| reliability | string | 可信度等级 |
| update_date | date | 记录更新时间 |

### 示例记录

```json
{"id":"CASE0001","title":"婚约财产返还争议案例","category":"legal","case_type":"婚约财产纠纷","summary":"整理公开裁判文书中关于婚约财产返还的争议及裁判规则。","key_issues":["彩礼认定","婚约财产返还"],"legal_basis":[],"court":"","judgment_date":"","source_url":"","source_type":"official","reliability":"A","update_date":"2026-11-14"}
```

注：示例仅展示数据格式，不代表真实司法案件。

## 6. 法律法规数据库：laws.csv

用于整理与婚姻家庭相关的法律法规及司法解释。

### 字段定义

| 字段 | 类型 | 说明 |
|---|---|---|
| id | string | 唯一编号 |
| law_name | string | 法律法规名称 |
| article | string | 条文编号 |
| title | string | 条文主题 |
| content_summary | string | 条文内容摘要 |
| applicable_category | string | 适用分类 |
| effective_date | date | 生效日期 |
| source_url | string | 官方来源链接 |
| update_date | date | 更新时间 |

## 7. 研究报告数据库：reports.csv

用于整理学术论文、社会调查和研究报告。

### 字段定义

| 字段 | 类型 | 说明 |
|---|---|---|
| id | string | 唯一编号 |
| title | string | 研究标题 |
| author | string | 作者或研究机构 |
| institution | string | 发布机构 |
| year | integer | 发布年份 |
| research_topic | string | 研究主题 |
| research_method | string | 研究方法 |
| sample_size | integer | 样本数量，适用时填写 |
| main_findings | string | 主要研究发现 |
| source_url | string | 原始来源链接 |
| reliability | string | 可信度等级 |
| update_date | date | 更新时间 |

## 8. 风险类型规范

关键词及案例可根据实际内容选择以下风险类型：

| 代码 | 中文名称 |
|---|---|
| information_asymmetry | 信息不对称 |
| financial_risk | 财务与经济风险 |
| relationship_conflict | 关系冲突 |
| emotional_boundary | 情感边界问题 |
| fraud_risk | 欺诈风险 |
| legal_dispute | 法律纠纷 |
| family_conflict | 家庭矛盾 |
| privacy_risk | 隐私风险 |
| child_custody | 子女抚养问题 |
| property_dispute | 财产争议 |
| social_research | 社会研究主题 |

## 9. 信息可信度等级

| 等级 | 来源 | 说明 |
|---|---|---|
| A | 官方法律、政府部门、法院 | 可核验的官方资料 |
| B | 学术期刊、高校研究 | 学术研究资料 |
| C | 正式社会调查、专业研究报告 | 有明确调查或研究方法的资料 |
| D | 新闻媒体、公开网络讨论 | 需要进一步核验的信息 |

注意：可信度等级表示来源类型及核验基础，不代表结论绝对正确。

## 10. 数据记录原则

1. 每条数据必须拥有唯一 ID，不得重复。
2. 重要信息应尽可能保留原始来源链接。
3. 法律内容应核对现行有效版本。
4. 统计数据应记录调查年份、样本范围和研究方法。
5. 公开案例应进行必要的隐私保护。
6. 不得将未经证实的网络传言作为客观事实。
7. 应区分客观事实、研究结论和个人观点。
8. 修改数据时同步更新 update_date。
9. 不确定的信息应明确标注，不得自行补造。
10. 同一问题可以关联多个关键词和知识分类。

## 11. 后续扩展方向

未来可根据知识库建设情况增加：

- 人物关系与概念关联数据
- 婚恋问题知识图谱
- 法律条文与案例关联
- 关键词语义向量索引
- AI RAG 检索数据格式
- 多来源信息交叉验证记录

---

**本规范由项目所有者持续维护和更新。**
