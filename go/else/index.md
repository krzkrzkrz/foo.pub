# Else

* Variables declared inside an if short statement are also available inside any of the else blocks.

```go
func pow(x, y int) int {
	if x := x * 10; x < y {
		return x
	} else {
		return y
	}
}

func main() {
	fmt.Println(
		pow(1, 50),
		pow(10, 50),
	) // Returns: 10 50
}
```

