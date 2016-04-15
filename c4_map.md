#C++_STL資料  

##今回の内容
* std::map

>参考ページ  
>[cpprefjp - C++ Library Reference  map](http://cpprefjp.github.io/reference/map/map.html)  


##std::map  
std::mapを使うと、2つの要素を関連付けて登録できる。  
例えば、通常の配列は整数の番号と要素が関連付けられていたが、mapを使えば任意の型同士の要素を関連付けることができる。  
例：文字列と自作した型を関連付ける  
std::mapを使用するには、mapヘッダーをインクルードする。  
std::mapは標準ライブラリのコンテナの一つである。コンテナはデータ構造を表現し、vectorもコンテナの一種である。  

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
	std::map<std::string, double> d_map;
	
	//配列表記を用いて登録
	d_map["PI"] = 3.14;
	d_map["e"] = 2.17;

	//表示
	std::cout << d_map["PI"] << std::endl;
	std::cout << d_map["e"] << std::endl;
}
```


上記の例だと配列表記を用いて値を登録していたが、以下のようにemplace関数を用いて値を登録することもできる。  

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
	std::map<std::string, double> d_map;
	
	//emplace関数を用いて登録
	d_map.emplace("PI", 3.14);
	d_map.emplace("e", 2.17);

	//表示
	std::cout << d_map["PI"] << std::endl;
	std::cout << d_map["e"] << std::endl;
}
```
	

##at関数  
以下のように配列表記で書くと、誤って登録していないキーにアクセスしようとした時に、そのキーが新しく登録されてしまう。  
以下の例では、誤ってaaaというキーでmapから値を取り出そうとした。  

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
	std::map<std::string, double> d_map;
	
	//emplace関数を用いて登録
	d_map.emplace("PI", 3.14);
	d_map.emplace("e", 2.17);

	//表示
	std::cout << d_map["PI"] << std::endl;
	std::cout << d_map["e"] << std::endl;
	
	//新しくaaaが登録される（値は0）
	std::cout << d_map["aaa"] << std::endl;
}
```

at関数を使うと、登録されていないキーにアクセスしようとした時にエラーが出る。  
（それ以外はだいたい配列表記と同じ）

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
	std::map<std::string, double> d_map;
	
	//emplace関数を用いて登録
	d_map.emplace("PI", 3.14);
	d_map.emplace("e", 2.17);

	//at関数を用いて表示
	std::cout << d_map.at("PI") << std::endl;
	std::cout << d_map.at("e") << std::endl;
	
	//コメントを外すとエラーが出る
	//std::cout << d_map.at("aaa") << std::endl;
}
```


##イテレータを用いたmapへのアクセス
以下の例では、for文を用いてmapの各要素を表示している。

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
	std::map<std::string, double> d_map;
	
	//emplace関数を用いて登録
	d_map.emplace("PI", 3.14);
	d_map.emplace("e", 2.17);
	d_map.emplace("one", 1.00);

	//表示
	for (auto i = d_map.begin(); i != d_map.end(); i++){
		std::cout << i->first << " " << i->second << std::endl;
	}
}
```

##find関数  
find関数は、map内でキーが x である要素を検索し、見つかった場合はそれへのイテレータを返し、見つからなかった場合は end() (コンテナの最後の要素の次）を指すイテレータを返す。  
(mapのイテレータ)->secondでmapの要素にアクセスすることができ、  
(mapのイテレータ)->firstでmapのキーにアクセスできる。  

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
	std::map<std::string, double> d_map;
	
	//emplace関数を用いて登録
	d_map.emplace("PI", 3.14);
	d_map.emplace("e", 2.17);
	d_map.emplace("one", 1.00);

	//find関数を用いる
	auto iter = d_map.find("PI");
	std::cout << iter->first << std::endl;
	std::cout << iter->second << std::endl;
}
```

返り値がend()かを調べることによって、要素があるかどうかを調べることができる。  

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
	std::map<std::string, double> d_map;
	
	//emplace関数を用いて登録
	d_map.emplace("PI", 3.14);
	d_map.emplace("e", 2.17);

	//find関数を用いる
	auto iter = d_map.find("PI");

	if (iter != d_map.end()){
		std::cout << iter->first << " " << iter->second << std::endl;
	}
	else{
		std::cout << "見つかりませんでした" << std::endl;
	}


	//find関数を用いる
	auto iter2 = d_map.find("aaa");

	if (iter2 != d_map.end()){
		std::cout << iter2->first << " " << iter2->second << std::endl;
	}
	else{
		std::cout << "見つかりませんでした" << std::endl;
	}
}
```

##unorderd_map
mapより探索速度が少し速く、中で値がソートされていないunorderd_mapというものがある。  
unorderd\_mapを使うには、unorderd\_mapヘッダーをインクルードする。その他の使い方はmapと同じである。  

```cpp
#include <iostream>
#include <string>
#include <unordered_map>

int main()
{
	std::unordered_map<std::string, double> d_map;

	//emplace関数を用いて登録
	d_map.emplace("aaa", 1.00);
	d_map.emplace("ccc", 2.00);
	d_map.emplace("bbb", 3.00);

	//表示
	for (auto i = d_map.begin(); i != d_map.end(); i++){
		std::cout << i->first << " " << i->second << std::endl;
	}
}
```


##演習問題  

1. 以下の様なクラスColorを用意した。string型がキーでColor型を要素に持つマップを用意し、"red", "green", "blue"を登録し、それぞれのr, g, bを表示せよ。  
ちなみにredは(r=255, g = 0, b = 0)である。  

	```cpp
	class Color{
	public:
		int r;
		int g;
		int b;

		Color(int r_, int g_, int b_) :
			r(r_),
			g(g_),
			b(b_)
		{
		}
	};
	```

	>r,g,bを一つ一つ表示するのは面倒なので、以下の関数を使ってよい。  

	```cpp
	void show_color(const Color& col){
		std::cout << "(r, g, b) = (" << col.r << ", " << col.g << ", " << col.b << ")" << std::endl;
	}
	```


1. 色を管理するColorManagerクラスを作ろう。ColorManagerクラスは、上記のmapをメンバに持ち、以下の関数を持つクラスである。

	* void add_color(const std::string& key, const Color& col)  
		新しい色を登録する関数。  

	* Color get_color(const std::string& key)  
		指定された色のクラスを返す関数（Colorクラスを返す)  



##演習問題(DxLib)  
「string型のキー、int型の要素を持つマップ」をメンバに持ち、画像を管理するクラスを作れ。    
※DxLibの画像ハンドルはint型である。  

>ヒント：必要な関数  

* void add_handle(std::string key, int graphHandle)  
	新しい画像ハンドルを登録する関数  
* int get_handle(std::string key)   
	キーを渡して画像ハンドルを取得する関数
