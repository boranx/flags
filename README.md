# flags - Golang command-line flag parser
[![GoDoc](https://godoc.org/github.com/simonleung8/flags?status.svg)](https://godoc.org/github.com/simonleung8/flags) [![Build Status](https://travis-ci.org/simonleung8/flags.png?branch=master)](https://travis-ci.org/simonleung8/flags)

- Simple to use, little learning curve
- Fully tested, reliable
- Catches any non-defined flags, and any invalid flag values
- Flags can come before or after the arguments. The followings are all valid inputs:
```bash
$ testapp -i 100 -m 500 arg1 arg2   # flags go first
$ testapp arg1 arg2 --i 100 -m 500  # flags go last
$ testapp arg1 -i 100 arg2 -m=500   # flags go in between arguments
```
The parsed results for all 3 statements are identical: `i=100`, `Args=[arg1, arg2]`, `m=500`

# Installation
```bash
go get github.com/simonleung8/flags  # installs the flags library
```

# Usage
```Go
package main

Import "github.com/simonleung8/flags"

func main(){
  fc := flags.New()
  fc.NewStringFlag("password", "p", "flag for password")  //name, short_name and usage of the string flag
  fc.Parse(os.Args...)  //parse the OS arguments
  println("Flag 'password' is set: ", fc.IsSet("s"))
  println("Flag 'password' value: ", fc.String("s"))
}
```
Running the above code
```
$ main -password abc
Flag 'password' is set: true
Flag 'password' value: abc
```

# Available Flag Constructor
Flags: String, Int, float64, Bool, String Slice
```Go
NewStringFlag(name string, shortName string, usage string)
NewStringFlagWithDefault(name string, shortName string, usage string, default string)
NewIntFlag(name string, shortName string, usage string)
NewIntFlagWithDefault(name string, shortName string, usage string, default int)
NewFloat64Flag(name string, shortName string, usage string)
NewFloat64FlagWithDefault(name string, shortName string, usage string, default float64)
NewStringSliceFlag(name string, shortName string, usage string) //this flag can be supplied more than 1 time
NewStringSliceFlagWithDefault(name string, shortName string, usage string, default []string)
NewBoolFlag(name string, shortName string, usage string)
```

# Functions for flags/args reading
```Go
IsSet(flag_name string)bool
String(flag_name string)string
Int(flag_name string)int
Float64(flag_name string)float64
Bool(flag_name string)bool
StringSlice(flag_name string)[]string  
Args()[]string
```

# Parsing flags and arguments
```Go
Parse(args ...string)error
```
Sample Code
```Go
fc := flags.New()
fc.NewIntFlag("i", "", "Int flag name i")  //set up a Int flag '-i'
fc.NewBoolFlag("verbose", "v", "Bool flag name verbose")  //set up a bool flag '-verbose'
err := fc.Parse(os.Args...) //Parse() returns any error it finds during parsing
If err != nil {
  fmt.Println("Parsing error:", err)
}
fmt.Println("Args:", fc.Args())  //Args() returns an array of all the arguments
fmt.Println("Verbose:", fc.Bool("verbose"))
fmt.Println("i:", fc.Int("i"))
```
Running above
```bash
$ app arg_1 -i 100 arg_2 -verbose  # run the code
Args: [arg_1 arg_2]
Verbose: true
i: 100
```
Parse( ) catches any non-defined flags and invalid value for Int, Float64 and Bool flag.

# Special function
```Go
SkipFlagParsing(bool)  //if set to true, all flags become arguments
ShowUsage(leadingSpace int)string  //string containing all the flags and their usage text
```
