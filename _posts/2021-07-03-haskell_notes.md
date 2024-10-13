---
title: Haskell Syntax Notes
date: 2021-07-03 10:00:00 +1000
categories: [Programming, Haskell]
tags: [haskell, functional programming, syntax]
description: "Detailed notes on Haskell syntax, covering setup, basic operations, data types, functions, recursion, and advanced concepts like higher-order functions and type classes."
---

## Setup
~~~console
// load ghci
$ stack repl

// compile main
prelude> :l fileName

// compile
*Main> :r
// run function
*Main> functionName

// get function type
*Main> :t functionName

// load ghc and compile file with main
$ stack exec ghc fileName
// run compiled file
$ ./fileName
~~~

## Basic
~~~haskell
-- comment
{-
multiline comment
-}
import Data.List
import System.IO
~~~

## Operators
* `<`, `>`, `<=`, `>=`
* `==` euqals to
* `/=` not equals to
* `&&` and
* `||` or
* `not` not
* `$` replace parentheses
    * 3 * (1 - 2) = 3 * $ 1 - 2 ???
* `.` chain functions to pass output on the right to the input on the left
    * `sumValue = putStrLn (show (1 + 2))` = `sumValue = putStrLn . show $ 1 + 2`

## Data Types
haskell -> `static typing`

~~~haskell
-- Int -> [-2^63, 2^63]
maxInt = maxBound :: Int -- return 9223372036854775807
minInt = minBound :: Int -- return -9223372036854775808

-- Integer -> unbounded whole number

-- Float -> single precision floating point numbers

-- Double -> precision up to 11 decimal points

-- Bool -> True / False

-- Char -> use single quote

-- Tuple

-- String
hello :: String
hello = "Hello World!"

-- permanent value of variable
always5 :: Int
always5 = 5
num9 = 9 :: Int -- initialise variable with 9
~~~

### Math Functions
~~~Haskell
sumOfNums = sum [1..1000] -- return 500500

addEx = 5 + 4
subEx = 5 - 4
multEx = 5 * 4
divEx = 5 / 4

-- modulus
modEx = mod 5 4 -- mod -> prefix operator, before numbers
modEx2 = 5 `mod` 4 -- mod -> infix operator, in between numbers

-- negative numbers -> parentheses
negNumEx = 5 + (-4)

trueAndFalse = True && False
trueOrFalse = True || False
notTrue = not(True)

piVal = pi
ePowVal = exp 9
logOfVal = log 9
squaredVal = 9 ** 2
sqrtVal = sqrt 9
integralVal = fromIntegral 9 -- convert 9 from Int to Float, sqrt (fromIntegral 9)
truncateVal = truncate 9.999 -- return 9, cut off decimals
roundVal = round 9.999 -- return 10
ceilingVal = ceiling 9.999 -- return 10
floorVal = floor 9.999 -- return 9
~~~

also: `sin`, `cos`, `tan`, `asin`, `acos`, `atan`, `sinh`, `cosh`, `tanh`, `asinh`, `acosh`, `atanh`

understanding functions
* addition funtion `(+) :: Num a => a -> a -> a`
    * `a -> a` take in 2 numbers
    * `-> a` then returns a number
* truncate function `truncate :: (RealFrac a, Integral b) => a -> b`
    * `a` expect a `RealFrac`(real fraction)
    * `-> b` returns an `Integral`(integer)

### List
`list` -> every elements has to be in the same data type
~~~Haskell
-- ways to initialise lists
primeNumbers = [3, 5, 7, 11]
favNumbers = 2 : 7 : 21 : 66 : [] -- [2,7,21,66]
zeroToTen = [0..10] -- [0,1,2,3,4,5,6,7,8,9,10]
evenList = [2, 4..20] -- [2,4,6,8,10,12,14,16,18,20]
letterList = ['A', 'C'..'Z'] -- "ACEGIKMOQSUWY"
infinitePow10 = [10, 20..] -- infinite list
many2s = take 10 (repeat 2) -- [2,2,2,2,2,2,2,2,2,2]
many3s = replicate 10 3 -- [3,3,3,3,3,3,3,3,3,3]
cycleList = take 10 (cycle [1, 2, 3, 4, 5]) -- [1,2,3,4,5,1,2,3,4,5]

-- 2D list
multiList = [[2, 3, 5], [7, 11, 13]]

-- check list
isListEmpty = null morePrimes2 -- check if list is null, returns Bool
is7InList = 7 `elem` morePrimes2 -- check if 7 in list, returns Bool

-- get list values
secondPrimes = morePrimes2 !! 1 -- get list value with index 1, returns prime 3
firstPrimes = head morePrimes2 -- get first value of list
lastPrimes = last morePrimes2 -- get last value of list
primeInit = init morePrimes2 -- get list without last element, returns list without 29
first3Primes = take 3 morePrimes2 -- get first 3 values in list, returns list with these 3 values
removedPrimes = drop 3 morePrimes2 -- remove first 3 values in list, returns list without these 3 values
maxPrimes = maximum morePrimes2 -- get max value in list
minPrimes = minumum morePrimes2 -- get min value in list

-- modify list
morePrimes = primeNumbers ++ [13, 17, 19, 23, 29] -- ++ concatenation
-- returns [3,5,7,11,13,17,19,23,29]
morePrimes2 = 2 : morePrimes -- can only add to the front of list
lenPrimes = length morePrimes2 -- length of list, returns 10
revPrimes = reverse morePrimes2 -- reverse list, returns list
sortedList = sort [2, 3, 5, 1, 4, 7, 6] -- sort list, returns [1,2,3,4,5,6,7]
listBeggerThan5 = filter (>5) morePrimes -- filter list values to be more than 5, returns [7,11,13,17,19,23,29]
evensUpTo20 = takeWhile (<=20) [2, 4..] -- while loop, list only values less than or equals to 20, returns [2,4,6,8,10,12,14,16,18,20]
multOfList = foldl (*) 1 [2, 3, 4, 5] -- multiply list values from left to right(foldl starts on left, foldr starts on right), returns 120

-- list comprehension
newList = [2, 3, 5]
prodPrime = product newList -- returns 2 * 3 * 5 = 30
sumOfList = zipWith (+) [1,2,3,4,5] [6,7,8,9,10] -- zipWith addition, returns [7,9,11,13,15]
-- pull a value out of list and store in variable x each time, and then times 2
listTimes2 = [x * 2 | x <- [1..10]] -- [2,4,6,8,10,12,14,16,18,20]
-- stop at which x * 3 equals to or greater than 50
listTimes3 = [x * 3 | x <- [10..20], x * 3 <= 50] -- [30,33,36,39,42,45,48]
divisibleBy9And13 = [x | x <- [1..500], x `mod` 9 == 0, x `mod` 13 == 0] -- [117,234,351,468]
pow3List = [3^n | n <- [1..10]] -- [3,9,27,81,243,729,2187,6561,19683,59049]
multiplicationTable = [[x * y | y <- [1..10]] | x <- [1..10]] -- [[1,2,3,4,5,6,7,8,9,10],[2,4,6,8,10,12,14,16,18,20],[3,6,9,12,15,18,21,24,27,30],[4,8,12,16,20,24,28,32,36,40],[5,10,15,20,25,30,35,40,45,50],[6,12,18,24,30,36,42,48,54,60],[7,14,21,28,35,42,49,56,63,70],[8,16,24,32,40,48,56,64,72,80],[9,18,27,36,45,54,63,72,81,90],[10,20,30,40,50,60,70,80,90,100]]
~~~

### Tuple
`tuple` -> elements can be in different data type
~~~Haskell
-- initialise Tuple
spongeBob = ("Sponge Bob", 18)

fruits = ["apple", "banana", "cherry"]
animals = ["monkey", "tiger", "elephant"]
fruitsAndAnimals = zip fruits animals -- [("apple","monkey"),("banana","tiger"),("cherry","elephant")]

-- get Tuple value
bobsName = fst spongeBob -- get first element in tuple, returns "Sponge Bob"
bobsAge = snd spongeBob -- get second element in tuple, returns 18

-- modify Tuple

~~~

## Functions
`Functions` must start with lowercase letters
~~~console
// can define functions and values in the GHCi with let
*Main> let num7 = 7
*Main> let getTriple x = x * 3

*Main> getTriple num7 = 21
~~~

simple main
~~~haskell
main = do
    putStrLn "What's your name?" -- print ends with \n
    name <- getLine -- store input in variable name
    putStrLn ("Hello " ++ name) -- print "Hello Zhiqing"
~~~

~~~haskell
-- functionName param1 param2 = operations(returned value)
addMe :: Int -> Int -> Int -- define function, if not can put params not of type Int
addMe x y = x + y -- addMe 10 25, returns 35

addTuple :: (Int, Int) -> (Int, Int) -> (Int, Int)
addTuple (x, y) (x2, y2) = (x + x2, y + y2) -- addTuple (1, 2) (3, 4), returns (4, 6)

whatAge :: Int -> String
whatAge 16 = "You can drive"
whatAge 18 = "You can vote"
whatAge 21 = "You are an adult"
whatAge x = "Nothing important" -- covers ages other than 16, 18, 21
whatAge _ = "Nothing important" -- same as above
~~~

### Recursion
~~~Haskell
factorial :: Int -> Int
factorial 0 = 1
factorial n = n * factorial (n - 1)

-- another way to do factorial
prodFact :: Int -> Int
prodFact n = product [1..n]
~~~

### Guard
~~~Haskell
isOdd :: Int -> Bool
isOdd n
    | n `mod` 2 == 0 = False
    | otherwise = True

-- another way
isEven :: Int -> Bool
isEven n = n `mod` 2 == 0

whatGrade :: Int -> String
whatGrade mark
    | (mark >= 85) && (mark <= 100) = "Higher Distinction"
    | (mark >= 75) && (mark < 85) = "Distinction"
    | (mark >= 65) && (mark < 75) = "Credit"
    | (mark >= 50) && (mark < 65) = "Pass"
    | (mark >= 0) && (mark < 50) = "Fail"
    | otherwise = "invalid mark"
~~~

### Where
~~~haskell
batAvgRating :: Double -> Double -> String
batAvgRating hits atBats
    | avg <= 0.200 = "Terrible Batting Average"
    | avg <= 0.250 = "Average Player"
    | avg <= 0.280 = "Your doing pretty good"
    | otherwise = "You're a Superstar"
    where avg = hits / atBats
~~~

### (x:y)
~~~haskell
-- access list items by separating letters with : or get everything but the first item with xs
-- show: convert to String
getListItems :: [Int] -> String
getListItems [] = "Your list is empty"
getListItems (x:[]) = "Your list contains " ++ show x -- getListItems [1], returns "Your list contains 1"
getListItems (x:y:[]) = "Your list contains " ++ show x ++ " and " ++ show y -- getListItems [1,2], returns "Your list contains 1 and 2"
getListItems (x:xs) = "The first item is " ++ show x ++ " and the rest are " ++ show xs -- getListItems [1,2,3,4,5], returns "The first item is 1 and the rest are [2,3,4,5]"

-- all: whole list
getFirstChar :: String -> String
getFirstChar [] = "Empty String"
getFirstChar all@(x:xs) = "The first letter in " ++ all ++ " is " ++ [x]
~~~

### Higher Order Functions
~~~haskell
-- pass in function as if it is variable
times4 :: Int -> Int
times4 x = x * 4
listTimes4 = map times4 [1,2,3,4,5] -- map applies function to every elements in list

multBy4 :: [Int] -> [Int]
multBy4 [] = []
multBy4 (x:xs) = times4 x : multBy4 xs -- multBy4 [1,2,3,4], returns [4,8,12,16]
-- Takes the 1st value off the list x, multiplies it by 4 and stores it in the new list
-- xs is then passed back into multBy4 until there is nothing left of the list to process (Recursion)

-- Check if strings are equal with recursion
areStringsEq :: [Char] -> [Char] -> Bool
areStringsEq [] [] = True
areStringsEq (x:xs) (y:ys) = x == y && areStringsEq xs ys
areStringsEq _ _ = False
~~~

### Pass Functions in Functions
~~~haskell
times4 :: Int -> Int
times4 x = x * 4
doMult :: (Int -> Int) -> Int -- (Int -> Int): expect a function that receives an Int and returns an Int
doMult func = func 3 -- receive the function and pass 3 into it
-- num3Times4 :: Int
num3Times4 = doMult times4 -- pass in the function that multiplies by 4
~~~

~~~haskell
getAddFunc :: Int -> (Int -> Int)
getAddFunc x y = x + y
-- adds3 :: Int -> Int
adds3 = getAddFunc 3
-- fourPlus3 :: Int
fourPlus3 = adds3 4
-- threePlusList :: [Int]
threePlusList = map adds3 [1,2,3,4,5]
~~~

### Lambda
~~~haskell
-- \ represents lambda then you have the arguments -> and result
dbl1To10 = map (\x -> x * 2) [1..10] -- [2,4,6,8,10,12,14,16,18,20]
~~~

### If Statement
`if` always needs `else`
~~~haskell
doubleEvenNumber y =
    if (y `mod` 2 /= 0)
        then y
        else y * 2
~~~

### Case Statement
~~~haskell
getClass :: Int -> String
getClass n = case n of
    5 -> "Go to Kindergarten"
    6 -> "Go to elementary school"
    _ -> "Go some place else"
~~~

### Module
~~~haskell
module SampleFuncs (getClass, doubleEvenNumber) where

import SampleFuncs
~~~

## Type
type name starts with upper case letter

### Enumeration Type
~~~haskell
-- used when you want a list of possible types
-- provide name, a list and then Show converts into a String for printing
data BaseballPlayer = Pitcher
                    | Catcher
                    | Infield
                    | Outfield
                deriving Show -- convert values to String

barryBonds :: BaseballPlayer -> Bool
barryBonds Outfield = True

barryInOF = print(barryBonds Outfield)
~~~

### Custom Type
~~~haskell
-- store multiple values sort of like a struct to create custom types
data Customer = Customer String String Double
    deriving Show

-- define Customer and its values
tomSmith :: Customer
tomSmith = Customer "Tom Smith" "123 Main St" 20.50

-- define how to find the right customer (By Customer) and the return value
getBalance :: Customer -> Double
getBalance (Customer _ _ b) = b -- getBalance tomSmith, returns 20.5
~~~

~~~haskell
data RPS = Rock
        | Paper
        | Scissors

shoot :: RPS -> RPS -> String
shoot Paper Rock = "Paper Beats Rock"
shoot Rock Scissors = "Rock Beats Scissors"
shoot Scissors Paper = "Scissors Beat Paper"
shoot Scissors Rock = "Scissors Loses to Rock"
shoot Paper Scissors = "Paper Loses to Scissors"
shoot Rock Paper = "Rock Loses to Paper"
shoot _ _ = "Error"
~~~
define 2 versions of a type
~~~haskell
-- first 2 floats are center coordinates and then radius for Circle
-- first 2 floats are for upper left hand corner and bottom right hand corner for the Rectangle
data Shape = Circle Float Float Float | Rectangle Float Float Float Float
    deriving (Show)

-- :t Circle = Float -> Float -> Float -> Shape

-- Create a function to calculate area of shapes
area :: Shape -> Float
area (Circle _ _ r) = pi * r ^ 2
area (Rectangle x y x2 y2) = (abs (x2 - x)) * (abs (y2 - y))
area (Rectangle x y x2 y2) = (abs $ x2 - x) * (abs $ y2 - y) -- same as above
areaOfCircle = area (Circle 50 60 20)
areaOfRectangle = area $ Rectangle 10 10 100 100
~~~

### Type Classes
type classes
* `Num`
* `Eq` testing for equality
* `Ord` used for totally ordered datatype
* `Show` return a function that prepends the output String to an existing String
~~~haskell
-- create an Employee and add the ability to check if they are equal
data Employee = Employee { name :: String,
                        position :: String,
                        idNum :: Int
                        } deriving (Eq, Show)

samSmith = Employee {name = "Sam Smith", position = "Manager", idNum = 1000}
pamMarx = Employee {name = "Pam Marx", position = "Sales", idNum = 1001}

isSamPam = samSmith == pamMarx -- Eq, returns False
samSmithData = show samSmith -- Show, "Employee {name = \"Sam Smith\", position = \"Manager\", idNum = 1000}"
~~~

### Type Instance
~~~haskell
-- Make a type instance of the typeclass Eq and Show
data ShirtSize = S | M | L

instance Eq ShirtSize where
    S == S = True
    M == M = True
    L == L = True
    _ == _ = False

instance Show ShirtSize where
    show S = "Small"
    show M = "Medium"
    show L = "Large"

smallAvail = S `elem` [S, M, L] -- Check if S is in the list
theSize = show S -- Get string value for ShirtSize
~~~

custom Type Classes
~~~haskell
-- a represents any type that implements the function areEqual
class MyEq a where
    areEqual :: a -> a -> Bool

data ShirtSize = S | M | L

-- allow Bools to check for equality using areEqual
instance MyEq ShirtSize where
    areEqual S S = True
    areEqual M M = True
    areEqual L L = True
    areEqual _ _ = False

newSize = areEqual M M
~~~

###  I/O
~~~haskell
-- do: chain different things together
sayHello = do
    putStrLn "What's your name: "
    name <- getLine
    putStrLn $ "Hello " ++ name

-- File IO
-- Write to a file
writeToFile = do
	theFile <- openFile "test.txt" WriteMode -- open the file using WriteMode
	hPutStrLn theFile ("Random line of text") -- put the text in the file
	hClose theFile -- close the file

readFromFile = do
	theFile2 <- openFile "test.txt" ReadMode -- open the file using ReadMode
	contents <- hGetContents theFile2 -- get file contents
	putStr contents
	hClose theFile2 -- close the file
~~~
