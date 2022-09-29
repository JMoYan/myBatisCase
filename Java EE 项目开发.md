# JAVA EE笔记

## 1.注释写法

>HTML注释。 该注释主要用于客户端动态的显示一个注释，又称输出注释
>
>语法格式：<!--  注释内容 --!>

> JSP注释。该注释隐藏在JSP源代码中，它不会输出到客户端，又称隐藏注释
>
> 语法格式：<% 注释内容 %>

>Script中的注释。通常使用“//”表示单行注释，使用“/** */”表示多行注释。

## 2.JSP指令

> 1.page指令
>
> page指令是针对当前页面的指令，作用于整个JSP页面。通常位于JSP页面顶端，一个JSP页面可以有多条page指令，但是属性只能出现一次，
>
> 重复的属性讲覆盖先前的设置。
>
> language：定义脚本使用语言，当前只能是java
>
> import：用于引用要使用的类
>
> session：指定页面是否使用session对象。默认值为true
>
> buffer：指到客户端输出流的缓冲模式。如果为none，则不缓冲，如果			指定数值就不能超出这个值的缓冲区进行缓冲。

## 3. 传值方式

### 3.1.从JSP到servlet传值的方式

1、利用超链接<a></a>来传递参数

例如：

~~~jsp
<td><a href="index.jsp?id='1'&name='bob'">删除</a></td>
~~~

​		点击a标签，就可以把id值传入到servlet中,id值为'1'。同理，name也为一个参数，如果想传递更多的参数，只需要用&隔开即可。

在servlet中的使用以下方法获取值

~~~java
String id = request.getParameter("id");
String name = request.getParameter("name");
~~~

2.利用form标签的input标签，将JSP页面输入的值传入servlet中

例如:

```jsp
<form action="login" method="post">
	姓名：<input type="text" name="name">
	学号：<input type="text" name="number">
	<input type="submit" value="登录">
</form>
```

则在servlet中的post方法写入以下代码就可以获取到值:

```java
String name = request.getParameter("name");
String number = request.getParameter("number");
```

3.利用form表单的action传值

例如:

```jsp
<form action="login.jsp/?oper='login'" method="post">
	姓名：<input type="text" name="name">
	学号：<input type="text" name="number">
<input type="submit" value="登录">
</form>
```

在servlet中的post方法写入以下代码就可以传值，跟a标签的传值是一样的。但是需要注意的是：==这里的method必须为post，如果为get，则oper的值将不会被传入servlet中，这是就会出现空指针的错误！==　　

~~~java
String name = request.getParameter("name");
String number = request.getParameter("number");
~~~

### 3.2.从servlet传值到JSP的方式

1、重定向 ( Redirect)

​		内容和 url都改变，不允许带 request参数( session参数可以)，即不允许在servlet里给 request对象使用setAttribute方法传给下一页面。在 servlet里使用 response.sendRedirect(url) 方法。注意这里的 url前不带斜线 /，如 response.sendRedirect(”test.jsp“)

~~~java
× request.setAttribut("参数名",参数);
√ response.sendRedirect("目标路径不带/");
~~~

2.url转发 ( Forward)

​		页面的跳转，页面内容发生改变，url不变。可以带 request和 session参数。在 servlet里使用 getServletConfig().getServletContext().getRequestDispatcher(url).forward(request, response)。而这里的url前需要带斜线 /，如getServletConfig().getServletContext().getRequestDispatcher(”/test.jsp“).forward(request, response)

## 4.JSP四大域对象

### 4.1.四大域对象作用范围

1.page范围

​	pageContext：只在一个页面中保存属性，跳转之后无效。

2.request范围

​	request：只在一次请求中保存，服务器跳转后依然有效。

3.session范围

​	session：在一次会话范围中，无论何种跳转都是使用。

4.application

​	application：在整个服务器上保存。

| 方法                                             | 类型 | 描述             |
| :----------------------------------------------- | :--- | :--------------- |
| public void setAttribute(String name,Object o){} | 普通 | 设置属性名称及   |
| public Object getAttribute(String name){}        | 普通 | 根据属性名取属性 |
| public void removeAttribute(String name){}       | 普通 | 移除指定属性     |

1.服务端跳转

~~~jsp
<jsp:forword page="跳转的页面地址"></jsp:forword>
~~~

2.客户端跳转

~~~jsp
<!-- a标签跳转 -->
<a href="跳转的页面地址"></a>
~~~

## 5.HTML不同input值的获取方法

### 5.1.输入框获取值方法



~~~ jsp
<form action="${pageContext.request.contextPath}/Servlet" method="post">
    请输入你的名字:<input type="type" name="uName">
</form>
<%
	String uName = request.getParameter("uName");
%>
~~~

### 5.2.单选按钮获取值

~~~ jsp
<form action="${pageContext.request.contextPath}/Servlet" method="post">
    男:<input type="radio" name="uSex" value="男">
    女:<input type="radio" name="uSex" value="女">
</form>
<%
	String uSex =req.getParameter("uSex");
%>
~~~

### 5.3.多选按钮获取值

~~~jsp
<form action="${pageContext.request.contextPath}/Servlet" method="post">
    游泳:<input type="checkbox" name="uHob" value="学习">
    看书:<input type="checkbox" name="uHob" value="打游戏">
    唱歌:<input type="checkbox" name="uHob" value="唱歌">
</form>
<%
	String[] uHob = req.getParameterValues("uHob");
	for(int i = 0;i<hob.length;i++){
        System.out.println("uHob" + i + ":" + hob[i]);
    }
%>
~~~

### 5.4.下拉框获取值

~~~jsp
<form action="${pageContext.request.contextPath}/Servlet" method="post">
   <select name="uSelect">
            <option>学号</option>
            <option>姓名</option>
            <option>性别</option>
   </select>
</form>
<%
	String uSelect = req.getParameter("uSelect");
%>
~~~

## 6.EL语法

## 7.JSTL语法

### 7.1.JSTL标签库的主要组成

​	（1）.==核心标签库==：核心标签库是整个JSTL最常用的部分。主要由以下几个部分组成:==基本输入输出==、==流程控制==、==迭代操作和URL操作==。负责Web应用的常见工作，如==循环==、==表示式赋值==、==基本输入输出控制==等......

​	（2）.i18N格式标签库：用来格式化显示数据的工作，如对不同区域的日期格式化等。

​	（3）.XML标签库：用来访问XML文件的工作，支持JSP对XML文档的处理。

​	（4）.==数据库标签库==：SQL标签库包括大部分访问数据库的逻辑操作，包括查询、更新、事务处理、设置数据源等。可以做访问数据库的工作。

### 7.2.JSTL核心标签库

#### 7.2.1.输出标签<c:out>

```jsp
<!-- 使用taglib指令引用标签库 -->
<%taglib prefix="c" url="http://java.sun.com/jsp/jstl/core"%>
<html>
    <head>
        <title>JSTL在JSP页面的使用</title>
    </head>
    <body>
        <!--<c:out>用于把内容输出到页面上-->
        <c:out value="hello JSTL"></c:out>
    </body>
</html>
```

#### 7.2.2.赋值标签<c:set>

~~~jsp
<%taglib prefix="c" url="http://java.sun.com/jsp/jstl/core"%>
<html>
    <head>
        <title>JSTL在JSP页面的使用</title>
    </head>
    <body>
        <!--<c:set>用于给变量或对象属性赋值
			value属性：表示赋值给变量或者对象属性
			sope属性：指定变量范围，默认值为page
			target属性：指定对象名称
			propety属性：指定对象属性名称
		-->
        <c:set target="${p}" property="name" value="jack"></c:set>
    </body>
</html>
~~~

#### 7.2.3.删除标签<c:remove>

~~~jsp
<%taglib prefix="c" url="http://java.sun.com/jsp/jstl/core"%>
<html>
    <head>
        <title>JSTL在JSP页面的使用</title>
    </head>
    <body>
        <!--<c:remove>用于删除变量或者对象
			var属性:变量名称
			scope属性：指定变量范围，默认值为page
		-->
        <c:remove var="keyword" scope="request"></c:remove>
    </body>
</html>
~~~

#### 7.2.4.异常处理标签<c:catch>

~~~jsp
<%taglib prefix="c" url="http://java.sun.com/jsp/jstl/core"%>
<html>
    <head>
        <title>JSTL在JSP页面的使用</title>
    </head>
    <body>
        <!--<c:catch>用于捕获代码中发生的异常
			var为指定保存异常对象的变量名。如果省略var属性，则只捕获异常，不保存。
		-->
        <c:catch var="e">
        	<%=10/0%>
        </c:catch>
    </body>
</html>
~~~

#### 7.2.5.条件标签<c:if>

~~~jsp
<%taglib prefix="c" url="http://java.sun.com/jsp/jstl/core"%>
<html>
    <head>
        <title>JSTL在JSP页面的使用</title>
    </head>
    <body>
        <!--<c:if>用于实现条件判断
			test属性：指定测试条件
			var属性：保存测试条件的计算结果
			scope属性：指定变量范围，默认值为page
		-->
        <c:if text="${a%2 == 0}" var="result">
            <c:out value="${a}是偶数"></c:out>
        </c:if>
        <c:if text="result==false">
             <c:out value="${a}是奇数"></c:out>
        </c:if>
    </body>
</html>
~~~

#### 7.2.6.选择标签<c:choose>，<c:when>，<c:otherwise>

~~~jsp
<%taglib prefix="c" url="http://java.sun.com/jsp/jstl/core"%>
<html>
    <head>
        <title>JSTL在JSP页面的使用</title>
    </head>
    <body>
        <!--<c:choose><c:when><c:otherwise>用于实现分支条件判断-->
        <c:choose>
            <c:when text="a>=0">
                 <c:when text="a>10">
                     <c:out value="${a}大于10"></c:out>
                 </c:when>
                 <c:otherwise>
                     <c:out value="${a}大于0小于10"></c:out>
           	 	 </c:otherwise>
            </c:when>
            <c:otherwise>
                 <c:out value="${a}小于0"></c:out>
            </c:otherwise>
        </c:choose>
    </body>
</html>
~~~

#### 7.2.7.迭代标签<c:foreach>

~~~jsp
<%taglib prefix="c" url="http://java.sun.com/jsp/jstl/core"%>
<html>
    <head>
        <title>JSTL在JSP页面的使用</title>
    </head>
    <body>
        <!--<c:foreach>用于实现集合和数组迭代
			var属性：迭代参数的名称
			items属性：要进行迭代的集合或者数组
			varstartus属性：迭代变量的名称
			begin属性：如果指定了items，那么就从items[begin]开始进行迭代；
						如果没有指定items，那么就从begin开始迭代。它的类型为整数
			end属性：如果指定了items，那么就在items[end]结束迭代；如果没有指定items，
					那就在end结束迭代。它的类型也是整数。
			step属性：迭代的步长。
		-->
        <%
        	List<int> ar = new ArrayList<>();
        	for(int i=0;i<10;i++){
                ar.add(i);
			}
        	request.setAttribute("ar",ar);
        %>
        <c:foreach var="index" items="${ar}" varstartus="startus">
            <c:out>${index}</c:out><br>
        </c:foreach>
    </body>
</html>
~~~

### 7.3.SQL标签库

#### 7.3.1.数据源标签<sql:setDataSource>

~~~jsp
<%taglib prefix="sql" url="http://java.sun.com/jsp/jstl/sql"%>
<html>
    <head>
        <title>JSTL在JSP页面的使用</title>
    </head>
    <body>
        <!--<sql:setDataSource>用于访问数据源
			driver属性：要注册的JDBC的驱动
			url属性：数据库连接的JDBC的URl
			user属性：数据库的用户名
			password属性：数据库密码
			dataSource属性：事先准备好的数据库
			var属性：代表数据库的变量
			scope属性：var属性的作用域
		-->
        <sql:setDataSource var="data" driber="com.mysql.cj.jdbc.Driver" 		url="jdbc:mysql://localhost:3306/chapter?useUnicode=true&characterEncoding=utf-8" user="root" password="123456"/>
    </body>
</html>
~~~

#### 7.3.2.执行查询标签<sql:query>

~~~jsp
<%taglib prefix="sql" url="http://java.sun.com/jsp/jstl/sql"%>
<%taglib prefix="c" url="http://java.sun.com/jsp/jstl/core"%>
<html>
    <head>
        <title>JSTL在JSP页面的使用</title>
    </head>
    <body>
        <!--<sql:query>用于执行查询
			sql属性：需要执行sql命令(返回一个ResultSet对象)
			dataSource属性：所使用的数据库连接(覆盖默认值)
			maxRows属性：存储在变量中的最大结果数
			startRow属性：开始记录的结果的行数
			var属性：代表数据库变量
			scope属性：var属性的作用域
			在EL表达式中，可以使用以下的属性访问查询结果：
			columnNames属性：返回查询结果集中的列名称
			rowCount属性：返回查询结果集中的总行数
			rows属性：返回包含结构的SortedMap数组
			rowsByIndex属性：返回包含查询结果的二维数组，第一维对应行，第二维对应列
		-->
        <sql:query var="rs" sql="SELECT * FROM User" dataSource="${ds}"></sql:query>
        <table width="100%" border="1" cellpadding="0" cellspacing="0">
            <tbody>
            	<tr>
                    <td>学号</td>
                    <td>性别</td>
                    <td>姓名</td>
                </tr>
                <c:forEach var="row" items="${rs.rows}">
                    <td>${row.id}</td>
                    <td>${row.sex}</td>
                    <td>${row.name}</td>
                </c:forEach>
            </tbody>
        </table>
    </body>
</html>
~~~

#### 7.3.3.执行查询参数标签<sql:param>

~~~jsp
<%taglib prefix="sql" url="http://java.sun.com/jsp/jstl/sql"%>
<html>
    <head>
        <title>JSTL在JSP页面的使用</title>
    </head>
    <body>
        <!--<sql:param>用于指定查询参数的值
			value属性：填写参数值
		-->
        <sql:query var="rs" dataSource="${ds}">
        	SELECT * FROM Student where uname=?
            <sql:param vlaue="lucy"></sql:param>
        </sql:query>
    </body>
</html>
~~~

#### 7.3.4.设置日期与时间值标签<sql:dataParam>

~~~~jsp
<%taglib prefix="fmt" url="http://java.sun.com/jsp/jstl/fmt"%>
<%taglib prefix="sql" url="http://java.sun.com/jsp/jstl/sql"%>
<html>
    <head>
        <title>JSTL在JSP页面的使用</title>
    </head>
    <body>
        <!--<sql:dataParam>用于设定日期与时间查询参数值-->
        <!--<fmt:dataParam>用于按本地格式或者自定义格式从字符串中解析日期和时间-->
        <fmt:dataParam value="2022年6月5日" type="date" dateStyle="long" var="thDate"></fmt:dataParam>
        <sql:query var="rs" dataSource="${ds}">
        	SELECT * FROM Student where createdtime>?
            <sql:dataparm value="${thdate}"></sql:dataparm>
        </sql:query>
    </body>
</html>
~~~~

#### 7.3.5.执行SQL更新标签<sql:update>

~~~jsp
<%taglib prefix="sql" url="http://java.sun.com/jsp/jstl/sql"%>
<html>
    <head>
        <title>JSTL在JSP页面的使用</title>
    </head>
    <body>
        <!--<sql:update>用于执行 insert,update,delete 等SQL命令-->
       <sql:update>
     		INSERT INTO Student(id,name,sex) values(?,?,?) 
           <sql:param> value="001"</sql:param>
           <sql:param> value="小明"</sql:param>
           <sql:param> value="男"</sql:param>
       </sql:update>
    </body>
</html>
~~~

#### 7.3.6.将子标签作为事务执行的标签<sql:transaction>

~~~jsp
<%taglib prefix="sql" url="http://java.sun.com/jsp/jstl/sql"%>
<html>
    <head>
        <title>JSTL在JSP页面的使用</title>
    </head>
    <body>
        <!--<sql:transaction>用于标签体内的<sql:query>和<sql:update>子标签作为事务执行
			dataSource属性：使用的数据源对象，使用java.util.DateSource类型，可以引用EL表达式
			isolation属性：事物的隔离属性，用力啊设置事务的安全级别
		-->
        <sql:update>
     		DELETE FROM Student where id=?
           <sql:param value="001"></sql:param>
       </sql:update>
       <sql:update>
     		INSERT INTO Student(id,name,sex) values(?,?,?) 
           <sql:param value="001"></sql:param>
           <sql:param value="小明"></sql:param>
           <sql:param value="男"></sql:param>
       </sql:update>
    </body>
</html>
~~~



张孝祥

黎和明





