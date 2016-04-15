#C++_STL資料  

##今回の内容
* using  
* range-based-forを用いたループ
* random
* staticメンバ


##using  
usingを使うと、型の名前の別名を定義することができる。  

```cpp
#include <iostream>

class HogeManagerKeeper{
public:
	void update(){
		std::cout << "HogeManagerKeeper update called" << std::endl;
	}
};

using HogeMK = HogeManagerKeeper;

int main()
{
	HogeMK hoge;
	hoge.update();
}

```



##range-based-forを用いたループ

range-based-forを使うと、配列をまわすfor文が簡潔に書ける。

```cpp
#include <iostream>
#include <vector>

int main(){

    std::vector<int> vec;
 
    // 10個の要素を追加していく
    for(int i = 0; i < 10; ++i ){
        vec.push_back(i);
    }
 
    //range-based-forを用いて出力
    for(const auto& element: vec){
        std::cout << element << std::endl;
    }
    
 
    return 0;

}
```


>書き方

```cpp
for(型名 代入する変数: まわしたいコンテナ名){
}
```




##random
Cで乱数を使うにはstdlib.hをインクルードし、rand()を使ったと思う。  
rand()は線形合同法で乱数を生成しており、生成される数値に偏りが有り、周期が短く、乱数としては性能が良くない。  
そこで、C++11では新しく乱数ライブラリrandomが追加された。randomはメルセンヌ・ツイスタで乱数を生成しており、randより良質な乱数を生成出来る。  
また、指定した範囲の乱数を生成するといった機能もついている。


```cpp
#include <iostream>
#include <random>

int main(void)
{
	// メルセンヌ・ツイスター法による擬似乱数生成器(mt19937)を、
	// ハードウェア乱数(random_device)をシードにして初期化
	std::random_device rd;
	std::mt19937 mt(rd());

	//一様分布生成器 uniform distributionを生成。生成範囲を指定できる
	std::uniform_int_distribution<int> dice(1, 6);
	std::uniform_real_distribution<double> pm(-1.0, 1.0);

	//乱数を出力
	for (int i = 0; i < 10; ++i) {
		std::cout << dice(mt) << std::endl;
	}
	std::cout << std::endl;

	for (int i = 0; i < 10; ++i) {
		std::cout << pm(mt) << std::endl;
	}
	std::cout << std::endl;


	return 0;
}
```


##staticメンバ
staticメンバを使用すると、同じクラスの異なるインスタンス同士で、変数や関数を共有出来る。 

```cpp
#include <iostream>

class myclass{
public:
	static int x;
	myclass(){

	}
};

//クラス外でstatic変数を定義しなければいけない
int myclass::x;

int main(){
	
	myclass obj1;
	myclass obj2;

	obj1.x = 64;
	
	std::cout << "obj1のx : " << obj1.x << std::endl;

	//obj2のxも書き換わっている
	std::cout << "obj2のx : " <<  obj2.x << std::endl;

	//直接表示できる
	std::cout << "直接表示 : " <<  myclass::x << std::endl << std::endl;
	

	//直接書き換えられる
	myclass::x = 100;
	
	std::cout << "obj1のx : " << obj1.x << std::endl;
	std::cout << "obj2のx : " <<  obj2.x << std::endl;
	std::cout << "直接表示 : " <<  myclass::x << std::endl;

	return 0;
}
```


