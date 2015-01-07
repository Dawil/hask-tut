% Haskell
% David Wilcox
% NCSS 2015

# 0.0 - Why Haskell?

> "Language shapes the way we think, and determines what we can think about." - Benjamin Lee Whorf

> "The limits of my language mean the limits of my world." - Ludwig Wittgenstein

> "A language that doesn’t affect the way you think about programming, is not worth knowing." - Alan Perlis

From [http://shuklan.com/haskell/lec01.html#/0/30](http://shuklan.com/haskell/lec01.html#/0/30).

# 0.1 - Haskell History

- Created in 1990
- By committee
- For language research

It's deliberately different to most languages.

# 0.2 - Getting Started

- Login to SIT via putty (ucpu2.ug.it.usyd.edu.au)
- Compiler is called `ghc`, interpreted is called `ghci`
- Use text editor of choice
- Can follow slides at https://dawil.github.com/hask-tut/tut.html

# 1.0 - Values

- 1, 20000/50, 2.71, -1
- "Hello world"
- True, False
- [1,2,3], ["Hello", "world"], [1..5], [1,3..10]
- (4, [False]), ("Hello",1,True)
- -- comments

# 1.1 - Functions

- 5 * 10, 10 - 2
- "Hello" ++ " world"
- length [1,2,3]
- 1:[2,3], True:[]
- not True, or [True, False, False]
- maximum 1 10
- tail ["cat", "dog", "horse"]
- let string50 = show 50
- length string50

Visit http://dawil.github.com/hask-tut/(sum [1,5..5000] / 20).html
(Don't include the parentheses: () )

# 1.2 - Custom Functions

- let f x y = x + y
- let addOne x = x + 1
- let showLength x = length (show x)
- let addOne' = (+1)
- let showLength' = length . show
- multiline:

```
    :{
    let fib 0 = 1
        fib 1 = 1
        fib n = fib (n-1) + fib (n-2)
    :}
```

http://dawil.github.com/hask-tut/(fib 12).html

# 1.3 - More Functions

- zip "hello" [1..5]
- myMap:
```
    let myMap f [] = []
        myMap f (x:xs) = f x : myMap f xs
```
- map (+1) [1,2,3,4]
- map addOne [1,2,3,4]
- let sum = foldr (+) 0 [1,2,3]
- let sumTwoLists = sum . zipWith (+)

http://dawil.github.com/hask-tut/(sumTwoLists [1..5000] [1,-1..(-333)]).html

# 1.3 - Control Flow: Looping and If-Statements

There are no loops.

There are If statements though:

```
if True then 5 else 4

:{
if False
  then [1,2,3]
  else []
:}
```

Bonus exercise: Using pattern matching, create a function `myIf` that takes a boolean, and returns the second argument if it's true and the third if it's false.

# 2.1 - Laziness

- let integers = [1..]
- take 10 integers
- let evenIntegers = filter even integers
- sum (take 10 evenIntegers)
- Can we `sum integers`?
- let foreverTrue = repeat True
- let myBooleans = [True, False] ++ foreverTrue
- and myBooleans

Also:

- let fibs = 1 : 1 : zipWith (+) fibs (tail fibs)
- take 40 fibs

http://dawil.github.com/hask-tut/(sum (take 40 fibs)).html

# 2.1b - (Aside) Lazy Text/Strict Text

- :m + Data.ByteString.Lazy.Char8
- let (bsMsgA, bsMsgB) = (pack "hello", pack " world")
- let fullBsMsg = append bsMsgA bsMsgB
- let string = unpack fullBsMsg

# 2.2 - Static Typing

- :type addOne
- :t (+)
- :t [1,2,3]
- :t []
- :t [] :: [Int]
- [1,2, "Hello"]
- type Age = Int
- type Person = (String,Int)
- let mkPerson name age = (name, age)
- data Person = Person String Age
- let p = Person "Dave" (-1)
- let getAge (Person n a) = a

# 2.2b - Make Invalid States Illegal

- let p = Person "Dave" (-1) -- Problematic!
- :t Just 5
- :t Nothing
- newtype CheckedAge = CheckedAge Int
- let checkAge n = if n < 0 then Nothing else Just (CheckedAge n)
- data Person = Person String CheckedAge

# 2.2c - Other Types of Types

- data Animal = Cat | Dog
- data Maybe a = Just a | Nothing
- data Either a b = Left a | Right b

# 2.3 - Input/Output (IO)

- putStrLn "Hello!"
- :t putStrLn
- data IO a = ??
- name <- getLine
- :t getLine

# 2.3b - Input/Output (IO)

```
:{
  let askName = do
        putStrLn "Hello, what is your name?"
        name <- getLine
        return name
  let askAge = do
        putStrLn "What is your age?"
        ageString <- getLine
        let age = read age
        return (checkAge age)
:}
name <- askName
age <- askAge
let p = Person name age
```


# 3.0 - Hello World

In a file `hello.hs`:
```
main = putStrLn "Hello World"
```

- ghc hello.hs
- ./hello

# 3.1 - Links I wanted to include but couldn't

* The story about the email that couldn't travel more than 500 miles: http://www.ibiblio.org/harris/500milemail.html
* Slow winter, a tale by a Microsoft employee that needs to take a break sometime: https://www.usenix.org/system/files/1309_14-17_mickens.pdf
