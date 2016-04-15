#C++_STL資料  

##今回の内容
* std::stringstream  

>参考ページ  
>[●C++編（標準ライブラリ）　第３２章　文字列ストリーム](http://ppp-lab.sakura.ne.jp/cpp/library/032.html)  


##std::stringstream  
std::stringstreamを使うと文字列とint型やdouble型を簡単に連結出来る。  
str()関数で文字列を取得することが出来る。  

```cpp
#include <iostream>
#include <sstream>

int main()
{
	std::stringstream ss;

	std::string text = "aaa";
	int x = 1;
	double y = 3.14;

	ss << text << " " << x << " " << y;

	std::cout << ss.str() << std::endl;

	return 0;
}

```



##std::to_string  
std::to_string関数を使うとint型やdouble型をstd::string型に変換できる。  
しかし、桁数の指定が出来ない。  

```cpp
#include <iostream>
#include <string>

int main()
{
	double x = 3.14;
	std::string text = std::to_string(x);
	
	std::cout << text << std::endl;

	return 0;
}
```



##iomanipヘッダーをインクルードし、書式設定を行う  
std::stringstreamはストリームなので、書式設定が可能である。  
以下のようにすると、小数点以下の桁数を指定することが出来る。  

```cpp
#include <iostream>
#include <sstream>
#include <iomanip>

int main()
{
	std::stringstream ss;
	double y = 3.14;

	ss << std::fixed << std::setprecision(5) << y;

	std::cout << ss.str() << std::endl;

	return 0;
}

```