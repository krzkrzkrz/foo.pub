# Creating a new project

1. Create the directory: `mkdir hello`
2. Navigate to directory: `cd hello`
3. Type `go mod init hello`

   You will end up with `go.mod` file in the directory.

4. Next, create a file called `hello.go` and add the following code:

   ```go
   package main

   import "fmt"

   func main() {
       fmt.Println("Hello, World!")
   }
    ```

5. Run the program: `go run hello.go`. Should output: `Hello, World!`


