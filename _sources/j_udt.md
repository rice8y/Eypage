# ユーザ定義型

ユーザによる定義型は以下のようにする.

```Julia
julia> struct Decimal <: Real
        value::BigInt
        point::Int
       end

```

これは, `Decimal` という複合型を定義している. 具体的には, `Decimal` は `Real` の派生型であり, フィールドとして, BigInt 型の `value` と Int 型の `point` を持っている.

```Julia
julia> isconcretetype(Decimal)
true

```

なお, `Decimal`は, 具象型であり, インスタンスを生成できる.

ユーザが定義できる型には, 抽象型, 構造体, 可変構造体, プリミティブ型が存在する.

## ユーザによる型定義

### 抽象型

```Julia
julia> abstract type MyNumber end

julia> abstract type MyReal <: MyNumber end

```

抽象型は, `abstract type (型名) end` または `abstract type (型名) <: (基本型) end` のように定義する. なお, 基本型の指定を省略した場合は, 基本型として `Any` が指定される.

### 構造体

```Julia
julia> struct Decimal <: Real
        value::BigInt
        point::Int
       end

```

`struct (型名) end` または `struct (型名) <: (基本型) end` のように定義する.

### 可変構造体

```Julia
julia> mutable struct Future
        value
        done::Bool
       end

```

`mutable struct (型名) end` または `mutable struct (型名) <: (基本型) end` のように定義する. また, 上記の `value` のようにフィールド定義時に型アノテーションを付けなかった場合, `::Any` が指定される.

ここで, 構造体と可変構造体の違いを比較するために以下の構造体を定義する.

```Julia
julia> struct Past
        value
        done::Bool
       end

```

構造体と可変構造体の違いを以下に示す.

```Julia
julia> f = Future(10, false)
Future(10, false)

julia> f.value = 20
20

julia> f.done = true
true

julia> f
Future(20, true)

julia> p = Past(10, false)
Past(10, false)

julia> p.value = 20
ERROR: setfield!: immutable struct of type Past cannot be changed
Stacktrace:
 [1] setproperty!(x::Past, f::Symbol, v::Int64)
   @ Base .\Base.jl:38
 [2] top-level scope
   @ REPL[54]:1

```

上記の例より, 可変構造体のフィールドは変更可能であるが, 構造体のフィールドは変更不可能であることが分かる.

### プリミティブ型

```Julia
julia> primitive type Int24 <: Signed 24 end

```

`primitive type (型名) (ビットサイズ) end` のように定義する.

上記の例では, `Signed` を基本型とする `Int24` (24ビット符号付き整数)を定義している. なお, 上記の例はプリミティブ型を定義しただけで, 中身はないためこのままでは使えない. 実際に使えるようにするための方法は, [ユーザ定義モジュール](j_module.md)で示す.
