### pymysql介绍
pymysql是帮助python操作mysql的一个模块

---

### 简单使用
```python
import pymysql

username = input('username: ').strip()
pwd = input('password: ').strip()

# 连接mysql
conn = pymysql.connect(host='127.0.0.1', user='root', password='1', database='db1', charset='utf8')
# 获取游标，数据操作用cursor
cursor = conn.cursor()

# 执行sql语句
sql = "select * from user where username = %s and password = %s"
cursor.execute(sql, [username, pwd])

# 取所有结果的第一条
res = cursor.fetchone()

# 关闭数据库
cursor.close()
conn.close()

if res:
    print('登陆成功')
else:
    print('登陆失败')
```

---

### 插入数据
```python
import pymysql

conn = pymysql.connect(host='127.0.0.1', user='root', password='1', database='db1', charset='utf8')
cursor = conn.cursor()

sql = "insert into user(username, password) values('bbb', 'bbb')"
cursor.execute(sql)

# insert，updata，delete都需要提交
conn.commit()

cursor.close()
conn.close()
```

---

### 插入多条数据
```python
import pymysql

conn = pymysql.connect(host='127.0.0.1', user='root', password='1', database='db1', charset='utf8')
cursor = conn.cursor()

sql = "insert into user(username, password) values(%s, %s)"

# 插入多条数据
cursor.executemany(sql,[('ccc', 'ccc'),('ddd', 'ddd')])
conn.commit()

cursor.close()
conn.close()
```

---

### execute返回值
```python
import pymysql

conn = pymysql.connect(host='127.0.0.1', user='root', password='1', database='db1', charset='utf8')
cursor = conn.cursor()

sql = "select * from user"

# 返回值是受影响的行数
res = cursor.execute(sql)
print(res)
cursor.close()
conn.close()
```

---

### 查操作
```python
import pymysql

conn = pymysql.connect(host='127.0.0.1', user='root', password='1', database='db1', charset='utf8')
cursor = conn.cursor()

sql = "select * from user"

cursor.execute(sql)

# 一次取一个
cursor.fetchone()

# 一次取3个
cursor.fetchmany(3)

# 一次全取
cursor.fetchall()

cursor.close()
conn.close()
```

---

### 指针位移
```python
import pymysql

conn = pymysql.connect(host='127.0.0.1', user='root', password='1', database='db1', charset='utf8')
cursor = conn.cursor()

sql = "select * from user"

cursor.execute(sql)
res = cursor.fetchone()
print(res)

# 相对位移
cursor.scroll(-1, mode='relative')
# 绝对位移
cursor.scroll(0, mode='absolute')

res = cursor.fetchone()
print(res)

cursor.close()
conn.close()
```

---

### 返回字典形式的值
```python
import pymysql

conn = pymysql.connect(host='127.0.0.1', user='root', password='1', database='db1', charset='utf8')
# 返回字典形式的值
cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)

sql = "select * from user"

cursor.execute(sql)
res = cursor.fetchall()
print(res)

cursor.close()
conn.close()
```

---

### 获取你插入数据的自增ID
```python
import pymysql

conn = pymysql.connect(host='127.0.0.1', user='root', password='1', database='db1', charset='utf8')
cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)

sql = "insert into user(username, password) values(%s, %s)"

cursor.execute(sql, ['aa1', 'a1'])
conn.commit()

# 获取你插入数据的自增ID
print(cursor.lastrowid)

cursor.close()
conn.close()
```

### 调用存储过程

调用无参存储过程

```python
import pymysql

conn = pymysql.connect(host='127.0.0.1', user='root', password='1', database='db1', charset='utf8')
cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)

# 调用无参存储过程
cursor.callproc('p1')
conn.commit()

cursor.close()
conn.close()
```

调用有参存储过程

```python
import pymysql

conn = pymysql.connect(host='127.0.0.1', user='root', password='1', database='db1', charset='utf8')
cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)

# 调用有参存储过程
cursor.callproc('p1', (12, 2))
conn.commit()

cursor.close()
conn.close()
```

掉用有out参数的存储过程

```python
import pymysql

conn = pymysql.connect(host='127.0.0.1', user='root', password='1', database='db1', charset='utf8')
cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)

# 查看返回的结果集
cursor.callproc('p1', (12, 2))
print(cursor.fetchall())

# 查看out参数的值
cursor.execute('select @_p1_0, @_p1_1')
print(cursor.fetchall())

cursor.close()
conn.close()
```
