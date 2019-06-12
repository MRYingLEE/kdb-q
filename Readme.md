
# This is Q Practice
## Philosophy
### 1. Interpreted. 
Q is interpreted, not compiled. The interpreter's eval and parse routines are exposed so that you can dynamically generate code in a controlled manner.
### 2.Types.  
Q is a dynamically typed language, in which type checking is mostly unobtrusive. 
### 3. Evaluation Order
While q is entered left-to-right, expressions are evaluated right-to-left or, as the q gods prefer, left of right – meaning that a function is applied to the argument on its right.
### 4. Null and Infinity Values 
In q, null values are typed and take the same space as non-nulls. Numeric types also have infinity values. Infinite and null values can participate in arithmetic and other operations with (mostly) predictable results.
### 5.Integrated I/O
 I/O is done through function handles that act as windows to the outside world. Once such a handle is initialized, passing a value to the handle is a write.
 
 ### 6. Table Oriented 
q has tables as first class entities.A table can be viewed as a list of record dictionaries.
### 7. Ordered Lists 
In q, data structures are based on ordered lists, so time series maintain the order in which they are created. Moreover, simple lists occupy contiguous storage, so processing big data is fast. Very fast.
### 8. Column Oriented 
Q tables are column lists in contiguous storage and operations apply on entire columns.
### 9. In-Memory Database 
One can think of kdb+ as an in-memory database with persistent backing. 
## Variable (Assignable)
":" instead of "="
"%" instead of "/" for division
```q
a:9 / intended to be a comment
```
```q
b:2+a:8 / calculate from RIGHT to LEFT, in other words, b<=(2+a=<8) 
b
```
    10

```q
a
```
    8

```q
long:121312412 / long
long
```
    121312412

```q
float:8798.2121 / Aften known as "DOUBLE" in many languages
float
```
    8798.212

```q
bool:199=200 / 
bool /1b for true, 0b for false
```
    0b

```q
date:2000.01.01 / this is actually 0, as base point
date+2
```
    2000.01.03

```q
time:12:00:00.000000000 / this is noon
time
```
    0D12:00:00.000000000


```q
symbol:`Similar_to_string_in_other_language
symbol
```
    `Similar_to_string_in_other_language

```q
list_of_int:(1; 2; 3; 4)
list_of_int
```
    1 2 3 4

```q
list_by_til: til 9
list_by_til
```
    0 1 2 3 4 5 6 7 8

```q
list_of_symbols:(`zero; `one; `two; `three)
list_of_symbols
```
    `zero`one`two`three

```q
list_op: 1+2* til 8
list_op
```
    1 3 5 7 9 11 13 15

```q
list_join: 1 2 , 3, 4 5 / join by ","
list_join
2#list_join / get the first 2
-2#list_join / get the last 2. Positive argument means take from the front, negative from the back.
0#list_join / get an empty list of the same type as the first item
```
    1 2 3 4 5

    1 2

    4 5

    `long$()

```q
/ Some tricks
5#8 / 8 8 8 8 8
6#1 2 3 / 1 2 3 1 2 3
list_join[0] / 0 based list
```
    8 8 8 8 8

    1 2 3 1 2 3

    1

## Functions- first class value
correspond to "lambda expressions" or "anonymous functions" in other languages.
```q
func:{[x;y;z] a:x*x; b:y;c:z; a-b+c} / to define a function, x;y are formal parameters
func2:{a:x*x; b:y;c:z; a-b+c} / The same but with the implicit parameters x, y, and z in that order.
func[10;8;5] / instead of func(10,8)
func2[10;8;5] / the same as the above
```
    87

    87

### Functions on List, similar to Python
```q
1+ til 10
1= til 10
1<til 10
1+/til 10 / = 1 + sum(0..9)
(+/) til 10 / Accumulate
(+/) 1+ til 10  / Accumulate
(*/) til 10 / Production
(*/) 1+ til 10 / Production
```
    1 2 3 4 5 6 7 8 9 10

    0100000000b

    0011111111b

    46

    45

    55

    0

    3628800

```q
0 {x+y}/ 1 2 3 4 5
0 {x+y}/ 1+til 10
```
    15

    55

|, which returns the larger of its operands
&, which returns the smaller of its operands.
```q
10|8
10&8
(|/) til 10
(&/) til 10
(*/) 3#10
sum 1+til 10 / this is +/
prd 1+til 10 / this is */ -- note missing "0"
max 20 10 40 30 / this is |/
min 20 10 40 30 / this is &/
```
    10

    8

    9

    0

    1000

    55

    3628800

    40

    10

#### Fibonacci Numbers (Example)
```q
F0:1 2
show 2#F0
sum -2#F0
F0,sum -2#F0
5 {x,sum -2#x}/ 1 1
```
    1 2

    3

    1 2 3

    1 1 2 3 5 8 13

#### Trading Example
```q
buys:2 1 4 3 5 4f
sells:2 4 3 2
deltas [buys]
sums [buys]
sums[sells] 
sums[sells] &\: sums[buys] / That adverb is called each left and it has the funky notation \:
```
    2 -1 3 -1 2 -1f

    2 3 7 10 15 19f

    2 6 9 11

    2 2 2 2  2  2 

    2 3 6 6  6  6 

    2 3 7 9  9  9 

    2 3 7 10 11 11

```q
count (til 6; 10 20 30) 
count each (til 6; 10 20 30) / count each
```
    2

    6 3

```q
deltas sums[sells] &\: sums[buys]
```
    2 2 2 2 2 2

    0 1 4 4 4 4

    0 0 1 3 3 3

    0 0 0 1 2 2

```q
deltas each deltas sums[sells] &\: sums[buys]
```
    2 0 0 0 0 0

    0 1 3 0 0 0

    0 0 1 2 0 0

    0 0 0 1 1 0

### Dictionaries (Key List - Value List) 
```q
```
```q
d:`a`b`c`d!til 4
d
d[`d]
```
    a| 0

    b| 1

    c| 2

    d| 3

    3

```q
dc:`c1`c2!(10 * til 5; 1+ til 3)
dc
dc[`c2]
dc[`c2;2]
dc[;1]
trans:flip `c1`c2!(10 20 30; 1.1 2.2 3.3)
trans
trans[0;`c1]
```
    c1| 0 10 20 30 40

    c2| 1 2 3

    1 2 3

    3

    c1| 10

    c2| 2

    c1 c2 

    ------

    10 1.1

    20 2.2

    30 3.3

    10

### Table-- A flipped column dictionary
A table is a flipped column dictionary.
It is also a list of record dictionaries.
The column names in table definition are not symbols, although they are converted to symbols under the covers.
```q
table1:([] c1:10 20 30; c2:1.1 2.2 3.3) 
 / [] are necessary to differentiate a table from a list
 / vs dictionary `c1`c2!(10 20 30; 1.1 2.2 3.3) 
table1
```
    c1 c2 

    ------

    10 1.1

    20 2.2

    30 3.3

### Q-SQL
it is not evaluated right-to-left. Rather, it is syntactic sugar designed to mimic SQL SELECT.
Whereas SQL SELECT operates on fields on a row-by-row basis, select performs vector operations on column lists.
```q
table2:([] c1:1000+til 6; c2:`a`b`c`a`b`a; c3:10*1+til 6)
table2
```
    c1   c2 c3

    ----------

    1000 a  10

    1001 b  20

    1002 c  30

    1003 a  40

    1004 b  50

    1005 a  60

```q
select c1, val:2*c3 from table2 / : To define a new column
```
    c1   val

    --------

    1000 20 

    1001 40 

    1002 60 

    1003 80 

    1004 100

    1005 120

```q
select count_c1: count c1, sum_c3: sum c3 by c2 from table2
```
    c2| count_c1 sum_c3

    --| ---------------

    a | 3        110   

    b | 2        70    

    c | 1        30    

```q
select count c2 by ovrund:c3<=40 from table2
```
    ovrund| c2

    ------| --

    0     | 2 

    1     | 4 

```q
update c3:10*c3 from table2 where c2=`a / Here ":" means assignment
 / update are vector operations on columns, not row-by-row.
table2
```
    c1   c2 c3 

    -----------

    1000 a  100

    1001 b  20 

    1002 c  30 

    1003 a  400

    1004 b  50 

    1005 a  600

    c1   c2 c3

    ----------

    1000 a  10

    1001 b  20

    1002 c  30

    1003 a  40

    1004 b  50

    1005 a  60

```q
`c2 xasc table2 / To sord ASC by `c2
```
    c1   c2 c3

    ----------

    1000 a  10

    1003 a  40

    1005 a  60

    1001 b  20

    1004 b  50

    1002 c  30

#### Random Data Generator
```q
10?20 /  first 20 integers starting at 0 (i.e., not including 20).
10?`aapl`ibm /  10 random selections from the items in a list
```
    12 8 10 1 9 11 5 6 1 5

    `aapl`aapl`ibm`ibm`aapl`ibm`aapl`ibm`aapl`ibm

```q
5 xbar til 15 /The left operand of xbar is an interval width, and the right operand is a list of numeric values.
```
    0 0 0 0 0 5 5 5 5 5 10 10 10 10 10

```q
1 2 3 wavg 50 60 70
```
    63.33333

#### string , a list of char
string is not an atom or even a first class entity in q; it is a list having count 6. And it should not be confused with its symbol cousin `string, which is an atom having count 1.
```q
("s";"t"; "r"; "i"; "n"; "g")
count "string"
count `string
```
    "string"

    6

    1

#### File IO
A symbolic file handle is a symbol of a particular form that represents the name of a resource on the file system. The leading ':' distinguishes the symbol as a handle.
```q
t:([] c1:`a`b`c; c2:1.1 2.2 3.3)
`:/q/save/t set t / write 
t2: get `:/q/save/t /read
t2
```
    `:/q/save/t

    c1 c2 

    ------

    a  1.1

    b  2.2

    c  3.3

```q
`:/q/life.txt 0: ("Meaning";"of";"life") 
/ write text to file
txt:read0 `:/q/life.txt / read text from file
txt
```
    `:/q/life.txt

    "Meaning"

    "of"

    "life"

```q
`:/q/examples/t.csv 0: csv 0: t
t2: read0 `:/q/examples/t.csv
t2
```
    `:/q/examples/t.csv

    "c1,c2"

    "a,1.1"

    "b,2.2"

    "c,3.3"

```q
t3: ("SF"; enlist ",") 0: `:/q/examples/t.csv / S: sysmbol, F: float
t3
```
    c1 c2 

    ------

    a  1.1

    b  2.2

    c  3.3

### Interprocess Communication
q)\p 5060 / on server
q)f:{x*y} / on server
echo:{show x} / on server
rsvp:{[arg;cb] show arg; (neg .z.w) (cb; 43); show `done;} 
```q
h:hopen `:localhost:5060 / on client
h "2*34"
h (`f; 6; 7) / on client
```
    evaluation error:
    
    hop. OS reports: No connection could be made because the target machine actively refused it.
    
      [0]  h:hopen `:localhost:5060 / on client

             ^

    
```q
echo:{show x} / Client
```
```q
(neg h) (`echo; 42) / on client
```
    evaluation error:
    
    h
    
      [0]  (neg h) (`echo; 42) / on client

                ^

    
```q
(neg h) (`rsvp; 42; `echo) / on client
```
    evaluation error:
    
    h
    
      [0]  (neg h) (`rsvp; 42; `echo) / on client

                ^

    
## Data Types: Atoms
### Datatype
![alt text](datatype2.jpg "comparation")
### Datatype Comparision
![alt text](datatype1.jpg "comparation")
```q
long: 4 / 8 bytes
long: 4j / not necessary, the same as the above
int: 4i / 4 bytes
short: 4h / 2 bytes
float: 3.14 / 8 bytes
float: 6e23 /scientific notation
real: 1.23e / 4 bytes
/P 10 / specify a display width of 10
1%3
bool: 0b / 0b for false and 1b for true , 1 byte
byte: 0x12a / leading with 0x
guid: 1?0Ng / GUID 16 bytes.
 / he positive case uses the same initial seed in each new q session 
 / whereas the negative case uses a random seed. The former is useful
 / for reproducible results during testing but only the latter should
 / be used in production; otherwise, your "GUIDs" will not be unique 
 / across q sessions.
char: "q" / 1 byte char
char: "\"" / quote
char:"\\" / back-slash
char:"\n" / newline
char:"\r" / return
char:"\t" / horizontal tab
`$"This is a symbol`" / A symbol with special characters
```
    0.3333333

    `This is a symbol`

```q
day: 2019.01.03 / Leading zeroes in months and days are required; their omission causes an error.
`int$2000.02.01
time:10:01:10.101 / hh:mm:ss.uuu , 4 bytes integer actually. leading 0s are required.
`int$10:01:10.101 / in miliseconds
timespan:12:34:56.123456789 / 0Dhh:mm:ss.nnnnnnnnn, in nanoseconds
`long$12:34:56.123456789 / 8 bytes integer actually
datetime: 2000.01.01T12:00:00.000 / A datetime (deprecated) is the lexical combination
 / of a date and a time, separated by T as in the ISO standard format. 
timestamp: 2014.11.22D17:43:40.123456789
`long$2014.11.22D17:43:40.123456789 / long, 8 bytes actually
datepart: `date$2014.11.22D17:43:40.123456789
timespan_part: `timespan$2014.11.22D17:43:40.123456789
month: 2015.11m
2001.01m=12  / 4 bytes integer actually, m is required.
`int$2015.01m 
2015.07m=2015.07.01 / true
minutes: 12:20 
12:00=12*60 
`int$12:00
second: 23:59:59 / 4 bytes
`int$12:34:56.000
datetime: 2000.01.02T03:04:05.000 
`year$datetime
`mm$datetime
`dd$datetime
time:12:34:56.789
time.hh
time.mm
time.ss
```
    31i

    36070101i

    45296123456789

    469993420123456789

    1b

    180i

    1b

    1b

    720i

    45296000i

    2000i

    1i

    2i

    12i

    34i

    56i

#### Arithmetic Infinities and Nulls
Literal	Value
0w	Positive float infinity
-0w	Negative float infinity
0n	Null float ; NaN, or not a number
0W	Positive long infinity
-0W	Negative long infinity
0N	Null long
****************************************
0N	MIN_INT	-9223372036854775808
-0W	MIN_INT+1	-9223372036854775807
0W	MAX_INT	+9223372036854775807
****************************************
##### The nulls to be handled
type	null
boolean*	0b
guid*	0Ng (00000000-0000-0000-0000-000000000000)
byte*	0x00
short	0Nh
Int	0N
long	0Nj
real	0Ne
float	0n
char*	" "
sym	`
timestamp	0Np
month	0Nm
date	0Nd
datetime	0Nz
timespan	0Nn
minute	0Nu
second	0Nv
time	0Nt
The binary types have no null values. There is no room since every bit pattern is a legitimate value.
```q
null `
null " "
null ""  / "" is not null, it's an empty list
```
    1b

    1b

    `boolean$()

# 3. Lists
All data structures in q are ultimately built from lists: a dictionary is a pair of lists; a table is a special dictionary; a keyed table is a pair of tables.
It may be instructive to think of q lists as dynamically allocated arrays.
```q
100 200 300~(100; 200 ; 300) / q verifies by testing for identity using ~.
```
    1b

```q
enlist 88 / we cannot use ,88 or (,88)
L:100 200 300 400
L[0] / 0 based index
L[1]:42 / assign to an element
L[] / the entire list
-3!L[()] / -3! is an utility
L[::] / The syntactic form double-colon :: denotes the nil item
/ , which allows explicit notation or programmatic generation of an empty index.
```
    ,88

    100

    100 42 300 400

    "()"

    100 42 300 400

```q
1 2 3,4 5 / join
x:1
(),x  /join
x,() /join
L1:10 0N 30
L2:100 200 0N
L1^L2 / 100 200 30 the right item prevails over the corresponding left item except when the right item is null.
```
    1 2 3 4 5

    ,1

    ,1

    100 200 30

```q
L:100 200 300 400
L[0 2] /same to L 0 2
I:0 2 
L[I] / same to L I
L[(0 1; 2 3)]
L[1 2 3]:2000 3000 4000 // Assign one by one
L ::
L
1001 1002 1003?1003 1001 / ? is overloaded to find
```
    100 300

    100 300

    100 200

    300 400

    100 2000 3000 4000

    100 2000 3000 4000

    2 0

```q
m:(1 2 3 4; 100 200 300 400; 1000 2000 3000 4000)
m
m[1;] / 100 200 300 400
m[;3] / 4 400 4000
m[1;]~m[1]
```
    1    2    3    4   

    100  200  300  400 

    1000 2000 3000 4000

    100 200 300 400

    4 400 4000

    1b

```q
distinct 1 2 3 2 3 4 6 4 3 5 6 / 1 2 3 4 6 5
where 101010b / 0 2 4
L:10 20 30 40 50
L[where L>20] / similar to python
L[where L>20]:45
L
group "i miss mississippi"
group 1 2 3 2 3 4 6 4 3 5 6
```
    1 2 3 4 6 5

    0 2 4

    30 40 50


    10 20 45 45 45

    i| 0 3 8 11 14 17

     | 1 6

    m| 2 7

    s| 4 5 9 10 12 13

    p| 15 16

    1| ,0

    2| 1 3

    3| 2 4 8

    4| 5 7

    6| 6 10

    5| ,9

# 4. Operators
operator, function, and verb – interchangeably.
Operators are built-in functions used with in-fix notation. 
Expressions are evaluated right-to-left
```q
1 2 3+10 20 30 /11 22 33, similar to Python
100+1 2 3 / 101 102 103
2*3+4 / 14
(2*3)+4 /10
4+2*3 /10
```
    11 22 33

    101 102 103

    14

    10

    10

```q
(())~enlist ()
(1; 2 3 4)~(1; (2; 3; 4))
(1 2;3 4)~(1;2 3 4)
```
    0b

    1b

    0b

```q
not 0b
not 1b
not 42
not 0
not 0xff
not 98.6
2 1 3=1 2 3
10 20 30<=30 20 10
2=1 2 3
"zaphod"="Arthur"
`a`b`a`d=`a`d`a`b
```
    1b

    0b

    0b

    1b

    0b

    0b

    001b

    110b

    010b

    000100b

    1010b

## Division % instead of /
```q
a=1
neg a /-1. -a is invalid
```
    0b

    -8

#### Maximum | and Minimum &
```q
42|43
98.6&101.9
0b|1b / similar to or
0b&1b / similar to and
11010101b&01100101b /item-wise
2|0 1 2 3 4 /item-wise
"zaphod"|"arthur"
```
    43

    98.6

    1b

    0b

    01000101b

    2 2 2 3 4

    "zrthur"

```q
a:43
a-:1 / simiar to a-=1 in c
a
a&:21 / simiar to a=a & 21
a
```
    42

    21

```q
L1:(1 2 3; 10 20 30)
L1[;2]+:100
L1
L:1 2 3
L,:4 / append
L
L,:100 200 /append
L
```
    1  2  103

    10 20 130

    1 2 3 4

    1 2 3 4 100 200

```q
sqrt 3
exp 3
log 3
2 xexp 5 / power
2 xexp .5 /power 0,5, actually sqrt
2 xlog 32
```
    1.732051

    20.08554

    1.098612

    32f

    1.414214

    5f

```q
/ Many languages use % for modulus, but it is division in q
7 div 2 / 3, rounded down
7 div 2.5 /2 , rounded down
-7 div 2 /-4, rounded down
7 mod 2 / 3, rounded down. mod = dividend – (dividend div divisor)
7 mod 2.5 /2 , rounded down
-7 mod 2 /-4, rounded down
```
    3

    2

    -4

    1

    2f

    1

```q
signum 42 /1i
signum -42.0 /-1i
signum 0 /0i
signum 1999.12.31
signum 12:00:00.000000000
reciprocal 0.02380952 / 42.00001
reciprocal 0.0 / 0w
reciprocal -0.0 / -0w
floor 4.2 /4
floor -4.2 / -5
ceiling 4.2 /5
ceiling 4 /4
ceiling -4.2 /-4
abs -42 /42
```
    1i

    -1i

    0i

    -1i

    1i

    42.00001

    0w

    -0w

    4

    -5

    5

    4

    -4

    42

```q
2000.01.01<2000.01.01D12:00:00.000000000
2015.01.01+til 31 / all days in January
2001.01.01-2000.01.01 /366i
2015.06m-2015.01m /5i
```
    1b

    2015.01.01 2015.01.02 2015.01.03 2015.01.04 2015.01.05 2015.01.06 2015.01.07 ..

    366i

    5i

### Alias ::
An alias is a variable that is an expression – i.e., it is not the result of expression evaluation but the expression itself.
Evaluation of an alias is lazy, meaning that it occurs only when necessary. More precisely, evaluation is forced when the variable is referenced, at which point a determination is made whether the expression needs to be (re)evaluated
```q
a:42
b::a
c:a
a:43
b / 43
c /42
```
    43

    42

```q
w::(0N!x*x)+y*y
x:3
y:4
w  / 9 25
w / 25
y:6
w / 9 45
.z.b / system dictionary, which is also obtainable via the command \b.
```
    9
    9
    
    25

    25

    45

    a| b

    x| w

    y| w

### Views
Aliasing is commonly used to provide a database view by specifying a query as the expression.
```q
t:([]c1:`a`b`c`a;c2:20 15 10 20;c3:99.5 99.45 99.42 99.4)
v::select sym:c1,px:c3 from t where c1=`a
v
update c3:42.0 from `t where c1=`a
v
```
    sym px  

    --------

    a   99.5

    a   99.4

    `t

    sym px

    ------

    a   42

    a   42

```q
.z.b
```
    a| b

    x| w

    y| w

    t| v

# 5. Dictionaries
A dictionary is a mapping defined by an explicit association between a key list and value list. 
The two lists must have the same count and the key list should be a unique collection.
```q
d:`a`b`c!100 200 300
key d
value d
count d
d[`a]
d[`a`c]
ks:`a`c
d ks
```
    `a`b`c

    100 200 300

    3

    100

    100 300

    100 300

```q
d:`a`b`c`a!10 20 30 10
d?10 /`a
d1:0 100 500000!10 20 30
d2:0 99 1000000!100 200 300
d1+d2
d[`b]:42 / update
d[`x]:100 / insert, upsert semantics.
d
`a`c#d
(enlist `c)#d
```
    `a

    0      | 110

    100    | 20

    500000 | 30

    99     | 200

    1000000| 300

    a| 10

    b| 42

    c| 30

    a| 10

    x| 100

    a| 10

    c| 30

    c| 30

```q
d:`a`b`c!10 20 30
`a`b _ d / remove `a and `b. A space before _ is required.
`a`b cut d / the same as the above
-3!`a`b`c _ d /"(`symbol$())!`long$()"
d
```
    c| 30

    c| 30

    "(`symbol$())!`long$()"

    a| 10

    b| 20

    c| 30

```q
/ Similar to Python
d:`a`b`c!10 20 30
neg d
2*d
f:{x*x}
f d
d1:`a`b`c!1 2 3
d2:`a`b`c!10 20 30
d1+d2
d1,d2 / right has the preference
d1^d2 / Similar to the above, but can deal with null
```
    a| -10

    b| -20

    c| -30

    a| 20

    b| 40

    c| 60

    a| 100

    b| 400

    c| 900

    a| 11

    b| 22

    c| 33

    a| 10

    b| 20

    c| 30

    a| 10

    b| 20

    c| 30

```q
dc1:(enlist `c)!enlist 10 20 30 / Not = `c!1 2 3
dc1 / c| 10 20 30
```
    c| 10 20 30

# 6. Functions
```q
a:2*3;6*7 
a / trick here
```

    42

    6

```q
{[x] 2*x} 42 / 84,= {[x] 2*x} [42]
f:{[x] x*x} / = f:{x*x} / implicit x
f 5 / 25, = f[5]
.my.name.space.f:{2*x}
`.my.name.space.f[5]
```
    84

    25

    10

```q
/ Similar to lambda
powers:({1}; {x}; {x*x}; {x*x*x})
selected:2
powers[selected] /{x*x}
```
    {x*x}

```q
::[`a`b`c]
```
    `a`b`c

```q
apply:{x y}
sq:{x*x}
apply[sq; 5]
a:42
get `a / 42 / get, which returns the value of the global variable whose name is passed.
`a set 43
a
```
    25

    42

    `a

    43

```q
f:{[p1] a:42; helper:{[a; p2] a*p2}[a;]; helper p1}
f[10]
a:42
f:{a:98.6; `a set x}
f 43
a
```
    420

    `a

    43

```q
f:{x-y}
g:f[42;] /{x-y}[42;] / Partial function of Python
g[6]
```
    36

```q
10 20 30 40[100] / 0N / out of bound
`a`b`c[-1]
(1.1; 1; `1)[3] / 0n
```
    0N

    `

    0n

```q
neg (1 2 3; 4 5)
("abc"; "uv"),'("de"; "xyz") / zip
("abc"; "de"; enlist "f") ,\: ">" /each left
"</" ,/: ("abc"; "de"; enlist "f") /each right
1 2 3,/:\:10 20 / cross product
raze 1 2 3,/:\:10 20 / cross product
1 2 3 cross 10 20 / cross product
```
    -1 -2 -3

    -4 -5

    "abcde"

    "uvxyz"

    "abc>"

    "de>"

    "f>"

    "</abc"

    "</de"

    "</f"

    1 10 1 20

    2 10 2 20

    3 10 3 20

    1 10

    1 20

    2 10

    2 20

    3 10

    3 20

    1 10

    1 20

    2 10

    2 20

    3 10

    3 20

 ### Over (/) for Accumulation
```q
0 +/ 1 2 3 4 5 6 7 8 9 10 /55 / Over (/) for Accumulation¶
addi:{0N!(x;y); x+y}
0 addi/ 1 2 3 4 5 /15 
(*/) 1 2 3 4 5 6 7 8 9 10 / product 3628800
(|/) 7 8 4 3 10 2 1 9 5 6 / maximum 10
(&/) 7 8 4 3 10 2 1 9 5 6 / minimum 1
```
    0 1
    1 2
    3 3
    6 4
    10 5
    
    55

    15

    3628800

    10

    1

```q
fib:{x,sum -2#x}
10 fib/ 1 2 / run 10 times
fib/[10; 1 2] / same as the above
fib/[{1000>last x}; 1 1] / loop until 
```
    1 2 3 5 8 13 21 34 55 89 144 233

    1 2 3 5 8 13 21 34 55 89 144 233

    1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597

#### Scan \
The scan adverb \ is a higher-order function that behaves just like / except that it returns all the intermediate accumulations instead of just the final one.
```q
f:{2*x}
0+\1 2 3 4 5 6 7 8 9 10 / 1 3 6 10 15 21 28 36 45 55
(*\)1 2 3 4 5 6 7 8 9 10 / 
(|\)7 8 4 3 10 2 1 9 5 6 /
(&\)7 8 4 3 10 2 1 9 5 6 /
3 f\1 2 3 4 5 6 7 8 9 10 /
(f\)[{1000>last x};1 2 3 4 5 6 7 8 9 10] / 
```
    1 3 6 10 15 21 28 36 45 55

    1 2 6 24 120 720 5040 40320 362880 3628800

    7 8 8 8 10 10 10 10 10 10

    7 7 4 3 3 2 1 1 1 1

    1 2  3  4  5  6  7  8  9  10

    2 4  6  8  10 12 14 16 18 20

    4 8  12 16 20 24 28 32 36 40

    8 16 24 32 40 48 56 64 72 80

    1   2   3   4   5   6   7   8    9    10  

    2   4   6   8   10  12  14  16   18   20  

    4   8   12  16  20  24  28  32   36   40  

    8   16  24  32  40  48  56  64   72   80  

    16  32  48  64  80  96  112 128  144  160 

    32  64  96  128 160 192 224 256  288  320 

    64  128 192 256 320 384 448 512  576  640 

    128 256 384 512 640 768 896 1024 1152 1280

### each-previous (':)
The adverb each-previous ': provides a declarative way to perform a dyadic operation on each item of a list with its predecessor.
During traversal of the list, the current item is the left operand and the previous item is the right operand.
```q
100 -': 100 99 101 102 101
(-':)100 99 101 102 101 / 100 -1 2 1 -1
deltas 100 99 101 102 101 / 100 -1 2 1 -1
(%':)100 99 101 102 101 / 100 0.98999999999999999 1.0202020202020201 1.0099009900990099 0.99019607843137258
ratios 100 99 101 102 101 /100 0.99 1.020202 1.009901 0.9901961
deltas0:{first[x] -': x}
deltas0 100 99 101 102 101 / 0 -1 2 1 -1
(~':) 1 1 1 2 2 3 4 5 5 5 6 6 / 011010001101b
not (~':) 1 1 1 2 2 3 4 5 5 5 6 6 / 100101110010b
differ 1 1 1 2 2 3 4 5 5 5 6 6 / 100101110010b
```
    0 -1 2 1 -1

    100 -1 2 1 -1

    100 -1 2 1 -1

    100 0.99 1.020202 1.009901 0.9901961

    100 0.99 1.020202 1.009901 0.9901961

    0 -1 2 1 -1

    011010001101b

    100101110010b

    100101110010b

```q
L:1 1 1 2 2 3 4 5 5 5 6 6
where differ L / 0 3 5 6 7 10
(where differ L) cut L / 1 1 1
```
    0 3 5 6 7 10

    1 1 1

    2 2

    ,3

    ,4

    5 5 5

    6 6

```q
L:9 8 7 11 10 12 13
(where -0W>':L) cut L 
/ 9 8 7
/ 11 10
/ ,12
/ ,13
(where 0W<':L) cut L
/,9
/,8
/7 11
/10 12 13
```
    9 8 7

    11 10

    ,12

    ,13

    ,9

    ,8

    7 11

    10 12 13

### Verb @
The higher-order function @ is the true form of basic application in q. It applies a monadic mapping to an argument. As with all built-in functions, it can be written infix or prefix.
```q
10 20 30 40@1 /20
L:10 20 30 40 /20
L@1 /20
@[L; 1] /4
count@L /4
@[count; L] /4
{x*x}@L /100 400 900 1600
d:`a`b`c!10 20 30
d@`a /10
@[d;`b] /20
```
    20

    20

    20

    4

    4

    100 400 900 1600

    10

    20

```q
t:([]c1:1 2 3; c2:`a`b`c)
t:([]c1:1 2 3; c2:`a`b`c)
t@1
/ c1| 2
/ c2| `b
d:`a`b`c!10 20 30
d@`b /20
kt:([k:`a`b`c] v:1.1 2.2 3.3)
kt@`c
/v| 3.3
```
    c1| 2

    c2| `b

    20

    v| 3.3

### Verb Dot .
he higher-order function . is the true form of multi-variate application in q. It applies a multi-variate mapping to multiple arguments and can be written infix or prefix.
The right argument of . must be a list.
Separate verb . from its operands by whitespace if they are names or literal constants, as proximity to dots used in name spacing and in decimal numbers can lead to confusion.
```q
L:(10 20 30; 40 50)
L[1][0] / 40
L[1; 0] / 40
L . 1 0 / 40
d:`a`b`c!(10 20 30; 40 50; enlist 60) 
d[`b][0] / 40
d[`b; 0] / 40
d . (`b; 0) / 40
g:{x+y}
g[1; 2] /3
g . 1 2 /3
```
    40

    40

    40

    40

    40

    40

    3

    3

```q
/ Similar to Python
m:(1 2 3;4 5 6)
m[0;] / 1 2 3
m . (0; ::) / 1 2 3
m . (::; 1) / 2 5
```
    1 2 3

    1 2 3

    2 5

### 6.8.4 General Apply (@) with Monadic Functions
@[L;I;f] / f: Monadic function
@[L; I; g; v] / g: Dynadic function
Effectively we reach inside (a copy of) the list, apply neg along the specified sub-domain, and return the modified list.
For compound data structures such as nested lists, tables and keyed tables, application of @ occurs along a sub-domain at the top level. We'll see how to reach down into data structure in the next section.
```q
d:`a`b`c!10 20 30
ks:`a`c
@[d; ks; neg]
/ a| -10
/ b| 20
/ c| -30
m:(10 20 30; 100 200 300; 1000 2000 3000)
@[m; 0 2; neg]
/ -10 -20 -30
/ 100 200 300
/ -1000 -2000 -3000
```
    a| -10

    b| 20

    c| -30

    -10   -20   -30  

    100   200   300  

    -1000 -2000 -3000

```q
L:10 20 30 40
@[L; 0; neg] / -10 20 30 40
L /10 20 30 40
@[`L; 0 ; neg] /`L
L /-10 20 30 40
/ Observe that the result of an application of pass-by-name is the passed symbolic name. 
/ This allows chaining of function applications that modify in place.
```
    -10 20 30 40

    10 20 30 40

    `L

    -10 20 30 40

```q
L:10 20 30 40
@[L; 0 1; +; 100 200] / 110 220 30 40
q)100 200+L@0 1 / 110 220
m:(10 20 30; 100 200 300; 1000 2000 3000)
@[m; 0 2; +; 1 2]
/ 11 21 31
/ 100 200 300
/ 1002 2002 3002
```
    110 220 30 40

    110 220

    11   21   31  

    100  200  300 

    1002 2002 3002

### 6.8.6 General Apply (.) for Monadic Functions
```q
m:(10 20 30; 100 200 300)
.[m; 0 1] / 20
d:`a`b`c!(10 20 30; 40 50; enlist 60)
.[d; (`a; 1)] /20
.[m; 0 1; neg] 
/ 10 -20 30
/ 100 200 300
.[d; (`a; 1); neg]
/a| 10 -20 30
/b| 40 50
/c| ,60
.[`d; (`a; 1); neg]
d
```
    20

    20

    10  -20 30 

    100 200 300

    a| 10 -20 30

    b| 40 50

    c| ,60

    `d

    a| 10 -20 30

    b| 40 50

    c| ,60

### 6.8.7  General Apply (.) for Dyadic Functions
.[L; I; g; v]
```q
m:(10 20 30; 100 200 300)
.[m; 0 1; +; 1]
 /10 21 30
/ 100 200 300
.[m; (::; 1); +; 1 2]
/ 10 21 30
/100 202 300
m
/ 10  20  30 
/ 100 200 300
.[`m; (::; 1); +; 1]
m
.[`m; (::; 1); :; 42]
`m
m
d:`a`b`c!(100 200 300; 400 500; enlist 600)
.[d; (`a; 1); +; 1]
.[d; (`a; ::); +; 1]
.[d; (::; 0); +; 1]
d
.[`d; (::; 0); :; 42]
d
```
    10  21  30 

    100 200 300

    10  21  30 

    100 202 300

    10  20  30 

    100 200 300

    `m

    10  21  30 

    100 201 300

    `m

    `m

    10  42 30 

    100 42 300

    a| 100 201 300

    b| 400 500

    c| ,600

    a| 101 201 301

    b| 400 500

    c| ,600

    a| 101 200 300

    b| 401 500

    c| ,601

    a| 100 200 300

    b| 400 500

    c| ,600

    `d

    a| 42 200 300

    b| 42 500

    c| ,42

# 7. Transforming Data
```q
type 42 / -7h
type 10 20 30 /7h
type 98.6 /-9h
type 1.1 2.2 3.3 /9h
type `a /-11h
type `a`b`c / 11h
type "z" / -10h
type "abc" /10h
/ The type of any general list is 0.
type (42h; 42i; 42j) / 0h
type (1 2 3; 10 20 30) /0h
type () /0h
/The type of any dictionary, including a keyed table, is 99h.
type (`a`b`c!10 20 30) / 99h
type ([k:`a`b`c] v:10 20 30) /99h
type ([] c1:`a`b`c; c2:10 20 30) /98h
```
    -7h

    7h

    -9h

    9h

    -11h

    11h

    -10h

    10h

    0h

    0h

    0h

    99h

    99h

    98h

```q
get `. / to list the global variables
```
    help           | {[gh;h;x]if[10=type u:gh[h]x;-2 u]}[{[h;x]$[i.isf x;h x;i.is..

    print          | {x y;}[{[f;x]embedPy[f;x]}[foreign]enlist]

    a              | 43

    b              | a

    long           | 4

    float          | 6e+023

    bool           | 0b

    date           | 2000.01.01

    time           | 12:34:56.789

    symbol         | `Similar_to_string_in_other_language

    list_of_int    | 1 2 3 4

    list_by_til    | 0 1 2 3 4 5 6 7 8

    list_of_symbols| `zero`one`two`three

    list_op        | 1 3 5 7 9 11 13 15

    list_join      | 1 2 3 4 5

    func           | {[x;y;z] a:x*x; b:y;c:z; a-b+c}

    func2          | {a:x*x; b:y;c:z; a-b+c}

    F0             | 1 2

    buys           | 2 1 4 3 5 4f

    sells          | 2 4 3 2

    d              | `a`b`c!(42 200 300;42 500;,42)

    dc             | `c1`c2!(0 10 20 30 40;1 2 3)

    ..

```q
value `. / to list the global variables
```
    help           | {[gh;h;x]if[10=type u:gh[h]x;-2 u]}[{[h;x]$[i.isf x;h x;i.is..

    print          | {x y;}[{[f;x]embedPy[f;x]}[foreign]enlist]

    a              | 43

    b              | a

    long           | 4

    float          | 6e+023

    bool           | 0b

    date           | 2000.01.01

    time           | 12:34:56.789

    symbol         | `Similar_to_string_in_other_language

    list_of_int    | 1 2 3 4

    list_by_til    | 0 1 2 3 4 5 6 7 8

    list_of_symbols| `zero`one`two`three

    list_op        | 1 3 5 7 9 11 13 15

    list_join      | 1 2 3 4 5

    func           | {[x;y;z] a:x*x; b:y;c:z; a-b+c}

    func2          | {a:x*x; b:y;c:z; a-b+c}

    F0             | 1 2

    buys           | 2 1 4 3 5 4f

    sells          | 2 4 3 2

    d              | `a`b`c!(42 200 300;42 500;,42)

    dc             | `c1`c2!(0 10 20 30 40;1 2 3)

    ..

#### 7.2.1 Casts that Widen
```q
7h$42i / int to long /42
6h$42 / long to int /42i
9h$42 / long to float /42f
// Same to the above 3
"j"$42i /42
"i"$42 /42i
"f"$42 /42f
// Same to the first 3
`int$42 /
`long$42i /
`float$42 /
```
    42

    42i

    42f

    42

    42i

    42f

    42i

    42

    42f

#### 7.2.2 Casts across Disparate Types
```q
`char$42 / "*"
`long$"\n" 
`date$0 / 2000.01.01
`int$2001.01.01 / millennium occurred on leap year
`long$12:00:00.0000000000 / 43200000000000
`timespan$0 /
```
    "*"

    10

    2000.01.01

    366i

    43200000000000

    0D00:00:00.000000000

#### 7.2.3 Casts that Narrow
```q
`long$12.345 / 12
`short$123456789 /32767h
```
    12

    32767h

### 7.3 Data to and from Text
```q
string 42 / "42"
string 4 /,"4"
string 42i /"42"
a:2.0
string a /,"2"
f:{x*x}
string f /"{x*x}"
string 1 2 3
/,"1"
/,"2"
/,"3"
string `Life`the`Universe`and`Everything
```
    "42"

    ,"4"

    "42"

    ,"2"

    "{x*x}"

    ,"1"

    ,"2"

    ,"3"

    "Life"

    "the"

    "Universe"

    "and"

    "Everything"

```q
/ 7.3.2 Creating Symbols from Strings
`$"abc" /`abc
`$"Hello World"
```
    `abc

    `Hello World

## 7.4 Creating Typed Empty Lists
```q
c1:`float$()
/c1,:42 /'type Error
c1:98.6 
c1
```
    98.6

## 7.5 Enumerations
```q
u:`g`aapl`msft`ibm
v:100?u / random generator
v
u2: distinct v
u2
k:u?v / enum to int
k 
u[k] /int to enum
```
    `aapl`msft`aapl`msft`ibm`ibm`g`g`msft`g`aapl`g`g`g`msft`msft`msft`aapl`aapl`i..

    `aapl`msft`ibm`g

    1 2 1 2 3 3 0 0 2 0 1 0 0 0 2 2 2 1 1 3 0 1 2 2 0 0 1 0 3 1 3 3 2 0 2 1 0 2 0..

    `aapl`msft`aapl`msft`ibm`ibm`g`g`msft`g`aapl`g`g`g`msft`msft`msft`aapl`aapl`i..

```q
`u$v / `u$`msft`aapl`aapl`ibm`ibm`aapl`g`g`g`ibm`g`msft`msft`aapl`msft..
ev:`u$v
`int$ev / 
```
    `u$`aapl`msft`aapl`msft`ibm`ibm`g`g`msft`g`aapl`g`g`g`msft`msft`msft`aapl`aap..

    1 2 1 2 3 3 0 0 2 0 1 0 0 0 2 2 2 1 1 3 0 1 2 2 0 0 1 0 3 1 3 3 2 0 2 1 0 2 0..

# 8. Tables
```q
t:flip `name`iq!(`Dent`Beeblebrox`Prefect;98 42 126)
t[;`iq] / 98 42 126
t[2;]
/ name| `Prefect
/ iq  | 126
t[2;`iq] /126
t[;`iq] /98 42 126
t[`iq] /98 42 126
t[`name`iq]
/ Dent Beeblebrox Prefect
/ 98   42         126
t.name / Not works inside function
t:([] name:`Dent`Beeblebrox`Prefect; iq:98 42 126) / Table define
t~flip `name`iq!(`Dent`Beeblebrox`Prefect;98 42 126) / the same
```
    98 42 126

    name| `Prefect

    iq  | 126

    126

    98 42 126

    98 42 126

    Dent Beeblebrox Prefect

    98   42         126    

    `Dent`Beeblebrox`Prefect

    1b

```q
cols t / columns / `name`iq
meta t /column define
/ The key column c of the result contains the column names.
/ The column t contains a symbol denoting the type char of the column.
/ The column f contains the domains of any foreign key or link columns.
/ The column a contains any attributes associated with the column.
tables `. / to list the names of tables in the context.
count t / Rows
value t[1] / a row
```
    `name`iq

    c   | t f a

    ----| -----

    name| s    

    iq  | j    

    `kt`t`t3`table1`table2`trans

    3

    `Beeblebrox

    42

## 8.4 Primary Keys and Keyed Tables
A keyed table is a dictionary mapping a table of key records to a table of value records. 
A keyed table is not a table – it is a dictionary and so has type 99h.
Keys should be unique but (sadly) this is not enforced. 
```q
kt:([eid:1001 1002 1003]
      name:`Dent`Beeblebrox`Prefect; iq:98 42 126)
kt
```
    eid | name       iq 

    ----| --------------

    1001| Dent       98 

    1002| Beeblebrox 42 

    1003| Prefect    126

```q
kt[1002]
/ name| `Beeblebrox
/ iq  | 42
kt[1002][`iq]
/42
kt[(enlist 1001;enlist 1002)]
/ name       iq
/ -------------
/ Dent       98
/ Beeblebrox 42
kt ([] eid:1001 1002) / the same
([] eid:1001 1002)#kt
/eid | name       iq
/----| -------------
/1001| Dent       98
/1002| Beeblebrox 42
```
    name| `Beeblebrox

    iq  | 42

    42

    name       iq

    -------------

    Dent       98

    Beeblebrox 42

    name       iq

    -------------

    Dent       98

    Beeblebrox 42

    eid | name       iq

    ----| -------------

    1001| Dent       98

    1002| Beeblebrox 42

```q
key kt
value kt
keys kt
cols kt
```
    eid 

    ----

    1001

    1002

    1003

    name       iq 

    --------------

    Dent       98 

    Beeblebrox 42 

    Prefect    126

    ,`eid

    `eid`name`iq

### 8.4.8 Tables vs. Keyed Tables
```q
t:([] eid:1001 1002 1003; name:`Dent`Beeblebrox`Prefect; iq:98 42 126)
`eid xkey t /1!t , the 1st col
/eid | name       iq 
/----| --------------
/1001| Dent       98 
/1002| Beeblebrox 42 
/1003| Prefect    126
kt:([eid:1001 1002 1003] name:`Dent`Beeblebrox`Prefect; iq:98 42 126)
kt
() xkey kt / or 0!t
/eid  name       iq 
/-------------------
/1001 Dent       98 
/1002 Beeblebrox 42 
/1003 Prefect    126
```
    eid | name       iq 

    ----| --------------

    1001| Dent       98 

    1002| Beeblebrox 42 

    1003| Prefect    126

    eid | name       iq 

    ----| --------------

    1001| Dent       98 

    1002| Beeblebrox 42 

    1003| Prefect    126

    eid  name       iq 

    -------------------

    1001 Dent       98 

    1002 Beeblebrox 42 

    1003 Prefect    126

```q
/ Key should be unique, but which is not guaranteed.
t:([] eid:1001 1002 1003 1001; name:`Dent`Beeblebrox`Prefect`Dup)
ktdup:`eid xkey t
ktdup
/eid | name
/----| ----------
/1001| Dent
/1002| Beeblebrox
/1003| Prefect
/1001| Dup
ktdup 1001
/name| Dent
select from ktdup where eid=1001
/eid | name
/----| ----
/1001| Dent
/1001| Dup
```
    eid | name      

    ----| ----------

    1001| Dent      

    1002| Beeblebrox

    1003| Prefect   

    1001| Dup       

    name| Dent

    eid | name

    ----| ----

    1001| Dent

    1001| Dup 

```q
/ Compount Key columns
ktc:([lname:`Dent`Beeblebrox`Prefect; fname:`Arthur`Zaphod`Ford]; iq:98 42 126)
ktc
ktc[`lname`fname!`Beeblebrox`Zaphod]
ktc[`Dent`Arthur]
```
    lname      fname | iq 

    -----------------| ---

    Dent       Arthur| 98 

    Beeblebrox Zaphod| 42 

    Prefect    Ford  | 126

    iq| 42

    iq| 98

## 8.5 Foreign Keys and Virtual Columns
A foreign key is one or more table columns whose values are defined as an enumeration over the key column(s) of a keyed table. As in the case of symbol enumeration `sym$, the enumeration restricts foreign key values to be in the list of primary key values.
```q
kt_fk:([eid:1001 1002 1003] name:`Dent`Beeblebrox`Prefect; iq:98 42 126)
kt
tdetails:([] eid:`kt_fk$1003 1001 1002 1001 1002 1001; sc:126 36 92 39 98 42)
tdetails
meta tdetails
fkeys tdetails
```
    eid | name       iq 

    ----| --------------

    1001| Dent       98 

    1002| Beeblebrox 42 

    1003| Prefect    126

    eid  sc 

    --------

    1003 126

    1001 36 

    1002 92 

    1001 39 

    1002 98 

    1001 42 

    c  | t f     a

    ---| ---------

    eid| j kt_fk  

    sc | j        

    eid| kt_fk

```q
select eid.name, sc from tdetails / similar to LINQ 
/ There is an implicit left join between tdetails and kt here.
```
    name       sc 

    --------------

    Prefect    126

    Dent       36 

    Beeblebrox 92 

    Dent       39 

    Beeblebrox 98 

    Dent       42 

```q
t:([] name:`Dent`Beeblebrox`Prefect; iq:98 42 126)
kt:([eid:1001 1002 1003] name:`Dent`Beeblebrox`Prefect; iq:98 42 126)
/ Append
t,:`name`iq!(`W; 26) / = t,:(`W; 26)
t,:`iq`name!(200; `Albert) / =t,:(`Albert;200;) 
first t
/name| `Dent
/iq  | 98
last t
2#t
-3#t
```
    name| `Dent

    iq  | 98

    name| `Albert

    iq  | 200

    name       iq

    -------------

    Dent       98

    Beeblebrox 42

    name    iq 

    -----------

    Prefect 126

    W       26 

    Albert  200

```q
t?`name`iq!(`Dent;98)
t?(`Dent;98)
t?((`Dent;98);(`Prefect;126))
kt?`name`iq!(`Dent;98)
t1?enlist each 1001 1002
/ Union with ,
t,`name`iq!(`Slaartibartfast; `123)
/ 8.6.5 Coalesce ^
/ The behavior of ^ is the same as , when there are no nulls in a column in the right table.
([k:`a`b`c] v:10 0N 30)^([k:`a`b`c] v:100 200 0N)
/The performance of ^ is slower than that of , since fields of the right operand must be checked for null.
/ Column Join
([] c1:`a`b`c),'([] c2:100 200 300)
([k:1 2 3] v1:10 20 30),'([k:3 4 5] v2:1000 2000 3000)
```
    0

    0

    0 2

    eid| 1001

    evaluation error:
    
    t1
    
      [0]  t1?enlist each 1001 1002

           ^

    
## 8.8 Attributes
Attributes (other than `g#) are descriptive rather than prescriptive. By this we mean that by applying an attribute you are asserting that the list has a special form, which q will check. It does not instruct q to (re)make the list into the special form; that is your job. A list operation that respects the form specified by the attribute leaves the attribute intact (other than `p#), while an operation that breaks the form results in the attribute being removed in the result.
The syntax for applying an attribute is (yet) another overload of #, whose left operand is a symbol specifying the attribute and whose right operand is the target list.
Sorted `s#
Unique `u#
Parted `p#
Grouped `g#
Remove Attribute `#
```q
`s#1 2 4 8 / `s#1 2 4 8
s#2 1 3 4 / 's-fail
```
    `s#1 2 4 8

    evaluation error:
    
    s
    
      [0]  s#2 1 3 4 / 's-fail

           ^

    
```q
asc 2 1 8 4 /`s#1 2 4 8
til 5 /0 1 2 3 4
/When a list with an attribute is amended with ,: 
/ the result is checked to see that the attribute is preserved; if not, it is removed.
L:`s#1 2 3 4 5
L,:6
L /`s#1 2 3 4 5 6
L,:0
L / 1 2 3 4 5 6 0
```
    `s#1 2 4 8

    0 1 2 3 4

    `s#1 2 3 4 5 6

    1 2 3 4 5 6 0

```q
`u#2 1 4 8 / `u#2 1 4 8
`u#2 1 4 8 2 /'u-fail
```
    `u#2 1 4 8

    evaluation error:
    
    u-fail
    
      [0]  `u#2 1 4 8 2 /'u-fail

             ^

    
```q
L:`u#2 1 4 8
L,:3
L /`u#2 1 4 8 3
L,:2
L /2 1 4 8 3 2
```
    `u#2 1 4 8 3

    2 1 4 8 3 2

```q
L:`p#1 1 2 3 3
L / `p#1 1 2 3 3
L,:3 / the parted attribute is not preserved under any operation on the list, 
     / even if the operation preserves the property.
L /1 1 2 3 3 3
```
    `p#1 1 2 3 3

    1 1 2 3 3 3

```q
/ The grouped attribute `g# differs from other attributes in that 
/ it can be applied to any list. It causes q to create and maintain an index
/ – essentially a hash table. Grouped can be applied to a list when no other
/ assumptions about its structure can be made.
`g#1 2 3 2 3 4 3 4 5 2 3 4 5 4 3 5 6
L:`g#100?100
L
L,:1 1 1 1
L
```
    `g#1 2 3 2 3 4 3 4 5 2 3 4 5 4 3 5 6

    `g#37 30 75 61 56 23 46 14 13 9 74 55 92 14 90 17 89 49 80 11 8 8 42 94 19 12..

    `g#37 30 75 61 56 23 46 14 13 9 74 55 92 14 90 17 89 49 80 11 8 8 42 94 19 12..

# 9. Queries: q-sql
Vs traditional SQL, there are some significant differences in the syntax and behavior.
q table has ordered rows and columns.
q table is stored physically as a collection of column lists. 
q-sql provides upsert semantics.
## 9.1 Insert
```q
/ Append Using Amend
t:([] name:`symbol$(); iq:`int$())
t,:`name`iq!(`Beeblebrox; 42)
`t insert (`name`iq!(`Slartibartfast; 134); (`name`iq!(`Marvin; 200))) / `t instead of t
```
    ,1

```q
/ 9.1.5 Insert into Keyed Tables
kt:([eid:1001 1002] name:`Dent`Beeblebrox; iq:98 42)
`kt insert (1005; `Marvin; 200) / ,2
`kt insert (1004;`Slartibartfast;158) /,3
```
    ,2

    ,3

## 9.2 Upsert
The upsert template is like insert, only better. Except for the last sub-section on keyed tables, all the examples in the previous section all work the same for upsert.
```q
/9.2.3 Upsert on Keyed Tables
`kt upsert (1001; `Beeblebrox; 42)
`kt upsert (1001; `Beeblebrox; 43)
/9.2.4 Upsert on Persisted Tables
`:/q4m/tser set ([] c1:`a`b; c2:1.1 2.2)
`:/q4m/tser upsert (`c; 3.3) /`:/q4m/tser
/ Upserting to a serialized table reads the entire table into memory, updates it and writes out the result.
get `:/q4m/tser
```
    `kt

    `kt

    `:/q4m/tser

    `:/q4m/tser

    c1 c2 

    ------

    a  1.1

    b  2.2

    c  3.3

## 9.3 The select Template
```q
t:([] c1:`a`b`c; c2:10 20 30; c3:1.1 2.2 3.3)
select i, c1, res:2*c2 from t 
/ A virtual column i represents the offset of each 
/ record in the table – i.e., i is the row number. 
/ It is implicitly available in the select phrase.
/9.3.2.3 select distinct
select distinct from t
/9.3.2.4 select[]
select[1] from t
/ to get the first 1
1#select[1] from t
select[-1] from t
-1#select[-1] from t
/ to get the last 1
```
    x c1 res

    --------

    0 a  20 

    1 b  40 

    2 c  60 

    c1 c2 c3 

    ---------

    a  10 1.1

    b  20 2.2

    c  30 3.3

    c1 c2 c3 

    ---------

    a  10 1.1

    c1 c2 c3 

    ---------

    a  10 1.1

    c1 c2 c3 

    ---------

    c  30 3.3

    c1 c2 c3 

    ---------

    c  30 3.3

The difference is that the # construct requires computing the entire result set and then keeping only the desired rows, whereas select[n] only extracts the desired number of rows. The latter will be faster and consume less memory for large tables.
This syntax is extended to select[n m] where m is the starting row number and n is the number of rows.
```q
select[1,1] from t
select[1;>c1] from t
```
    c1 c2 c3 

    ---------

    b  20 2.2

    c1 c2 c3 

    ---------

    c  30 3.3

```q
t:([] c1:`a`b`c; c2:10 20 30; c3:1.1 2.2 3.3)
select from t where (c2>15)&c3<3.0
select from t where c2 within 10 20
```
    c1 c2 c3 

    ---------

    b  20 2.2

    c1 c2 c3 

    ---------

    a  10 1.1

    b  20 2.2

```q
t:([] f:1.1 2.2 3.3; s:("abc";enlist "d";"ef"))
select from t where s~\:"ef"
/ f s
/ --------
/ 3.3 "ef"
```
No having 
```q
t:([]sym:`IBM`IBM`MSFT`IBM`MSFT;
    ex:`N`O`N`N`N;
    time:12:10:00 12:30:00 12:45:00 12:50:00 13:30:00;
    price:82.1 81.95 23.45 82.05 23.40)
select from t where price=(max;price) fby ([]sym;ex)
```
    sym  ex time     price

    ----------------------

    IBM  N  12:10:00 82.1 

    IBM  O  12:30:00 81.95

    MSFT N  12:45:00 23.45

## 9.4 The exec Template
The syntax of the exec template is identical to that of select.
exec <ps> <by pb> from texp <where pw>
Whereas select always returns a table, the result type of exec depends on the number of columns in its select phrase. One column yields a list; more than one column yields a dictionary.
```q
t:([] name:`a`b`c`d`e; state:`NY`FL`OH`NY`HI)
/select name, distinct state from t
/'length
exec name, distinct state from t
/name | `a`b`c`d`e
/state| `NY`FL`OH`HI
select name from t
exec name from t
```
    name | `a`b`c`d`e

    state| `NY`FL`OH`HI

    name

    ----

    a   

    b   

    c   

    d   

    e   

    `a`b`c`d`e

## 9.5 The update Template¶
The update template has identical syntax to select.
update <pu> <by pb> from texp <where pw>
The semantic difference is that colons in the update phrase pu identify modified or new columns instead of simply assigning column names. 
To modify the table in place, pass it by name.
```q
t:([] c1:`a`b`c; c2:10 20 30)
update c1:`x`y`z from `t / `t instead of t
t
update c3:`x`y`z from `t / `t instead of t
t
```
    `t

    c1 c2

    -----

    x  10

    y  20

    z  30

    `t

    c1 c2 c3

    --------

    x  10 x 

    y  20 y 

    z  30 z 

### 9.5.2 update-by
When the by phrase is present, the update operation is performed along groups. This is most useful with aggregate and uniform functions. For an aggregate function, the entire group gets the value of the aggregation on the group.
update avg weight by city from p
p | name  color weight city  
--| -------------------------
p1| nut   red   15     london
p2| bolt  green 14.5   paris 
p3| screw blue  17     rome  
p4| screw red   15     london
p5| cam   blue  14.5   paris 
p6| cog   red   15     london
## 9.6 The delete Template¶
The final template, delete, allows either rows or columns to be deleted. Its syntax is a simplified form of select, with the restriction that either pcols or pw can be present but not both.
delete <pcols> from texp <where pw>
If pcols is present as a comma-separated list of columns, the result is texp with the specified columns removed. If pw is present, the result is texp after records meeting the criteria of pw are removed. Someone always asks, can you delete rows and column simultaneously? But the rest of us meditating on the Zen of q realize this makes no sense.
```q
t:([] c1:`a`b`c; c2:10 20 30)
delete c1 from t
delete from t where c2>15
t
```
    c2

    --

    10

    20

    30

    c1 c2

    -----

    a  10

    c1 c2

    -----

    a  10

    b  20

    c  30

## 9.7 Sorting
Recall that tables and keyed tables comprise lists of records and therefore have an inherent order. A table or keyed table can be reordered by sorting on any column(s). In contrast to SQL, there is no equivalent to ORDER BY in the select template. Instead, built-in functions that sort tables are applied after select.
```q
```
    evaluation error:
    
    list
    
      [0]  list `c*

           ^

    
```q
t:([] c1:`a`b`c`a; c2:20 10 40 30)
`c1`c2 xasc t
t
`c1`c2 xasc `t / inplace by `t instead of t
t
```
    c1 c2

    -----

    a  20

    a  30

    b  10

    c  40

    c1 c2

    -----

    a  20

    b  10

    c  40

    a  30

    `t

    c1 c2

    -----

    a  20

    a  30

    b  10

    c  40

```q
t:([] c1:`a`b`c; c2:10 20 30; c3:1.1 2.2 3.3)
`new1`new2 xcol t
t /There is no pass-by-name version of xcol. To modify the source, reassign it.
`c3 xcols t
`c3`c2 xcols t
/The source operand cannot be a keyed table.
/There is no pass-by-name version of xcols. To modify the source, you must reassign it.
```
    new1 new2 c3 

    -------------

    a    10   1.1

    b    20   2.2

    c    30   3.3

    c1 c2 c3 

    ---------

    a  10 1.1

    b  20 2.2

    c  30 3.3

    c3  c1 c2

    ---------

    1.1 a  10

    2.2 b  20

    3.3 c  30

    c3  c2 c1

    ---------

    1.1 10 a 

    2.2 20 b 

    3.3 30 c 

## 9.9 Joins
select sname:s.name, pname:p.name, qty from sp / implicit left join.
### 9.9.2 Ad hoc Left Join (lj)
To create an ad-hoc left outer join between tables that could have a foreign-key relationship, use the dyadic lj. When the foreign key exists, that linkage is used; otherwise, the linkage is constructed dynamically. The join is 2-3 times faster if the foreign key already exists.
```q
t:([] k:1 2 3 4; c:10 20 30 40)
kt:([k1:2 3 4 5]; v:2.2 3.3 4.4 5.5)
select c, v:kt[([] k1:k); `v] from t
select k:k1,v from kt
select c,v from t lj `k xkey select k:k1,v from kt
```
    c  v  

    ------

    10    

    20 2.2

    30 3.3

    40 4.4

    k v  

    -----

    2 2.2

    3 3.3

    4 4.4

    5 5.5

    c  v  

    ------

    10    

    20 2.2

    30 3.3

    40 4.4

### 9.9.4 Ad Hoc Inner Join (ij)
```q
t:([] k:1 2 3 4; v:10 20 30 40)
kt:([k:2 3 4 5]; v:200 300 400 500)
t ij kt
```
### 9.9.5 Equijoin ej
The triadic equijoin operator ej corresponds to a SQL inner join between tables in the second and third parameters along specified column names in the first parameter. The right operand does not have to be a keyed table. Unlike ij, all matching records in the right table appear in the result.
```q
t1:([] k:1 2 3 4; c:10 20 30 40)
t2:([] k:2 2 3 4 5; c:200 222 300 400 500; v:2.2 22.22 3.3 4.4 5.5)
t1 ij `k xkey t2
ej[`k;t1;t2]
```
    k c   v  

    ---------

    2 200 2.2

    3 300 3.3

    4 400 4.4

    k c   v    

    -----------

    2 200 2.2  

    2 222 22.22

    3 300 3.3  

    4 400 4.4  

### 9.9.7 Union Join
```q
t1:([] c1:`a`b; c2:1 2)
t2:([] c1:`c`d; c2:3 4)
t1,t2 / t1 uj t2
```
    c1 c2

    -----

    a  1 

    b  2 

    c  3 

    d  4 

### 9.9.8 As-of Joins
As-of joins are so-named because they most often join tables along time columns to obtain a value in one table that is current as of a time in another table. As-of joins are non-equijoins that match on less-than-or-equal. They are not restricted to time values.
```q
show t:([] ti:10:01:01 10:01:03 10:01:04;sym:`msft`ibm`ge;qty:100 200 150)
show q:([] ti:10:01:00 10:01:01 10:01:01 10:01:02;sym:`ibm`msft`msft`ibm;px:100 99 101 98)
aj[`sym`ti;t;q]
aj0[`sym`ti;t;q]
/If you want the time of the matching quote in the result instead of the time of the trade, use aj0.
t asof `sym`ti!(`ibm;10:01:04)
```
    ti       sym  qty

    -----------------

    10:01:01 msft 100

    10:01:03 ibm  200

    10:01:04 ge   150

    ti       sym  px 

    -----------------

    10:01:00 ibm  100

    10:01:01 msft 99 

    10:01:01 msft 101

    10:01:02 ibm  98 

    ti       sym  qty px 

    ---------------------

    10:01:01 msft 100 101

    10:01:03 ibm  200 98 

    10:01:04 ge   150    

    ti       sym  qty px 

    ---------------------

    10:01:01 msft 100 101

    10:01:02 ibm  200 98 

    10:01:04 ge   150    

    qty| 200

### 9.9.9 Window Join
Window joins are a generalization of as-of joins and are specifically geared for analyzing the relationship between trades and quotes in finance. The idea is that you want to investigate the behavior of quotes in a neighborhood of each trade. For example, to determine how well a trade was executed, you need to examine the range of bid and ask prices that were prevalent around the trade time.
```q
show t:([]sym:3#`aapl;time:09:30:01 09:30:04 09:30:08;price:100 103 101)
show q:([] sym:8#`aapl;
    time:09:30:01+(til 5),7 8 9;
    ask:101 103 103 104 104 103 102 100;
    bid:98 99 102 103 103 100 100 99)
show w:-2 1+\:t `time
show c:`sym`time
wj[w;c;t;(q;(::;`ask);(::;`bid))] 
/To see all the values in each window, pass the identity function :: 
/in place of the aggregates. The result is similar to grouping without
/aggregate in a query and is helpful to see what is happening within each window.
wj[w;c;t;(q;(max;`ask);(min;`bid))]
```
    sym  time     price

    -------------------

    aapl 09:30:01 100  

    aapl 09:30:04 103  

    aapl 09:30:08 101  

    sym  time     ask bid

    ---------------------

    aapl 09:30:01 101 98 

    aapl 09:30:02 103 99 

    aapl 09:30:03 103 102

    aapl 09:30:04 104 103

    aapl 09:30:05 104 103

    aapl 09:30:08 103 100

    aapl 09:30:09 102 100

    aapl 09:30:10 100 99 

    09:29:59 09:30:02 09:30:06

    09:30:02 09:30:05 09:30:09

    `sym`time

    sym  time     price ask             bid           

    --------------------------------------------------

    aapl 09:30:01 100   101 103         98 99         

    aapl 09:30:04 103   103 103 104 104 99 102 103 103

    aapl 09:30:08 101   104 103 102     103 100 100   

    sym  time     price ask bid

    ---------------------------

    aapl 09:30:01 100   103 98 

    aapl 09:30:04 103   104 99 

    aapl 09:30:08 101   104 100

## 9.10 Parameterized Queries
```q
t:([] c1:`a`b`c; c2:10 20 30; c3:1.1 2.2 3.3)
select from t where c2>15
proc:{[sc] select from t where c2>sc}
proc 15
proc2:{[nms;sc] select from t where c1 in nms, c2>sc}
proc2[`a`c; 15]
```
    c1 c2 c3 

    ---------

    b  20 2.2

    c  30 3.3

    c1 c2 c3 

    ---------

    b  20 2.2

    c  30 3.3

    c1 c2 c3 

    ---------

    c  30 3.3

```q
t:([] c1:`a`b`c; c2:10 20 30; c3:1.1 2.2 3.3)
proc3:{[t;nms;delta] update c2+delta from t where c1 in nms}
proc3[t;`a`c;100]
t
proc3[`t;`a`c;100]
t
```
    c1 c2  c3 

    ----------

    a  110 1.1

    b  20  2.2

    c  130 3.3

    c1 c2 c3 

    ---------

    a  10 1.1

    b  20 2.2

    c  30 3.3

    `t

    c1 c2  c3 

    ----------

    a  110 1.1

    b  20  2.2

    c  130 3.3

## 9.11 Views
A SQL view is effectively a query expression whose result set can be used like a table. Views are useful for encapsulating data – e.g., hiding columns, or simplifying complex queries.
A q-sql view is a named table expression created as an alias with the double-colon operator ::. It is common to use the templates in views but this is not a limitation.
```q
t:([] c1:`a`b`c; c2:10 20 30)
u:select from t where c2>15
v::select from t where c2>15 
u
v
update c2:15 from `t where c1=`b
u
v
/Observe that when the underlying table changed,
/u did not change but the next reference to v does reflect the update.
/ To find the underlying query of a view, or any alias, apply the function view to the symbol alias name.
view `v /"select from t where c2>15"
a:42
b::a
view `b /"a"
views `.
```
    c1 c2

    -----

    b  20

    c  30

    c1 c2

    -----

    b  20

    c  30

    `t

    c1 c2

    -----

    b  20

    c  30

    c1 c2

    -----

    c  30

    "select from t where c2>15 "

    ,"a"

    `s#`b`v

## 9.12 Functional Forms
In the experience of the author, functional form is the most difficult q topic for most qbies. 
The high level of abstraction and generalization
There is nothing like it in SQL or other programming languages
In vintage Arthurian fashion, information density is maximized
Full disclosure: functional form is difficult. Along with adverbs and generalized application, it completes the Big Three aspects of q that separate q pretenders from contenders. Fortunately there is a cheat that can be helpful in most situations.
The function parse can be applied to a string containing a template query to produce a parse tree whose items (nearly) work in the equivalent functional form. A complication is that the operators are displayed in k form instead of q.
There are two functional forms, one for select and exec, the other for update and delete. The types of the arguments passed determine the overload. The forms are,
?[t;c;b;a] / select and exec
![t;c;b;a] / update and delete
where
t is a table or the name of a table
a is a dictionary of aggregates
b is a dictionary of groupbys or a flag controlling other aspects of the query
c is a list of constraints.
### 9.12.1 Functional select
```q
t:([] c1:`a`b`a`c`a`b`c; c2:10*1+til 7; c3:1.1*1+til 7)
ft:{([] c1:`a`b`a`c`a`b`c; c2:10*1+til 7; c3:1.1*1+til 7)}
select from t
?[t; (); 0b; ()]
select from t where c2>35,c1 in `b`c
?[t; ((>;`c2;35); (in;`c1;enlist `b`c)); 0b; ()]
select max c2, c2 wavg c3 from t
?[t; (); 0b; `maxc2`wtavg!((max;`c2); (wavg;`c2;`c3))]
t:([] c1:`a`b`a`c`b`c; c2:1 1 1 2 2 2; c3:10 20 30 40 50 60)
select distinct c1,c2 from t
?[t; (); 1b; `c1`c2!`c1`c2]
t:([] c1:`a`b`c; c2:10 20 30)
select[2] from t
?[t;();0b;();2]
t:([] c1:`a`b`c`a; c2:10 20 30 40)
select[>c1] c1,c2 from t
?[t;();0b;`c1`c2!`c1`c2; 0W; (>:;`c1)]
```
    c1 c2 c3 

    ---------

    a  10 1.1

    b  20 2.2

    a  30 3.3

    c  40 4.4

    a  50 5.5

    b  60 6.6

    c  70 7.7

    c1 c2 c3 

    ---------

    a  10 1.1

    b  20 2.2

    a  30 3.3

    c  40 4.4

    a  50 5.5

    b  60 6.6

    c  70 7.7

    c1 c2 c3 

    ---------

    c  40 4.4

    b  60 6.6

    c  70 7.7

    c1 c2 c3 

    ---------

    c  40 4.4

    b  60 6.6

    c  70 7.7

    c2 c3 

    ------

    70 5.5

    maxc2 wtavg

    -----------

    70    5.5  

    c1 c2

    -----

    a  1 

    b  1 

    c  2 

    b  2 


    c1 c2

    -----

    a  1 

    b  1 

    c  2 

    b  2 

    c1 c2

    -----

    a  10

    b  20

    c1 c2

    -----

    a  10

    b  20

    c1 c2

    -----

    c  30

    b  20

    a  10

    a  40

    c1 c2

    -----

    c  30

    b  20

    a  10

    a  40

### 9.12.1 Functional select
The functional form for exec is nearly identical to that of select. Slight variations in the parameters indicate which is intended. Since the constraint parameter carries over unchanged, we omit its discussion in this section.
```q
t:([] c1:`a`b`c`a; c2:10 20 30 40; c3:1.1 2.2 3.3 4.4)
exec distinct c1 from t
?[t; (); (); (distinct; `c1)]
exec c1:distinct c1 from t
?[t; (); (); (enlist `c1)!enlist (distinct; `c1)]
exec distinct c1, c2 from t
?[t; (); (); `c1`c2!((distinct; `c1); `c2)]
exec c2 by c1 from t
?[t; (); `c1; `c2]
```
    `a`b`c

    `a`b`c

    c1| a b c

    c1| a b c

    c1| `a`b`c

    c2| 10 20 30 40

    c1| `a`b`c

    c2| 10 20 30 40

    a| 10 40

    b| ,20

    c| ,30

    a| 10 40

    b| ,20

    c| ,30

### 9.12.3 Functional update¶
The syntax of functional form of update is identical to that of select except that ! is used in place of ?. In the following examples you will need to keep track of the different uses of enlist:
making a list of parse trees from a single parse expression
creating singleton dictionaries
distinguishing literal symbols from column names.
```q
t:([] c1:`a`b`c`a`b; c2:10 20 30 40 50)
update c2:100 from t where c1=`a
c:enlist (=;`c1;enlist `a)
b:0b
a:(enlist `c2)!enlist 100
![t;c;b;a]
update c2:sums c2 by c1 from t
![`t; (); (enlist `c1)!enlist `c1; (enlist `c2)!enlist(sums; `c2)]
t
```
    c1 c2 

    ------

    a  100

    b  20 

    c  30 

    a  100

    b  50 

    c1 c2 

    ------

    a  100

    b  20 

    c  30 

    a  100

    b  50 

    c1 c2

    -----

    a  10

    b  20

    c  30

    a  50

    b  70

    `t

    c1 c2

    -----

    a  10

    b  20

    c  30

    a  50

    b  70

### 9.12.4 Functional delete¶
The syntax of functional delete is a simplified form of functional update.
![t;c;0b;a]
where t is a table, or the name of a table, c is a list of parse trees for where subphrases and a is a list of column names. Either c or a, but not both, must be present. If c is present, it specifies which rows are to be deleted. If a is present it is a list of symbol column names to be deleted. In the former case you must specify a as an empty list of symbols.
As before, distinguish the various uses of enlist in the examples.
```q
t:([] c1:`a`b`c`a`b; c2:10 20 30 40 50)
delete from t where c1=`b
![t;enlist (=;`c1;enlist `b);0b;`symbol$()]
delete c2 from t
![`t;();0b;enlist `c2]
t
```
    c1 c2

    -----

    a  10

    c  30

    a  40

    c1 c2

    -----

    a  10

    c  30

    a  40

    c1

    --

    a 

    b 

    c 

    a 

    b 

    `t

    c1

    --

    a 

    b 

    c 

    a 

    b 

```q
```
