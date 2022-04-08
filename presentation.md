
---

# Lets start...

---

# What is Generics ? 

```html
    Generics enable types to be parameters (aka `Type Parameter`) when defining types, interfaces and methods.
```
---

# Something familiar first

Before generics -

```java
        List list = new ArrayList();
        list.add("hello");
        String s = (String) list.get(0);
```

After generics -

```java
        List<String> list = new ArrayList<String>();
        list.add("hello");
        String s = list.get(0);   // no cast
```

---

# Other Java Examples

Generic method inside a class - 
```java
        public <T> List<T> fromArrayToList(T[] a) {   
            return Arrays.stream(a).collect(Collectors.toList());
        }
```

All subclasses - 
```java
        List<? extends T>
```

All superclasses -
```java
        List<? super T>
```

Unbounded - 
```java
        List<?> list = new ArrayList(); 
```

---

# Refresher - golang interfaces and types
Interface - 
```go
type Computer interface {
    Compute()
}
type MyComputer struct {
  CPU int
  Memory int
}
func (c *MyComputer) Compute()  {
}
// empty function
func SolveUsingComputer(computer Computer) {}

func main() {
  var computer MyComputer
  // call method
  SolveUsingComputer(&computer)
}
```
Type - 
```go
type StudentAttendances map[string]int
type BankBalance float64
```

---

# Type Constraint vs Normal Interface

Normal Interface

```go
  type ConstraintInterface interface {
      DoSomething()
  }
  type ConstrainedType *ConstraintInterface
```

Constraint type interface

```go
  type ConstraintInterface interface {
    comparable
    DoSomething()
  }

  // NO LONGER WORKS!!!
  type ConstrainedType *ConstraintInterface
```

---

# Type Constraints

* Type constraints are interface types
  * `any` is a type constraint that permits any type
  * `T` constraint - restricts to that type
  * `~T` constraint - restricts to all who's underlying type is `T`
  * a union element `T1 | T2 | ...` restricts to any of the listed elements

---

# Example use of constraints to define generics

  * Functions -
      ```go
         func F[T constraint](p T) { ... }
      ```
  * Types -
      ```go
         type M[T constraint] []T
      ```
  * Structs -
      ```go
         type Lockable[T constraint] struct {
            T
            mu sync.Mutex
         }
         func (l *Lockable[T]) Get() T {
            l.mu.Lock()
            defer l.mu.Unlock()
            return l.T
         }
      ```
---

# Type Inference

```go
    var s []int
    f := func(i int) int64 { return int64(i) }
    var r []int64
    // Specify both type arguments explicitly.
    r = Map[int, int64](s, f)
    // Specify just the first type argument, for F,
    // and let T be inferred.
    r = Map[int](s, f)
    // Don't specify any type arguments, and let both be inferred.
    r = Map(s, f)    
```

---

# Some excerpts from official proposal 

**Q** : Why not use the syntax F<T> like C++ and Java?

**A** : When parsing code within a function, such as v := F<T>, at the point of seeing the < it's ambiguous whether we are seeing a type instantiation or an expression using the < operator.

More Examples - [Official Proposal Examples](https://go.googlesource.com/proposal/+/refs/heads/master/design/43651-type-parameters.md#examples)

Including - 
  * Map/Filter/Reduce/Keys methods
  * Implementing a `Set` type (similar to Java)
  * `Sort` for any type
  * Merge two channels into a result channel
  * ....

---

# Benefits Of Generics

* Stronger type checks at compile time
* Elimination of casts
* Enabling programmers to implement generic algorithms and data structures

---

# Let's look at some examples