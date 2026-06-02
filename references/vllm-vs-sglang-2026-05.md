# vLLM vs SGLang 功能对比快照

> 生成时间：2026-05-26  
> 数据来源：GitHub Releases、官方博客、社区讨论

## 框架版本状态

| 框架 | 版本 | 发布日期 | 状态 |
|------|------|----------|------|
| vLLM | v0.21.0 | 2026-05-15 | 最新稳定版 |
| vLLM | v0.20.2 | 2026-05-10 | Bug修复 |
| TensorRT-LLM | v1.3.0rc16 | 2026-05-26 | RC最新版 |
| SGLang | v0.5.12 | 2026-05-16 | 最新版 |

## 功能对比（截至 2026-05-26）

| 维度 | vLLM | SGLang | 领先方 |
|------|------|--------|--------|
| 主线状态 | ✅ 核心支持已合并 main (#40860) | ✅ Day0 PR 已合并 (#23600) | — |
| Roadmap | Issue #40902 | Issue #23602 | — |
| Hopper 支持 | ✅ 完整 (#40860) | ✅ W4A16 完整 (Marlin + mxfp4) | — |
| SM120 (Blackwell) | ✅ PR #40991 closed | ✅ PR #24303 open | vLLM 进度领先 |
| FP4 Indexer | ✅ #40860 | 🔜 进行中 | vLLM 领先 |
| MegaMoE | 🔜 优化中 (#40833) | ✅ 已实现 + w4a4 | SGLang 领先 |
| Pipeline Parallel | 🔜 计划中 | ✅ 已完成 (#24704) | SGLang 领先 |
| HiCache | — | ✅ (#24691) | SGLang 独有 |
| FlashMLA Prefill | 🔜 计划中 | 🔜 集成中 (#25418) | — |
| DeepEP v2 | 🔜 调研中 | 🔜 调研中 | — |
| KV Offloading | 🔜 PD+CPU + 分布式 | — | vLLM 进行中 |
| 超长上下文 | 🔜 Chunked PP + Prefill CP | — | vLLM 进行中 |
| Model Runner V2 | 🔜 @WoosukKwon | — | — |

## 关键差异总结

1. **SM120 支持**：vLLM PR#40991 已关闭合入主线，SGLang PR#24303 仍在 open
2. **策略选择**：两者都采用"不依赖官方 DeepGEMM fork"的策略，在框架侧实现 fallback kernel
3. **MegaMoE**：SGLang 已实现 + w4a4，vLLM 仍在优化中
4. **Pipeline Parallel**：SGLang 已完成，vLLM 计划中
5. **HiCache**：SGLang 独有功能

## 本周热门博客文章

### vLLM 官方博客
- [PegaFlow: Production-Grade External KV Cache](https://vllm.ai/blog/2026-05-18-pegaflow) (05-18)
- [Elastic Expert Parallelism in vLLM](https://vllm.ai/blog/2026-05-14-elastic-expert-parallelism) (05-14)
- [vLLM Tops Artificial Analysis Leaderboard](https://vllm.ai/blog/2026-05-11-vllm-tops-artificial-analysis) (05-11)

### NVIDIA 官方博客
- NVIDIA Vera CPU 发布 (05-26)
- GTC Taipei @ COMPUTEX (05-21)
- Jensen: Demand Is Going Parabolic (05-18)

### LMSYS/SGLang
- [DeepSeek-V4 on Day 0: From Fast Inference to Verified RL with SGLang](https://lmsys.org/blog/2026-04-25-deepseek-v4/) (04-25)

## 相关链接

- vLLM GitHub: https://github.com/vllm-project/vllm
- SGLang GitHub: https://github.com/sgl-project/sglang
- TensorRT-LLM: https://github.com/NVIDIA/TensorRT-LLM
- InferenceX Dashboard: https://inferencex.surianalytics.com/inference
