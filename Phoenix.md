# Phoenix

## 下载安装

地址及版本：https://phoenix.apache.org/download.html

### 通过cdh5.12.2安装

`wget http://www.apache.org/dist/phoenix/apache-phoenix-4.14.0-cdh5.12.2/parcels/APACHE_PHOENIX-4.14.0-cdh5.12.2.p0.3-el7.parcel`

`wget http://www.apache.org/dist/phoenix/apache-phoenix-4.14.0-cdh5.12.2/parcels/manifest.json`

在manifest.json中找到相对应版本的hash，创建对应验证文件APACHE_PHOENIX-4.14.0-cdh5.12.2.p0.3-el7.sha，将hash粘贴保存。将下载好的.parcel和创建的.sha放到path/cloudera/parcel-repo/路径下。

打开cloudera Manager->主机->Parcel，检查新的Parcel，分配并激活APACHE_PHOENIX。

重启Hbase server。

`phoenix-sqlline.py`

`！tables`

![](F:\pijiupapa\Phoenix\img\p3.jpg)

安装成功

## 创建table，view

Phoenix会自动将小写转为大写，在建表或查询语句中但凡有小写，均需要使用双引号，“test"，”Test“。

### table

`create table "test" (pk varchar primary key, "cf"."name" varchar, "cf"."age" integer) COLUMN_ENCODED_BYTES = 0`

创建了一个表名为test，字段为PK，name，age的表，PK为索引字段。

`<!--COLUMN_ENCODED_BYTES = 0`取消column mapping，此时在hbase中同时生成的表test，相对应的qualify为`cf:_0`,`cf:name`,`cf:age-->`

`upsert into "test" values('1', 'someone', 18);`

`select * from "test";`

![](F:\pijiupapa\Phoenix\img\p1.png)

hbase shell

![](F:\pijiupapa\Phoenix\img\p2.png)

<!--对table的操作会直接影响hbase表的数据。-->

### view

对已有的hbase表创建view。

`create view hbasename (pk varchar primary key, "cf"."c1" varchar, "cf"."c2" unsigned_int, "cf"."c3" unsigned_double);`

<!--integer:-2147483648 to 2147483647-->

<!--unsigned_int:0 to 2147483647-->

<!--view是只读的，会与hbase的表同步，删除view不会对hbase表有影响。-->

