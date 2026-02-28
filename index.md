---
layout: home
author_profile: true
sidebar:
  nav: "sidebar-nav"
---

DirectX 기반 렌더링 / 엔진 구조 / 디버깅 중심 개발자

---

## 🔥 Tech Stack

- C++
- DirectX 9 / 11
- HLSL
- Win32
- Unreal Engine (학습 중)

---

## 🚀 Featured Projects

- [DX11 Mini Engine](/projects/dx11-engine/)
- [2D AutoChess Prototype](/projects/autochess/)
- [VS6 Dump Debugging 경험 정리](/debugging/vs6-dump/)

{% assign projects = site.projects %}
{% for project in projects %}
  <div style="margin-bottom:40px;">
    <h3><a href="{{ project.url }}">{{ project.title }}</a></h3>
    <p>{{ project.excerpt }}</p>
  </div>
{% endfor %}