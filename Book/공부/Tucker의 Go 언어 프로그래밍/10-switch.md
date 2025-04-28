# switch문 동작 원리
switch문은 값에 따라 다른 로직을 수행할 때 사용한다.
```go
switch 비교값 {
case 값1:
	문장
case 값2:
	문장
default:
	문장
}
```

첫 번째 case 부터 검사한다. 만약 비교값과 case 값이 같으면 해당 case 문장을 수행하고 switch문을 종료한다. 같은 값이 없으면 default 문장을 수행한다. default는 생략가능하다.

- switch 문을 사용하면 복잡한 if else 문을 보기 좋게 정리할 수 있다.

```go
package main

import "fmt"

func main() {
	day := 3

	if day == 1 {
		fmt.Println("첫째 날입니다.")
	} else if day == 2 {
		fmt.Println("둘째 날입니다.")
	} else if day == 3 {
		fmt.Println("셋째 날입니다.")
	} else {
		fmt.Print("프로젝트를 진행하세요.")
	}
}
```

```go
package main

import "fmt"

func main() {
	day := 3

	switch day {
	case 1:
		fmt.Println("첫째 날입니다.")
	case 2:
		fmt.Println("둘째 날입니다.")
	case 3:
		fmt.Println("셋째 날입니다.")
	default:
		fmt.Print("프로젝트를 진행하세요.")
	}
}
```

# 다양한 switch 문 형태
- 하나의 case 는 하나 이상의 값을 비교할 수 있다. 각 값은 `,` 로 구분한다.
```go
package main

import "fmt"

func main() {
	
	day := "sunday"

	switch day {
	case "monday", "tuesday":
		fmt.Println("평일입니다.")
	case "saturday", "sunday":
		fmt.Println("주말입니다.")
	}

}
```

- 비교값에 부울린 값을 넣고 조건문을 비교할 수도 있다. case에서 조건문이 비교값과 같아지는 경우에 해당 case 문장이 실행된다. 비교값을 적지 않는 경우 기본 값으로 `true`를 사용한다. 

- if문과 마찬가지로 초기문을 넣을 수 있다.
```go
switch 초기문; 비교값 {
case 값1:
	...
default:
}
```

# `const` 열거값과 `switch`
`const` 열거값에 따라 수행되는 로직을 변경할 때 `switch`문을 주로 사용한다.

```go
package main

import "fmt"

type ColorType int

const (
	Red ColorType = iota
	Blue
	Green
	Yellow
)

func colorToString(color ColorType) string {
	switch color {
	case Red:
		return "Red"
	case Blue:
		return "Blue"
	case Green:
		return "Green"
	case Yellow:
		return "Yello"
	default:
		return "Undefined"
	}
}
func getMyFavoriteColor() ColorType {
	return Red
}

func main() {
	fmt.Println("My favoraite color is", colorToString(getMyFavoriteColor()))
}
```

```
My favoraite color is Red
```

# break와 fallthrough 키워드
- 일반적으로 다른 언어에서는 switch 문과 각 case 종료시에 break 문을 사용해야 다음 case로 코드가 이어서 실행되지 않는다. 하지만 Go 언어에서는 break를 사용하지 않아도 case 하나를 실행 후 자동으로 switch 문을 빠져나가게 된다. 
- 다음 case도 검사하게 하고 싶다면 fallthrough 키워드를 써주면 된다.
