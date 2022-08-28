在学ts时，刷到的一些有意思的题目并用ts实现。


将'10000000000'形式的字符串，以每3位进行分隔展示'10.000.000.000'.

方法一（推荐）：

```
type SplitStr<
  Str extends string,
  Ele extends string = '.'
> = Str extends `${infer First}${infer Second}${infer Third}${infer Rest}`
      ? `${First}${Second}${Third}${Ele}${SplitStr<Rest>}`
      : Str

type ReverseStr<
  Str extends string,
  Result extends string = ''
> = Str extends `${infer First}${infer Rest}`
      ? ReverseStr<Rest , `${First}${Result}`>
      : Result

type test= ReverseStr<SplitStr<ReverseStr<'10000000000'>>>

let a:test = '10.000.000.000'   //代码提示即为所求结果
```

方法二：

```
type SplitStr<
  Arr extends string,
  Ele extends string = '.',
  LimitLength extends number = 3, 
> = Arr extends `${infer First}${infer Second}${infer Rest}` | `${infer Left}${infer Right}`
  ? Divide<Subtract<StrLen<Arr> , 1> , LimitLength> extends never
    ? Divide<Subtract<StrLen<Arr> , 2> , LimitLength> extends never
      ? AddPoint<Arr , Ele>
      : `${First}${Second}${Ele}${AddPoint<Rest>}`
    : `${Left}${Ele}${AddPoint<Right>}`
  : never

type AddPoint<
  Str extends string ,
  Ele extends string = '.'
> = Str extends `${infer First}${infer Second}${infer Third}${infer Rest}`
      ? StrLen<Rest> extends 0
        ? Str
        : `${First}${Second}${Third}${Ele}${AddPoint<Rest , Ele>}`
      : Str

type BuildArray<
  Length extends number,
  Ele = unknown,
  Arr extends unknown[] = []
> = Arr['length'] extends Length
      ? Arr
      : BuildArray<Length , Ele , [...Arr , Ele]>

type Subtract<Num1 extends number , Num2 extends number> = 
  BuildArray<Num1> extends [...arr1: BuildArray<Num2> , ...arr2: infer Rest]
    ? Rest['length']
    : never

type Divide<
  Num1 extends number,
  Num2 extends number,
  CountArr extends unknown[]=[]
> = Num1 extends 0? CountArr['length']
    : Divide<Subtract<Num1, Num2> , Num2 , [unknown, ...CountArr]>

type StrLen<
  Str extends string,
  CountArr extends unknown[]=[]
> = Str extends `${string}${infer Rest}`
    ? StrLen<Rest , [...CountArr , unknown]>
    : CountArr['length']

type test = SplitStr<'10000000000'>

let a:test = '10.000.000.000'   //代码提示即为所求结果
```
