---
layout: single
title:  "Math - Unity의 MoveTowards()"
categories:
  - Math
---

---

### MoveTowards() 함수

---

```cs
public static Vector3 MoveTowards(Vector3 current, Vector3 target, float maxDistanceDelta)
{
    float num = target.x - current.x;
    float num2 = target.y - current.y;
    float num3 = target.z - current.z;
    float num4 = num * num + num2 * num2 + num3 * num3;
    if (num4 == 0f || (maxDistanceDelta >= 0f && num4 <= maxDistanceDelta * maxDistanceDelta))
    {
        return target;
    }

    float num5 = (float)Math.Sqrt(num4);
    return new Vector3(current.x + num / num5 * maxDistanceDelta, current.y + num2 / num5 * maxDistanceDelta, current.z + num3 / num5 * maxDistanceDelta);
}
```

