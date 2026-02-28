---
title: 【Maya】リギング中に見たいアトリビュートだけスプレッドシート表示
parent: Rigging
---

リギング作業をやっているときに、骨関連のアトリビュートだけスプレッドシートで見たいなあというシチュエーションが多かったので書きました。

attrsの中身を書き換えれば、お好きなアトリビュートをフィルターできます。

```python
import maya.cmds as cmds

def make_spread_sheet(attrs, window_name):
    if cmds.window(window_name, exists=True):
        cmds.deleteUI(window_name)

    window = cmds.window(window_name, widthHeight=(640, 160), title=window_name)
    cmds.paneLayout()
    activeList = cmds.selectionConnection(activeList=True)
    cmds.spreadSheetEditor(mainListConnection=activeList, fixedAttrList=attrs, keyableOnly=False)
    cmds.showWindow(window)
    return window


def main():
    attrs = [
        "tx", "ty", "tz",
        "rx", "ry", "rz",
        "sx", "sy", "sz",
        "jox", "joy", "joz",
        "rotateOrder",
        "drawStyle",
        "side",
        "type",
        "radius",
        "segmentScaleCompensate"
    ]

    make_spread_sheet(attrs, "uwe_rig_spreadsheet")

main()
```