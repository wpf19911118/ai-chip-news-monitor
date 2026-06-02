# 飞书文档 API 参考（ai-chip-news-monitor 专用）

## 文档可见性：核心问题

飞书开放平台应用创建的文档，默认位于**机器人私密空间**，用户在自己的飞书文档列表中**完全看不到**，只能通过直接链接访问。

### API 错误码速查

| 错误码 | 含义 | 解决方案 |
|--------|------|----------|
| `99991672` | 应用缺少 `drive:drive` 权限 | 管理员在开放平台给应用开通权限 |
| `99991401` | parent_token 不合法或无权限 | 检查文件夹 token |
| `99991664` | 无文档操作权限 | 先添加用户为文档成员 |
| `1770001` | invalid param | block 结构格式错误 |
| `10014` | app secret invalid | APP Secret 已过期，更新 .env 凭证 |

## Block Type 实测结果（2026-06-02 实测）

| block_type | 字段名 | 结果 | 备注 |
|------------|--------|------|------|
| 1 | page | ✅ 读取成功 | 文档根节点，只读 |
| 2 | text | ✅ 始终可用 | 最安全，全部用这个 |
| 5 | heading3 | ❌ 400 invalid param | heading3 字段报错 |
| 12 | bullet | ❌ 400 invalid param | bullet 字段报错 |
| 13 | ordered | ❌ 400 invalid param | ordered 字段报错 |

**结论**：只用 `block_type=2` (text)，内容本身表达格式（粗体=标题，"• "=无序，"1. "=有序，"💡"=注释）。42 个块一次写入成功。

## 实测有效的完整写入流程

```python
import urllib.request, json
from datetime import datetime, timezone, timedelta

# 1. 获取 token（每次必须重新获取，过期报 10014）
with open("/home/wangpf/.hermes/.env") as f:
    for line in f:
        if "FEISHU_APP_ID" in line: app_id = line.split("=")[1].strip()
        elif "FEISHU_APP_SECRET" in line: app_secret = line.split("=")[1].strip()

url = "https://open.feishu.cn/open-apis/auth/v3/tenant_access_token/internal"
data = json.dumps({"app_id": app_id, "app_secret": app_secret}).encode()
req = urllib.request.Request(url, data=data, headers={"Content-Type": "application/json"})
with urllib.request.urlopen(req, timeout=10) as r:
    resp = json.loads(r.read())
    if resp.get("code") != 0: print(f"Auth error: {resp}"); return
    token = resp["tenant_access_token"]

# 2. 创建文档（北京时间命名）
bj_tz = timezone(timedelta(hours=8))
date_str = datetime.now(bj_tz).strftime("%Y-%m-%d")
create_url = "https://open.feishu.cn/open-apis/docx/v1/documents"
req = urllib.request.Request(create_url,
    data=json.dumps({"title": f"{date_str} AI芯片资讯速报"}).encode(),
    headers={"Authorization": f"Bearer {token}", "Content-Type": "application/json"})
with urllib.request.urlopen(req, timeout=10) as r:
    doc_id = json.loads(r.read())["data"]["document"]["document_id"]

# 3. root_block = doc_id（不需要单独查询）
root_block = doc_id

# 4. 文本块构造（唯一可靠的块类型）
def text(content, bold=False):
    return {"block_type": 2, "text": {
        "elements": [{"text_run": {"content": content,
            "text_element_style": {"bold": True} if bold else {}}}],
        "style": {}
    }}

blocks = [
    text("中国芯片动态", bold=True),
    text("华为昇腾（Ascend）", bold=True),
    text("• CANN 8.0 RC1 发布，完整支持 PyTorch 2.4"),
    text("开源框架 Releases", bold=True),
    text("1. vLLM v0.6.4 — PagedAttention 2.0"),
    text("💡 更多见 GitHub Trending"),
]

# 5. 一次写入（index=-1 追加，不需要 parent_block_id）
children_url = f"https://open.feishu.cn/open-apis/docx/v1/documents/{doc_id}/blocks/{root_block}/children"
req = urllib.request.Request(children_url,
    data=json.dumps({"children": blocks, "index": -1}).encode(),
    headers={"Authorization": f"Bearer {token}", "Content-Type": "application/json"})
with urllib.request.urlopen(req, timeout=15) as r:
    resp = json.loads(r.read())
    print(f"written={len(resp.get('data',{}).get('children',[]))}")

print(f"文档链接: https://my.feishu.cn/docx/{doc_id}")
```
