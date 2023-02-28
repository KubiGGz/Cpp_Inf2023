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
![Текст с описанием картинки](StandartTime(graph)_page-0001.jpg)
Как мы видим зависимость линейная, что подтверждает асимптотику O(N).
Теперь будем генерировать случайный элемент в массиве и искать его с помощью полного перебора. Для этого будем использовать следующий код:
```C++
#include <iostream>
#include <fstream>
#include <chrono>
#include <string>
#include <random>
using namespace std;

int binary_search(int m[], int n, int element)
{
	int length = n;
	int mid = 0;
	int left = 0;
	int right = length - 1;

	while (right >= 0)
	{
		mid = (left + right) / 2;
		if (m[mid] == element)
		{
			return 1;
		}
		if (m[mid] < element)
		{
			left = mid + 1;
		}
		else
		{
			right = mid - 1;
		}
	}
	return 0;

}


int main()
{
	int arr[100000];
	int time_bn = 0;


	for (int N = 100; N < 100000; N += 10)
	{
		int current_size = N;
		int tmp = 0;
		int num = 0;

		for (int i = 0; i < current_size; i++)
		{
			arr[i] = tmp;
			tmp += 1;
		}

		unsigned seed = 1001;
		default_random_engine rng(seed);
		uniform_int_distribution<unsigned> dstr(0, current_size);
		num = dstr(rng);

		int n = current_size;
		int element = -1;

		auto begin = std::chrono::steady_clock::now();
		for (unsigned cnt = 5000; cnt != 0; --cnt)
		{
			binary_search(arr, n, num);
		}
		auto end = std::chrono::steady_clock::now();
		auto time_span = std::chrono::duration_cast<std::chrono::microseconds>(end - begin);
		time_bn = time_span.count();

		fstream fs;
		fs.open("E:\\timeBN.txt", fstream::in | fstream::out | fstream::app);
		if (fs.is_open())
		{
			fs << time_bn << endl;
		}
		fs.close();
	}
}

```
