#C++_STL資料  

##今回の内容
* デザインパターンのFactoryの基本的なやり方  


##factory  
今回は敵のFactoryを作る。  
下の例では、受け取ったstringのキーに応じて、生成される敵が分岐する。  

```cpp
#include <iostream>
#include <memory>
#include <string>

class IEnemy{
public:
	virtual void update() = 0;
};


class EnemyA : public IEnemy{
public:
	void update(){
		std::cout << "EnemyA update called" << std::endl;
	}
};

class EnemyB : public IEnemy{
public:
	void update(){
		std::cout << "EnemyB update called" << std::endl;
	}
};

class EnemyC : public IEnemy{
public:
	void update(){
		std::cout << "EnemyC update called" << std::endl;
	}
};


class EnemyFactory{
public:
	std::shared_ptr<IEnemy> make(std::string key){
		if (key == "EnemyA"){
			return std::make_shared<EnemyA>();
		}
		else if (key == "EnemyB"){
			return std::make_shared<EnemyB>();
		}
		else if (key == "EnemyC"){
			return std::make_shared<EnemyC>();
		}
		return nullptr;
	}
};


int main()
{
	EnemyFactory e_fac;
	
	std::shared_ptr<IEnemy> ptrA = e_fac.make("EnemyA");
	std::shared_ptr<IEnemy> ptrB = e_fac.make("EnemyB");
	std::shared_ptr<IEnemy> ptrC = e_fac.make("EnemyC");

	ptrA->update();
	ptrB->update();
	ptrC->update();
}
```



##mapを使ったfactory  
上の例では、elseifが沢山出てきたが、stirng型をキーとし、functionを要素に持つマップを使うことで、それを回避できる。  


```cpp
#include <iostream>
#include <memory>
#include <string>
#include <map>
#include <functional>

class IEnemy{
public:
	virtual void update() = 0;
};


class EnemyA : public IEnemy{
public:
	void update(){
		std::cout << "EnemyA update called" << std::endl;
	}
};

class EnemyB : public IEnemy{
public:
	void update(){
		std::cout << "EnemyB update called" << std::endl;
	}
};

class EnemyC : public IEnemy{
public:
	void update(){
		std::cout << "EnemyC update called" << std::endl;
	}
};


class EnemyFactory{
public:
	EnemyFactory(){
		fac_map.emplace("EnemyA", [](){
			return std::make_shared<EnemyA>();
		});
		fac_map.emplace("EnemyB", [](){
			return std::make_shared<EnemyB>();
		});
		fac_map.emplace("EnemyC", [](){
			return std::make_shared<EnemyC>();
		});
	}

	std::map<std::string, std::function<std::shared_ptr<IEnemy>()> > fac_map;

	std::shared_ptr<IEnemy> make(std::string key){
		if (fac_map.find(key) != fac_map.end()){
			return fac_map[key]();
		}
		return nullptr;
	}
};


int main()
{
	EnemyFactory e_fac;
	
	std::shared_ptr<IEnemy> ptrA = e_fac.make("EnemyA");
	std::shared_ptr<IEnemy> ptrB = e_fac.make("EnemyB");
	std::shared_ptr<IEnemy> ptrC = e_fac.make("EnemyC");

	ptrA->update();
	ptrB->update();
	ptrC->update();
}
```