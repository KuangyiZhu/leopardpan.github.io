### Atomic
```cpp
    using AtomicDefaultSet::Set;

    template <typename T>
    static void Exchange(volatile T* target, T value, T* old)
    {   
        __asm__ __volatile__(
            "xchgq %0, %1"
            :"=r"(*old)
            :"m"(*target), "0"(value)
            :"memory");
    }   

    template <typename T>
    static T ExchangeAdd(volatile T* target, T value)
    {   
        __asm__ __volatile__
        (   
            "lock; xaddq %0, %1;"
            :"=r"(value)
            :"m"(*target), "0"(value)
        );  
        return value;
    }   

    template <typename T>
    static bool CompareExchange(volatile T* target, T compare, T exchange, T* old)
    {   
        bool result;
        __asm__ __volatile__(
            "lock; cmpxchgq %2,%3\n\t"
            "setz %1"
            : "=a"(*old), "=q"(result)
            : "r"(exchange), "m"(*target), "0"(compare)
            : "memory");
        return result;
    }   

```


### reference 
* [1] https://zhuanlan.zhihu.com/p/31386431 memory order
* [2] http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.152.5245&rep=rep1&type=pdf Memory Barriers: a Hardware View for Software Hackers
* [3] https://blog.csdn.net/pbymw8iwm/article/details/8227839 GCC在C语言中内嵌汇编 asm __volatile__
* [4] http://senlinzhan.github.io/2017/12/04/cpp-memory-order/ 理解 C++ 的 Memory Order
* [5] http://hpca23.cse.tamu.edu/taco/utsa-www/cs5513-fall07/lecture5.html Lecture 5: Out-of-order Execution
* https://git.kernel.org/pub/scm/virt/kvm/kvm.git/tree/Documentation/memory-barriers.txt kvm/kvm.git
* [6] https://blog.csdn.net/dd864140130/article/details/56494925 谈乱序执行和内存屏障
* [7] https://blog.csdn.net/gjq_1988/article/details/39520729 （转）CPU乱序执行原理
* [8] https://www.stardog.com/blog/writing-cache-friendly-code/ writing-cache-friendly-code
* [9] http://wiki.csie.ncku.edu.tw/embedded/perf-tutorial Linux 效能分析工具: Perf
