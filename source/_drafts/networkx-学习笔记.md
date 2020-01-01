---
title: networkx 学习笔记
date: 2019-06-04 14:54:00
categorises:
tags:
- python
- networkx
- visualization
---

## Networkx 简介

## Cheat Sheet

### 绘制节点

### 绘制边

### 绘制边标签

```python
import networkx as nx

edges = [['A','B'],['B','C'],['B','D']]
labels = ["AB", "BC", "BD"]
edges_labels = dict(zip(edges, labels))
G=nx.Graph()
G.add_edges_from(edges)
pos = nx.spring_layout(G)

plt.figure()
nx.draw(G,pos,edge_color='black', width=1, linewidths=1,
        node_size=500, node_color='pink', alpha=0.9,
        labels={node:node for node in G.nodes()})
nx.draw_networkx_edge_labels(G, pos, edge_labels=edges_labels,font_color='red')
plt.axis('off')
plt.show()
```

{% asset_img edges_labels.png %}

## 引用
