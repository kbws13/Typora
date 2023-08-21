# 介绍和安装

xorm是一个简单而强大的Go语言ORM框架，可以边界的操控数据库

> 特性

- 支持Struct和数据库表之间的灵活映射，并且支持自动同步
- 事务支持
- 同时支持原生SQL语句和ORM操作的混合执行
- 使用连写来简化调用
- 支持使用ID，In，Where，Limit，Join，Having，Table，SQL，Cols等函数和结构体等方式作为条件

> 安装

```shell
go get xorm.io/xorm
```



# 同步结构体和数据库

创建一个引擎

```go
ngine, err := xorm.NewEngine(driverName, dataSourceName)


var (
    username  string = "root"
    password  string = "hsy031122"
    ipAddress string = "127.0.0.1"
    port      int    = 3360
    dbName    string = "query"
    charset   string = "utf8mb4"
)
//构建数据库连接信息
dataSourceName := fmt.Sprint("%s:%s@tcp(%s:%d)/%s?charset=%s", username, password, ipAddress, port, dbName, charset)
ngine, err := xorm.NewEngine("mysql", dataSourceName)
```

定义一个与表同步的结构体

```go
type User struct {
    Id      int64
    Name    string
    Age     int
    Passwd  string    `xorm:"varchar(200)"`
    Created time.Time `xorm:"created"`
    Updated time.Time `xorm:"updated"`
}
err = engine.Sync(new(User))
```

# 数据插入

- Insert插入一条或多条记录

    ```go
    package main
    
    import (
    	"fmt"
    	_ "github.com/go-sql-driver/mysql"
    	"time"
    	"xorm.io/xorm"
    )
    
    func main() {
    	//数据库连接基本信息
    	var (
    		username  string = "root"
    		password  string = "hsy031122"
    		ipAddress string = "127.0.0.1"
    		port      int    = 3306
    		dbName    string = "go_test"
    		charset   string = "utf8mb4"
    	)
    	//构建数据库连接信息
    	dataSourceName := fmt.Sprintf("%s:%s@tcp(%s:%d)/%s?charset=%s", username, password, ipAddress, port, dbName, charset)
    	engine, err := xorm.NewEngine("mysql", dataSourceName)
    	if err != nil {
    		fmt.Println("连接数据库失败")
    	}
    
    	type User struct {
    		Id      int64
    		Name    string
    		Age     int
    		Passwd  string    `xorm:"varchar(200)"`
    		Created time.Time `xorm:"created"`
    		Updated time.Time `xorm:"updated"`
    	}
    	err = engine.Sync(new(User))
    	if err != nil {
    		fmt.Println("表结构同步失败")
    		fmt.Println(err)
    	}
    
    	//构建插入的用户信息.Insert方法使用指针作为参数
    	user1 := User{Id: 1001,Name: "hsy",Age: 21,Passwd: "123456"}
    	user2 := User{Id: 1002,Name: "hsy",Age: 21,Passwd: "123456"}
    	user3 := User{Id: 1003,Name: "hsy",Age: 21,Passwd: "123456"}
    	n,_ := engine.Insert(&user1,&user2,&user3)
    	if n>=1 {
    		fmt.Println("数据插入成功")
    		fmt.Println(n)
    	}
    	//也可以用切片
    	var users []User
    	users = append(users,User{Id: 1004,Name: "hsy",Age: 21,Passwd: "123456"})
    	users = append(users,User{Id: 1005,Name: "hsy",Age: 21,Passwd: "123456"})
    	n,_ = engine.Insert(&users)
    }
    ```

- 更新和删除

    - update更新数据，默认只更新非空和非0字段

        更新数据使用`Update`方法，Update方法的第一个参数为需要更新的内容，可以为一个结构体指针或者一个Map[string]interface{}类型。当传入的为结构体指针时，只有非空和0的field才会被作为更新的字段。当传入的为Map类型时，key为数据库Column的名字，value为要更新的内容。

        `Update`方法将返回两个参数，第一个为 更新的记录数，需要注意的是 `SQLITE` 数据库返回的是根据更新条件查询的记录数而不是真正受更新的记录数。

        ```Go
        user := new(User)
        user.Name = "myname"
        affected, err := engine.ID(id).Update(user)
        ```

        注意，Update会自动从user结构体中提取非0和非nil得值作为需要更新的内容，如果需要更新一个值为0，则该方法将无法实现，有两种选择：

        - 1.通过添加Cols函数指定需要更新结构体中的哪些值，未指定的将不更新，指定了的即使为0也会更新。

        ```Go
        affected, err := engine.ID(id).Cols("age").Update(&user)
        ```

        - 2.通过传入map[string]interface{}来进行更新，但这时需要额外指定更新到哪个表，因为通过map是无法自动检测更新哪个表的。

        ```Go
        affected, err := engine.Table(new(User)).ID(id).Update(map[string]interface{}{"age":0})
        ```

        有时候希望能够指定必须更新某些字段，而其它字段根据值的情况自动判断，可以使用 `MustCols` 来组合 `Update` 使用。

        ```Go
        affected, err := engine.ID(id).MustCols("age").Update(&user)
        ```

        另外，如果需要更新所有的字段，可以使用 `AllCols()`。

        ```Go
        affected, err := engine.ID(id).AllCols().Update(&user)
        ```

    - delete删除记录，删除必须有一个条件

        删除数据用 `Delete`方法，参数为struct的指针并且成为查询条件。

        ```Go
        user := new(User)
        affected, err := engine.ID(id).Delete(user)
        ```

        `Delete`的返回值第一个参数为删除的记录数，第二个参数为错误。

        注意1：当删除时，如果user中包含有bool,float64或者float32类型，有可能会使删除失败。具体请查看 [FAQ](https://xorm.io/zh/docs/chapter-07/readme/#160) 注意2：必须至少包含一个条件才能够进行删除，这意味着直接用

        ```Go
        engine.Delete(new(User))
        ```

        将会报一个保护性的错误，如果你真的希望将整个表删除，你可以

        ```Go
        engine.Where("1=1").Delete(new(User))
        ```

    - Exec，执行一个SQL语句

        直接执行一个SQL命令，即执行Insert， Update， Delete 等操作。此时不管数据库是何种类型，都可以使用 ` 和 ? 符号。

        ```Go
        sql = "update `userinfo` set username=? where id=?"
        res, err := engine.Exec(sql, "xiaolun", 1)
        ```

# 数据查询

