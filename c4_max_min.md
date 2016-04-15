#C++_STL資料  

##今回の内容
* std::max, std::min


>参考ページ  
>[cpprefjp - C++ Reference Site リファレンス algorithm max](http://cpprefjp.github.io/reference/algorithm/max.html)


##std::max, std::min  
std::maxは2値、または複数の値の最大値を返す。  
minは最小値を返す。  
std::maxを使うにはalgorithmヘッダーをインクルードする。  


```cpp
#include <iostream>
#include <algorithm>

int main()
{
	int result1 = std::max(2, 3);
	std::cout << result1 << std::endl;

	int result2 = std::max({ 2, 3, 4 });
	std::cout << result2 << std::endl;

	double result3 = std::max<double>(2, 3.14);
	std::cout << result3 << std::endl;


	//コメントアウトを外すとコンパイルエラー
	//異なる型を自動的に判定は出来ない。上記result3のように指定することが必要
	//int result4 = std::max(2, 3.14);
	//std::cout << result4 << std::endl;

	return 0;
}
```

以下の様なコードをいちいち書く必要がないので便利である。

```cpp
#include <iostream>

int main()
{
	int a = 2;
	int b = 3;

	int max = a;
	if (b > a){
		max = b;
	}

	std::cout << max << std::endl;

	return 0;
}
```


std::maxは「<」演算子がオーバーロードされているクラスなら自作クラスでも使用可能である。  

```cpp
#include <iostream>
#include <algorithm>

class Val{
public:
	double x;
	Val(double x_){
		x = x_;
	}

	bool operator<(const Val &a) const{
		return this->x < a.x;
	}
};

int main()
{
	Val a(2.0);
	Val b(3.0);

	Val maxVal = std::max(a, b);

	std::cout << maxVal.x << std::endl;

	return 0;
}
```


std::maxは「<」演算子がオーバーロードされていないクラスでも、第三引数に大小比較の叙述関数が入っていれば使用可能である。  
叙述関数とは、真偽値(trueまたはfalse)を返す関数のこと。  

```cpp
#include <iostream>
#include <algorithm>

class Val{
public:
	double x;
	Val(double x_){
		x = x_;
	}
};

int main()
{
	Val a(2.0);
	Val b(3.0);

	Val maxVal = std::max(a, b, 
		[](const Val &a, const Val &b){
		return a.x < b.x;
	}
	);

	std::cout << maxVal.x << std::endl;

	return 0;
}
```