# Go

Links to resources that have helped me learn Go.

## Courses

- Learn GO with Tests https://quii.gitbook.io/learn-go-with-tests/

## Testing

- TDD with respect to Go https://ankur-a22.medium.com/tdd-with-respect-to-go-df943cc6ca3a
- Learn how t.Parallel works https://engineering.mercari.com/en/blog/entry/20220408-how_to_use_t_parallel/

## Language Specs

### Concurrency

- Using Goroutines, Channels, Contexts, Timers, WaitGroups and Errgroups https://ankur-a22.medium.com/using-goroutines-channels-contexts-timers-waitgroups-and-errgroups-24b6062c1c93

### Types

- Struct And Interface Embedding https://ankur-a22.medium.com/struct-and-interface-embedding-f0da8517fa8a

## Statements

- [Select](https://go.dev/ref/spec#Select_statements)

    The cases are evaluated in order once. If a case (other than `default`) can proceed it will be chosen. If more than one case can proceed. one will be chosen via a uniform pseudo-random selection. If no cases are ready (no `default` present), it will block. If no cases are ready and there is a `default`, the `default` will be chosen.

    This demonstrates that `a` or `b` are uniformly selected, and `default` is never selected:

    ```go
    package main

    import "fmt"

    func main() {
        var a, b, def int
        for i := 0; i < 100; i++ {
            v := test()
            switch v {
            case "a":
                a++
            case "b":
                b++
            case "default":
                def++
            }
        }

        fmt.Printf("a:%d b:%d, default:%d", a, b, def)
    }

    func test() string {
        a := make(chan bool, 1)
        b := make(chan bool, 1)
        done := make(chan bool)

        go func() {
            a <- true
            b <- true
            close(done)
        }()

        <-done

        select {
        case <-a:
            return "a"
        case <-b:
            return "b"
        default:
            return "default"
        }
    }
    ```
