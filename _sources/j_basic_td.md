# 型

Julia の型システムは動的型付けであるが, 明示的に型宣言を行うことができる.

## 型宣言

型宣言は `::` 演算子を用いて行う. `::` は, 数値や式の左側に配置する. 右側の型が具象型の場合, 左側の値はその具象型, 右側の型が抽象型の場合, 左側の値はその抽象型のサブタイプである必要がある.

```Julia
julia> (1+2)::AbstractFloat
ERROR: TypeError: in typeassert, expected AbstractFloat, got a value of type Int64
Stacktrace:
 [1] top-level scope
   @ REPL[19]:1

julia> (1+2)::Int
3

julia> abstract type Real end

julia> struct Integer <: Real
         x::Int
       end

julia> Integer(1)::Int
ERROR: TypeError: in typeassert, expected Int64, got a value of type Integer
Stacktrace:
 [1] top-level scope
   @ REPL[7]:1

julia> Integer(1)::Real
Integer(1)
```
