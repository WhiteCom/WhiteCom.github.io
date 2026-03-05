---
layout: single
title: "DirectX Shader 컴파일 관련 이것저것"
date: 2026-03-06
categories: [Graphics]
tags: [Shader]
toc: true
toc_sticky: true
---

* 전제조건 : Visual Studio 환경, DirectX 11 이상 사용한다고 가정

## 미리 컴파일된 Shader 파일 사용 vs 런타임 컴파일 (Shader 직접 컴파일)

DirectX shader 파일 HLSL을 사용하는경우, 미리 컴파일된 셰이더를 사용하는 방식과, 프로그램 실행 시 HLSL 을 컴파일해서 사용하는 방식이 있다. 

아래는 각각의 방식에 대한 특징을 정리한 내용이다.

---

### 1. Shader 를 직접 컴파일 하는 방식 (런타임 컴파일)
```cpp
    {
        // Sample Code
        D3DCompileFromFile(
            L"shader.hlsl",
            nullptr,
            D3D_COMPILE_STANDARD_FILE_INCLUDE,
            "main",
            "vs_5_0",
            0,
            0,
            &blob,
            &error);
    }
```

( 특징 )
- 프로그램 실행 중 HLSL -> Bytecode 컴파일
- CPU가 컴파일 수행

### 장점
1. 디버깅 용이함
1. 개발 단계에서 편리

### 단점
1. 로딩시간이 증가 (CPU 비용 큼)

특히 Shader 갯수가 수십~수백개, 복잡한 Material Shader, Permutation Shader 이면 로딩시간 크게 증가한다.

(예시) : Shader 200개 × 10~30ms
=> 2~6초 로딩 증가

2. 런타임 오류 발생 가능
- 컴파일 실패 => 게임 실행 중 Crash 발생 (비정상 종료)
- Release 빌드에서는 매우 위험

---

### 2. .CSO 파일 사용 (Precompiled Shader)

이 방식은 미리 컴파일 된 Shader bytecode 사용

(예시) : shader.hlsl -> fxc.exe -> shader.cso

런타임에서는 아래와 같이 사용

```cpp
D3DReadFileToBlob(L"shader.cso", &blob);

d3dDevice->CreateVertexShader(
    blob->GetBufferPointer(),
    blob->GetBufferSize(),
    nullptr,
    &m_vertexShader
);
```

( 특징 )
- 이미 컴파일된 Bytecode
- CPU 컴파일 과정 없음
- 로딩만 수행

### 성능차이

| 항목 | 런타임 컴파일 | CSO |
|------|--------------|-----|
| 로딩 시간 | 느림 | 빠름 |
| CPU 사용 | 높음 | 낮음 |
| GPU 실행 | 동일 | 동일 |
| 배포 안정성 | 낮음 | 높음 |

* 여기서 GPU 실행 성능은 동일하다
=> GPU는 Compiled Bytecode 만 실행하기 때문.

---

### 실제 게임 엔진 방식

특이사항 없는경우, 전부 Precompiled Shader 사용

1. Unreal Engine
- HLSL -> Shader Compiler -> DerivedDataCache (미리 컴파일)

2. Unity
- ShaderLab -> Precompile -> 플랫폼 별 Shader Cache

---

### 개발 Workflow

보통은 아래와 같이 진행한다. (특수한 경우는 예외)

- 개발 단계 : Runtime Compile
( 이유 : Shader 수정 즉시 반영 + 디버깅 용이함 )

- 배포 단계 (Release 빌드) : Precompiled CSO

(예시)
```cpp
#ifdef _DEBUG
CompileShaderFromFile(...)
#else // NDEBUG
LoadCSO(...)
#endif
```

---

### 정리

* 가장 좋은 방식
1. 개발단계 : Runtime Compile
2. 배포 : CSO 사용 (Precompiled Shader)

* 성능 기준
- CSO > Runtime Compile
=> CPU 로딩 차이만 존재하며, GPU 성능 차이 없음
