# JavaScript
#cs 

JS is a dynamically typed, weakly typed language. 

Dynamic typing is essentially no type declarations. 

Weak typing is when operations involving incompatible types will perform type coercions. 
- `>'a' + 100`
- `'a100'`

### Running JS

Using node
- Server side JS runtime and networking framework
- node as interpreter
- node as repl - read eval print loop



## Types

1. string
2. number
3. boolean
4. null -> typeof gives back 'object'
5. undefined
6. objects --> functions are objects, but typeof gives back function'
7. bigint
8. symbol


### String

- UTF-16
- Operators
	- '+' concatenation
	- `.repeat` 
- `""`, `''`
- backticks...template string


### Number

- represents floating point and integer values
- 64 bits


### Null vs Undefined

- Undefined is default value if there is no value
- Null is something you intentionally set something to
	- No object