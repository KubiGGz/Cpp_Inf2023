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
Представим на графике полученный резульат:
![Текст с описанием картинки](Standart_mid_Time(graph)_page-0001.jpg)
Из графика видно, что случайной распределение заключено между функциями T = N и T = const. Это подтверждает асимптотику алгоритма.

Далее рассмотрим бинарный поиск несуществуюшего элемента в массиве:
```C++
#include <iostream>
#include <fstream>
#include <chrono>
#include <string>
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

		for (int i = 0; i < current_size; i++)
		{
			arr[i] = tmp;
			tmp += 1;
		}

		int n = current_size;
		int element = -1;

		auto begin = std::chrono::steady_clock::now();
		for (unsigned cnt = 10000; cnt != 0; --cnt)
		{
			binary_search(arr, n, element);
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
На графике прослеживается логарифмическая зависимость, что подтверждает асимптотику алгоритма.
![Текст с описанием картинки](Binary_low_Time(graph)_page-0001.jpg)

Теперь будем искать случайный элемент в массиве с помощью бинарного поиска:
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
На полученном графике явно видна логарифмическая зависимость с некоторым распределением. Это подтверждает асимптотику алгоритма и корректность работы кода.
![Текст с описанием картинки](Binary_mid_Time(graph)_page-0001.jpg)



### 2. Сумма двух
Представим код, который с помощью перебора ищет сумму двух элементов, образующих заданное число:
```C++
#include <iostream>
#include <fstream>
#include <chrono>
#include <string>
#include <random>
using namespace std;

int pair_search(int m[], int n, int Num)
{
	for (int i = 0; i < n; i++)
	{
		for (int j = i; j < n; j++)
		{
			if ((m[i] + m[j]) == Num)
			{
				return m[i], m[j];
				return 1;
			}
		}
	}
	return 0;
}


int main()
{
	int arr[6100];
	int time_bn = 0;
	int Num = 0;


	for (int l = 100; l < 6100; l += 10)
	{
		int current_size = l;

		for (int k = 0; k < current_size; k++)
		{
			int tmp = 0;
			arr[k] = tmp;
			tmp += 1;
		}

		int lngth = (2 * current_size) + (current_size / 2);
		unsigned seed = 1001;
		default_random_engine rng(seed);
		uniform_int_distribution<unsigned> dstr(0, lngth);
		Num = dstr(rng);

		auto begin = std::chrono::steady_clock::now();
		for (unsigned cnt = 100; cnt != 0; --cnt)
		{
			pair_search(arr, current_size, Num);
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
Представим график, отражающий асимптотику написанного выше алгоритма:
![Текст с описанием картинки](pair_SimpleFinder(graph)_page-0001.jpg)
Видно, что алгоритм имеет квадратиную асимптотическую сложность от N, количества элементов в массиве.

Теперь приведём более "простой" алгоритм, работающий с линейной асимптотикой:
```C++
#include <iostream>
#include <fstream>
#include <chrono>
#include <string>
#include <random>
using namespace std;

int pair_searcher_advanced(int m[], int n, int N)
{
	int left = 0; //Левая граница
	int right = n - 1; //Правая граница

	while (left != right)
	{
		int sum = m[left] + m[right];
		if (sum < N)
		{
			++left;
		}
		else if (sum > N)
		{
			++right;
		}
		else
		{
			return 1;
		}
	}
	return 0;

}


int main()
{
	int arr[50100];
	int time_bn = 0;
	int Num = 0;


	for (int l = 100; l < 50100; l += 10)
	{
		int current_size = l;

		for (int k = 0; k < current_size; k++)
		{
			int tmp = 0;
			arr[k] = tmp;
			tmp += 1;
		}

		int lngth = (2 * current_size) + (current_size / 2);
		unsigned seed = 1001;
		default_random_engine rng(seed);
		uniform_int_distribution<unsigned> dstr(0, lngth);
		Num = dstr(rng);

		auto begin = std::chrono::steady_clock::now();
		for (unsigned cnt = 5000; cnt != 0; --cnt)
		{
			pair_searcher_advanced(arr, current_size, Num);
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
График подтверждает, что время суммарное время поиска линейно зависит от количества элементов в массиве:
![Текст с описанием картинки](pair_AdvancedFinder(graph)_page-0001.jpg)


### 3. Часто используемый элемент
Приведём стратегию А:
```C++
#include <iostream>
#include <fstream>
#include <chrono>
#include <string>
#include <random>
using namespace std;

int A_strategy(int *m, int n, int key)
{
	for (int i = 0; i < n; i++)
	{
		if (m[i] == key) 
		{
			swap(m[0], m[i]);
			return 1;
		}
	}
	return 0;
}

int main()
{
	int arr[100000];
	int time_bn = 0;
	int Num = 0;


	for (int l = 0; l < 100000; l += 10)
	{
		int current_size = l;

		for (int k = 0; k < current_size; k++)
		{
			int tmp = 0;
			arr[k] = tmp;
			tmp += 1;
		}

		int lngth = (2 * current_size) + (current_size / 2);
		unsigned seed = 1001;
		default_random_engine rng(seed);
		uniform_int_distribution<unsigned> dstr(0, lngth);
		Num = dstr(rng);

		auto begin = std::chrono::steady_clock::now();
		for (unsigned cnt = 5000; cnt != 0; --cnt)
		{
			A_strategy(arr, current_size, Num);
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
Ниже приведём два графика: на первом графике изображена зависимость суммарного времени поиска от количества элементов в массиве при равномерном распределении запросов
![Текст с описанием картинки](Astrat(norm)_page-0001.jpg)
Теперь приведём график, когда запросы распределены неравномерно:
![Текст с описанием картинки](Astrat(not_ravnom)_page-0001.jpg)
Как мы можем видеть, в обоих случаях асимптотика остаётся линейной.

Теперь реализуем стратегию B:
```C++
#include <iostream>
#include <fstream>
#include <chrono>
#include <string>
#include <random>
using namespace std;

int B_strategy(int *m, int n, int key)
{
	for (int i = 0; i < n; i++)
	{
		if ((m[i] == key) && (m[i] != m[0]))
		{
			swap(key, m[i-1]);
			return 1;
		}
	}
	return 0;
}

int main()
{
	int arr[100000];
	int time_bn = 0;
	int Num = 0;


	for (int l = 0; l < 100000; l += 10)
	{
		int current_size = l;

		for (int k = 0; k < current_size; k++)
		{
			int tmp = 0;
			arr[k] = tmp;
			tmp += 1;
		}

		int lngth = (2 * current_size) + (current_size / 2);
		unsigned seed = 1001;
		default_random_engine rng(seed);
		uniform_int_distribution<unsigned> dstr(0, lngth);
		Num = dstr(rng);

		auto begin = std::chrono::steady_clock::now();
		for (unsigned cnt = 5000; cnt != 0; --cnt)
		{
			B_strategy(arr, current_size, Num);
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
Аналогично стратегии A, для стратегии B, приведём зависимость суммарного времени поиска от количества элементов в массиве. На первом графике представлена зависимость при равномерном распределении запросов:
![Текст с описанием картинки](Bstrat(norm)_page-0001.jpg)
Теперь представим зависимость при неравномерном распределении запросов.
![Текст с описанием картинки](Bstrat(not_ravnom)_page-0001.jpg)

Разберёмся теперь со стратегией С. Ниже приведён код, реализовывающий её:
```C++
#include <iostream>
#include <fstream>
#include <chrono>
#include <string>
#include <random>
using namespace std;

int C_strategy(int* m, int n, int key) {
	int elem_check[100000];
	for (int i = 0; i < n; i++)
	{
		if ((m[i] == key) && (i != 0))
		{
			if (elem_check[i - 1] < elem_check[i]) {
				swap(m[i], m[i - 1]);
				swap(elem_check[i], elem_check[i - 1]);
				return 1;
			}
		}
	}
	return 0;
	
}

int main()
{
	int arr[100000];
	int time_bn = 0;
	int Num = 0;


	for (int l = 0; l < 100000; l += 10)
	{
		int current_size = l;

		for (int k = 0; k < current_size; k++)
		{
			int tmp = 0;
			arr[k] = tmp;
			tmp += 1;
		}

		int lngth = (2 * current_size) + (current_size / 2);
		unsigned seed = 1001;
		default_random_engine rng(seed);
		uniform_int_distribution<unsigned> dstr(0, lngth);
		Num = dstr(rng);

		auto begin = std::chrono::steady_clock::now();
		for (unsigned cnt = 5000; cnt != 0; --cnt)
		{
			C_strategy(arr, current_size, Num);
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
