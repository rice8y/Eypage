# 特別な型

## Tuple 型・NamedTuple 型

タプル (Tuple) とは, 複数の値の組み合わせを指し, `()` の中に, 値をカンマ区切りで記述する.

```Julia
julia> typeof((1, 2))
Tuple{Int64, Int64}

julia> typeof((1, 99.9, :a))
Tuple{Int64, Float64, Symbol}

julia> typeof((1,))
Tuple{Int64}

julia> typeof(())
Tuple{}

```

Tuple 型は型パラメータ数が可変なパラメトリック型と見做せる. ここで, 要素が1つだけのタプルは `(1,)` のようにカンマも記述することに注意. カンマを記述せずに要素を1つだけにすると, `typeof((1)) === Int64` となり, だた括弧 `()` で括っただけとなる. また, 型パラメータを指定しない `Tuple` と空の `{}` を指定する `Tuple{}` は別物である.

```Julia
julia> Tuple === Tuple{}
false

```

また, 同じ型が並ぶタプルを表す型として, `NTuple{N, T}` が存在する.

```Julia
julia> (1, 2) isa NTuple{2, Int}
true

julia> (1, 2.0) isa NTuple{2, Int}
false

```

`Tuple` と `Tuple{}` についての検証を以下に示す.

```Julia
julia> isabstracttype(Tuple)
false

julia> isconcretetype(Tuple)
false

julia> isconcretetype(Tuple{})
true

julia> isconcretetype(Tuple{Int})
true

julia> typeof(Tuple)
DataType

julia> typeof(Tuple{Int})
DataType

julia> supertype(Tuple)
Any

julia> subtypes(Tuple)
Type[]

julia> Tuple{Int} <: Tuple
true

julia> Tuple{Int, Int} <: Tuple{Int}
false

julia> Tuple{Int} <: Tuple{Integer} <: Tuple{Any}
true

julia> Tuple{Int64, Float64, Symbol} <: NTuple{3, Any}
true

```

上記の例より, `Tuple` は抽象型でもなく具象型でもないが, 型パラメータを指定すると具象型となる. また, 空の型パラメータを指定した `Tuple{}` は具象型となる. さらに, Tuple 型の公称的基本型は `Any` , 公称的派生型は存在せず, 型パラメータを指定した Tuple 型は, 型パラメータを指定していない Tuple 型の派生型となる.

`Tuple{Int, Int} <: Tuple{Int}` が `false` より, 2つの `Int` を要素として持つタプルは, 1つの `Int` を要素として持つタプルの派生型ではないことが分かる. 具体的には, `(1, 2)` と `(1,)` の間にサブタイピング関係はないということである. また, `Tuple{Int} <: Tuple{Integer} <: Tuple{Any}` が `true` である. パラメトリック型では, 型パラメータが異なれば, サブタイピングの関係にあるかどうかに関わらず型として互換性のない別物になる (不変). 例としては, `Array{Int} <: Array{Integer}` が `false` . しかし, `Tuple` ではこれが当てはまらず, `A <: B` が `true` / `false` なら `Tuple{A} <: Tuple{B}` は `true` / `false` となる. このように, 型パラメータの基本型-派生型の関係がそのまま型の基本型-派生型の関係に反映される性質のことを共変という.

すなわち, Tuple 型は要素数を重視する型であるといえる.

```Julia
julia> Tuple{Int64, Symbol, Bool} <: NTuple{3, Any}
true

```

上記の例より, 各要素が全く互換性のない3つの型からなるタプルは, `NTuple{3, Any}` の派生型, つまり要素の型に制約はないが要素数は3であるタプルということである.

```Julia
julia> nt = (a=1, b=2.1, c=:OK)
(a = 1, b = 2.1, c = :OK)

julia> typeof(nt)
NamedTuple{(:a, :b, :c), Tuple{Int64, Float64, Symbol}}

julia> nt1 = (a=1,)
(a = 1,)

julia> nt2 = (;a=1)
(a = 1,)

julia> typeof(nt1)
NamedTuple{(:a,), Tuple{Int64}}

julia> nt0 = (;)
NamedTuple()

julia> typeof(nt0)
NamedTuple{(), Tuple{}}

```

上記の例のように, `(a=1, b=2.1, c=:OK)` として各要素に名前を付けたタプルを名前付きタプルといい, その型を NamedTuple 型という.

```Julia
julia> (1, 2.1, :OK) isa Tuple{Int64, Float64, Symbol}
true

```

また, 上記の例より分かる通り, NamedTuple 型は, Tuple 型をラップして各要素に名前でアクセスできるようにした型である. さらに, 名前付きタプルは以下のように `@NamedTuple` マクロを用いて簡単に記述することができる.

```Julia
julia> @NamedTuple begin
        a::Int
        b::Float64
        c::Symbol
       end
NamedTuple{(:a, :b, :c), Tuple{Int64, Float64, Symbol}}

julia> @NamedTuple{a::Int, b::Float64, c::Symbol}
NamedTuple{(:a, :b, :c), Tuple{Int64, Float64, Symbol}}

julia> @NamedTuple{a, b}
NamedTuple{(:a, :b), Tuple{Any, Any}}

julia> @NamedTuple{}
NamedTuple{(), Tuple{}}

```

`NamedTuple` についての検証を以下に示す.

```Julia
julia> supertype(NamedTuple)
Any

julia> subtypes(NamedTuple)
Type[]

julia> isabstracttype(NamedTuple)
false

julia> isconcretetype(NamedTuple)
false

julia> isconcretetype(NamedTuple{})
false

julia> isconcretetype(NamedTuple{Int})
false

julia> typeof(NamedTuple)
UnionAll

julia> typeof(NamedTuple{})
UnionAll

julia> typeof(@NamedTuple{})
DataType

julia> @NamedTuple{a::Int, b::Float64, c::Symbol} <: @NamedTuple{a, b, c}
false

```

`NamedTuple` は抽象型でもなく具象型でもない. また, `NamedTuple` の公称的基本型は `Any` であり, 公称的派生型は存在しない. なお, `Tuple` に見られた共変は `NamedTuple` にはない.

## Union 型
