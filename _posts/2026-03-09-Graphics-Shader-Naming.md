---
title: "Graphics API 별 Shader 네이밍 차이 (DirectX vs OpenGL vs Vulkan)"
date: 2026-03-09
categories: Graphics
tags: [Graphics, DirectX, OpenGL, Vulkan, Shader]
toc: true
toc_sticky: true
---

# Graphics API 별 Shader 네이밍 차이 (DirectX vs OpenGL vs Vulkan)

그래픽 API를 공부하다 보면 같은 역할의 셰이더인데도 **API마다 이름이 조금 다른 경우**를 볼 수 있다.

대표적인 예가 바로 다음이다.

- **DirectX** → `Pixel Shader`
- **OpenGL / Vulkan** → `Fragment Shader`

처음 보면 서로 다른 개념처럼 보이지만, 실제로는 **거의 동일한 역할의 셰이더 단계**이다.  
이 글에서는 그래픽 파이프라인 기준으로 **API별 셰이더 네이밍 차이**를 정리해본다.

---

# 1. Graphics Pipeline 기준 Shader Stage

일반적인 GPU Rendering Pipeline은 다음과 같은 단계로 구성된다.

Vertex Shader
↓
Primitive Assembly
↓
Rasterizer
↓
Fragment / Pixel Shader
↓
Depth Test
↓
Blending
↓
Frame Buffer


이 중에서 **Rasterization 이후 색을 계산하는 단계**를 정리하면 아래와 같다.

- DirectX → **Pixel Shader**
- OpenGL / Vulkan → **Fragment Shader**


---

# 2. DirectX vs OpenGL Shader 네이밍

| Pipeline Stage | DirectX | OpenGL |
|---|---|---|
| Vertex 처리 | Vertex Shader | Vertex Shader |
| Primitive 처리 | Geometry Shader | Geometry Shader |
| 픽셀 색 계산 | **Pixel Shader** | **Fragment Shader** |
| 범용 GPU 연산 | Compute Shader | Compute Shader |

차이가 나는 부분은 사실상 하나다.

**Pixel Shader = Fragment Shader**

---

# 3. Pixel vs Fragment 개념 차이

두 이름은 **그래픽스 이론 용어 차이**에서 비롯된다.

## Fragment

Rasterization 단계에서는 삼각형이 화면 픽셀로 변환되면서 **fragment**라는 단위가 생성된다.


Triangle
↓
Rasterization
↓
Fragment 생성
↓
Fragment Shader 실행
↓
Depth Test / Stencil Test
↓
Pixel 생성


Fragment는 아직 **프레임버퍼에 기록되기 전의 픽셀 후보**이다.

즉

- Depth test 실패
- Alpha test 실패
- Overdraw

등의 이유로 **버려질 수도 있다.**

---

## Pixel

Pixel은 실제로 **FrameBuffer에 기록되는 최종 값**이다.

DirectX에서는 실용적인 관점에서

fragment ≈ pixel

로 보고 이름을 **Pixel Shader**로 정했다.

---

# 4. Vulkan Shader 네이밍

Vulkan은 OpenGL과 동일한 용어를 사용한다.

| Pipeline Stage | DirectX | Vulkan |
|---|---|---|
| Vertex Shader | Vertex Shader | Vertex Shader |
| Tessellation | Hull Shader | Tessellation Control Shader |
| Tessellation | Domain Shader | Tessellation Evaluation Shader |
| Geometry Shader | Geometry Shader | Geometry Shader |
| Pixel 처리 | **Pixel Shader** | **Fragment Shader** |
| Compute | Compute Shader | Compute Shader |

특히 Tessellation 단계에서도 이름이 다르다.

DirectX
-> Hull Shader
-> Domain Shader

OpenGL / Vulkan
-> Tessellation Control Shader
-> Tessellation Evaluation Shader

OpenGL / Vulkan 쪽이 **기능 설명형 네이밍**이다.

---

# 5. Vulkan Shader Stage Enum

Vulkan에서는 Shader Stage가 다음과 같이 정의된다.

```cpp
VK_SHADER_STAGE_VERTEX_BIT
VK_SHADER_STAGE_FRAGMENT_BIT
VK_SHADER_STAGE_GEOMETRY_BIT
VK_SHADER_STAGE_TESSELLATION_CONTROL_BIT
VK_SHADER_STAGE_TESSELLATION_EVALUATION_BIT
VK_SHADER_STAGE_COMPUTE_BIT
```

DirectX에서는 보통 다음과 같이 줄여 부른다.

VS (Vertex Shader)
PS (Pixel Shader)
GS (Geometry Shader)
HS (Hull Shader)
DS (Domain Shader)
CS (Compute Shader)

---

# 6. 실제 엔진에서는 어떻게 사용할까?

그래픽 엔진에서는 보통 API별 이름을 그대로 사용하지 않고 추상화 레이어를 둔다.

예시)
```cpp
enum ShaderStage
{
    Vertex,
    Fragment,
    Compute
};
```

그리고 내부에서 API별로 매핑한다.

DirectX  → Pixel Shader
OpenGL   → Fragment Shader
Vulkan   → Fragment Shader

대표적인 엔진들도 이런 구조를 사용한다.

Unreal Engine

Unity

Frostbite

idTech

---

# 7. 정리

그래픽 API 별 Shader 네이밍 차이를 정리하면 다음과 같다.

| API     | 픽셀 단계 셰이더       |
|:------- |:--------------- |
| DirectX | Pixel Shader    |
| OpenGL  | Fragment Shader |
| Vulkan  | Fragment Shader |


즉 Pixel Shader = Fragment Shader
개념적으로 동일한 단계이다. 

차이는 **그래픽스 이론 용어 vs 실용적인 API 네이밍 차이** 일 뿐이다.

---

## Reference

* OpenGL Pipeline
=> [https://www.khronos.org/opengl/wiki/Rendering_Pipeline_Overview](https://www.khronos.org/opengl/wiki/Rendering_Pipeline_Overview)

* DirectX Graphics Pipeline
=> [https://learn.microsoft.com/en-us/windows/win32/direct3d11/overviews-direct3d-11-graphics-pipeline](https://learn.microsoft.com/en-us/windows/win32/direct3d11/overviews-direct3d-11-graphics-pipeline)
