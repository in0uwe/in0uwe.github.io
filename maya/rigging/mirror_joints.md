---
title: 【cymel】ジョイントのミラー
parent: Rigging
---

[cymel](https://github.com/ryusas/cymel)が必要なスクリプトです。

殴り書きなので、あとでdocstringとか書きます。

```python
import maya.cmds as cmds
import cymel.main as cm

def make_hie_joints_data(root):
    joints = cm.Os(cmds.ls(root, dag=True, type="joint"))

    datas = []
    for joint in joints:
        _dict = {
            "wm"        : joint.getM(ws=True),
            "jo"        : joint.jo.get(),
            "name"      : joint.nodeName(),
            "parent"    : joint.parent().nodeName(),
            "radius"    : joint.radius.get(),
        }
        datas.append(_dict)

    return datas


def make_mirrord_joints(datas, from_side, to_side, freeze_rotate=True, neg_axis="AXIS_X"):
    for data in datas:
        mirrored_node_name = data.get("name").replace(from_side, to_side)
        mirrored_joint = cm.O(cmds.createNode("joint", name=mirrored_node_name))


        mirrored_wm = data.get("wm").mirror(mirrorAxis=0, negAxis=neg_axis)
        mirrored_joint.setM(mirrored_wm, ws=True)

        mirrored_joint.radius.set(data.get("radius"))

        if data.get("parent") is None:
            continue

        if not cmds.objExists(data.get("parent")):
            continue

        parent_node = data.get("parent").replace(from_side, to_side)
        mirrored_joint.setParent(parent_node)

        if freeze_rotate:
            cmds.makeIdentity(mirrored_joint, apply=True, rotate=True)


def main():
    _datas = make_hie_joints_data(cmds.ls(sl=True)[0])
    print(_datas)

    make_mirrord_joints(_datas, "_l", "_r")

main()
```