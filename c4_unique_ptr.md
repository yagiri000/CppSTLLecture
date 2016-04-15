#C++_STL資料  

##今回の内容
* std::unique_ptr

>参考ページ   
>[C++11スマートポインタ入門](http://qiita.com/hmito/items/db3b14917120b285112f)  
>[cpprefjp - C++ Reference Site リファレンス memory unique_ptr (C++11)](http://cpprefjp.github.io/reference/memory/unique_ptr.html) 


##std::unique_ptr  
newしたポインタは必ずdeleteしなくてはいけない。これは非常に面倒だし、忘れるとメモリリークの原因になる。そんな時に便利なのがスマートポインタだ。
スマートポインタはほぼ通常のポインタと同じ使い方でしかも適切なタイミングで自動的にdeleteされる。  
unique\_ptr, shared\_ptr, weak\_ptrをまとめてスマートポインタと呼ぶ。  


```cpp
#include <iostream>
#include <memory>

class Hoge{
public:
	int x;
	Hoge(int x_) :
		x(x_)
	{
		std::cout << "コンストラクタが呼ばれました" << std::endl;
	}

	~Hoge(){
		std::cout << "デストラクタが呼ばれました" << std::endl;
	}
};


int main(void){

	std::cout << "メイン関数に入りました" << std::endl;

	{
		std::cout << "スコープに入りました" << std::endl;
		std::unique_ptr<Hoge> ptr(std::make_unique<Hoge>(10));
		std::cout << "objのxの値:" << ptr->x << std::endl;
		std::cout << "スコープを抜けます" << std::endl;
	}

	std::cout << "メイン関数を抜けます" << std::endl;

	return 0;
}
```

std::shared\_ptrとstd::unique\_ptrの違いは、コピーが可能かどうかである。  
unique\_ptrはコピーが不可能である。std::moveを使って所有権を移動することができる。  
また、std::unique\_ptrの方がstd::shared\_ptrより処理速度が早い。  

```cpp
#include <iostream>
#include <memory>

class Hoge{
public:
	int x;
	Hoge(int x_) :
		x(x_)
	{
	}
};


int main(void){

	std::unique_ptr<Hoge> ptr1(std::make_unique<Hoge>(10));

	//コメントアウトを外すとコンパイルエラー
	//コピー不可能
	//std::unique_ptr<Hoge> ptr2 = ptr1;


	//std::moveを使用することで所有権の移動が可能
	std::unique_ptr<Hoge> ptr2 = std::move(ptr1);
	std::cout << ptr2->x << std::endl;


	//コメントアウトを外すと実行時エラー
	//ptr1は何も指していない
	//std::cout << ptr1->x << std::endl;


	//if文の中に入れることで中身があるか判定可能
	if (ptr1){
		std::cout << "ptr1中身あり" << std::endl;
	}
	else{
		std::cout << "ptr1中身なし" << std::endl;
	}

	if (ptr2){
		std::cout << "ptr2中身あり" << std::endl;
	}
	else{
		std::cout << "ptr2中身なし" << std::endl;
	}

	return 0;
}

```



##vectorでの使用例  

```cpp
#include <iostream>
#include <memory>
#include <vector>

class Hoge{
public:
	int x;
	Hoge(int x_) :
		x(x_)
	{
	}
};

int main(void)
{
	std::vector<std::unique_ptr<Hoge>> vec;

	for (int i = 0; i < 5; i++){
		vec.emplace_back(std::make_unique<Hoge>(i));
	}

	for (auto &i = vec.begin(); i < vec.end(); i++){
		std::cout << (**i).x << std::endl;
	}



	return 0;
}
```


uniqur_ptrのvectorを持つクラスへ受け渡す例
```cpp
#include <iostream>
#include <vector>
#include <memory>

class Hoge{
public:
	int x;
	Hoge(int x_) :
		x(x_)
	{
	}
};

class HogeManager{
public:
	std::vector<std::unique_ptr<Hoge>> vec;
	void add(std::unique_ptr<Hoge> ptr){
		vec.emplace_back(std::move(ptr));
	}
	void show(){
		for (auto &i = vec.begin(); i < vec.end(); i++){
			std::cout << (**i).x << std::endl;
		}
	}
};


int main(void)
{
	HogeManager hogeManager;
	for (int i = 0; i < 5; i++){
		hogeManager.add(std::make_unique<Hoge>(i));
	}
	hogeManager.show();


	return 0;
}
```

