# Go Syntax

[TOC]

## Hello World
This is an example hello world program.

``` go
package main						// Package declaration, create an executable or a library. Main == executable.

import "fmt"						// Libraries to import

func main() {						// Main function
	fmt.Println("Hello World")
}
/*
Example multi-line comment
*/
```

It can be compiled with `go build <filename>`, or run without producing an executable file with `go run <filename>`.

## Libaries
A full list of standard go libraries is here: https://golang.org/pkg/. There are two ways to import packages:

``` go
import "package1"

import (
	"package2"
	"package3"
)

// Or you can import with an alias:
import (
	p1 "package1"
)

p1.SampleFunc()
```

## In-Built Help
You can access the in-built help with `go doc <packagename>`. More specific information can be obtained by adding the function that you want to get help on, eg `go doc fmt.PrintLn`. Alternately you can use the web help: https://golang.org/pkg/fmt/


## Variables and Types

``` go
package main

import "fmt"

func main(){
	var stationName string
	var nextTrainTime int8
	var isUptownTrain bool

	stationName = "Union Square"
	nextTrainTime = 12
	isUptownTrain = false

	fmt.Println("Current Station:", stationName)
	fmt.Println("Next Train:", nextTrainTime, "minutes")
	fmt.Println("Is uptown:", isUptownTrain)
}
```

``` go
// Literals
fmt.Println(20 * 3) // Prints: 60

// Constants - cannot be updated while the program is running
const funFact = "Hummingbirds' wings can beat up to 200 times a second"

// Numbers are ints or float. You wont use complex numbers.

bool a   // min 0 (false)			max 1 (true)
int8 b   // min -128				max 127
int16 c  // min -32,768				max 32,767
int32 d  // min -2.1 billion (~)	max 2.1 billion (approx)
int64 e  // min very low			max very large

// Unsigned integers, which are always positive
uint8 f  // min 0					max 255
uint16 h // min 0					max 65,535
uint32 i // min 0					max 4.2 billion & a bit
uint64 k // min 0					max very, very large

float32, float64 // differing levels of precision

// Declare a variable as follows
var lengthOfSong uint16
var isMusicOver bool
var songRating float32

// Assigning value
var kilometersToMars int32
kilometersToMars = 62100000

// or
var kilometersToMars int32 = 62100000

// Strings
var nameOfSong string = "Stop Stop"
var nameOfArtist string = "The Julie Ruin"
var description string = nameOfSong + " is by: " + nameOfArtist + "."
fmt.Println(description)

// Default values for variables
var classTime uint32     // 0
var averageGrade float32 // 0
var teacherName string   // Doesn't print anything ""
var isPassFail bool      // Prints false
fmt.Println(classTime, averageGrade, teacherName, isPassFail)

// You can infer type with := This uses float64 and int32/int64 so it's not as efficient. Also
// doesn't require 'var', though you can add it
nuclearMeltdownOccuring := true

```

