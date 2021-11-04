---
title: 유니티 - 키보드 입력을 통한 리지드바디 이동, 점프 기본 코드
author: Rito15
date: 2021-11-04 20:30:00 +09:00
categories: [Unity, Unity Memo - Shorts]
tags: [unity, csharp, shorts]
math: true
mermaid: true
---

# Note
---
- 입력은 `Update()`, 물리 처리는 `FixedUpdate()`로 철저히 구분하는 것이 핵심

- 다음과 같은 증상들이 발생한다면 `Update()`에서 리지드바디를 조작하고 있는지 의심할 필요가 있다.
  - 왠지 모르게 캐릭터가 벽을 너무 잘 뚫고 지나간다.
  - 콜라이더에 자주 끼인다.
  - 지터링(덜덜 떨림)이 빈번하게 발생한다.
  - 점프할 때마다 점프하는 높이가 달라진다.


# Source Code
---

{% include codeHeader.html %}
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

[RequireComponent(typeof(Rigidbody))]
public class InputAndPhysicsMove : MonoBehaviour
{
    private Vector3 moveDir;
    private bool isMoving = false;
    private bool isJumpRequired = false;
    private Rigidbody rb;

    [SerializeField, Range(0f, 100f)]
    private float moveSpeed = 5f;

    [SerializeField, Range(0f, 100f)]
    private float jumpForce = 5f;

    private void Start()
    {
        if (!TryGetComponent(out rb))
            rb = gameObject.AddComponent<Rigidbody>();
    }

    private void Update()
    {
        // == MOVE ==
        float h = Input.GetAxisRaw("Horizontal");
        float v = Input.GetAxisRaw("Vertical");

        isMoving = (h != 0f || v != 0f);

        if (isMoving)
        {
            moveDir = transform.forward * v + transform.right * h;
            moveDir.Normalize();
        }

        // == JUMP ==
        if (Input.GetKeyDown(KeyCode.Space))
            isJumpRequired = true;
    }

    private void FixedUpdate()
    {
        if (isMoving)
        {
            Vector3 moveOffset = moveDir * (moveSpeed * Time.fixedDeltaTime);
            rb.MovePosition(rb.position + moveOffset);
        }

        if (isJumpRequired)
        {
            rb.AddForce(new Vector3(0f, jumpForce, 0f), ForceMode.VelocityChange);
            isJumpRequired = false;
        }
    }
}
```

