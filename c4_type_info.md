#C++_STL資料  

##今回の内容
* typeid

>参考ページ  
>[cpprefjp - C++ Library Reference　typeinfo](http://cpprefjp.github.io/reference/typeinfo/type_info.html)  


##typeid  
typeidを使うと、型の情報を得ることが出来る。  
typeidは型名、変数どちらにも使うことが出来る。  
name()関数は型名を返す。  
==演算子を用いてtypeidで得た型が同じかどうかを調べられる。  
下の例の *ptr1 のように、実行時の型情報も得ることが可能である。  
Baseのvertualを消し、Derived1とDerived2を非多相的なクラスにすると、 *ptr1はBaseクラスと表示される。  




```cpp
#include <iostream>
#include <memory>


class Base
{
public:
	Base(){

	}
	virtual void update() = 0;
};


class Derived1 : public Base
{
public:
	Derived1() :
		Base()
	{
	}
	void update(){}
};


class Derived2 : public Base
{
public:
	Derived2() :
		Base()
	{
	}
	void update(){}
};

int main()
{
	Derived1 derived1;

	std::cout << typeid(Base).name() << std::endl;
	std::cout << typeid(Derived1).name() << std::endl;
	std::cout << typeid(derived1).name() << std::endl;
	std::cout << std::endl;

	std::shared_ptr<Base> ptr1 = std::make_shared < Derived1 >();

	std::cout << typeid(ptr1).name() << std::endl;
	std::cout << typeid(*ptr1).name() << std::endl;
	std::cout << std::endl;

	std::cout << (typeid(Derived1) == typeid(Derived1)) << std::endl;
	std::cout << (typeid(Derived1) == typeid(Derived2)) << std::endl;
	std::cout << (typeid(*ptr1) == typeid(Derived1)) << std::endl;
	std::cout << (typeid(*ptr1) == typeid(Derived2)) << std::endl;

	return 0;
}
```

##関連項目  
* type_traitsヘッダー  
>[cpprefjp - C++ Library Reference　type_traits](http://cpprefjp.github.io/reference/type_traits.html)  

