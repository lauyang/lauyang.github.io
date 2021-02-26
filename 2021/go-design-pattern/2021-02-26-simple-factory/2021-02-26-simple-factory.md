#### 简单工厂模式
> go 语言没有构造函数一说，所以一般会定义NewXXX函数来初始化相关类。
NewXXX 函数返回接口时就是简单工厂模式，也就是说Golang的一般推荐做法就是简单工厂。  
在这个simplefactory包中只有API 接口和NewAPI函数为包外可见，封装了实现细节。

#### 代码实现
```
#api.go
//API is interface
type API interface {
	Say() string
}

//NewAPI return Api instance by type
func NewAPI(t string) API {
	switch t {
	case "walk":
		return &walkAPI{}
	case "run":
		return &runAPI{}
	default:
		return &walkAPI{}
	}
}

```
```
#walk.go
import "fmt"

//walkAPI is one of API implement
type walkAPI struct{}

//say i am walk
func (*walkAPI) Say(action string) string {
	return fmt.Sprintf("i am walk")
}
```
```
#run.go
import "fmt"

//runAPI is another of API implement
type runAPI struct{}

//say i am run
func (*runAPI) Say() string {
	return fmt.Sprintf("i am run")
}
```

```
#api_test.go
import "testing"

//TestType1 test get hiapi with factory
func TestWalkApi(t *testing.T) {
	api := NewAPI("walk")
	s := api.Say()
	if s != "i am walk" {
		t.Fatal("TestWalkApi test fail")
	}
}

func TestRunApi(t *testing.T) {
	api := NewAPI("run")
	s := api.Say()
	if s != "i am run" {
		t.Fatal("TestRunApi test fail")
	}
}
```