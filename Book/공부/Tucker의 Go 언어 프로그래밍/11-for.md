# for
Go 언어는 반복문으로 for문 하나만 지원하지만, 여러 형태가 있기 때문에 각 형태를 적재적소에 잘 사용해야한다.
기본형태는 다음과 같다.
```go
for 초기문; 조건문; 후처리 {
	코드 블록
}
```
- for 문이 실행될 때 초기문이 먼저 실행된다.
- 조건문 결과가 `true`이면 `{}` 내부 코드를 실행한다. 그리고 후처리 구문을 실행한다.
- 조건문 결과가 `false`이면 후처리 없이 for문을 종료한다. 

```go
package main

import "fmt"

func main() {
	for i := 0; i < 10; i++ {
		fmt.Printf("%d, ", i)
	}
}
```

```
0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 
```

- 여기서 변수는 내부에서만 사용된다.

## 초기문 생략
```go
for ; 조건문; 후처리 {
	코드 블록
}
```

## 후처리 생략
```go
for 초기문; 조건문; {
	코드 블록
}
```

## 둘 다 생략
```go
for ; 조건문; {
	코드 블록
}
```

```go
for 조건문 {
	코드 블록
}
```

## 무한루프
```go
for true {
	코드 블록
}
```

```go
for {
	코드 블록
}
```

- 조건문 생략되는 경우 switch 에서 처럼 true 이다.

# continue 와 break
- continue 는 이후 코드 블록을 수행하지 않고 곧바로 후처리를 한 후 다시 조건문으로 넘어간다.
- break 는 반복문을 종료시킨다. 

# 중첩 for문과 break, 레이블
중첩 for문에서 break만 사용하면 break가 속한 for문에서만 빠져나온다. 모든 for문에서 빠져나올때 불리언 변수를 사용해 플래그를 만들 수 있다.

```go
package main

import "fmt"

func main() {
	a := 1
	b := 1
	found := false
	for ; a <= 9; a++ {
		for b = 1; b <= 9; b++ {
			if a*b == 45 {
				found = true
				break
			}
		}
		if found {
			break
		}
	}
	fmt.Printf("%d * %d = %d", a, b, a*b)
}
```

```
5 * 9 = 45
```

- 플래그로 나오는 조건을 작성하는 것이 번거로울 때에는 레이블을 사용할 수 있다. 레이블을 사용하면 그 레이블에서 가장 먼저 속한 for문까지 모두 종료하게 된다. 

```go
package main

import "fmt"

func main() {
	a := 1
	b := 1
OuterFor:
	for ; a <= 9; a++ {
		for b = 1; b <= 9; b++ {
			if a*b == 45 {
				break OuterFor
			}
		}
	}
	fmt.Printf("%d * %d = %d", a, b, a*b)
}
```

- 레이블을 사용하는 방법이 편리할 수 있으나 혼동을 불러일으킬 수 있고 잘못 사용하면 예기치 못한 버그가 발생할 수 있다. 되도록 플래그를 사용하고 레이블은 꼭 필요한 경우에만 사용하기를 권장한다고 한다. 클린 코드를 지향하려면 중첩된 내부 로직을 함수로 묶어 중첩을 줄이고, 플래그 변수나 레이블 사용을 최소화해야한다.

```go
package main

import "fmt"

func find45(a int) (int, bool) {
	for b := 1; b <= 9; b++ {
		if a*b == 45 {
			return b, true
		}
	}
	return 0, false
}

func main() {
	a := 1
	b := 0
	for ; a <= 9; a++ {
		var found bool
		if b, found = find45(a); found {
			break
		}
	}
	fmt.Printf("%d * %d = %d", a, b, a*b)
}
```