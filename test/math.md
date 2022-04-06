# Arithmetic

## Addition

Tests integer and floating point addition.

```c
#include "stdio.h"
int main() {
    printf("%d\n", 1 + 2);          // 3
    printf("%d\n", 4 + 1);          // 5
    printf("%d\n", 100 + 5);        // 105
}
```

```c
#include "stdio.h"
int main() {
    printf("%f\n", 1.0f + 2.0f);    // 3.000000
    printf("%f\n", 1.5f + 0.25f);   // 1.750000
}
```

## Division

Tests integer and floating point division.

```c
#include "stdio.h"
int main() {
    printf("%d\n", 4 / 2);          // 2
    printf("%d\n", 9 / -3);         // -3
    printf("%d\n", 13 / 2);         // 6
}
```

```c
#include "stdio.h"
int main() {
    printf("%f\n", 3.0f / 2.0f);    // 1.500000
    printf("%f\n", 8.0f / 4.0f);    // 2.000000
}
```

# Trigonometry

## Sine

```c
#include "math.h"
#include "stdio.h"

int main() {
    printf("%f\n", sin(0));             // 0.000000
    printf("%f\n", sin(M_PI / 2));      // 1.000000
    printf("%f\n", sin(3 * M_PI / 2));  // -1.000000
}
```

## Cosine

```c
#include "math.h"
#include "stdio.h"

int main() {
    printf("%f\n", cos(0));             // 1.000000
    printf("%f\n", cos(M_PI / 2));      // 0.000000
    printf("%f\n", cos(3 * M_PI / 2));  // -0.000000
}
```