+++
date = "2016-12-06T15:06:27+09:00"
title = "그래픽스 렌더링 파이프라인 (Graphics Rendering Pipeline)"
subtitle = "OpenGL 렌더링 파이프라인"
tags = ["Computer Graphics"]
+++

# 사설
컴퓨터 그래픽스 분야를 처음 공부하면, 삼각형 그리기부터 시작해서 버텍스 트랜스폼, 라이팅, 쉐이딩 등 [고정 함수 파이프라인(Fixed Function Pipeline)](https://www.opengl.org/wiki/Fixed_Function_Pipeline)을 이용하는 방법부터 익히게 된다. 그리고 이런 방법이 어느정도 익숙해지면, Programmable Pipeline을 이용한 쉐이더 프로그래밍을 시작한다.

그러나 쉐이더 프로그래밍에 입문하고 나서도, 렌더링 파이프라인을 제대로 이해하지 못해 그저 결과만 낼 수 있도록 구현 방법만 익히는 경우가 많다. 결국 이 상태가 유지된 채로 고급 쉐이딩 기술을 구현해야 하는 때가 오고, 그래픽스 파이프라인을 이해하지 않고선 구현조차 못 하는 지경에 이르기 때문에 처음부터 다시 공부하게 된다.

물론, 학교 커리큘럼에서 그래픽스 파이프라인에 대해 다루지 않는 것은 아니다. 다만, 막 걸음마를 뗐는데 전체 과정을 아우르는 개념을 이해하는 것도 어렵고, 사용하는 방법을 가르치기에도 시간이 빠듯하기 때문인지 학생들에게 완벽한 이해를 바라지 않는 것 같다. (적어도 내가 다닌 학교에서는 그러했다.)  

그래서 컴퓨터 그래픽스 분야를 공부한다면 반드시 꿰고 있어야할 **그래픽스 렌더링 파이프라인**을 정리해보고자 한다.  
다만, 각각의 단계에 대한 심도있는 이해는 여전히 공부가 필요할 것이다.

# OpenGL 렌더링 파이프라인
사용할 줄 아는 그래픽스 라이브러리가 OpenGL이므로 OpenGL의 렌더링 파이프라인을 통해 설명한다. DirectX는 써본 적이 없어 잘 모르지만, 기본적인 골격은 비슷할 것이다.

OpenGL 렌더링 파이프라인의 단계를 나열했다. 괄호는 생략 가능한 단계이다.

> 1. Vertex Specification
> 2. Vertex Processing
>     1. Vertex Shader
>     2. [Tessellation]
>     3. [Geometry Shader]
> 3. Vertex Post-Processing
>     1. [Transform Feedback]
>     2. Clipping, Perspective Divide, Viewport Transform
> 4. Primitive Assembly
>     * [Face Culling]
> 5. Rasterization
> 6. [Fragment Shader]
> 7. Per-sample Operations
>     1. [Scissor Test]
>     2. [Stencil Test]
>     3. [Depth Test]
>     4. [Blending]
>     5. [Logical Operation]
>     6. Write Mask

## Vertex Specification
응용프로그램으로부터 버텍스를 읽고 렌더링(입력 데이터를 해석)한다.
## Vertex Processing
#### Vertex Shader
#### Tessellation
번역하면 [쪽매맞춤](https://ko.wikipedia.org/wiki/쪽매맞춤)이지만, 컴퓨터 그래픽스에서 사용되는 기술은 **테셀레이션**이라고 영문 그대로 읽는다.
#### Geometry Shader

## Vertex Post-Processing
#### Transform Feedback
#### Clipping, Perspective Divide, Viewport Transform

## Primitive Assembly
전달받은 데이터를 primitive로 묶는 단계. Face culling
#### Face Culling

## Rasterization

## Fragment Shader

## Per-sample Operations
Fragment Shader의 결과물(Samples=Fragments)을 
#### Scissor Test
#### Strencil Test
#### Depth Test
#### Blending
#### Logical Operation
Blending이 수행되지 않는다면 Logical Operation이 수행된다. Fragments가 Framebuffer에 어떻게 쓰여질지 결정한다.
#### Write Mask

# References
* https://www.opengl.org/wiki/Rendering_Pipeline_Overview