# CoWindow Attention 论文精读(Project Page)

arXiv:2609.32704v1 · *CoWindow Attention: Full Causal Coverage Is a Collective Property*

> Jingze Shi, Zhangyang Peng, Xianduo Li, Yanlin Qi, Xiaotian Lin, Haoxian Chen, Liangdong Wang, Guang Liu, Yuyu Luo
> HKUST(GZ) · 北京智源研究院(BAAI) · Université Paris Cité
> 论文:<https://arxiv.org/abs/2609.32704>

## 这是什么

一篇中文精读笔记的单页站点。全部 16 张图表以 **300 DPI** 从论文原文(正文 + 附录)提取、等比展示(**未裁剪**),
按「背景 → 方法 → 执行 → 三组实验 → 点评 → 术语」的讲述顺序与讲解段落穿插排布。

精读页入口:**[`CoWindow-Attention论文精读_HTML.html`](CoWindow-Attention论文精读_HTML.html)**(或直接访问 GitHub Pages)

## 论文一句话

把「每个注意力 head 都要重复看到完整因果历史」这条隐含约定,换成「**一组 KV head 合起来看到完整因果历史**」:
所有 KV head 共享近邻窗与前缀 sink 窗,剩余长程距离被均分成互补区间、每个 KV head 认领一段,于是
单个 head 对远处稀疏、合起来对整条因果前缀稠密。不引入任何学习路由器或索引器,训练前向/反向、
推理 prefill 与自回归解码共用同一条位置定义的窗口规则,并按**全局 KV-head 编号**分配窗口以对齐张量并行的 KV-head 切分。

## 精读页要点

本页除逐图讲解外,另有三处自建内容(均已标注「整理」或「推算」):

- **配置走查**:核对论文披露的全部 per-QO-head 预算,发现它们都恰好等于 `1024 + (N_k − 1024)/8`
  ——即「共享窗 1024 + 8 段等宽长程 + 100% 集体覆盖」这一个点(本页推算);
- **张量并行分工**:由论文式 (6) 推出 `H_k = TP = 8` 时 8 段互补长程窗恰好一个 rank 一段;
- **待补测清单**:6 条论文未披露或值得补测的点(含 scaling-law 阶段的窗口拆分未单列、`s̄` 口径与名义预算的差距未展开、
  更长原生序列的质量曲线缺失、$H_k$ 敏感性未测、32B 只与 FullAttn 对照、带干扰的检索与训练稳定性诊断)。

## 目录

```
├── CoWindow-Attention论文精读_HTML.html   # 单文件精读页(中文)
├── figs/                                  # 论文全部图表,300 DPI 原图,共 16 张
│   ├── fig1.png … fig4.png                # 论文 Figure 1–4
│   ├── table1.png … table10.png           # 论文 Table 1–10
│   └── algo1.png, algo2.png               # 论文 Algorithm 1–2
└── README.md
```

## 图片使用说明

`figs/` 内的图表均直接摘自论文 PDF 的对应页面区域(300 DPI 渲染,`fitz.get_pixmap(dpi=300, clip=rect)`),
未做任何缩放、裁剪或重绘;提取后经三重校验(与源页渲染逐像素比对、图注文本框核对、四边留白检查)。
版权归论文作者所有,此处仅用于阅读辅助。

## 反馈

若发现解读错误或图表提取问题,欢迎开 issue。