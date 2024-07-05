---
layout: single
title:  "Math - Unity의 MoveTowards()"
categories:
  - Math
---

---

### MoveTowards() 함수

---

[Unity - MoveTowards(Vector3 current, Vector3 target, float maxDistanceDelta)](https://docs.unity3d.com/ScriptReference/Vector3.MoveTowards.html)

유니티에서 `MoveTowards()` 함수는 물체를 `current`부터 `target`까지 이동시키는 데 사용된다.

```cs
public static Vector3 MoveTowards(Vector3 current, Vector3 target, float maxDistanceDelta)
{
    // target 위치에서 current 위치를 뺀다 (벡터의 뺄셈)
    // current에서 target을 향하는 벡터의 각각의 성분을 구한다
    float num = target.x - current.x;
    float num2 = target.y - current.y;
    float num3 = target.z - current.z;

    // current에서 target까지의 거리(크기)의 Square(^)을 구한다
    // root 연산은 연산 비용이 크기때문에 최적화를 위해 아직 sqrt()를 하지 않는다
    float num4 = num * num + num2 * num2 + num3 * num3;

    // 두 벡터 사이의 거리(크기)가 3번째 인자 maxDistanceDelta보다 작다면
    // 이미 충분히 가까워진 것이므로 target 위치를 반환하고 함수를 종료한다
    if (num4 == 0f || (maxDistanceDelta >= 0f && num4 <= maxDistanceDelta * maxDistanceDelta))
    {
        return target;
    }

    // 아직 current와 target간의 거리가 멀다면 그 다음 연산들을 동작한다
    
    // current와 target간의 거리(크기)를 root 연산을 통해 구한다
    float num5 = (float)Math.Sqrt(num4);
    
    // 최종적으로 이동할 위치를 계산한다
    //
    // x 성분을 예로들어, num / num5 * maxDistanceDelta를 해석해 보자면
    //
    // (num / num5) : x 방향으로의 방향벡터 (x 성분을 전체 벡터의 크기로 나누었기 때문에 크기를 가지지 않는다)
    // maxDistanceDelta : 내가 이동하고 싶은 값 (즉 스피드)
    //
    // 이후 각각의 성분 y와 z에도 적용이 되고, 다음 프레임에서의 위치값 (Vector3)를 반환한다
    // 이게 물체를 시간 흐름에 따라 속도를 가지고 이동시키는 함수 MoeTowards(Vector3 current, Vector3 target, float maxDistanceDelta)이다.
    return new Vector3(current.x + num / num5 * maxDistanceDelta, current.y + num2 / num5 * maxDistanceDelta, current.z + num3 / num5 * maxDistanceDelta);
}
```

[제곱근을 구하는 연산 sqrt() 함수가 연산량이 많은 이유](https://joonyle99.github.io/datastructure_algorithm/Math-Newton_Raphson-Algorithm/)
