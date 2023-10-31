The `pipe` and `flow` functions from [[fp-ts]] are functions that allow you to compose operations.

### Pipe
The `Pipe` function pipes the value of an expression into a pipeline of functions.
![[Pasted image 20231019133013.png]]
##### Simple Case

We pipe a value A into a single function f that takes A and returns B.
`f = A -> B`
![[Pasted image 20231019133232.png]]
Example:
```typescript
const size = (s: string) => s.length;
pipe(
	'hello',
	size
);
// same as
size('hello');
```


Now let's try with `pipe(A, f, g) -> B`.
```typescript
const size = (s: string) => s.length;
const isAtLeast3 = (n: number) => n >= 3;

pipe(
	'hello',
	size,
	isAtLeast3
);
// same as
isAtLeast3(size('hello'));
```

##### Arity of Pipes
All functions passed to `pipe` must be unary, meaning they only one argument.
When you see a function being called inside a call to `pipe`, it means that function is returning another function
```typescript
import * as either from "fp-ts/Either";

const checkEligibility = (person: Person) => {  
	// pipe just feeds the data as an input to the next function and so on  
	const result = pipe(  
		person,  
		// Check the age first by using fromPredicate  
		either.fromPredicate(  
			(person) => person.age <= 18,  
			() => "The Person's age must be under 18"  
		),  
		// Check the nationality next  
		either.chain(either.fromPredicate(  
			(person) => person.nationality === 'American',  
			() => "The Person must be a U.S. citizen"  
		))  
	)  
	// Return the error string of original Person object (Either<string, Person>)  
	return result;  
};
```



### Flow
The `flow` function performs [[function composition]].

##### Simple case: Composing Two Functions
![[Pasted image 20231019134132.png]]
Suppose we have two functions `f = A -> B` and `g = B -> C`. Calling `flow(f, g)` will create a function that maps from A to C.
```typescript
const size = (s: string) => s.length;
const isAtLeast3 = (n: number) => n >= 3;

const sizeIsAtLeast3 = flow(size, isAtLeast3); // (s: string) => boolean
```

##### Arity of Flows
The first function passed to `flow` can take any number of arguments, while the rest must be unary.