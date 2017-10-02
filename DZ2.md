# DZ2 | Scilab | IDE : Visual Studio Code

***NGTU Волков Евгений Александрович 15-ПМ.***
---

# Python

![4](https://github.com/YoungGolden/YoungGolden.github.io/blob/master/img/4.png)

```phyton
import math
from decimal import Decimal
import numpy as np


def func(n):
    for x in np.arange(0 , 6 * math.pi , 0.01): 
        y = Decimal(x)
        s = 1
        for i in range(3 , n + 1 , 2):
            y += Decimal( Decimal(((-1) ** s)*Decimal(x ** i)) / Decimal(math.factorial(i)) )
            i += 2 
            s += 1
        if(math.fabs(Decimal (Decimal(math.sin(Decimal(x))) - Decimal(y))) > e):
            print(n,' ', x-0.01)
            break   

e = Decimal(0.01)


n = int(input('>>>'))
for i in range(1 , n+1 , 2):
    func(i)
```

# Scilab

![3](https://github.com/YoungGolden/YoungGolden.github.io/blob/master/img/3.png)

```scilab
x=-6*%pi:0.1:6*%pi;
x1=-%pi/2:0.1:%pi/2;
y=sin(x);
xlabel('X [ ]');
ylabel('Y [ ]');
title('Разложение sin в ряд')
xgrid();
plot(x,y);
mtlb_hold('on');
z=x1;
plot(x1,z,'black');
mtlb_hold('on');
x2=-%pi/2:0.1:%pi/2;
v=x1-(x1^3)/factorial(3);
plot(x1,v,'y');
mtlb_hold('on');
x3=-%pi:0.1:%pi;
s=x3-((x3^3)/factorial(3))+((x3^5)/factorial(5));
plot(x3,s,'g');
mtlb_hold('on');
f=x3-((x3^3)/factorial(3))+((x3^5)/factorial(5))-((x3^7)/factorial(7));
plot(x3,f,'r');
legend('sin (x)','x','x-(x^3)/3!','x-((x^3)/3!)+((x^5)/5!)','x-((x^3)/3!)+((x^5)/5!)-((x^7)/7!)');
```
