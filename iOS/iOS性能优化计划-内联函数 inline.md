# iOS性能优化计划-内联函数 inline

我看iOS底层源码，很多地方使用了 `ALWAYS_INLINE` 的宏：

```
#define ALWAYS_INLINE inline __attribute__((always_inline))
```

在 YYModel 中发现也使用了同样的宏：

```
#define force_inline __inline__ __attribute__((always_inline))
```

