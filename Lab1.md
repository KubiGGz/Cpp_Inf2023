### 1. Поиск
Для сравнения асимпттотической сложности алгоритма полного перебора массива и алгоритма бинарного поиска рассмотрим два случая: наихудший - поиск несуществующего элемента в массиве и средний - поиск случайного элемента в массиве.
Ниже представлена программа, перебирающая массив с целью найти несуществующий элемент:
```C++
#include <iostream>
#include <fstream>
#include <chrono>
#include <string>
using namespace std;

int standart_search(int m[], int n, int element)
{
	for (int i = 0; i < n; i++)
	{
		if (m[i] == element)
		{
			return 0;
		}
	}
	return 1;
}


int main()
{
	int arr[100000];
	int time_st = 0;


	for (int N = 100; N < 100000; N += 10)
	{
		int current_size = N;
		int tmp = 0;

		for (int i = 0; i < current_size; i++)
		{
			arr[i] = tmp;
			tmp += 1;
		}

		int n = current_size;
		int element = -1;

		auto begin = std::chrono::steady_clock::now();

		for (unsigned cnt = 1000; cnt != 0; --cnt)
		{
			standart_search(arr, n, element);
		}

		auto end = std::chrono::steady_clock::now();
		auto time_span = std::chrono::duration_cast<std::chrono::microseconds>(end - begin);
		time_st = time_span.count();

		fstream fs1;
		fs1.open("D:\\C++\\Project1\\timeST.txt", fstream::in | fstream::out | fstream::app);
		if (fs1.is_open())
		{
			fs1 << time_st << endl;
		}
		fs1.close();
	}
}

```
На графике представлена зависимость суммарного времени работы функции перебора от количества элементов в массиве:
