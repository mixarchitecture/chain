# this repository has been moved into [cillop](https://github.com/cilloparch/cillop).

# Chain

that simplifies error checking and cleaning your code.

## Installation

```bash
go get github.com/mixarchitecture/chain
```

## If there was no chain

```go
package main

import (
    "fmt"
    "github.com/mixarchitecture/chain"
)

func main() {
    var err error
    var a, b, c int

    a, err = getA()
    if err != nil {
        fmt.Println(err)
        return
    }

    b, err = getB()
    if err != nil {
        fmt.Println(err)
        return
    }

    c, err = getC()
    if err != nil {
        fmt.Println(err)
        return
    }

    fmt.Println(a, b, c)
}

func getA() (int, error) {
    return 1, nil
}

func getB() (int, error) {
    return 2, nil
}

func getC() (int, error) {
    return 3, nil
}
```

## With chain

```go
package main

import (
    "fmt"
    "github.com/mixarchitecture/chain"
    "github.com/mixarchitecture/i18np"
)

type config struct {
    a, b, c int
}

type res struct {
    a, b, c int
}

func main() {
    ch := chain.New[config, res]()
    ch.Use(foo, bar, baz, end)
    res, err := ch.Run(config{})
    if err != nil {
        fmt.Println(err.Error())
        return
    }
    fmt.Println(res) // &{1 2 3}
}

func getA() (int, *i18np.Error) {
    return 1, nil
}

func getB() (int, *i18np.Error) {
    return 2, nil
}

func getC() (int, *i18np.Error) {
    return 3, nil
}

func foo(cnf config) (*res, *i18np.Error)  {
    a, err := getA()
    if err != nil {
        return nil, err
    }
    config.a = a
    return nil, nil
}
func bar(cnf config) (*res, *i18np.Error)  {
    b, err := getB()
    if err != nil {
        return nil, err
    }
    config.b = b
    return nil, nil
}

func baz(cnf config) (*res, *i18np.Error)  {
    c, err := getC()
    if err != nil {
        return nil, err
    }
    config.c = c
    return nil, nil
}

func end(cnf config) (*res, *i18np.Error)  {
    return &res{
        a: cnf.a,
        b: cnf.b,
        c: cnf.c,
    }, nil
}
```

## Contributing

Contributions are always welcome!

## License

[MIT](LICENSE)
