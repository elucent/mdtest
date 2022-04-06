# Vector Math

## Dot Products

```c
#include "stdio.h"

typedef struct {
    float x, y, z;
} Vec3;

float dot(Vec3 a, Vec3 b) {
    return a.x * b.x + a.y * b.y + a.z * b.z;
}

int main() {
    Vec3 a = {1, 0, 0};
    Vec3 b = {0, 1, 0};
    Vec3 c = {0, 0, 1};
    printf("%f\n", dot(a, b)); // 0.000000
    printf("%f\n", dot(b, c)); // 0.000000
    printf("%f\n", dot(c, a)); // 0.000000
}
```
