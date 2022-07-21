# Composite Types

## Arrays
creates array of three ints initialized to zero.
  `var x [3]int`

If you have initial values for the array, you specify them with an array literal
  `var x = [3]int{10, 20, 30}`

If you have a sparse array (an array where most elements are set to their zero value), you can specify only the indices with values in the array literal:
  `var x = [12]int{1, 5: 4, 6, 10: 100, 15}` //[1, 0, 0, 0, 0, 4, 6, 0, 0, 0, 100, 15]

### compare Arrays
You can use == and != to compare arrays
  `var x = [...]int{1, 2, 3}
  var y = [3]int{1, 2, 3}
  fmt.Println(x == y) // prints true`

### 2 dimensional arrays
  `var x [2][3]int`

### Get element and length
  `fmt.Println(a[0])`
  `fmt.Println(len(a))`

### Array limitations
  -  go considers size to be part of the type of the array. so `[3]int != [4]int`.
  - Also variables cant be used to specify the size as types must be resolved at compile time and not runtime.
  -  can’t use a type conversion to convert arrays of different sizes to identical types
  - you can’t assign arrays of different sizes to the same variable

## Slices
  - size is not part of the type.
  - no need to specify the length
  - cant use negative index to get element like python
  - cant compare two slices like array using == and !=. can use `reflect DeepEqual` to compare.

    `var x = []int{10, 20, 30}`
   creates 3 ints using slice literal.

   `var x []int` x is assigned to nil and has no type.so it can be compared with values of other types.

### append
  `x:= []int{1,2,3}
  y:= []int{4,5,6}
  x=append(x, y...)
  `
  - since go is a call by value language, a copy is created for x and new number gets added and the copy is returned.
  - when length reaches capacity, go doubles the size of slice when capacity is below 1024 and grow by 25% afterward.

  ### capacity
- Every slice has a capacity, which is the number of consecutive memory locations reserved. This can be larger than the length.
 If you try to add additional values when the length equals the capacity, the append function uses the Go runtime to allocate a new slice with a larger capacity. The values in the original slice are copied to the new slice, the new values are added to the end, and the new slice is returned.

 ### make
 `x := make([]int, 5, 6)`
 make slice of type, size and capacity.

 ### ways to declare
 `var x []int` - nil slice

 `var x = []int{}` - using slice literal; creates zero length slice which is non-nil. this type of slice is useful only in encoding/json.

use make if you know the size of slice but dont know the values.
`var x= make([]int, 5)`

if you know all the values of slice then use slice literal
` var x = []int{2,3,4,5}`

prefer using append with a slice initialized to a zero length. It might be slower in some situations, but it is less likely to introduce a bug

### slicing
  - slice expression written inside brackets has starting and ending offset, seperated by colon (:)
  - slice doesnt make a copy instead 2 variables share memory. change in one affects other.
  - `x[:2], x[3:], x[:], x[1:4]`
  - append with slicing
  `x := []int{1, 2, 3, 4}
  y := x[:2]
  y = append(y, 30)
  x: [1 2 30 4]
  y: [1 2 30]`