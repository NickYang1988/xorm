# xorm
基于c++11 标准的orm  

## 特性  

1. 使用简单  
2. head only,方便用于其他项目  

## RoadMap  
1. 增加sqlite,postgre等数据库支持  


### 实例  
#### 初始化配置
````
#include <iostream>
#include "mysql.hpp"
#include "dao.hpp"
using namespace xorm;

struct test {
	mysql::Integer id;
	mysql::Integer a;
	std::string b;
	mysql::MysqlDateTime time;
	mysql::MysqlDate date;
	mysql::MysqlTime tm;
};
REFLECTION(test, id, a, b, time, date, tm)

int main(){

        init_database_config({ "127.0.0.1","root","root","xorm",3306,2 }); //全局初始化配置
	test data;
	data.id = 0;
	data.a = i;
	data.b = "hello,world";
	data.time = "2019-10-25 18:05:01";
	data.date = "2019-10-09";
	data.tm = "18:19:01";
}
````
####  新增数据
````
#include <iostream>
#include "mysql.hpp"
#include "dao.hpp"
using namespace xorm;
int main(){
	dao<mysql> t;
	auto pr = t.insert(data);
	std::cout<<"insert row "<<pr.first<<" insert key "<<pr.second<<std::endl;
}
````

####   删除数据
````
#include <iostream>
#include "mysql.hpp"
#include "dao.hpp"
using namespace xorm;
int main(){
  dao<mysql> t;
  bool r = t.del<test>("where id = 1");
}
````

####   修改数据
````
#include <iostream>
#include "mysql.hpp"
#include "dao.hpp"
using namespace xorm;
int main(){
  test data;
  data.id = 1;
  data.a = 1024;
  dao<mysql> t;
  bool r = t.update(data);
  bool r1 = t.update(data,"where id = 1");
}
````

####   查询数据
````
#include <iostream>
#include "mysql.hpp"
#include "dao.hpp"
using namespace xorm;
int main(){
   dao<mysql> t;
   auto pr = t.query<test>("where id = 1");
   if(pr.first){
     std::vector<test>& vec = pr.second;
     std::cout<< vec.size()<<std::endl;
     if(!vec.empty()){
       bool a_isnull =  vec[0].a.is_null();
     }
   }
   
   auto pr1 = t.query<std::tuple<mysql::Integer,mysql::Integer,std::string,mysql::MysqlDateTime,mysql::MysqlDate,mysql::MysqlTime>>("select * from test");
   if(pr1.first){
      std::cout<< pr1.second.size()<<std::endl;
   }
}
````
