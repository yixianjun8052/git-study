sympy，可用来解方程。

```Python
import sympy as sp

# 解方程：x*x + y*y - 205 = 0, x + y - 7 = 0
x = sp.Symbol('x')
y = sp.Symbol('y')
print(sp.solve([x*x + y*y - 205, x + y - 7], [x, y]))
```