### Fibonacci: F(n) = F(n-1) + F(n-2)
### In Matrix:
```
  [F(1),F(0)]  *  [1,1]^(n-1)   = [F(n),F(n-1)]
  [0   ,0   ]     [1,0]           [0   , 0    ]
```

```
const fn = (a,n) => {
    let temp = a , ans = [[1,0],[0,1]]
    while (n) {
        if( n % 2 ) ans = matrix(temp , ans)
        n = parseInt(`${n/2}`)
        temp = matrix(temp)
    }
    return ans
}

const matrix = (a, b = a) => {
    let c = new Array(a.length).fill(0).map(arr => new Array(b[0].length).fill(0));
    for(let i = 0; i<a.length;i ++){
        for(let j = 0; j<b[0].length; j++){
            for(let k =0; k<b.length;k++){
                c[i][j] += a[i][k] * b[k][j]
            }
        }
    }
    return c
}

const Fib = (n) => {
    let a = [[1,0],[0,0]]
    let b = [[1,1],[1,0]]
    if(n===0) return 0
    if(n<2) return 1
    return matrix(a , fn(b, n-1))[0][0]
}

console.log(Fib(0)) //0
console.log(Fib(1)) //1
console.log(Fib(2)) //1
console.log(Fib(3)) //2
console.log(Fib(4)) //3
console.log(Fib(5)) //5
```
