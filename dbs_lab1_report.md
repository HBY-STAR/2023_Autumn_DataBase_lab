<center><h1>dbs_lab1_report</h1></center>
<p align="right">21302010042</p>
<p align="right">侯斌洋</p>
<h2>一、源代码介绍</h2>
<p>
<ol>
<li>dbs_lab1文件夹：maven项目，运行src/main/java/org/using_jdbc/Shell.java文件即可。使用jdbc操作数据库。<br>（注：请在InitSql.java中修改MySql配置信息）

&ensp;
<li>dbs_lab1_using_util文件夹：maven项目，提供了一个使用mybatis-plus框架自动创建表的测试用例，仅作参考。运行MybatisPlusDeomApplication即可<br>（注：请在application.yml中修改MySql配置信息）

&ensp;
<li>dbs_lab1_report.md: lab1实验报告

&ensp;
</ol>
</p>

<h2>二、使用JDBC创建表</h2>
<p>
<ol>
<li>Shell.java: 提供了一个交互式的命令行界面，main函数所在位置。

&ensp;
<li>InitSql.java: 数据库连接。

&ensp;
<li>CreatTable.java: 提供了三种可选函数:
    <ol>
        <li>creat_table_auto_index_with_repeat: 自动为表中每一行的数据生成索引，以此索引为主键，然后将数据插入数据库，不对表格中每一行数据去重。
        <li>creat_table_auto_index_without_repeat: 自动为表中每一行的数据生成索引，以此索引为主键，然后将数据插入数据库，对表格中每一行数据去重。
        <li>creat_table_with_primary_keys: 接受一个List作为参数，List若为空则指定主键为所有列名，List不为空则指定主键为List中的所有项。根据主键在插入时去重。
    </ol>

&ensp;
<li>注：creat_table语句保存在sql_creat_table_log中，insert语句存放在sql_insert.log中

&ensp;
</ol>
</p>

<h2>三、使用mybatis框架创建表</h2>
<p>
&ensp;
&ensp;
首先在entity中创建一个映射到数据库的实体类Test，并以@TableName，@TableField，@Column等标注。之后在mapper中创建TestMapper。然后在application.yml中配置mybatis。最后运行MybatisPlusDemoApplicaation.java
<html>

    #自动建表设置
    mybatis:
    table:
    auto: update
    model:
    #扫描用于创建表的对象的包名
    pack: com.test.lab.entity
    database:
    type: mysql
    #mybatis
    mybatis-plus:
    mapper-locations: classpath*:com/gitee/sunchenbin/mybatis/actable/mapping/*/*.xml

</html>

这样以Test实体类中的配置，就可以利用mybatis的mapper来执行create table操作，不需要写sql语句。
&ensp;
</p>

<h2>四、思考</h2>
<ol>
<li>如外部数据不完整，则程序会在终端中提示空数据。若空数据不为主键，则插入null；若空数据为指定的主键则数据导入失败并提示。

&ensp;
<li>如果出现数据不一致问题，如不满足引用完整性约束，则也应提示，然后给出可选项，是要继续导入并置控制为null还是停止导入。

&ensp;
<li>在处理原始数据时，若指定的操作会使数据库报错，则应直接撤回该行为并提示操作失败，如主键为空。若指定的操作不会使数据库报错但在逻辑上有问题，则给出提示并提供可选项，让用户决定是继续操作还是停止操作，例如mysql中的外键可为空。

&ensp;
</ol>


