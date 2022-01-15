# Task 1
```go
package main

import (
	"fmt"
	"os"
)

// Hello returns a greeting for the named person.
func main() {
	// Return a greeting that embeds the name in a message.
	arg := os.Args
	fmt.Println("Hello World!")
	fmt.Println("Hello", arg[1])
}
```

# Task 2
```go
package main

import (
	"fmt"
	"os"
	"strconv"
)

func main()  {
	// Write your code here
	args := os.Args[1:]
	num_1, err := strconv.ParseInt(args[0], 10, 0)
	num_2, err := strconv.ParseInt(args[1], 10, 0)
	if err != nil {
		fmt.Println("Invalid number")
		os.Exit(1)
	}
	fmt.Println(num_1+num_2)
}
```
# Task 3
```go
package main

import (
	"flag"
	"fmt"
	"os"
	"time"
)

func rangeDate(start, end time.Time) func() time.Time {
	y, m, d := start.Date()
	start = time.Date(y, m, d, 0, 0, 0, 0, time.UTC)
	y, m, d = end.Date()
	end = time.Date(y, m, d, 0, 0, 0, 0, time.UTC)

	return func() time.Time {
		if start.After(end) {
			return time.Time{}
		}
		date := start
		start = start.AddDate(0, 0, 1)
		return date
	}
}
func main()  {
	// Write your code here

	// Init args Parser
	start := flag.String("start","", "Date in yyyy-mm-dd format")
	end := flag.String("end","", "Date in yyyy-mm-dd format")

	flag.Parse()

	// Parse string to date
	// Date Layout: "2006-01-02"
	// ref
	/*
	ANSIC	“Mon Jan _2 15:04:05 2006”
	UnixDate	“Mon Jan _2 15:04:05 MST 2006”
	RubyDate	“Mon Jan 02 15:04:05 -0700 2006”
	RFC822	“02 Jan 06 15:04 MST”
	RFC822Z	“02 Jan 06 15:04 -0700”
	RFC850	“Monday, 02-Jan-06 15:04:05 MST”
	RFC1123	“Mon, 02 Jan 2006 15:04:05 MST”
	RFC1123Z	“Mon, 02 Jan 2006 15:04:05 -0700”
	RFC3339	“2006-01-02T15:04:05Z07:00”
	RFC3339Nano	“2006-01-02T15:04:05.999999999Z07:00”
	*/
	layout := "2006-01-02"
	dStart, err := time.Parse(layout, *start)
	if err != nil {
		fmt.Println("Invalid start date input")
		os.Exit(1)
	}
	dEnd, err:= time.Parse(layout, *end)
	if err != nil {
		fmt.Println("Invalid end date input")
		os.Exit(1)
	}

	count := 0
	for d := rangeDate(dStart, dEnd); ;{
		date := d()
		if date.IsZero() {
			break
		}
		fmt.Println(date.Format("2006-01-02"))
		if (date.Weekday() == time.Sunday) || (date.Weekday() == time.Saturday) {
			count = count + 1
		}
	}

	fmt.Printf("There are %d weekend(s)\n", count)
}
```