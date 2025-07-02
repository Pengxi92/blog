# 概述

LRU（Least Recently Used）算法是一种常见的缓存淘汰策略，也被称为最近最少使用算法。

LRU 算法的核心思想是，当缓存空间不足时，优先淘汰最近最少使用的缓存数据。 具体来说，LRU 算法维护一个缓存的列表，每次访问一个数据时，就将该数据移到列表的最前端，表示该数据最近被访问过。当需要淘汰数据时，就将列表尾部的数据淘汰，因为这些数据是最近最少被访问的。

## 优缺点

优点：能够保证缓存中的数据都是热点数据，也就是最近被访问的数据，因此可以有效地提高缓存命中率。
缺点：实现较为复杂，需要维护一个有序的缓存列表，并且每次访问数据都需要更新列表，对性能有一定的影响。

lru 缓存：
1、大小为 n，最近使用(包括 set、get)的放在最前，超过则删除最后的
2、提供 set、get 方法

```
class LRU {
  this.limit = 0;
  this.cache = new Map();
  super(limit) {
    this.limit = limit
  }

  set(key, value) {
    if (this.cache.size() === this.limit) {
      ; Array.form(this.cache)[0]
      // 删除
      const key = this.cache.next().value
      this.cache.del(key)
    }
    this.cache.set(key, value)
  }

  get(key) {
    if (this.cache.has(key)) {

      const resValue = this.cache.get(key)

      this.cache.del(key)
      this.cache.set(key, resValue)

      return resValue
    } else {
      return null
    }
  }
}

```
