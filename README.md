




<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="dist/mydatabase.min.js"></script>
    <script>
    //实例化一个数据库对象
    var o = new myDatabase('students');
    //你也可以对数据库进行版本号、描述、大小进行设置

    var o = new myDatabase('students', '1.1', "学生", 1024 * 1024);

    //是否开启调试模式,默认为：false
    o.isdebug = true;

    //定义表
    o.define("student", {
        id: myDatabase.dataType.pk,
        name: {
            type: myDatabase.dataType.text,
            required: true
        },
        addDate: myDatabase.dataType.date,
        age: myDatabase.dataType.integer
    });
    //你也可以一次定义多个表
    var dbTable = [{
            table: 'course',
            properties: {
                sid: {
                    type: 'text',
                    required: true
                },
                duedate: 'date',
                sname: myDatabase.dataType.text
            }
        }, {
            table: 'fav',
            properties: {
                task: {
                    type: 'text',
                    required: true
                },
                duedate: 'date'
            }
        }

    ];

    //创建多个表
    o.defineMutile(dbTable);


    //删除表
    o.dropTable("fav");

    //增加表字段 表明、字段、字段类型
    o.addColumn("fav", "name", "text")
        //插入单条数据

    o.insert('todo', {
        task: 'shipeifeifie',
        duedate: new Date(2013, 11, 31)
    });
    //插入多条数据
    o.insertMutile('todo', [{
        task: 'shipeifeifie',
        duedate: new Date()
    }, {
        task: 'shipeifeifie',
        duedate: new Date()
    }]);

    //修改数据
    o.update('todo', {
        task: 'ffff'
    }, {
        ' id =': '2'
    });

    //删除数据
    o.delete('todo', {
        ' id = ': '2'
    });


    //查询表，进行排序和分页
    o.myQuery('todo', 'task,id', "", {
        id: 'desc',
        task: 'asc'
    }, 1, 7);
    //查询表
    //o.query();
    //
    //查询所有的数据
    o.all("todo", ["task", "id", "name"], function(ok, tx, result) {
        for (var item in result.rows) {
            console.log(result.rows[item].id);
        }
    });

    //查询单条数据
    o.queryOne("select * from todo where id=?", [2], function(ok, tx, result) {
        console.log(result);
    });
    </script>
</head>

<body>
</body>

</html>