---
title: "DX11 Forward Rendering 정리"
date: 2026-02-26
categories: Graphics
tags: [dx11, rendering]
toc: true
toc_sticky: true
---

## 1. Forward Rendering 이란?

Forward Rendering은 ...

### 특징

- 구현이 단순
- MSAA 사용 가능
- 라이트 수 많으면 비효율

---

## 2. Deferred 와 비교

| 항목 | Forward | Deferred |
|------|----------|----------|
| 구조 | 단순 | 복잡 |
| 성능 | 라이트 적으면 좋음 | 다수 라이트 유리 |

---

### < Test Math >

$$
MVP = Projection \times View \times Model
$$

### < Test Code >

```cpp
void Render()
{
    context->DrawIndexed(...);
}
```