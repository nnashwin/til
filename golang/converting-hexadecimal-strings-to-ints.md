# Converting Hexadecimal Strings to Ints

Recently, I have had the opportunity to work on a project that involves analyzing the logs of an Ethereum Smart Contract.
Many of the fields in these logs do not use base-10 numbers, choosing rather to have many large decimal values shortened by storing them in hexadecimal strings.

At first, I wasn't sure if the conversion was based on a different format that was contract specific.  But after digging around for a bit and using multiple hexadecimal to base-10 (plain decimals) converters online, I realized that the format was consistent.

In Golang, this can be done through by converting a hex string to a big.Int string in the math/big standard package.  But first we need to remove the 0x or 0X from the beginning of the hex string.

```golang
import (
    "strings"
    "math/big"
)

func ConvertHexToDecimal(hexStr string, bi *big.Int) *big.Int {
    s := strings.Replace(hexStr, "0x", "", -1)
    s = strings.Replace(s, "0X", "", -1)
    // ... call to bigInt function

    return bi
}
```

I personally have not worked with hex strings that have a capital X after the preceeding 0, but calling strings.Replace twice does not seem to add too much overhead to the function. Your mileage will probably vary based on your use case.

The finished code and an example is below.

```golang
import (
    "strings"
    "math/big"
)

func ConvertHexToDecimal(hexStr string, bi *big.Int) {
    s := strings.Replace(hexStr, "0x", "", -1)
    s = strings.Replace(s, "0X", "", -1)
    bi.SetString(s, 16)
}


func main() {
    hexStr := "0xe5376d30bed6efe49339232bca35299f5cbb84cd3f252bab5939d687aab4b2e5"
    bi := new(big.Int)
    ConvertHexToDecimal(hexStr, bi)
    fmt.Print(bi.String())
    // You should get 103677572518657809674126080327732760672691301797143299412757054441268188590821.  Truly a BIG number.
}
```

It's cool how simple it is in Golang to convert to multiple bases and handle really big numbers with precision.