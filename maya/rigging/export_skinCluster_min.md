---
title: 【Maya】ウェイトをJSONで書き出す最小構成
parent: Rigging
---

pathに渡すのはディレクトリパス  
第一引数に渡すのがファイル名

```python
import maya.cmds as cmds

filename = “test_skin_wgt.json”
skin_node = “skinCluster1”
out_dir = r”C:\Users\username\AppData\Local\Temp\_test”

cmds.deformerWeights(
    filename,
    ex=True,
    deformer=skin_node,
    path=out_dir,
    fm=“JSON”,
    method=“index”
)
```
