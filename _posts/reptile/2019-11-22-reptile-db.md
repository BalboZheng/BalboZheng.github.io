---
layout:     post
title:      "数据库存储"
subtitle:   "数据库存储"
date:       2019-11-22 11:54:02
author:     "Balbo"
header-img: "img/post-bg-2019.jpg"
tags:
    - reptile
---
## 关系型数据库存储

### MySQL 存储
1. 连接数据库

    ```python
    import pymysql

    db = pymysql.connect(host='localhost', user='root', password='123456', port=3306)
    cursor = db.cursor()
    cursor.execute('SELECT VERSION()')
    data = cursor.fetchone()
    print('Database version:', data)
    cursor.execute("CREATE DATABASE spiders DEFAULT CHARACTER SET utf-8")
    db.close()
    ```
    运行结果：
    ```
    Database version: ('5.5.53',)
    ```

2. 创建表

    ```python
    import pymysql

    db = pymysql.connect(host='localhost', user='root', password='123456', port=3306, db='spiders')
    cursor = db.cursor()
    sql = 'CREATE TABLE IF NOT EXISTS students (id VARCHAR(255) NOT NULL, name VARCHAR(255) NOT NULL, age INT NOT NULL, PRIMARY KEY (id))'
    cursor.execute(sql)
    db.close()s
    ```

3. 插入数据

    ```python
    import pymysql

    data = {
        'id': '20120001',
        'name': 'Bob',
        'age': 20,
    }
    table = 'students'
    keys = ', '.join(data.keys())
    values = ', '.join(['%s'] * len(data))
    sql = 'INSERT INTO {table}({keys}) VALUE ({values})'.format(table=table, keys=keys, values=values)

    db = pymysql.connect(host='localhost', user='root', password='123456', port=3306, db='spiders')
    cursor = db.cursor()

    try:
        if cursor.execute(sql, tuple(data.values())):
            print('Successful')
            db.commit()
    except:
        print('Failed')
        db.rollback()
    db.close()
    ```

4. 更新数据

    ```python
    import pymysql

    data = {
        'id': '20120001',
        'name': 'Bob',
        'age': 20,
    }
    table = 'students'
    keys = ', '.join(data.keys())
    values = ', '.join(['%s'] * len(data))
    sql = 'INSERT INTO {table}({keys}) VALUE ({values}) ON DUPLICATE KEY UPDATE '.format(table=table, keys=keys, values=values)
    update = ', '.join([" {key} = %s".format(key=key) for key in data])
    sql += update

    db = pymysql.connect(host='localhost', user='root', password='123456', port=3306, db='spiders')
    cursor = db.cursor()

    try:
        if cursor.execute(sql, tuple(data.values())*2):
            print('Successful')
            db.commit()
    except:
        print('Failed')
        db.rollback()
    db.close()
    ```

5. 删除数据

    ```python
    import pymysql

    table = 'students'
    condition = 'age > 20'

    sql = 'DELETE FROM {table} WHERE {condition}'.format(table=table, condition=condition)

    db = pymysql.connect(host='localhost', user='root', password='123456', port=3306, db='spiders')
    cursor = db.cursor()

    try:
        cursor.execute(sql)
        db.commit()
    except:
        db.rollback()
    db.close()
    ```

6. 数据查询

    ```python
    import pymysql

    sql = 'SELECT * FROM students WHERE age >= 20'

    db = pymysql.connect(host='localhost', user='root', password='123456', port=3306, db='spiders')
    cursor = db.cursor()

    try:
        cursor.execute(sql)
        print('Count:', cursor.rowcount)
        one = cursor.fetchone()
        print('One:', one)
        results = cursor.fetchall()
        print('Results:', results)
        print('Result Type:', type(results))
        for row in results:
            print(row)
    except:
        print('Error')
    ```

    在这里我们构造了一条 SQL 语句，将年龄 20 岁及以上的学生查询出来，然后将其传给 execute() 方法即可，注意在这里不再需要 db 的 commit() 方法。然后我们可以调用 cursor 的 rowcount 属性获取查询结果的条数，当前示例中获取的结果条数是 4 条。

    然后我们调用了 fetchone() 方法，这个方法可以获取结果的第一条数据，返回结果是元组形式，元组的元素顺序跟字段一一对应，也就是第一个元素就是第一个字段 id，第二个元素就是第二个字段 name，以此类推。随后我们又调用了fetchall() 方法，它可以得到结果的所有数据，然后将其结果和类型打印出来，它是二重元组，每个元素都是一条记录。我们将其遍历输出，将其逐个输出出来。

    但是这里注意到一个问题，显示的是4条数据，fetall() 方法不是获取所有数据吗？为什么只有3条？这是因为它的内部实现是有一个偏移指针来指向查询结果的，最开始偏移指针指向第一条数据，取一次之后，指针偏移到下一条数据，这样再取的话就会取到下一条数据了。所以我们最初调用了一次 fetchone() 方法，这样结果的偏移指针就指向了下一条数据，fetchall() 方法返回的是偏移指针指向的数据一直到结束的所有数据，所以 fetchall() 方法获取的结果就只剩 3 个了，所以在这里要理解偏移指针的概念。

    所以我们还可以用 while 循环加 fetchone() 的方法来获取所有数据，而不是用 fetchall() 全部一起获取出来，fetchall() 会将结果以元组形式全部返回，如果数据量很大，那么占用的开销会非常高。所以推荐使用如下的方法来逐条取数据：
    ```python
    import pymysql

    sql = 'SELECT * FROM students WHERE age >= 20'

    db = pymysql.connect(host='localhost', user='root', password='123456', port=3306, db='spiders')
    cursor = db.cursor()

    try:
        cursor.execute(sql)
        print('Count:', cursor.rowcount)
        row = cursor.fetchone()
        while row:
            print('Row', row)
            row = cursor.fetchone()
    except:
        print('Error')
    ```

## 非关系型数据库存储

非关系型数据库可细分如下：

- 键值存储数据库：Redis、Voldemort 和 Oracle BDB 等
- 列存储数据库：Cassandra、HBase 和 Rick 等
- 文档型数据库：CouchDB 和 MongoDB 等
- 图形数据库：Neo4J、InfoGrid 和 Infinit Graph 等

### MongoDB 存储
1. 连接 MongoDB

    ```python
    import pymongo

    client = pymongo.MongoClient(host='localhost', port=27017)
    # client = pymongo.MongoClient('mongodb://localhost:27017/')
    ```

2. 指定数据库

    ```python
    db = client.test
    # db = client['test']
    ```

3. 指定集合

    ```python
    collection = db.students
    # collection = db['students']
    ```

4. 插入数据

    - 新建学生数据

        ```python
        student1 = {
            'id': '20170101',
            'name': 'Jordan',
            'age': 20,
            'gender': 'male'
        }
        student2 = {
            'id': '20170102',
            'name': 'Mike',
            'age': 21,
            'gender': 'male'
        }
        ```

    - 插入一条数据

        ```python
        result = collection.insert_one(student1)
        print(result)
        print(result.inserted_id)
        ```
        运行结果：
        ```
        <pymongo.results.InsertOneResult object at 0x0000029188063F48>
        5dd8be4a475adf926a44d21b
        ```

    - 插入多条数据

        ```python
        result = collection.insert_many([student1, student2])
        print(result)
        print(result.inserted_ids)
        ```
        运行结果：
        ```
        <pymongo.results.InsertManyResult object at 0x00000249740C27C8>
        [ObjectId('5dd8becc6c94b9c18bab42f8'), ObjectId('5dd8becc6c94b9c18bab42f9')]
        ```

5. 查询

    - 查询一个结果

        ```python
        result = collection.find_one({'name': 'Mike'})
        print(type(result))
        print(result)
        ```
        运行结果：
        ```
        <class 'dict'>
        {'_id': ObjectId('5dd8be3466741231fe29341e'), 'id': '20170102', 'name': 'Mike', 'age': 21, 'gender': 'male'}
        ```

    - 根据 ObjectId 查询

        ```python
        from bson import ObjectId
                
        result = collection.find_one({'_id': ObjectId('5dd8eb565053ed048d9a21ec')})
        print(result)
        ```
        运行结果：
        ```
        {'_id': ObjectId('5dd8eb565053ed048d9a21ec'), 'id': '20170101', 'name': 'Jordan', 'age': 20, 'gender': 'male'}
        ```

    - 查询多个结果

        ```python
        results = collection.find({'age': 20})
        print(results)
        for result in results:
            print(result)
        ```
        如果要查找大于20的数据
        ```python
        results = collection.find({'age': {'$gt': 20}})
        ```

        | 符号 | 含义 | 示例 |
        | ---- | ---- | ---- |
        | $lt | 小于 | {'age': {'$lt': 20}} |
        | $gt | 大于 | {'age': {'$gt': 20}} |
        | $lte | 小于等于 | {'age': {'$lte': 20}} |
        | $gte | 大于等于 | {'age': {'$gte': 20}} |
        | $ne | 不等于 | {'age': {'$ne': 20}} |
        | $in | 在范围内 | {'age': {'$in': 20}} |
        | $nin | 不在范围内 | {'age': {'$nin': 20}} |

    - 正则匹配查询

        ```python
        results = collection.find({'name': {'$regex': '^J.*'}})
        ```
        | 符号 | 含义 | 示例 | 示例含义 |
        | ---- | ---- | ---- | ---- |
        | $regex | 匹配正则表达式 | {'name': {'$regex': '^J.*'}} | name以 J 开头 |
        | $exists | 属性是否存在 | {'name': {'$exists': True}} | name 属性存在 |
        | $type | 类型判断 | {'age': {'$type': 'int'}} | age 的类型为 int |
        | $mod | 数字模操作 | {'age': {'$mod': [5, 0]}} | 年龄模 5 余 0 |
        | $text | 文本查询 | {'$text': {'$search': 'Mike'}} | text 类型的属性中包含 Mike 字符串 |
        | $where | 高级条件查询 | {'$where': 'obj.fans_count == obj.follows_count'} | 自身粉丝数等于关注数 |

6. 计数

    ```python
    count = collection.count_documents({'age': 20})
    print(count)
    ```

7. 排序

    ```python
    results = collection.find().sort('name', pymongo.ASCENDING)
    # results = collection.find().sort('name', pymongo.DESCENDING)
    print([result['name'] for result in results])
    ```

8. 偏移

    ```python
    results = collection.find().sort('name', pymongo.ASCENDING).skip(2)
    print([result['name'] for result in results])
    ```
    limit() 方法指定要取的结果个数
    ```python
    results = collection.find().sort('name', pymongo.ASCENDING).skip(2).limit(2)
    ```
    在数据库非常庞大的时候，最好不要使用大的偏移量来查询数据，可能导致内存溢出，可以使用以下操作
    ```python
    results = collection.find({'_id': {'$gt': ObjectId('5dd8eb565053ed048d9a21ec')}})
    ```

9. 更新

    ```python
    condition = {'name': 'Mike'}
    student = collection.find_one(condition)
    student['age'] = 26
    result = collection.update_one(condition, {'$set': student})
    print(result)
    print(result.matched_count, result.modified_count)
    ```
    运行结果：
    ```
    <pymongo.results.UpdateResult object at 0x000002748B94A4C8>
    1 1
    ```
    符合条件的第一条信息进行修改
    ```python
    condition = {'age': {'$gt': 20}}
    result = collection.update_one(condition, {'$inc': {'age': 20}})
    print(result)
    print(result.matched_count, result.modified_count)
    ```
    运行结果：
    ```
    <pymongo.results.UpdateResult object at 0x000001919904B2C8>
    1 1
    ```
    符合条件的所有信息进行修改
    ```python
    condition = {'age': {'$gt': 20}}
    result = collection.update_many(condition, {'$inc': {'age': 21}})
    print(result)
    print(result.matched_count, result.modified_count)
    ```
    运行结果：
    ```
    <pymongo.results.UpdateResult object at 0x000002013C6CB388>
    4 4
    ```

10. 删除

    ```python
    result = collection.delete_one({'name': 'Kevin'})
    print(result)
    print(result.deleted_count)
    result = collection.delete_many({'age': {'$lt': 25}})
    print(result.deleted_count)
    ```

### Redis 存储
1. 连接 Redis
    - 直接连接

        ```python
        from redis import StrictRedis

        redis = StrictRedis(host='localhost', port=6379, db=0, password='foobared')
        redis.set('name', 'Bob')
        print(redis.get('name'))
        ```

    - 连接池连接

        ```python
        from redis import StrictRedis, ConnectionPool

        pool = ConnectionPool(host='localhost', port=6379, db=0, password='foobared')
        redis = StrictRedis(connection_pool=pool)
        redis.set('name', 'Bob')
        print(redis.get('name'))
        ```

2. 键操作

    | 方法 | 作用 | 参数说明 | 示例 | 示例说明 | 示例结果 |
    | ---- | ---- | ---- | ---- | ----| ----|
    | exists(name) | 判断一个键是否存在 | name：键名 | redis.exists('name') | 是否存在 name 这个键 | True |
    | delete(name) | 删除一个键 | name：键名 | redis.delete('name') | 删除 name 这个键 | 1 |
    | type(name) | 判断键类型 | name：键名 | redis.type('name') | 判断 name 这个键类型 | b'string' |
    | keys(patten) | 获取所有符合规则的键 | patten：匹配规则 | redis.keys('n*') | 获取所有以 n 开头的键 | \[b'name'] |
    | randomkey() | 获得随机一个键 |  | randomkey() | 获取随机的一个键 | b'name' |
    | rename(src, dst) | 重命名键 | src：原键名；dst：新键名 | redis.rename('name', 'nickname') | 将 name 重命名为 nickname | True |
    | dbsize() | 获取当前数据库中键的数目 |  | dbsize() | 获取当前数据库中键的数目 | 100 |
    | expire(name, time) | 设定键的过期时间，单位为秒 | name：键名；time：键名 | redis.expire('name', 2) | 将 name 键的过期时间设置为2秒 | True |
    | ttl(name) | 获取键的过期时间，单位为秒，-1代表永不过期 | name：键名 | redis.ttl('name') | 获取 name 这个键的过期时间 | -1 |
    | move(name, db) | 将键移动到其他数据库 | name：键名;db: 数据库代号 | moce('name', 2) | 将 name 移到2号数据库 | True |
    | flushdb() | 删除当前选择数据库中的所有键 |  | flushdb() | 删除当前选择数据库中的所有键 | True |
    | flushall() | 删除所有数据库中的所有键 |  | flushall() | 删除所有数据库中的所有键 | True |

3. 字符串操作

    | 方法 | 作用 | 参数说明 | 示例 | 示例说明 | 示例结果 |
    | ---- | ---- | ---- | ---- | ----| ----|
    | set(name, value) | 给数据库中键名为 name 的 string 赋予值 value | name： 键名；value： 值 | redis.set('name', 'Bob') | 给 name 这个键的 value 赋予为 Bob | True |
    | get(name) | 返回数据库中键名为 name 的 string 的 value | name: 键名 | redis.get('name') | 返回 name 这个键的 value | b'Bob' |
    | getset(name, value) | 给数据库中键名为 name 的 string 赋予值 value 并返回上次的 value | name: 键名；value： 新值 | redis.getset('name', 'Mike') | 赋值 name 为 Mike 并得到上次的 value | b'Bob' |
    | mget(keys, *args) | 返回多个键对应的 value 组成的列表 | keys: 键名序列 | redis.mget(\['name', 'nickname']) | 返回 name 和 nickname 的 value | \[b'Mike', b'Miker'] |
    | setnx(name, value) | 如果不存在这个键值对，则更新 value，否则不变 | name： 键名 | redis.setnx('newname', 'James') | 如果 newname 这个键不存在，则设置值为 James | 第一次运行结果是 True，第二次运行结果的 False |
    | setex(name, time, value) | 设置可以对应的值为 string 类型的 value，并指定键值对应的有效期 | name: 键名；time： 有效期；value： 值 | redis.setex('name', 1, 'James') | 将 name 这个键的值设为 James，有效期为1秒 | True |
    | setrange(name, offset, value) | 设置指定键的 value 值的子字符串 | name: 键名；offset： 偏移量；value: 值 | redis.set('name', 'Hello') redis.setrange('name', 6, 'World') | 设置 name 为 Hello 字符串，并在 index 为6的位置补 World | 11， 修改后的字符串长度 |
    | mset(mapping) | 批量赋值 | mapping: 字典或关键字参数 | redis.mset({'name1': 'Durant', 'name2': 'James'}) | 将 name1 设为 Durant，name2 设为 James | True |
    | msetnx(mapping) | 键均不存在时才批量赋值 | mapping: 字典或关键字参数 | redis.msetnx({'name3': 'Smith','name4': 'Curry'}) | 在 name3 和 name4 均不存在的情况下才设置二者值 | True |
    | incr(name, amount=1) | 键名为 name 的 value 增值操作，默认为1，键不存在则被创建并设为 amount | name： 键名；amount： 增长的值 | redis.incr('age', 1) | age 对应的值增1， 若不存在。则会创建并设置为 1 | 1, 即修改后的值 |
    | decr（name， amount=1) | 键名为 name 的 value 减值操作，默认为1，键不存在则被创建并将 value 设为 -amount | name： 键名；amount： 减少的值 | redis.decr('age', 1) | age 对应的值减1， 若不存在。则会创建并设置为 -1 | -1, 即修改后的值 |
    | append(key, value) | 键名为 key 的 string 的值附加 value | key： 键名 | redis.append('nickname', 'OK') | 向键名为 nickname 的值后追加 OK | 13，即修改后的字符串长度 |
    | substr(name, value) | 返回键名为 name 的 string 的子字符串 | name: 键名；start: 起始索引；end： 终止索引，默认为 -1，表示截取到末尾 | redis.substr('name', 1, 4) | 返回键名为 name 的 值的字符串，截取索引为1~4的字符 | b'ello' |
    | getrange(key, start, end) | 获取键的 value 值从 start 到 end 的子字符串 | key: 键名；start： 起始索引；end： 终止索引 | redis.getrange('name', 1, 4) | 返回键名为 name 的值的字符串，截取索引为1~4的字符 | b'ello' |

4. 列表操作

    | 方法 | 作用 | 参数说明 | 示例 | 示例说明 | 示例结果 |
    | ---- | ---- | ---- | ---- | ----| ----|
    | rpush(name, *values) | 在键名为 name 的列表末尾添加为 value 的元素，可以传多个 | name: 键名；values： 值 | redis.rpush('list', 1, 2, 3) | 向键名为 list 的列表尾添加1、2、3 | 3，列表大小 |
    | lpush(name, *values) | 在键名为 name 的列表头添加值为 value 的元素，可以传多个 | name: 键名；values： 值 | redis.lpush('list', 0) | 向键名为 list 的列表头部添加0 | 4，列表大小 |
    | llen(name) | 返回键名为 name 的列表的长度 | name: 键名 | redis.llen('list') | 返回键名为 list 的列表的长度 | 4 |
    | lrange(name, start, end) | 返回键名为 name 的列表中 start 至 end 之间的元素 | name: 键名；start： 起始索引；end： 终止索引 | redis.lrange('list', 1, 3) | 返回起始索引为1终止索引为3的所有范围对应的列表 | \[b'3', b'2', b'1'] |
    | ltrim(name, start, end) | 截取键名为 name 的列表，保留索引为 start 到 end 的内容 | name: 键名；start： 起始索引；end： 终止索引 | redis.ltrim('list', 1, 3) | 保留键名为 list 的所有为1到3的元素 | True |
    | lindex(name, index) | 返回键名为 name 的列表中 index 位置的元素 | name: 键名；index： 索引 | redis.index('list', 1) | 返回键名为 list 的列表所有为1的元素 | b'2' |
    | lset(name, index, value) | 给键名为 name 的列表中 index 位置的元素赋值，越界则报错 | name: 键名；index： 索引位置；value： 值 | redis.lset('list', 1, 5) | 将键名为 list 的列表中索引为1的位置赋值为5 | True |
    | lrem(name, count, value) | 删除 count 个键的列表中值为 value 的元素 | name: 键名；count： 删除个数；value： 值 | redis.lrem('list', 2, 3) | 将键名为 list 的列表删除两个3 | 1,即删除的个数 |
    | lpop(name) | 返回并删除键名为 name 的列表中的首元素 | name: 键名 | redis.lpop('list') | 返回并删除名为 list 的列表中的第一个元素 | b'5' |
    | rpop(name) | 返回并删除键名为 name 的列表中的尾元素 | name: 键名 | redis.rpop('list') | 返回并删除名为 list 的列表中的最后一个元素 | b'2' |
    | blpop(keys, timeout=0) | 返回并删除名称在 keys 中的 list 中的首个元素，如果列表为空，则会一直阻塞等待 | keys: 键名序列；timeout：超时等待时间，0为一直等待 | redis.blpop('list') | 返回并删除键名为 list 的列表中的第一个元素 | \[b'5']
    | brpop(keys, timeout=0) | 返回并删除键名为 name 的列表中的尾元素，如果 list 为空，则会一直阻塞等待 | keys: 键名序列；timeout： 超时等待时间，0为一直等待 | redis.brpop('list') | 返回并删除名为 list 的列表中的最后一个元素 | \[b'2'] |
    | rpoplpush(src, dst) | 返回并删除名称为 src 的列表的尾元素，并将该元素添加到名称尾 dst 的列表头部 | src： 源列表的键；dst： 目标列表的 key | redis.rpoplpush('list', 'list2') | 将键名尾 list 的列表尾元素删除并将其添加到键名尾 list2 的列表头部，然后返回 | b'2' |

5. 集合操作

    | 方法 | 作用 | 参数说明 | 示例 | 示例说明 | 示例结果 |
    | ---- | ---- | ---- | ---- | ----| ----|
    | sadd(name, *values) | 向键名尾 name 的集合中添加元素 | name： 键名；values： 值，可为多个 | redis.sadd('tags', 'Book', 'Tea', 'Coffee') | 向键名为 tags 的集合中添加 Book、Tea 和 Coffee 这3个内容 | 3， 即插入的数据个数 |
    | srem(name, *values) | 从键名为 name 的集合中删除元素 | name： 键名；values： 值，可为多个 | redis.srem('tags', 'Book') | 从键名为 tags 的集合中删除 Book | 1，即删除的数据个数 |
    | spop(name) | 随机返回并删除键名为 name 的集合中的一个元素 | name： 键名 | redis.spop('tags') | 从键名为 tags 的集合并随机删除并返回该元素 | b'Tea'  |
    | smove(scr, dst, value) |  src 对应的集合中一处元素并将其添加到 dst 对应的集合中 | src： 源集合；dst： 目标集合；value： 元素值 | redis.smove('tags', 'tags2', 'Coffee') | 从键名为 tags 的集合中删除元素 Coffee 并将其添加到键为 tags2 的集合 | True |
    | scard(name) |返回键名为 name 的集合的元素个数 | name: 键名 | redis.scard('tags') | 获取键名为 tags 的集合中的元素个数 | 3 |
    | sismember(name, value) | 测试 member 是否是键名为 name 的集合的元素 | name: 键值 | redis.sismember('tags', 'Book') | 判断 Book 是否是键名为 tags 的集合元素 | True |
    | sinter(keys, *args) | 返回所有给定键的集合的交集 | keys： 键名序列 | redis.sinter(\['tags', 'tags2']) | 返回键名为 tags 的集合和键名为 tags2 的集合的交集 | {b'Coffee'} |
    | sinterstore(dest, keys, *args) | 求交集并将交集保存到 dest 的集合 | dest: 结果集合；keys： 键名序列 | redis.sinterstore('inttag', \['tags', 'tags2']) | 求键名为 tags 的集合和键名为 tags2 的集合的交集并将其保存为 inttag |1 |
    | sunion(keys, *args) | 返回所有给定键的集合的并集 | keys： 键名序列 | redis.sunion(\['tags', 'tags2']) | 返回键名为 tags 的集合和键名为 tags2 的集合的并集 | {b'Coffee', b'Book', b'Pen'} |
    | sunionstore(dest, keys, *args) | 求并集并将并集保存到 dest 的集合 | dest： 结果集合；keys： 键名序列 | redis.sunionstore('inttag', \['tags', 'tags2']) | 求键名为 tags 的集合和键名为 tags2 的集合并集并将其保存为 inttag | 3 |
    | sdiff(keys, *args) | 返回所有给定键的集合的差集 | keys： 键名序列 | redis.sdiff(\['tags', 'tags2']) | 返回键名为 tags 的集合和键名为 tags2 的集合的差集 | {b'Book', 'b'Pen'} |
    | sdiffstore(dest, keys, *args) | 求差集并将差集保存到 dest 集合 | dest: 记过集合；keys： 键名序列 | redis.sdiffstore('inttag', \['tags', 'tags2']) | 求键名为 tags 的集合和键名为 tags2 的集合的差集并将其保存为 inttag | 3 |
    | smembers(name) | 返回键名为 name 的集合的所有元素 | name： 键名 | redis.smembers('tags) | 返回键名为 tags 的集合的所有元素 | {b'Pen', b'Book', b'Coffee'} |
    | srandmember(name) | 随机返回键名为 name 的集合中的一个元素，但不删除元素 | name: 键值 | redis.srandmember('tags') | 随机返回名为 tags 的集合中的一个元素 | Srandmember(name) |

6. 有序集合操作

    | 方法 | 作用 | 参数说明 | 示例 | 示例说明 | 示例结果 |
    | ---- | ---- | ---- | ---- | ----| ----|
    | zadd(name, *values) | 向键名为 name 的 zset 中添加元素 member，score 用于排序。如果该元素存在，则更新其顺序 | name： 键名；args： 可变参数 | redis.zadd('grade', 100, 'Bob', 98, 'Mike') | 向键名为 grade 的 zset 中添加 Bob（其 score 为100),并添加 Mike（其 score 为98) | 2, 即添加的元素个数 |
    | zrem(name, *values) | 删除键名为 name 的 zset 中的元素 | name： 键名；values： 元素 | redis.zrem('grade', 'Mike') | 从键名为 grade 的 zset 中删除 Mike | 从键名为 grade 的 zset 中删除 Mike | 1， 即删除的元素个数 |
    | zincrby(name, value, amount=1) |如果在键名为 name 的 zset 中已经存在元素，value 则将该元素的 score 增加 amount；否则向该集合中添加该元素，其 score 的值为 amount | name： 键名；value： 元素；amount： 增长的 score 的值 | redis.zincrby('grade', 'Bob', -2) | 键名为 grade 的 zset 中 Bob 的 score 减2 | 98.0，即修改后的值 |
    | zrank(name, value) | 返回键名为 name 的 zset 中元素的排名，按 score 从小到大排序，即名次 | name： 键名；value： 元素值 | redis.zrank('grade', 'Amy') | 得到键名为 grade 的 zset 中 Amy 的排名 | 1 |
    | zrevrank(name, value) | 返回键名为 name 的 zset 中元素的倒数排名，按 score 从小到大排序，即名次 | name： 键名；value： 元素值 | redis.zrevzrank('grade', 'Amy') | 得到键名为 grade 的 zset 中 Amy 的倒数排名 | 2 |
    | zrevrange(name, start, end, withscores=False) | 返回键名为 name 的 zset（按 score 从小到大排序）中 index 从 start 到 end 的所有元素 | name： 键值；start： 开始索引；end： 借宿索引；withscores： 是否带 score | redis.zrevrange('grade', 0, 3) | 返回键名为 grade 的 zset 中前四名元素 | \[b'Bob', b'Mike', b'Amy', b'James'] |
    | zrangebysocre(name. min, max, start=None, num=None, withscores=False) | 返回键名为 name 的 zset 中 score 在给定区间的元素 | name： 键值；min： 最低 score；max： 最高 score；start： 起始索引；end： 结束索引；num： 个数； withscore： 是否带score | redis.zrangebysocre('grade', 80, 95) | 返回键名为 grade 的 zset 中 score 在 80 和 95 之间的元素 | \[b'Bob', b'Mike', b'Amy', b'James'] |
    | zcount(name, min, max) | 返回键名为 name 的 zset 中 score 在给定区间的数量 | name： 键名；min：最低score;max: 最高 score | redis.zcount(’grade‘， 80， 95) | 返回键名为 grade 的 zset 中 score 在80到95的元素个数 | 2 |
    | zcard(name) | 返回键名为 name 的 zset 的元素个数 | name： 键名 | redis.zcard('grade') | 获取键名为 grade 的 zset 中元素个数 | 3 |
    | zremrangebyrank(name, min, max) | 删除键名为 name 的 zset 中 score 在 给定区间的元素 | name： 键名；min： 最低 score；max： 最高 score | redis.zremrangebyscore('grade', 0, 0) | 删除键名为 grade 的 zset 中排名第一的元素 | 1，即删除的元素个数 |
    | zremrangebyscore(name, min, max) | 删除键名为 name 的 zset 中 score 在 给定区间的元素 | name： 键名；min： 最低 score；max： 最高 score | redis.zremrangebyscore('grade', 80, 90) | 删除 score 在80到90之间的元素 | 1，即删除的元素个数 |

7. 散列操作

    | 方法 | 作用 | 参数说明 | 示例 | 示例说明 | 示例结果 |
    | ---- | ---- | ---- | ---- | ----| ----|
    | hset(name, key, value) | 向键名为 name 的散列表中添加映射 | name： 键名；key： 映射键名；value： 映射键值 | hset('price', 'cake, 5) | 向键名为 price 的散列表中添加映射关系，cake的值为5 | 1，即添加的映射个数 |
    | hestnx(name, key, value) | 如果映射键名不存在，则向键名为 name 的散列表中添加映射 | name： 键名；key： 映射键名；value： 映射键值 | hsetnx('price', 'book', 6) | 向键名为 price 的散列表中添加映射关系， book的值为6 | 1，即添加的映射个数 |
    | hget(name, key) | 返回键名为 name 的散列表中 key 对应的值 | name: 键名；key： 映射键名 | redis.hget(’price', 'cake') | 获取键名为 price 的散列表中键名为 cake 的值 | 5 |
    | hmget(name, keys, *args) | 返回键名为 name 的散列表中各个键对应的值 | name： 键名；keys： 键名序列 | redis.hmget('price', \['apple', 'orange']) | 获取键名为 price 的散列表中 apple 和 orange 的值 | \[b'3', b'7'] |
    | hmset(name, mapping) | 向键名为 name 的散列表中批量添加映射 | name： 键名；mapping： 映射字典 | redis.hmset('price', {'banana': 2, 'pear': 6}) | 向键名为 price 的散列表中批量添加映射 | True |
    | hincrby(name, key, amount=1) | 将键名为 name 的散列表中的映射的值增加 amount | name： 键名；key：映射键名；amount： 增长量 | redis.hincrby('price', 'apple', 3) | key 为 price 的散列表中 apple 的值增加3 | 6，修改后的值 |
    | hexists(name, key) | 键名为 name 的散列表中是否存在键名为键的映射 | name： 键名；key： 映射键名 | redis.hexists('price', 'banana') | 键名为 orice 的散列表中 banana 的值是否存在 | True |
    | hdel(name, *keys) | 在键名为 name 的散列表中，删除键名为键的映射 | name： 键名；keys： 键名序列 | redis.hdel('price', 'banana) | 从键名为 price 的散列表中删除键名为 banana 的映射 | True |
    | hlen(name) |在键名为 name 的散列表中获取映射个数 | name： 键名 | redis.hlen('price) | 从键名为 price 的散列表中获取映射个数 | 6 |
    | hkeys(name) | 从键名为 name 的散列表中获取所有映射键名 | name： 键名 | redis.hkeys('price') | 从键名为 price 的散列表中获取所有映射键名 | \[b'cake', b'book', b'banana', b'pear'] |
    | hvals(name) | 从键名为 name 的散列表中获取所有映射键值 | name： 键名 | redis.hvals('price') | 从键名为 price 的散列表中获取所有映射键值 | \[b'5', b'6', b'2', b'6'] |
    | hgetall(name) | 从键名为 name 的散列表中获取所有映射键值对 | name: 键名 | redis.hgetall('price') | 从加密为 price 的散列表中获取所有映射键值对 | {b'cake': b'5', b'book': b'6', b'organe': b'2', b'pear': b'6'} 

