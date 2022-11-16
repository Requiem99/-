# Mybatis-Study

## Mybatis-Study

JDBC **Java 数据库连接（Java DataBase Connectivity）**就是Java规定的接口，各种关系型数据库来实现这个接口，就叫做驱动。如mysql驱动

jdbcTemplate会自动释放连接。

## MyBatis组件关系

![mybatis关键组件关系](image/mybatis关键组件关系.png)

## mybatisUtil

```java
public class MybatisUtil {
    private static SqlSessionFactory sqlSessionFactory; //提高作用域
    static {

        try {
            //使用Mybatis的第一步：获取SqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory =  new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    //有了SqlSessionFactory就可以获取SqlSession实例。
    //SqlSession 完全包含了面向数据库执行SQL命令的所有方法
    public static SqlSession getSqlSession(){
        return  sqlSessionFactory.openSession(true);/*进行增删改会自动提交事务*/
    }
}
```

### resultType

```xml
<select id="getPaymentList"  resultType="Payment">
    select * from payment
</select>
```

resultType:

**1、基本类型  ：resultType=基本类型**

**2、List类型：  resultType=List中元素的类型**

**3、Map类型   resultType =map**