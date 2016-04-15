#C++講座資料 vol_EX

* range-based-forを用いたループ
* テンプレート関数
* random
* staticメンバ
* \#if~\#endif DEBUGの値で分岐する場合
* \#ifdef~\#endif
* \#pragma region~\#pragma endregion
* rand関数


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

##テンプレート関数

引数の２つの値を入れ替えるswap関数について考えよう。int,float,double型についてswap関数を実装すると以下のようになる。

```cpp
#include <iostream>


void swap(int& a, int& b){
	int tmp = a;
	a = b;
	b = tmp;
	return;
}

void swap(float& a, float& b){
	float tmp = a;
	a = b;
	b = tmp;
	return;
}

void swap(double& a, double& b){
	double tmp = a;
	a = b;
	b = tmp;
	return;
}


int main(){
	
	int ia = 2;
	int ib = 3;
	std::cout << ia << " " << ib << std::endl;
	swap(ia,ib);
	std::cout << ia << " " << ib << std::endl << std::endl;

	float fa = 4.12f;
	float fb = 5.35f;
	std::cout << fa << " " << fb << std::endl;
	swap(fa,fb);
	std::cout << fa << " " << fb << std::endl << std::endl;

	double da = 3.14;
	double db = 2.71;
	std::cout << da << " " << db << std::endl;
	swap(da,db);
	std::cout << da << " " << db << std::endl << std::endl;


	return 0;
}
```


内容が全く同じ関数を３つ書くのは手間がかかるし、自作のクラスにswap関数を作るとswap関数はもっと増えていってしまう。
そこでテンプレートという機能を利用する。

```cpp
#include <iostream>

template<typename T>
void swap(T& a, T& b){
	T tmp = a;
	a = b;
	b = tmp;
	return;
}

int main(){
	
	int ia = 2;
	int ib = 3;
	std::cout << ia << " " << ib << std::endl;
	swap(ia,ib);
	std::cout << ia << " " << ib << std::endl << std::endl;

	float fa = 4.12f;
	float fb = 5.35f;
	std::cout << fa << " " << fb << std::endl;
	swap(fa,fb);
	std::cout << fa << " " << fb << std::endl << std::endl;

	double da = 3.14;
	double db = 2.71;
	std::cout << da << " " << db << std::endl;
	swap(da,db);
	std::cout << da << " " << db << std::endl << std::endl;


	return 0;
}
```


追加されたのは

```cpp
template<typename T>
```

という部分だ。 ここでTが任意の型を表すことを明示している。 あとはそのTを引数の型として指定すればいいだけだ。  型の処理はコンパイル時に行われる。したがって、コンパイル時に

```cpp
template<typename T>
void swap(T& a, T& b){
    auto tmp = a;
    a = b;
    b = tmp;
    return;
}
```


の部分は

```cpp
void swap(double& a, double& b){
    auto tmp = a;
    a = b;
    b = tmp;
    return;
}
```


に置き換えられる。 ちなみにここのTはTという名前でなくてもいい。

また、もちろん今までのように型を明示せずにswapを書くこともできるし、 以下のように角括弧の中に型を書く(テンプレートパラメータを指定する)ことで明示的に型を指定することができる。



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





##\#if~\#endif DEBUGの値で分岐する場合

```cpp
#define DEBUG 0

#if DEBUG == 0

#include <iostream>

int main(void){
	std::cout << "aaa" << std::endl;
	return 0;
}

#endif


#if DEBUG == 1

#include <iostream>

int main(void){
	std::cout << "bbb" << std::endl;
	return 0;
}

#endif


#if DEBUG == 2

#include <iostream>

int main(void){
	std::cout << "ccc" << std::endl;
	return 0;
}

#endif
```



##\#ifdef~\#endif
\#ifdefと\#endifで囲んだ部分は\#ifdefの右に来る値が定義されている場合はコンパイルされ、定義されていない場合はコンパイルされない。

```cpp
#define HOGE 123
//#define PIYO 0

#ifdef HOGE

#include <iostream>

int main(void){
	std::cout << "aaa" << std::endl;
	return 0;
}

#endif

#ifdef PIYO

#include <iostream>

int main(void){
	std::cout << "bbb" << std::endl;
	return 0;
}

#endif
```



##\#pragma region~\#pragma endregion

\#pragma region\#pragma endregionで囲んだ部分は折りたたむことが出来るようになる。

```cpp
#pragma region　hoge

#include <iostream>

int main(void){
	std::cout << "aaa" << std::endl;
	return 0;
}

#pragma endregion piyo
```



##rand関数
rand関数を使うと乱数を発生させることが出来る。rand関数で返ってくる値は0～RAND_MAXのどれかである。  
下の例ではrandで返ってきた値を100で割った余りを10回表示している。  
srandで乱数のシードを設定できる。time(NULL)で現在時刻（秒）が返ってくるので、下の例では現在時刻で乱数の種を設定している。シードを設定しないと処理系によるが毎回同じ値が返ってくる。（srandをコメントアウトしてみよう。毎回同じ値が出るはず）


```cpp
#include <iostream>
#include <time.h>

int main(void){

	srand((unsigned int)time(NULL));
	
	for(int i = 0; i < 10; i++){
		std::cout << rand()%100 << " " << std::endl;
	}

	return 0;
}
```