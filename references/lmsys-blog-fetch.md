# LMSYS Blog 抓取参考（2026-06 实测）

## 信源特点

LMSYS 网站（`https://lmsys.org`）使用 **纯客户端渲染**（Next.js），特点：

| 特性 | 说明 |
|------|------|
| RSS Feed | ❌ 无 |
| sitemap | ✅ `https://lmsys.org/sitemap.xml` 有完整博客文章列表 |
| 直接 curl HTML | ❌ 返回空内容 / 404 / Next.js hydration shell |
| 内容获取 | ⚠️ 只能从 sitemap 获取 URL + 日期，无法直接获取正文 |

## 实测可行的抓取方案

### 方案 A：解析 sitemap.xml（推荐，用于 skill）

```python
import subprocess, xml.etree.ElementTree as ET
from datetime import datetime, timezone, timedelta

bj_tz = timezone(timedelta(hours=8))

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
                # 注意：无法获取真实标题，只能从 slug 推断
                'title': slug.replace('-', ' ').title()
            })

    posts.sort(key=lambda x: x['date'], reverse=True)
    return posts

lmsys_posts = fetch_lmsys_recent_posts(max_age_days=30)
for p in lmsys_posts[:10]:
    print(f"• [{p['date']}] {p['title']} — {p['url']}")
```

### 方案 B：通过 browser 工具获取完整正文（用于重要文章）

当某篇 LMSYS 文章需要完整内容时，使用 browser 工具：

```
browser: navigate to https://lmsys.org/blog/2026-06-01-hetero-epd
```

注意：browser 工具抓取速度慢，建议仅对 7 天内的新文章使用，对旧文章只展示 URL 即可。

## 常见 slug 主题映射

根据历史文章 slug 规律，可推断主题：

| slug 关键词 | 主题 |
|------------|------|
| `sglang-*` | SGLang 框架相关（如 `sglang-hisparse`、`sglang-jax`） |
| `deepseek-*` | DeepSeek 相关（如 `deepseek-v4`、`deepseek-V32`） |
| `gb200` / `gb300` | NVIDIA GB200/GB300 测试 |
| `hetero-epd` | 异构 EPD 相关 |
| `mori` | Mori 相关 |
| `leaderboard` | Chatbot Arena 排行榜更新 |
| `llama3` / `glm4` | 特定模型评估 |
| `rocm-miles` | AMD ROCm / Miles 相关 |

## 近期文章示例（2026-06 实测）

```
2026-06-01: https://lmsys.org/blog/2026-06-01-hetero-epd
2026-05-28: https://lmsys.org/blog/2026-05-28-mori
2026-04-29: https://lmsys.org/blog/2026-04-29-p2p-update
2026-04-25: https://lmsys.org/blog/2026-04-25-deepseek-v4
2026-04-10: https://lmsys.org/blog/2026-04-10-sglang-hisparse
2026-03-25: https://lmsys.org/blog/2026-03-25-eep-partial-failure-tolerance
```

## 局限与注意事项

1. **无法获取真实标题**：sitemap 中只有 URL 和日期，没有 `<loc>` 内的 title 标签。标题只能从 slug 推断，不 100% 准确。
2. **无法获取摘要**：没有 RSS description 可用。正文需 browser 工具。
3. **sitemap 更新频率**：LMSYS sitemap 每周更新，过旧的博客文章不会出现在近期筛选结果中。
4. **sitemap 访问**：需要 `--compressed` 参数，否则 gzip 压缩内容可能解析失败。
5. **日期格式**：`lastmod` 字段格式为 ISO 8601（如 `2026-06-01T00:00:00.000Z`），带 `Z` 后缀，需 `replace('Z', '+00:00')` 后再用 `fromisoformat` 解析。
