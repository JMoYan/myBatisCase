# MyBatis基础应用

# 一、MyBati入门案例

## 1.表结构

~~~~sql
CREATE TABLE `Demo` (
  `sid` int(11) NOT NULL AUTO_INCREMENT,
  `sname` varchar(20) DEFAULT NULL,
  `spwd` varchar(10) DEFAULT NULL,
  PRIMARY KEY (`sid`)
) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8
~~~~

## 2.创建对象

~~~~java
package com.svse.entity;

import java.io.Serializable;

public class Demo implements Serializable {
    private int sid;
    private String sname;
    private String spwd;

    public int getSid() {return sid;}
    public void setSid(int sid) {this.sid = sid;}
    public String getSname() {return sname;}
    public void setSname(String sname) {this.sname = sname;}
    public String getSpwd() {return spwd;}
    public void setSpwd(String spwd) {this.spwd = spwd;}
}
~~~~

## 3.创建全局配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
          <!--数据库驱动-->
        <property name="driver" value="${driver}"/>
          <!--数据库地址-->
        <property name="url" value="${url}"/>
          <!--数据库用户名-->
        <property name="username" value="${username}"/>
          <!--数据库密码-->
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
    <!--映射文件-->
  <mappers>
    <mapper resource="com/svse/mapping/Demo.xml"/>
  </mappers>
</configuration
```

## 4.创建映射文件

insert  映射插入语句 update 映射更新语句 delete 映射删除语句 select 映射查询语句

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.svse.mapping.Demo">
    <!-- 
		        id: 方法名称(唯一的) 
		resultType: 结果类型
	 parameterType: 传入值类型
  	-->
    <select id="selOne" resultType="com.svse.entity.Demo">
        select * from Demo
  	</select>
    <insert id="addOne" parameterType="com.svse.entity.Demo">
    	isert into Demo(sid,sname,spwd) values(#{sid},#{sname},#{spwd})
    </insert>
    <delete id="delOne" parameterType="com.svse.entity.Demo">
        delete from Demo where sid = #{sid}
    </delete>
    <update id="upOne" parameterType="com.svse.entity.Demo">
        update Demo set sname = #{sname},spwd = #{sname} where sid = #{sid}
    </update>
</mapper>
```

## 5.测试运行

测试类

~~~java
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

public class test1 {

    private static final String NAMESPACE = "com.svse.mapping.Demo";

    public static void main(String[] args) {
       	SqlSession sqlSession = MyTool.getSqlSession();
        List<Demo> list = sqlSession.selectList(NAMESPACE + ".selOne");
        System.out.println(list.size());
        System.out.println(list.get(0).getSname());
    }
}
~~~

工具类

~~~java
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;

public class MyTool {
    private static SqlSessionFactory factory = null;
    static {
        String config = "config.xml";
        try {
            InputStream in = Resources.getResourceAsStream(config);
            factory = new SqlSessionFactoryBuilder().build(in);
        } catch (Exception e){
            e.printStackTrace();
        }
    }
    public static SqlSession getSqlSession(){
        SqlSession sqlSession = null;
        if(factory!=null){
            sqlSession = factory.openSession();
        }
        return sqlSession;
    }
}
~~~

# 二、增删改查操作

## 1.查询用户信息

~~~java
public static List<Demo> selOne(){
	
    private static final String NAMESPACE = "com.svse.mapping.Demo";
    
    //全查询
    SqlSession sqlSession = MyTool.getSqlSession();
    List<Demo> list = sqlSession.selectList(NAMESPACE + ".selOne");
    
    //查询一个
    SqlSession sqlSession = MyTool.getSqlSession();
    Demo demo = new Demo();
    Demo.setSid(1);
    List<Demo> list = sqlSession.selectList(NAMESPACE + ".selOne",demo);
}
~~~

## 2.添加用户信息

~~~java
public static void addOne(){
    
   	private static final String NAMESPACE = "com.svse.mapping.Demo";
    
    SqlSession sqlSession = MyTool.getSqlSession();
    
    Demo demo = new Demo();
    Demo.setSid(1);
    Demo.setSname("jack");
    Demo.setSpwd("123");
    
    sqlSession.insert("NAMESPACE"+".addOne",demo);
}
~~~

## 3.更新用户信息

~~~java
public static void upOne(){
    
   	private static final String NAMESPACE = "com.svse.mapping.Demo";
    
    SqlSession sqlSession = MyTool.getSqlSession();
    
    Demo demo = new Demo();
    Demo.setSid(1);
    Demo.setSname("sary");
    Demo.setSpwd("456");
    
    sqlSession.insert("NAMESPACE"+".upOne",demo);
}
~~~

## 4.删除用户信息

~~~java
public static void addOne(){
    
   	private static final String NAMESPACE = "com.svse.mapping.Demo";
    
    SqlSession sqlSession = MyTool.getSqlSession();
    
    Demo demo = new Demo();
    Demo.setSid(1);
    
    sqlSession.insert("NAMESPACE"+".selOne",demo);
}
~~~

# 三、基于Mapper接口的使用方式

