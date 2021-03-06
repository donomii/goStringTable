[![Build Status](https://travis-ci.org/donomii/goStringTable.svg?branch=master)](https://travis-ci.org/donomii/goStringTable)
[![GoDoc](https://godoc.org/github.com/donomii/goStringTable?status.svg)](https://godoc.org/github.com/donomii/goStringTable)

# goStringTable
A thread-safe symbol table (string table) for Go

## Description

A string table turns strings into numbers, and back again.  Popularised by LISP and Scheme, a string table is an efficient (and sometimes quick) way to store a lot of strings, when many of them are expected to be identical.  e.g. when parsing a program, function and variable names are usually put into a symbol table.

It works like a hashmap, but it is lock-free to read from multiple threads(unlike Go hashmaps).  Unlike a normal map, a string table works both ways, which is why it is restricted to strings.

## Speed

Going from a symbol(a number) to string is an array lookup, so almost instant.

Going from a string to a number requires a lookup in a Patricia (radix) tree, which means the time should be roughly log(n), where n is the number of stored strings.

Adding a string is very slow, for various reasons, so it is best used in situations where the reads will vastly outnumber the writes.  

## Use

    import "github.com/donomii/goStringTable"
    s := goStringTable.New()
    n := s.LookupOrCreate("Hello World")
    fmt.Println(s.GetString(n))

or lookup a string without creating it

    sym, err := s.Lookup("Hello World")

## Similar projects

* [Concurrent Map](https://github.com/streamrail/concurrent-map)
