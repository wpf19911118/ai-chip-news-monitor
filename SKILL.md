---
name: ai-chip-news-monitor
description: 监控 NVIDIA、AMD、Google TPU、华为昇腾、海光、沐曦、寒武纪等 AI 芯片动态，以及 vLLM、SGLang、TensorRT-LLM、llama.cpp、LMSYS Blog 等推理框架与基准测试的最新发布，并整理成飞书文档报告
triggers:
  - AI 芯片新闻监控
  - 推理框架更新
  - AI 芯片日报/周报
  - NVIDIA/AMD 最新动态
  - 华为昇腾/海光/寒武纪新闻
  - 生成 AI 资讯
---

# AI芯片与推理框架新闻监控

## 用途
监控全球 AI 芯片动态（训练/推理）和主流推理框架的最新发布，整理成结构化的飞书文档日报/周报。

**输出文件命名**：`2026-06-02 ai-chip-news-monitor`（日期 + ai-chip-news-monitor）

## 信源总览

### 🏢 官方博客（RSS 或 HTML 解析）
| 信源 | URL | 关键词 | 筛选标准 |
|------|-----|--------|----------|
| NVIDIA Blog | https://blogs.nvidia.com/feed/ | AI chip, GPU, Blackwell, CUDA, TensorRT | 7天内 |
| AMD Instinct Blog | https://www.amd.com/en/products/instinct/blog | AMD Instinct, MI300X, ROCm | 7天内 |
| Google TPU Blog | https://cloud.google.com/blog/topics/tpus | TPU, JAX, ML hardware | 7天内 |
| vLLM Blog | https://vllm.ai/blog/ | vLLM, inference, DeepSeek, MoE | 7天内 |
| NVIDIA Dev Blog | https://developer.nvidia.com/blog/ | CUDA, TensorRT-LLM, Triton | 7天内 |
| Interconnects AI | https://www.interconnects.ai/ | AI infrastructure, ML systems | 7天内 |
| LMSYS Blog | https://lmsys.org/blog/ + sitemap | benchmark, Chatbot Arena | 7天内 |

### 🇨🇳 中国芯片厂商官方信源
| 厂商 | 产品线 | 信源 | URL | 备注 |
|------|--------|------|------|------|
| **华为海思** | 昇腾 910B/C | 华为计算官网 | https://www.huaweicloud.com/computing/ascend/ | 昇腾社区文章 |
| **华为海思** | 昇腾 910B/C | 华为云开发者社区 | https://developer.huaweicloud.com/developermap/ascend.html | 技术文章 |
| **海光光电** | DCU (Z100) | 海光官网新闻 | https://www.hygon.cn/ | 需 HTML 解析，无 RSS |
| **寒武纪** | MLU290 | 官网新闻 | https://www.cambricon.com/ | 需 HTML 解析 |
| **沐曦** | GX00 系列 | 官网新闻 | https://www.metax-tech.com/ | 需 HTML 解析，有 WAF 防护 |
| **摩尔线程** | MTT X400 | 官网新闻 | https://www.moorethreads.com/ | 需 HTML 解析 |
| **壁仞科技** | BR100 | 官网新闻 | https://www.biren.com/ | 需 HTML 解析 |

### 🇺🇸 其他美国芯片厂商
| 厂商 | 产品线 | 信源 | URL | 备注 |
|------|--------|------|------|------|
| **Intel Gaudi** | Gaudi 2/3 | Intel IPT Releases (GitHub) | intel/intel-extension-for-pytorch | 最新 3 个版本 |
| **Qualcomm** | Cloud AI 100 | Qualcomm AI Hub Blog | https://developer.qualcomm.com/blog/ai | 需 HTML 解析 |
| **Cerebras** | Wafer Scale Engine | Cerebras-GPT GitHub | cerebras/Cerebras-GPT | 模型权重 releases |
| **Groq** | GroqCard | Groq GitHub | groq/grolp | 最新 releases |
| **Tesla** | Dojo | Tesla AI 官网 | https://www.tesla.com/AI | 需人工确认信息 |

### 📦 推理框架 GitHub Releases（最可靠的结构化数据）
```bash
curl -sL -H "Accept: application/vnd.github.v3+json" \
  "https://api.github.com/repos/{owner}/{repo}/releases?per_page=3"
```

| 仓库 | 框架/用途 | 更新频率 |
|------|----------|----------|
| vllm-project/vllm | vLLM 主框架 | 频繁 |
| sgl-project/sglang | SGLang 框架 | 频繁 |
| NVIDIA/TensorRT-LLM | TensorRT-LLM | RC 版本更新 |
| ggerganov/llama.cpp | llama.cpp (CPU/GPU 推理) | 频繁 (每日) |
| ray-project/ray | Ray Serve (分布式推理) | 每周 |
| ollama/ollama | Ollama 本地推理 | 频繁 |
| huggingface/text-generation-inference | TGI | 季度 |
| mlc-ai/mlc-llm | MLC-LLM (端侧部署) | 不频繁 |
| flashinfer-ai/flashinfer | FlashInfer (CUDA Kernel) | 频繁 |
| deepseek-ai/DeepGemm | DeepGemm (MoE 专用 GEMM) | 频繁 |
| ModelTC/LightLLM | LightLLM (商汤开源) | 不频繁 |

### 🔥 推理技术创新 / 关键开源项目
| 项目 | 机构 | 仓库 | 用途 |
|------|------|------|------|
| FlashMLA | DeepSeek | deepseek-ai/FlashMLA | MLA  Attention CUDA Kernel |
| DeepGEMM | DeepSeek | deepseek-ai/DeepGemm | FP8 MoE 专用 GEMM 实现 |
| Continuous Batching 论文 | 学术界 | - | Infinite-LLM 等新调度算法 |
| Marlin | IST Berlin | ist-ds/marlin | W4A16 GPTQ Kernel |
| mxfp4 | 社区 | 整合于 SGLang | 超低精度量化 Kernel |
| FBGEMM | Meta/Pytorch | pytorch/FBGEMM | 低精度推理算子 |

### 📰 行业分析 / 新闻聚合
| 信源 | URL | 类型 |
|------|------|------|
| AnandTech | https://www.anandtech.com/ | 硬件深度分析 |
| EET-China (电子工程专辑) | https://www.eet-china.com/news | 半导体行业新闻 |
| Semiconductor Engineering | https://semiengineering.com/news/ | 先进制程/设计新闻 |
| The Chip Letter | https://www.thechipletter.com/ | 行业分析订阅 |
| 36kr AI | https://36kr.com/feed/AI | 中国 AI 科技媒体 |

### 🔧 基准测试 / 性能榜单
| 信源 | URL | 内容 |
|------|------|------|
| LMSYS Chatbot Arena | https://lmarena.ai/ | 模型能力排行榜 |
| LMSYS Blog | https://lmsys.org/blog/ | 技术博客 + [sitemap](https://lmsys.org/sitemap.xml) |
| Artificial Analysis | https://artificialanalysis.ai/ | 推理性能/性价比榜单 |
| OpenLLM Leaderboard | https://huggingface.co/spaces/open-llm-leaderboard | 开放模型排名 |
| LiveBench | https://livebench.ai/ | 最新基准测试 |

#### LMSYS Blog 抓取方法
LMSYS 网站使用纯客户端渲染（Next.js），无 RSS，直接 curl HTML 返回空内容。正确方式是**解析 sitemap.xml**：

```python
import subprocess, xml.etree.ElementTree as ET
from datetime import datetime, timedelta

def fetch_lmsys_recent_posts(max_age_days=30):
    """
    通过 sitemap.xml 获取 LMSYS 最新博客文章列表。
    LMSYS 博客为纯前端渲染，无 RSS，直接爬 HTML 拿不到内容，
    因此通过 sitemap 获取 URL 列表，再对近期文章做标注。
    """
    result = subprocess.run(
        ["curl", "-sL", "--max-time", "15", "--compressed",
         "https://lmsys.org/sitemap.xml"],
        capture_output=True, text=True, timeout=20
    )
    root = ET.fromstring(result.stdout)
    ns = {'sm': 'http://www.sitemaps.org/schemas/sitemap/0.9'}
    
    cutoff = (datetime.now(bj_tz) - timedelta(days=max_age_days)).timestamp()
    posts = []
    
    for url in root.findall('sm:url', ns):
        loc = url.findtext('sm:loc', '', ns)
        lastmod_str = url.findtext('sm:lastmod', '', ns)
        if not loc or '/blog/' not in loc:
            continue
        try:
            lastmod = datetime.fromisoformat(lastmod_str.replace('Z', '+00:00')).timestamp()
        except:
            lastmod = 0
        if lastmod >= cutoff:
            slug = loc.rsplit('/', 1)[-1]
            posts.append({
                'url': loc,
                'slug': slug,
                'date': lastmod_str[:10],
                'title': slug.replace('-', ' ').title()
            })
    
    posts.sort(key=lambda x: x['date'], reverse=True)
    return posts

lmsys_posts = fetch_lmsys_recent_posts(max_age_days=30)
for p in lmsys_posts[:10]:
    print(f"• [{p['date']}] {p['title']} — {p['url']}")
# 输出示例：
# • [2026-06-01] 2026 06 01 Hetero EPD — https://lmsys.org/blog/2026-06-01-hetero-epd
# • [2026-05-28] 2026 05 28 Mori — https://lmsys.org/blog/2026-05-28-mori
# • [2026-04-25] 2026 04 25 Deepseek V4 — https://lmsys.org/blog/2026-04-25-deepseek-v4
```

**注意**：由于是纯前端渲染，无法直接获取文章正文。近期文章（7天内）可标注 `[新]`，旧文可从文章 slug 推断主题（如 `deepseek-v4`、`sglang-hisparse` 等）。如需完整内容，建议通过 browser 工具抓取。

## 报告结构模板

生成的飞书文档命名格式：`YYYY-MM-DD ai-chip-news-monitor`

```
📋 本期概要
  - 芯片厂商要闻（2-3 条）
  - 推理框架更新（3-5 条）
  - 重要技术突破

🇨🇳 中国芯片动态
  - 华为昇腾 / 海光 / 寒武纪 / 沐曦 / 摩尔线程 / 壁仞

🇺🇸 海外芯片动态
  - NVIDIA / AMD / Intel Gaudi / 其他

📦 推理框架 Releases
  - vLLM（最新 3 个版本）
  - SGLang（最新 3 个版本）
  - TensorRT-LLM（最新 RC 版本）
  - llama.cpp（最新 3 个版本）

🔥 关键开源项目动态
  - FlashMLA / DeepGemm / FlashInfer 等

🔗 相关链接
  - 各官方 Release 页面
  - 基准测试榜单
```

## 飞书云文档创建流程

### 步骤 1: 获取 Access Token
```python
import urllib.request, json, os

# 读取 .env 中的飞书凭证
env_vars = {}
with open('/home/wangpf/.hermes/.env') as f:
    for line in f:
        line = line.strip()
        if line and not line.startswith('#') and '=' in line:
            k, v = line.split('=', 1)
            env_vars[k.strip()] = v.strip()

app_id = env_vars.get('FEISHU_APP_ID')
app_secret = env_vars.get('FEISHU_APP_SECRET')

url = "https://open.feishu.cn/open-apis/auth/v3/tenant_access_token/internal"
data = json.dumps({"app_id": app_id, "app_secret": app_secret}).encode()
req = urllib.request.Request(url, data=data, headers={'Content-Type': 'application/json'})
with urllib.request.urlopen(req, timeout=10) as resp:
    token = json.loads(resp.read()).get('tenant_access_token')
```

### 步骤 2: 创建文档（命名格式：日期 + ai-chip-news-monitor）
```python
from datetime import datetime, timezone, timedelta

bj_tz = timezone(timedelta(hours=8))  # 北京时间 (UTC+8)

def create_doc(title):
    url = "https://open.feishu.cn/open-apis/docx/v1/documents"
    headers = {'Content-Type': 'application/json', 'Authorization': f'Bearer {token}'}
    data = json.dumps({"title": title}).encode()
    req = urllib.request.Request(url, data=data, headers=headers, method='POST')
    with urllib.request.urlopen(req, timeout=10) as resp:
        result = json.loads(resp.read())
    return result['data']['document']['document_id']

today = datetime.now(bj_tz).strftime("%Y-%m-%d")  # 明确使用北京时区
doc_title = f"{today} ai-chip-news-monitor"
doc_id = create_doc(doc_title)
print(f"✅ 文档创建完成！链接：https://my.feishu.cn/docx/{doc_id}")
# ✅ 正确链接: https://my.feishu.cn/docx/{doc_id}  (个人云文档域名)
# ❌ 错误链接: https://feishu.cn/docx/{doc_id}     (公司版域名，用户打不开)
```

### 步骤 3: 获取文档根 Block ID
```python
def get_root_block(doc_id):
    url = f"https://open.feishu.cn/open-apis/docx/v1/documents/{doc_id}/blocks?document_revision_id=-1&page_size=50"
    headers = {'Authorization': f'Bearer {token}'}
    req = urllib.request.Request(url, headers=headers)
    with urllib.request.urlopen(req, timeout=10) as resp:
        result = json.loads(resp.read())
    items = result.get('data', {}).get('items', [])
    return items[0]['block_id'] if items else None

root_block = get_root_block(doc_id)
```

### 步骤 4: 插入内容块
```python
import time

def insert_blocks(doc_id, root_block, blocks):
    """
    插入内容块到文档。
    关键发现（2026-06 实测）：
    - URL: /blocks/{root_block}/children（root_block 就是 doc_id）
    - Payload: {"children": [...], "index": -1} — 不需要 parent_block_id
    - index=-1 表示追加到末尾，比计算位置更可靠
    - block_type=2 (text) 始终可用，其他类型（heading3/bullet/ordered）可能报 400
    - 实测可一次写入 42+ 个块，无需分批（但仍建议分批以防万一）
    """
    url = f"https://open.feishu.cn/open-apis/docx/v1/documents/{doc_id}/blocks/{root_block}/children"
    headers = {'Content-Type': 'application/json', 'Authorization': f'Bearer {token}'}
    data = json.dumps({"children": blocks, "index": -1}).encode()
    req = urllib.request.Request(url, data=data, headers=headers, method='POST')
    with urllib.request.urlopen(req, timeout=15) as resp:
        return json.loads(resp.read())

# 块类型对照：
#   2  = 文本段 (text)
#   3  = 一级标题 (heading1)
#   4  = 二级标题 (heading2)
#   5  = 三级标题 (heading3)
#   12 = 项目列表 (bullet)
```

### 步骤 5: 块结构示例
```python
# ⚠️ 重要：飞书 docx API 对 block_type 的字段名有严格要求。
# 实测只有以下几种可靠可用（2026-06 实测）：
#
# 块类型        field_name  可用性
# block_type=1  page        ✅ 文档根节点，读取时返回
# block_type=2  text        ✅ 始终可用，最安全的选择
# block_type=3  heading1    ⚠️ 需精确字段名，实测可能报错
# block_type=4  heading2    ⚠️ 同上
# block_type=5  heading3    ❌ 实测返回 400 invalid param
# block_type=12 bullet      ❌ 实测返回 400 invalid param
# block_type=13 ordered     ❌ 实测返回 400 invalid param
#
# 推荐方案：全部使用 block_type=2 (text)，用内容本身表达格式
# 这样既可靠，又减少了 API 调用次数（可一次写入所有块，index=-1）

# 辅助函数：生成可靠的文本块
def text(content, bold=False):
    style = {"bold": True} if bold else {}
    return {
        "block_type": 2,
        "text": {
            "elements": [{"text_run": {"content": content, "text_element_style": style}}],
            "style": {}
        }
    }

# 使用方式：
# 区块标题（如"中国芯片动态"）→ 粗体文本
blocks.append(text("📡 中国芯片动态", bold=True))

# 子标题（如"华为昇腾"）→ 粗体文本
blocks.append(text("华为昇腾（Ascend）", bold=True))

# 无序列表项 → 纯文本，"• " 前缀
blocks.append(text("• CANN 8.0 RC1 发布，完整支持 PyTorch 2.4"))

# 有序列表项 → 纯文本，"1. " 前缀
blocks.append(text("1. vLLM v0.6.4 — PagedAttention 2.0"))

# 注释/提示 → 斜体文本（或带 💡 前缀）
blocks.append(text("💡 更多 Releases 见 GitHub Trending", italic=True))
```

## 完整执行流程

### 1. 获取所有 GitHub Releases 数据
```python
import subprocess, json, re
from datetime import datetime, timezone, timedelta

bj_tz = timezone(timedelta(hours=8))  # 北京时间

def github_api(endpoint):
    url = f"https://api.github.com/repos/{endpoint}"
    result = subprocess.run(
        ["curl", "-sL", "-H", "Accept: application/vnd.github.v3+json",
         "--max-time", "15", url],
        capture_output=True, text=True, timeout=20
    )
    try:
        return json.loads(result.stdout)
    except:
        return []

def github_tags(endpoint):
    url = f"https://api.github.com/repos/{endpoint}/tags?per_page=5"
    result = subprocess.run(
        ["curl", "-sL", "-H", "Accept: application/vnd.github.v3+json",
         "--max-time", "15", url],
        capture_output=True, text=True, timeout=20
    )
    try:
        return json.loads(result.stdout)
    except:
        return []

# 推理框架 Releases
repos = {
    "vllm": "vllm-project/vllm",
    "sglang": "sgl-project/sglang",
    "tensorrt-llm": "NVIDIA/TensorRT-LLM",
    "llama.cpp": "ggerganov/llama.cpp",
    "ray": "ray-project/ray",
    "ollama": "ollama/ollama",
    "flashinfer": "flashinfer-ai/flashinfer",
    "deepgemm": "deepseek-ai/DeepGemm",
    "tgi": "huggingface/text-generation-inference",
    "lightllm": "ModelTC/LightLLM",
}

all_releases = {}
for name, repo in repos.items():
    releases = github_api(f"{repo}/releases?per_page=3")
    if isinstance(releases, list):
        all_releases[name] = [
            {"tag": r.get("tag_name"), "date": r.get("published_at", "")[:10],
             "body": (r.get("body") or "")[:300].replace("\r\n", " ").replace("\n", " ")}
            for r in releases[:3]
        ]
        print(f"✅ {name}: {all_releases[name][0]['tag'] if all_releases[name] else 'N/A'}")
    else:
        print(f"❌ {name}: {releases}")
        all_releases[name] = []
```

### 2. 解析博客 RSS Feeds（利用标准 XML 解析）
```python
import subprocess, re, xml.etree.ElementTree as ET
from datetime import datetime, timedelta

def fetch_feed(url, max_age_days=7):
    """解析 RSS/Atom feed，返回指定天数内的新文章"""
    result = subprocess.run(
        ["curl", "-sL", "--max-time", "15", url],
        capture_output=True, text=True, timeout=20
    )
    xml_content = result.stdout
    
    articles = []
    try:
        # 处理 RSS 2.0
        root = ET.fromstring(xml_content)
        channel = root.find("channel")
        if channel is not None:
            cutoff = (datetime.now(bj_tz) - timedelta(days=max_age_days)).timestamp()
            for item in channel.findall("item"):
                title = (item.findtext("title") or "").strip()
                link = (item.findtext("link") or "").strip()
                pub_date = item.findtext("pubDate")
                description = (item.findtext("description") or "")[:200].replace("<[^>]+>", "").strip()
                
                # 解析日期
                try:
                    from email.utils import parsedate_to_datetime
                    dt = parsedate_to_datetime(pub_date)
                    if dt.timestamp() < cutoff:
                        continue
                    date_str = str(dt)[:10]
                except:
                    date_str = ""
                
                if title and link:
                    articles.append({
                        "title": title, "link": link, "date": date_str,
                        "description": description, "source": url.split("//")[1].split("/")[0]
                    })
        else:
            # 处理 Atom
            for entry in root.findall(".//{http://www.w3.org/2005/Atom}entry"):
                title = (entry.findtext("{http://www.w3.org/2005/Atom}title") or "").strip()
                link_el = entry.find("{http://www.w3.org/2005/Atom}link")
                link = link_el.get("href") if link_el is not None else ""
                summary = (entry.findtext("{http://www.w3.org/2005/Atom}summary") or "")[:200]
                articles.append({"title": title, "link": link, "summary": summary, "source": url})
    except Exception as e:
        print(f"Feed 解析失败 {url}: {e}")
    
    return articles[:10]

# 解析各博客 RSS
nvidia_articles = fetch_feed("https://blogs.nvidia.com/feed/")
amd_articles = fetch_feed("https://blogs.nvidia.com/feed/")  # 替换为 AMD RSS
vllm_articles = fetch_feed("https://vllm.ai/blog/feed.xml")  # 如果有 RSS

print(f"NVIDIA 博客: {len(nvidia_articles)} 篇新文章")
```

### 3. 解析 HTML 新闻页面（无 RSS 时）
```python
import subprocess, re

def fetch_html(url, max_age_days=7):
    result = subprocess.run(
        ["curl", "-sL", "-H", "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36",
         "--max-time", "15", url],
        capture_output=True, text=True, timeout=20
    )
    return result.stdout

def parse_chinese_news(url, selector_pattern, title_key="title"):
    """通用 HTML 解析器，用于没有 RSS 的中文站点"""
    html = fetch_html(url)
    # 需要根据具体网站结构调整选择器
    # 以下为通用正则示例
    articles = []
    # 匹配 <a> 标签中的新闻标题和链接
    matches = re.findall(r'<a[^>]+href=["\']([^"\']+)["\'][^>]*>\s*([^<]{10,100})\s*</a>', html)
    for link, title in matches[:15]:
        title = title.strip()
        if len(title) > 8:
            articles.append({
                "title": title,
                "link": link if link.startswith("http") else url.rsplit("/", 1)[0] + "/" + link,
                "source": url.split("//")[1].split("/")[0]
            })
    return articles

# 中国厂商新闻解析示例
# 华为昇腾
huawei_ascend = parse_chinese_news(
    "https://www.huaweicloud.com/computing/ascend/",
    None
)
# 海光
hygon_news = parse_chinese_news(
    "https://www.hygon.cn/",
    None
)
```

### 4. 构建并写入飞书文档
```python
from datetime import datetime

def text(content, bold=False):
    """可靠文本块生成 — 只用 block_type=2，规避 API 字段名陷阱"""
    style = {"bold": True} if bold else {}
    return {
        "block_type": 2,
        "text": {
            "elements": [{"text_run": {"content": content, "text_element_style": style}}],
            "style": {}
        }
    }

def build_blocks(data, all_releases):
    blocks = []
    bj_tz = timezone(timedelta(hours=8))
    today = datetime.now(bj_tz).strftime("%Y-%m-%d")

    # ✅ 标题 — 用粗体文本代替 heading1（heading1 字段名可能报错）
    blocks.append(text(f"🤖 AI芯片与框架资讯日报 — {today}", bold=True))
    blocks.append(text(f"生成时间：{datetime.now(bj_tz).strftime('%Y-%m-%d %H:%M')} (北京时间)"))

    # ===== 本期概要 =====
    blocks.append(text("📋 本期概要", bold=True))
    if all_releases.get("vllm"):
        v = all_releases["vllm"][0]
        blocks.append(text(f"• vLLM {v['tag']} 发布 — {v['date']}"))
    if all_releases.get("sglang"):
        s = all_releases["sglang"][0]
        blocks.append(text(f"• SGLang {s['tag']} 发布 — {s['date']}"))
    if all_releases.get("tensorrt-llm"):
        t = all_releases["tensorrt-llm"][0]
        blocks.append(text(f"• TensorRT-LLM {t['tag']} — {t['date']}"))

    # ===== 推理框架 Releases =====
    blocks.append(text("📦 推理框架 Releases", bold=True))
    for fw, releases in all_releases.items():
        if releases:
            blocks.append(text(fw.upper(), bold=True))
            for r in releases[:3]:
                tag = r.get("tag", "N/A")
                date = r.get("date", "N/A")
                body = r.get("body", "")[:200]
                blocks.append(text(f"• {tag} ({date})"))
                if body:
                    blocks.append(text(f"  {body}..."))

    # ===== 相关链接 =====
    blocks.append(text("🔗 相关链接", bold=True))
    links = [
        "• vLLM Releases: https://github.com/vllm-project/vllm/releases",
        "• SGLang Releases: https://github.com/sgl-project/sglang/releases",
        "• TensorRT-LLM: https://github.com/NVIDIA/TensorRT-LLM/releases",
        "• NVIDIA Blog: https://blogs.nvidia.com/",
        "• LMSYS Arena: https://lmarena.ai/",
        "• Artificial Analysis: https://artificialanalysis.ai/",
    ]
    for link in links:
        blocks.append(text(link))

    return blocks

# 构建块
blocks = build_blocks(nvidia_articles, all_releases)

# 分批插入（每批 50 个块）
batch_size = 50
for i in range(0, len(blocks), batch_size):
    batch = blocks[i:i+batch_size]
    result = insert_blocks(doc_id, root_block, batch)
    if result.get('code') != 0:
        print(f"Insert error at batch {i//batch_size}: {result}")
    time.sleep(0.5)  # 防飞书 API 限流

print(f"✅ 飞书文档已生成：https://my.feishu.cn/docx/{doc_id}")
```

## 定时任务配置

建议创建每日 cron job 自动生成报告：

```
hermes cron create \
  --name "AI芯片日报" \
  --prompt "请执行 ai-chip-news-monitor skill，生成当日的 AI 芯片与推理框架资讯报告，写入飞书文档，文档命名格式为：YYYY-MM-DD ai-chip-news-monitor" \
  --schedule "0 9 * * *" \
  --skills ai-chip-news-monitor \
  --deliver origin
```

## ⚠️ 已知限制

1. **飞书文档链接格式**：必须是 `https://my.feishu.cn/docx/{document_id}`（个人云文档），不是 `feishu.cn`（公司版）或 `/doc/`（旧格式）
2. **块插入限制**：每批最多 50 个块，超出报 99992402 → 必须分批
3. **块类型字段名陷阱（最常见 bug）**：飞书 docx API 对 block_type 有严格校验。**heading3（block_type=5）、bullet（block_type=12）、ordered（block_type=13）会返回 `invalid param`（1770001）**。实测只有 `text` 块（block_type=2）100% 可靠。**正确做法**：全部使用 text 块，用内容本身表达格式（粗体=标题，"• "=列表，"1. "=编号）
4. **索引 index=-1 优于指定位置**：写入内容时用 `index: -1`（追加到末尾），比计算位置更可靠，也避免 offset 偏移问题
5. **中国厂商新闻**：大多数无 RSS，需 HTML 解析，建议以 GitHub 结构和公开报道为主
6. **WAF 防护**：沐曦等站点有阿里云 WAF，直接 curl 会返回验证页面
7. **Qualcomm/Tesla**：官网内容受限，建议关注官方 GitHub 和第三方报道
8. **Intel Gaudi/TGI**：Releases 更新较慢（季度级别），注意判断时效性
9. **Feed 解析**：不同 RSS 格式（RSS 2.0 / Atom）需分别处理，优先尝试 ET.parse
10. **⚠️ 时区陷阱（最常见 bug）**：`datetime.now()` 不带时区信息，系统时间可能与实际日期不符。**必须**使用 `bj_tz = timezone(timedelta(hours=8))` + `datetime.now(bj_tz)` 来获取正确的北京日期
11. **LightLLM 仓库**：正确的仓库是 `ModelTC/LightLLM`（非 `Intelligent-Accelerator/lightllm`，后者已 404）

## ⚠️ 关键：飞书文档可见性陷阱

**机器人创建的文档对用户不可见**。飞书开放平台应用默认在机器人私密空间创建文档，用户在自己的飞书文档列表里**看不到也搜不到**这些文档，只有通过直接链接才能访问。链接有效期有限，且用户无法将文档移动到自己的空间。

**正确做法（二选一）**：

### 方案 A：通过飞书消息发送链接（推荐，立即可用）
文档创建后，通过飞书消息把链接发给用户：
```python
# 获取用户 open_id（可在飞书后台或对话上下文中找到）
# 使用发消息 API 把文档链接发给用户
```
用户在消息中点击链接即可访问，不需要文档出现在列表里。适合日报/周报推送场景。

### 方案 B：申请 drive 权限（需管理员操作，一劳永逸）
在[飞书开放平台](https://open.feishu.cn/app/cli_aa9fcb1ebbb81bd6/auth?q=drive:drive)给应用开通 `drive:drive` 权限。开通后：
- 可以创建共享文件夹（用户在自己的文档列表能看到）
- 可以把文档移动到用户个人空间
- 可以设置公开分享链接

如果用户坚持要"在文档列表里看到"，选方案 B。否则默认使用方案 A。

## ⚠️ execute_code 执行环境限制

`execute_code` 的 Python 状态**不跨调用共享**。每次调用是独立的命名空间，上一次调用中定义的变量（如 `all_rel`）在下次调用中不可用。

**正确做法**：把需要用到的数据以内联方式硬编码在脚本中，或通过文件（如 `/tmp/`）中转，不要假设上次定义的变量还存在。

```python
# ❌ 错误：假设 all_rel 在上次调用中定义
all_rel.get(fw, [])

# ✅ 正确：内联数据（适合少量数据）
all_rel = {"vllm": [{"tag": "v0.22.0", ...}], ...}

# ✅ 正确：文件人中转（适合大量数据）
import pickle
with open("/tmp/releases.pkl", "rb") as f:
    all_rel = pickle.load(f)
```

> 📎 LMSYS Blog 详细抓取方法（含近期文章列表、slug 推断、browser 工具技巧）见 `references/lmsys-blog-fetch.md`
> 📎 飞书 API 详细参考（含权限错误码速查、发消息推送方案、Block Type 实测结果 2026-06）见 `references/feishu-doc-api.md`

## 更新频率建议

| 频率 | 内容 | 适用场景 |
|------|------|----------|
| **每日** | GitHub Releases 检查 + 框架更新 | 快速跟进框架迭代 |
| **每周** | 完整报告（框架 + 芯片厂商 + 博客） | 周报汇总 |
| **每月** | 深度分析 + 路线图 + 性能榜单变化 | 战略参考 |
