---
layout: post
title: Project Armstrong
categories: [Armstrong]
tags: [Game, Project, Armstrong]
---
# Armstrong D+1

> * In Progress

## [Move Player](https://answers.unity.com/questions/1203035/any-way-to-access-the-global-position-and-rotation.html)

 * 2-D 게임에서는 Canvas안에서 만들어진 UI로 게임이 만들어지니, inspector의 position 값을 script에서 가져오기 위해서는 parent인 Canvas를 참조해서 가져와야 한다. 고로 ``transform.localPosition`` 을 통해서 비교가 가능하다.
 * player의 위치와 boundary 위치를 비교 하기 위해서는 absolute position(global)이 아닌 relative positio(local)로 비교하는 것이 옳다. B/C 하나의 object 내부에서 world position과 관계 없이 서로를 비교 대상으로 비교하기에, local로 비교한다. **단, 새로운 좌표로 옮길 때는, world position으로 할 것 인지, local position으로 할 것 인지 고민해야한다.**
 * 참고: [Code](https://github.com/J-Kyu/Armstrong/blob/a3efde08dc1460f159496a6c866183bfa2eca0bc/Assets/Scripts/TouchObject.cs#L43)

>* [transform.position](https://docs.unity3d.com/ScriptReference/Transform-position.html) - The position of the transform in world space.
>* [transform.localPosition](https://docs.unity3d.com/ScriptReference/Transform-localPosition.html) - Position of the transform relative to the parent transform.
>* [transform.TransformPoint](https://docs.unity3d.com/ScriptReference/Transform.TransformPoint.html) - Transforms position from local space to world space. Note that the returned position is affected by scale.
>* [transform.rotation](https://docs.unity3d.com/ScriptReference/Transform-rotation.html) - The rotation of the transform in world space stored as a Quaternion.
>* [transform.localRotation](https://docs.unity3d.com/ScriptReference/Transform-localRotation.html) - The rotation of the transform relative to the parent transform's rotation.

