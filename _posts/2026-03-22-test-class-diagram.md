---
title: "Mermaid 클래스 다이어그램 테스트"
date: 2026-03-22
categories: [dev, blog]
tags: [github-pages, jekyll, Mermaid, UML]
toc: true
toc_sticky: true
---

## [ Sample 클래스 다이어그램 (Mermaid UML) ]

현재 내가 사용하는 github 블로그 테마는 **Minimal-Mistakes** 기반이라 포스팅 파일 형식이 마크다운 형식이다.

해당 형식의 마크다운 파일 내에서, UML 다이어그램을 지원하는 것을 확인하였고 샘플로 Mermaid UML 다이어그램을 임의로 만들어 보고 싶었다.
아래가 결과물이다. 

### 결과물

```mermaid
classDiagram
    direction LR

    class Character{
        +int hp
        +int attack
        +Move()
        +TakeDamage(int dmg)
    }

    class Player{
        +Input()
        +UseSkill()
    }

    class Monster{
        +AIUpdate()
    }

    class BossMonster{
        +PhaseChange()
    }

    Character <|-- Player
    Character <|-- Monster
    Character <|-- BossMonster
```