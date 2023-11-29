<h1><center>Homework2</center></h1>

<center>BY  唐志鹏  SA23011068</center>

## 3.58

```c
long decode(long x, long y, long z) {
    long tmp = y - z;
    return (tmp * x) ^ (tmp << 63 >> 63);
}
```

## 3.60

- A
    
    - x的值存在%rdi，n的值存在%cl，result的值存在%rax，mask的值存在%rdx
    
- B
    
    - result初始值是0，mask初始值是1
    
- C
    
    - mask的测试条件是不为0
    
- D
    
    - mask被左移n位
    
- E
    
    - result与mask&x的结果取或
    
- F

    ```c
    long loop(long x, int n) {
        long result = 0;
        long mask;
        for(mask = 1; mask!=0; mask <<= n) {
            result |= (mask & x);
        }
        return result;
    }
    ```

## 3.63

```c
long switch_prob(long x, long n) {
    long result = x;
    switch(n) { 
        case 60:
        case 62:
            result = 8 * x;
            break;
        case 63:
            result = x >> 3;
            break;
        case 64:
            result = (x << 4) - x;
            x = result;
        case 65:
            x *= x;
        default:
            result = 0x4b + x;
    }
    return result;
}
```

- `sub $0x3c, %rsi`，说明做switch跳转表的值要是n-60
- `cmp $0x5, %rsi`，跳转表的上限是5，超过就跳转0x4005c3
- 跳转表的地址是0x4006f8+8*（n-60）
    - n-60=0：0x00000000004005a1
    - n-60=1：0x00000000004005c3
    - n-60=2：0x00000000004005a1
    - n-60=3：0x00000000004005aa
    - n-60=4：0x00000000004005b2
    - n-60=5：0x00000000004005bf

## 3.69

```C
%ecx = (*bp + 0x120)   说明last的偏移量是0x120=288
%ecx存了first和last, 也就是n
%rax = 5*i
%rax = 8*5*i + *bp , 也就是*ap
%rdx = (8*5*i + *bp + 8), 也就是ap->idx
%rcx = n
(8+8 + 8*5*i + *bp  + 8*ap->idx) = n
```
- *ap的地址是40*i+8，last的地址是288，那么CNT就是7，一个a_struct的空间是40

```c
struct a_struct {
    long idx;
    long x[4];
}
```

## 3.70

- A
    - e1.p：0
    - e1.y：8
    - e2.x：0
    - e2.next：8
    
- B：总共16字节

- C

    ```c
    void proc(union ele *up) {
        up->e2.x = *(up->e2.next->e1.p) - up->e2.next->e1.y;
    }
    ```
