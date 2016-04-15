#C++_STL資料  

##今回の内容
* std::weak_ptr

>参考ページ  
>[cpprefjp - C++ Library Reference  weak_ptr (C++11)](http://cpprefjp.github.io/reference/memory/weak_ptr.html)  

今回は、シェアードの監視者weak_ptrについて扱う。  


##std::weak_ptr  
weak\_ptrは、shared\_ptrを監視するためのものである。  
shared\_ptrが参照しているメモリは、shared\_ptrが参照されている間は開放されない。  
weak\_ptrを使う例：  
敵へのホーミング弾を実装する時に、弾が敵へのshared\_ptrを持っていると、敵が撃破されても弾があるかぎり敵のメモリが解放されない。  
しかし、代わりにweak\_ptrを使うと、敵が撃破された時に敵のメモリは開放され、またweak\_ptrが指している対象のメモリが解放されたことを知ることができる。  

expired()関数は、監視対象のメモリが解放されていればtrue,そうでなければfalseを返す関数。  
lock()関数は、監視対象のshared\_ptrを返す関数。  
以下の例では、Enemyクラスのshared\_ptrにweak\_ptrを使っている。  

```cpp
#include <iostream>
#include <memory>

class Enemy{
public:
	double x;
	Enemy(double x_) : x(x_){}
};

int main()
{
	std::weak_ptr<Enemy> w_ptr;

	{
		std::shared_ptr<Enemy> s_ptr = std::make_shared<Enemy>(6);

		//ウィークポインタにs_ptrを登録
		w_ptr = s_ptr;

		//expried()を使用し、メモリが解放されているか調べる
		if (w_ptr.expired()){
			std::cout << "w_ptrが指しているメモリはすでに解放されています" << std::endl;
		}
		else{
			//lock()を使用してシェアードポインタを取得し、表示
			std::cout << w_ptr.lock()->x << std::endl;
		}

		//ここでスコープを抜けるので、s_ptrは解放される
	}


	if (w_ptr.expired()){
		std::cout << "w_ptrが指しているメモリはすでに解放されています" << std::endl;
	}
	else{
		std::cout << w_ptr.lock()->x << std::endl;
	}
}
```
