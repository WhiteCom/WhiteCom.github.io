---
layout: single
title: "< Singleton 패턴 >"
date: 2026-03-27
categories: [Software Architecture, Design Pattern]
tags: [C++, Design Pattern]
toc: true
toc_sticky: true
---

## <span class="text-orange">C++ Singleton 패턴</span>

디자인 패턴 중, 싱글턴 패턴에 대해 알아보자. 

싱글턴 패턴의 경우, 본질적으로 전역에서 접근가능한 전역변수, 전역객체이다. 

따라서 아래와 같은 샘플코드와 같이 설계하는 경우, 해당 객체는 프로그램이 시작하고 끝날때까지 메모리에 잡혀있게 된다. 

### 샘플 코드

```cpp
#include <iostream>

//
// Case 1: Singleton pattern implementation
//
class FileManager
{
public:
	FileManager()
	{
		std::cout << "===== FileManager Create =====\n";
	}

	~FileManager()
	{
		std::cout << "===== FileManager Destroy =====\n";
	}

	static FileManager& GetInstance()
	{
		return m_instance;
	}

private:
	static FileManager m_instance;
};

// Define FileManager's static member variable
FileManager FileManager::m_instance;

//
// Main Function
//

int main()
{
	std::cout << "Start Main Function\n";

	FileManager::GetInstance();

	std::cout << "End Main Function\n";

	return 0;
}
```

### [ 결과물 ]

![Result 01](/assets/images/2026-03-27-Singleton/SingleTon_SampleCode_Result_01.webp)

다만 위와같이 쓰게 되면 내가 필요한 순간 외 메모리를 낭비하게 되는 이슈가 있다. 

여기서 Lazy Initialization 이란 방법을 통해 런타임중에 해당 싱글턴 인스턴스를 호출한 시점부터 메모리에 할당되게 할 수도 있다.

Single 객체 Lazy Initialize 샘플코드를 살펴보면 아래와 같다. 

```cpp
#include <iostream>

//
// Case 2: Singleton pattern implementation (Lazuy Initialization)
//
class FileManager
{
public:
	FileManager()
	{
		std::cout << "===== FileManager Create =====\n";
	}

	~FileManager()
	{
		std::cout << "===== FileManager Destroy =====\n";
	}

	static FileManager& GetInstance()
	{
		static FileManager m_instance; // Lazy initialization
		return m_instance;
	}

private:
};

//
// Main Function
//

int main()
{
	std::cout << "Start Main Function\n";

	FileManager::GetInstance();

	std::cout << "End Main Function\n";

	return 0;
}
```

### [ 결과물 ]

![Result 02](/assets/images/2026-03-27-Singleton/SingleTon_SampleCode_Result_02.webp)

위와 같이 Singleton 을 처음 호출했을 시점부터 메모리에 할당되게 처리할 수도 있다.
다르게 말하면, 한번도 해당 Single 을 호출하지 않은경우 메모리에 할당되지 않는다.

물론 Lazy Initialize 방식으로 사용했을때도 단점은 존재한다. 
여전히 전역객체이기 때문에 호출한 이후 프로그램 종료시까지 계속 메모리에 할당되어있는건 이전과 다른점이 없다. 

그리고 게임같은 경우, Lazy Initailize 를 사용하는 경우, 아래와 같은 상황에선 부적합하다. 

### [ 예시 상황 ]

게임의 경우, 시스템 초기화 시, 메모리 할당, 리소스 로딩 등 시간이 소요되는 작업이 존재한다.

1. 오디오 시스템을 초기화 할 때, 수백 ms 이상 소요될 경우 초기화 시점을 제어해야한다. 
만약 처음 소리를 재생할 때 Lazy Initialize 하게 되면, 전투 도중에 초기화 될 수 있으며 이런 경우, 화면 프레임이 떨어지고 게임경험에 악영향을 준다. 

2. 게임에서는 메모리 단편화 (Memory Fragmentation) 를 막기 위해, Heap 영역에 메모리를 할당하는 방식을 세밀하게 제어하는게 보통이다. 
위에서 말한 오디오 시스템이 초기화 될 때, 상당한 메모리를 Heap 영역에 할당하는 경우 Heap 어디에 할당할 지를 제어할 수 있도록 적절한 초기화 시점을 찾아야하는 문제가 발생한다. 

3. Singleton 생성자, 초기화 과정에서 에러가 발생하는 경우 런타임 중간에 프로그램이 비정상종료되는 이슈가 존재한다. (예 : 파일열기 실패, 리소스 로드 실패, 네트워크/Handle 생성 실패)
=> 이러한 경우, Exception 을 던질지, 실패상태를 저장할 지, 아니면 초기화를 재시도 할 건지 미리 정책을 결정해야한다.

위 문제로 인해, Lazy Initailize 를 사용하는 것이 무조건 좋은것은 아니다. 

### [ 결론 ]

Single 패턴의 경우, 관리하는 매니저역할의 객체가 갖게 되는경우, 캡슐화된 클래스 객체를 통해서만 접근가능하게 처리할 수 있어, 이점이 있는건 사실이다. 

하지만, 결국 프로그램 시작 후 종료될 때까지 메모리에 존재하는 문제 및, 멀티스레드 환경일 경우 여러 스레드가 해당 객체에 접근해서 값을 수정하는 경우가 발생할 경우, Race Condition이 발생할 수 있다. 

따라서 이러한 문제점들로 인해 사용은 하되 가능하면 많이 남용하지 않는것을 추천하며, 이로인해 프로그램 설계과정에서 어떤식으로 설계할지 판단이 중요하다. 정말로 현재 사용하는 객체의 경우, 싱글턴 객체가 필요한가? 



