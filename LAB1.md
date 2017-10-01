# Lab1 | Linux 16.04 | Scilab |IDE : Visual Studio Code

Компилируем так:

**c++:**
* Первый способ (позволяет задавать данные системы уравнения уравнений из командной строки) 
>g++ gauss.cpp -o return 
>./return
```txt
3
1 2 5
6 2 1
5 4 3
2 70 3
```

* Второй способ (позволяет считывать данные из заранне подготовленного файла) 
>g++ gauss.cpp -o return 
>./return < basecpp.in

# C++ (gauss.cpp)

![cpp](https://github.com/YoungGolden/YoungGolden.github.io/blob/master/img/cpp.png)

```c++
#include <iostream>
#include <cmath>
#include <vector>

using namespace std;

void print(vector< vector<double> > A) {
    int n = A.size();
    // Выводим матрицу
    for (int i=0; i<n; i++) {
        for (int j=0; j<n+1; j++) {
            cout << A[i][j] << "\t";
            if (j == n-1) {
                cout << "| ";
            }
        }
        cout << "\n";
    }
    cout << endl;
}

vector<double> gauss(vector< vector<double> > A) {
    int n = A.size();

    for (int i=0; i<n; i++) {
        // Поиск максимума в этом столбце
        double maxEl = abs(A[i][i]);
        int maxRow = i;
        for (int k=i+1; k<n; k++) {
            if (abs(A[k][i]) > maxEl) {
                maxEl = abs(A[k][i]);
                maxRow = k;
            }
        }

        // Поменяем местами максимальную строку с текущей строкой (по столбцу)
        for (int k=i; k<n+1;k++) {
            double tmp = A[maxRow][k];
            A[maxRow][k] = A[i][k];
            A[i][k] = tmp;
        }

        // Сделаем все строки ниже 0 в текущем столбце
        for (int k=i+1; k<n; k++) {
            double c = -A[k][i]/A[i][i];
            for (int j=i; j<n+1; j++) {
                if (i==j) {
                    A[k][j] = 0;
                } else {
                    A[k][j] += c * A[i][j];
                }
            }
        }
    }

    // Решим уравнение Ax = b для верхней треугольной матрицы A
    vector<double> x(n);
    for (int i=n-1; i>=0; i--) {
        x[i] = A[i][n]/A[i][i];
        for (int k=i-1;k>=0; k--) {
            A[k][n] -= A[k][i] * x[i];
        }
    }
    return x;
}

int main() {
    cout << "| ";
    int n;
    cin >> n;

    vector<double> line(n+1,0);
    vector< vector<double> > A(n,line);

    // Чение выходных данных
    for (int i=0; i<n; i++) {
        for (int j=0; j<n; j++) {
            cin >> A[i][j];
        }
    }

    for (int i=0; i<n; i++) {
        cin >> A[i][n];
    }

    // Вывод входных данных
    print(A);

    // Делаем подсчет
    vector<double> x(n);
    x = gauss(A);

    // Показываем результат
    cout << "Result:\t";
    for (int i=0; i<n; i++) {
        cout << x[i] << " ";
    }
    cout << endl;
}
```

# basecpp.in
```txt
3
1 2 5
6 2 1
5 4 3
2 70 3
```
---

**python:**
* Первый способ (позволяет задавать данные системы уравнения уравнений из командной строки) 
>python gauss.py
```txt
3
1 2 5
6 2 1
5 4 3
    (Enter)
2 70 3
```

* Второй способ (позволяет считывать данные из заранне подготовленного файла) 
>python gauss.py < datapy.in

# Python (gauss.py)
![py](https://github.com/YoungGolden/YoungGolden.github.io/blob/master/img/py.png)
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-


def pprint(A):
    n = len(A)
    # Вывод матрицы
    for i in range(0, n):
        line = ""
        for j in range(0, n+1):
            line += str(A[i][j]) + "\t"
            if j == n-1:
                line += "| "
        print(line)
    print("")


def gauss(A):
    n = len(A)

    for i in range(0, n):
        # Поиск максимума в этом столбце
        maxEl = abs(A[i][i])
        maxRow = i
        for k in range(i+1, n):
            if abs(A[k][i]) > maxEl:
                maxEl = abs(A[k][i])
                maxRow = k

        # Поменяем местами максимальную строку с текущей строкой (по столбцу)
        for k in range(i, n+1):
            tmp = A[maxRow][k]
            A[maxRow][k] = A[i][k]
            A[i][k] = tmp

        # Сделаем все строки ниже 0 в текущем столбце
        for k in range(i+1, n):
            c = -A[k][i]/A[i][i]
            for j in range(i, n+1):
                if i == j:
                    A[k][j] = 0
                else:
                    A[k][j] += c * A[i][j]

    # Решим уравнение Ax = b для верхней треугольной матрицы A
    x = [0 for i in range(n)]
    for i in range(n-1, -1, -1):
        x[i] = A[i][n]/A[i][i]
        for k in range(i-1, -1, -1):
            A[k][n] -= A[k][i] * x[i]
    return x


if __name__ == "__main__":
    from fractions import Fraction
    n = input()

    A = [[0 for j in range(n+1)] for i in range(n)]

    # Чение выходных данных
    for i in range(0, n):
        line = map(Fraction, raw_input().split(" "))
        for j, el in enumerate(line):
            A[i][j] = el
    raw_input()

    line = raw_input().split(" ")
    lastLine = map(Fraction, line)
    for i in range(0, n):
        A[i][n] = lastLine[i]

    # Вывод входных данных
    pprint(A)

    # Делаем подсчет
    x = gauss(A)

    # Показываем результат
    line = "Result:\t"
    for i in range(0, n):
        line += str(x[i]) + "\t"
    print(line)
```

# basepy.in

```txt
3
1 2 5
6 2 1
5 4 3
    (Enter)
2 70 3
```

**scilab**
```txt
// В командной строке передаем данные в функцию

-->A = [2,4,6;3,-2,1;4,2,-1]; b = [14;-3;-4];
-->gausselim(A,b)    //Вызов функции без назначения
 ans  =
! - 1. !
!   1. !
!   2. !
-->x = gausselim(A,b)  //Вызов функции с назначением
 x  =
! - 1. !
!   1. !
!   2. !
```

# Scilab

![scilab](https://github.com/YoungGolden/YoungGolden.github.io/blob/master/img/scilab.png)

```scilab
function [x] = gausselim(A,b)
    
//Эта функция получает решение системы
//Линейных уравнений A * x = b, учитывая матрицу коэффициентов A
//и вектор правой стороны, b

[nA,mA] = size(A)
[nb,mb] = size(b)

if nA<>mA then
    
                    error('gausselim - Матрица А должна быть квадратной');
                    abort;
elseif mA<>nb then
        error('gausselim - несовместимые размеры между A и b');
        abort;
end;

a = [A b];  //Матричное увеличение
//Прямое устранение

n = nA;
for k=1:n-1
    for i=k+1:n
                     for j=k+1:n+1
                     a(i,j)=a(i,j)-a(k,j)*a(i,k)/a(k,k);
end;
    end;
end;
//Обратная подстановка

x(n) = a(n,n+1)/a(n,n);

for i = n-1:-1:1
                      sumk=0
                      for k=i+1:n
                      sumk=sumk+a(i,k)*x(k);
                      end;
                      x(i)=(a(i,n+1)-sumk)/a(i,i);
end;

//End function
```
