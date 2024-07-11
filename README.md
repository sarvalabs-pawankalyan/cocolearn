
# Cocolang Documentation Issues and suggestions

## COCO Version: 0.4.1

---
### 1. MOI Manifests for PISA

**Provided Steps in documentation**:
```
Compiling on Logiclab

1. Generate a compiled manifest using the Coco compiler
2. Start the CLI session with `logiclab start`
3. Register a participant using `register [username]`
4. Designate a participant as the sender, as a sender is needed to deploy a
   module
5. Compile the manifest with `compile [name] from manifest("[filepath]")`
6. Verify the presence of compiled logic with `logics` command
7. Deploy persistent state using `deploy [module].<deployer>()`
8. Invoke any module function with `invoke [module].<invokable>()`
```

### Suggestions and Issues

- In the **first point**, a sample command can be provided or link to coco commands.
- In the **second point**, the correct command is **`logiclabs --mode repl start`**, according to the remaining steps.
- In the **fourth point**, provide a sample command to designate a participant as a sender or a link to the Logic Labs commands so that users can easily follow the steps.
- First-time users may not fully understand the **seventh** and **eighth steps**. Try to explain what "deployer" and "invokable" mean or provide some examples. Alternatively, this information can be removed from here and added to the **Modules & Packages** section.

---
### 2. Primitive & Numeric Types

**Provided Example in documentation**:
```
coco Types

endpoint invokable Test1() -> (opbool Bool, opstring String, opbytes Bytes, opaddr Address, opu64 U64, opi64 I64, opu256 U256, opi256 I256):
    opbool = true
    opstring = "Hello"
    opbytes = 0x
    opaddr = Address(0)
    opu64 = 10
    opi64 = -10
    opu256 = 100
    opi256 = -100

endpoint invokable Test2(name String, age U64) -> (output U64):
    throw "{self.name} is {self.age} years old"


endpoint invokable Test3() -> (opbool Bool, opstring String, opbytes Bytes, opaddr Address, opu64 U64, opi64 I64, opu256 U256, opi256 I256):
    memory zbool Bool
    memory zstring String
    memory zbytes Bytes
    memory zaddr Address
    memory zu64 U64
    memory zi64 I64
    memory zu256 U256
    memory zi256 I256

    return (opbool: zbool, opstring: zstring, opbytes: zbytes, opaddr:zaddr, opu64:zu64, opi64:zi64, opu256:zu256, opi256:zi256}
```

### Suggestions and Issues

- In the given example, the return statement (last line) has a syntax issue: the opening brace is "(" and the closing brace is "}", which causes an error when compiling the code.

- The documentation of Primitive & Numeric Types can be more understandable if a brief description is provided for each type.

---
## 3.  Array & Varray Collections

**Provided Example in documentation**
```
coco Collections

endpoint invokable TestArray() -> (res1 [3]U64, res2 [3]U64, length U256):

	memory arr = [3]U64{1, 2, 3}
	arr2 = make([3]U64)
	arr2 = [3]U64{2, 3, 4}
	return (res1: arr, res2:arr2, length: len(arr))


endpoint invokable TestVarray() ->(res1 []U64, res2 []U64, output Bool, length U256, joined []U64):

	memory vrr1 []U64
	append(vrr1, 1)
	append(vrr1,2)
	memory vrr2 = make([]U64, 2)
	append(vrr2, 2)
	append(vrr2, 3)
	append(vrr2, 4)
	shrink(vrr2)
	memory check = vrr2[1]?
	memory s = len(vrr2)
	memory merged = merge(vrr1, vrr2)
	return (res1:vrr1 , res2:vrr2, output: check, length: s, joined: merged)
```

### Suggestions and Issues

The provided example has some issues that cause errors when compiling it:

- In the `TestArray()` endpoint, the **return type for the variable `length` is mentioned as `U256`**, but **`len(arr)` returns `U64`**.
- Similarly, in the `TestVarray()` endpoint, the **return type for the variable `length` is mentioned as `U256`**, but **`len(arr)` returns `U64`.

The content can be made more understandable by organising it into separate subtopics, such as:
```
- Array
- Varray
- Operations
    - Append
    - Shrink
- Appending to Stored Varrays
- Membership Check with `?` Operator
```

Other good structures can also be followed to improve clarity.

---
## 4. Mapping Collections

**Provided Example in documentation**
```
coco Maps

endpoint invokable Test1()->(output1 Map[String]U64, output2 Map[String]U64, length U256, joined Map[U64]String):
    memory mp = make(Map[U64]String)
    mp[1] = "Hello"
    mp[2] = "Hi"

    memory mp2 = Map[U64]String{3: "No", 4: "Yes", 5:"Wrong"}
    remove(mp2, 5) // removes the key/value 5:Wrong
    
    length = len(mp)

    joined = merge(mp, mp2)
    
	return (output1: mp, output2: mp2, length:length, joined:joined)
```

### Suggestions and Issues

The provided example has some issues that cause errors when compiling it:

- In the code, the output type of **output1**, **output2**, and **joined** is `Map[String, U64]`, but it is returning a type of `Map[U64, String]`.
- The output type of **length** is `U256`, but it is returning a type of `U64`.

---
## 5. Conditionals

**Provided Example in documentation**
```
coco Conditionals

endpoint invokable DoIf(confirm Bool) -> (out String):
	if confirm:
		return (out: {res} <- do())

endpoint invokable DoIfLarge(number U64) -> (out String):
	if number > 5000:
		return (out: {res} <- do())
	else if number > 2500:
		return (out: "almost")
	else:
		return (out: "not done")
	
func do() -> (res String):
	yield res "done!"
```

### Suggestions and Issues

The provided Example throws the following error:
```
[2024-07-11T07:57:49Z ERROR coco] "conditions.coco: Unrecognized token `Lbrace` found at [5:22]:[5:23]
    Expected one of "!", "(", "+", "-", "Env", "Ixn", "Map", "Sender", "[", "depolorize", "false", "make", "self", "true", "~", fstring_start, identifier, int or string"
```

Please provide a valid example.

The error can be cleared by modifying the code as follows:
```
coco Conditionals

endpoint invokable DoIf(confirm Bool) -> (out String):
if confirm:
	return (out: (res) <- do())

endpoint invokable DoIfLarge(number U64) -> (out String):
	if number > 5000:
		return (out: (res) <- do())
	else if number > 2500:
		return (out: "almost")
	else:
		return (out: "not done")
		
func do() -> (res String):
	yield res "done!"
```

--- 
### 6.  Looping & Iteration

**Provided Example in documentation**:
```
coco Loops

func Simple():
		memory sequence = []String{"foo", "bar", "boo", "far"}

		for idx, value in sequence:
				sequence[idx] = value.ToUpper()
				
func Optional():
		memory sequence = []String{"foo", "bar", "boo", "far"}

		for _, value in sequence:
				if value.IsUpper():
						return

func Range() -> (sum U64):
    for i in range(5):
				sum += i

func BreakContinue() -> (filtered []U64):
		for number in range(10):
				if number%2 == 0:
						continue
				
				if number == 9:
						break

				append(filtered, number)
```

### Suggestions and Issues

The provided Example throws the following error:
```
[2024-07-11T08:53:04Z ERROR coco] 
loopit.coco: variable id mismatch: reading return variable 'sum' is not allowed at line 19 column 3
    loopit.coco: variable id mismatch: reading return variable 'filter' is not allowed at line 29 column 16
```
Please provide a valid example.

- The error was caused in the **Range()** and **BreakContinue()** functions because we are **trying to modify the output variables instead of assigning values to them in a less verbose way**.

The **Range()** and **BreakContinue()** functions can be modified as shown,

```
func Range() -> (sum U64):
	memory total U64
	for i in range(5):
		total += i
	sum = total

func breakAndContinue() -> (filter []U64):
	memory new []U64
	for number in range(10):
		if number % 2 == 0:
			continue
		if number == 9:
			break
		append(new, number)
	filter = new
```

---

### 7. Yielding Return Values

**Provided Example in documentation**:
```
coco Yielding

func SenderAddress() -> (addr Address):
   // assignment can be used instead of yield
   addr = Address(Sender)

func MapCheck(a []U64, m Map[U64]String) -> (has Bool):
    memory has2 = a[2]?
    has = m[a[1]]?

    // if !has: would produce a compiler error
    // as reading return values is not allowed
    if !has2:
        return {has:has2}
		
    yield has = m[a[2]]?

func DoNothing():
	pass
```

### Suggestions and Issues

The provided Example throws the following error:
```
[2024-07-11T10:10:40Z ERROR coco] "test.coco: Unrecognized token `Lbrace` found at [14:16]:[14:17]
    Expected one of "\n", "(" or identifier"
```
    
```

[2024-07-11T10:11:01Z ERROR coco] "test.coco: Unrecognized token `Assign` found at [16:15]:[16:16]
    Expected one of "!", "(", "+", "-", "Env", "Ixn", "Map", "Sender", "[", "depolorize", "false", "make", "self", "true", "~", fstring_start, identifier, int or string"
```

The errors were caused by syntax issues at the return and yield statements.

It can be changed to this,

```
func MapCheck(a []U64, m Map[U64]String) -> (has Bool):
	memory has2 = a[2]?
	has = m[a[1]]?
	// if !has: would produce a compiler error
	// as reading return values is not allowed
	if !has2:
		return (has:has2)
	yield has m[a[2]]?
```


---
### 8.  Throwing & Catching Exceptions

**Provided Example in documentation**:
```
coco Throw
class TooLong:
    method __throw__()->(err String):
        yield err "string too long"

endpoint invokable TestThrow(s String):
    throw s

endpoint invokable TestLen(s String) -> (l U64):
    memory l1 = len(s)
    memory l2 = s.__len__()
    if l2 > 10:
        memory tl = TooLong{}
        throw tl

    if l1 != l2:
        memory ts = f"Lengths not equal {l1} != {l2}".__throw__()
        throw ts

    yield l l1
```

### Suggestions and Issues

The provided Example throws the following error:
```
[2024-07-11T11:03:38Z ERROR coco] test.coco: invalid argument method needs at least 'self' argument at line 3 column 5
```

This error is caused because `self` was not passed as an argument in the `__throw__()` method.

This can be fixed by passing the `self` keyword in the argument as shown below,
```
class TooLong:
	method __throw__()->(err String):
	yield err "string too long"
```


---
### 9. Observing & Mutating State

**Provided Example in documentation**:
```
coco ModMutate

state persistent:
    num Map[String]U64
    check U64

endpoint deployer persistent Init():
    pass

endpoint invokable persistent Sum(num1 U64, str String):
    mutate num <- ModMutate.State.num:
        num[str] += num1

endpoint invokable Num() -> (output Map[String]U64):
	observe output <- ModMutate.State.num

endpoint invokable Numplusone(str String) -> (output U64):
	observe map <- ModMutate.State.num:
		output = map[str] + 1

endpoint invokable persistent SetCheck(n U64):
    mutate n -> ModMutate.State.check
```

### Suggestions and Issues

The provided Example throws the following error:
```
[2024-07-11T12:00:56Z ERROR coco] test.coco: invalid type: can't return storable value that's not simple at line 15 column 2
```

I've not been able to figure out how to get the whole map and return it out. But a single value can be returned. Here's how to do it

```
endpoint invokable Num(str String) -> (output U64):
	observe map <- ModMutate.State.num:output = map[str]
```


---
