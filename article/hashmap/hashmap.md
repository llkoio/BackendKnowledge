# HashMap(jdk8)

**

## **resize()方法（扩容方法）**

```java

final Node < K, V > [] resize() {
    // 把全局的table赋给oldTab
    Node < K, V > [] oldTab = table;
    // oldCap代表旧table的容量，如果oldTab==null，说明之前没有table，将其赋值零，否则赋值实际旧table容量
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    // 将全局阈值threshold赋值给记录旧table阈值的变量（存在旧table的情况下）
    int oldThr = threshold;
    int newCap, newThr = 0;
    // 如果旧的table存在的话oldCap>0
    if (oldCap > 0) {
        // 判断旧的table的容量是不是大于或者等于HashMap规定的最大table容量
        if (oldCap >= MAXIMUM_CAPACITY) {
            // 如果大于等于将Integer最大值21亿多赋值给全局阈值
            threshold = Integer.MAX_VALUE;
            // 不用扩容了，因为已经超过或等于HashMap的最大容量了，直接返回旧table
            return oldTab;
        // 否则就进行扩容，旧table的容量左移一位，相当于乘以2，赋值给新的table的容量，判断是否小于HashMap的最大容量限制，然后判断旧table的容量是否大于等于默认初始化容量（这里的意思就是必须大于等于默认初始化容量）
        } else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
            oldCap >= DEFAULT_INITIAL_CAPACITY)
            // 计算新table的阈值赋值给新table的阈值变量，阈值的计算公式是：table的容量 * 负载因子，扩容之后新的table的容量是旧table容量的两倍，也就是新table的阈值的计算公式是：旧table容量 * 2 * 负载因子，其实就是直接在旧table阈值的基础上乘以2就可以，即oldThr << 1
            newThr = oldThr << 1; // double threshold
    } else if (oldThr > 0) // initial capacity was placed in threshold
        newCap = oldThr;
    // 这个else是调用HashMap的无参构造方法的时候走的，把新table的容量赋值成默认初始化容量，把新table的阈值赋值成默认的阈值，即默认负载因子0.75 * 默认初始化容量16
    // zero initial threshold signifies using defaults
    else {
        newCap = DEFAULT_INITIAL_CAPACITY;
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    if (newThr == 0) {
        float ft = (float) newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float) MAXIMUM_CAPACITY ?
            (int) ft : Integer.MAX_VALUE);
    }
    threshold = newThr;
    @SuppressWarnings({
        "rawtypes",
        "unchecked"
    })
    Node < K, V > [] newTab = (Node < K, V > []) new Node[newCap];
    table = newTab;
    if (oldTab != null) {
        for (int j = 0; j < oldCap; ++j) {
            Node < K, V > e;
            if ((e = oldTab[j]) != null) {
                oldTab[j] = null;
                if (e.next == null)
                    newTab[e.hash & (newCap - 1)] = e;
                else if (e instanceof TreeNode)
                    ((TreeNode < K, V > ) e).split(this, newTab, j, oldCap);
                else { // preserve order
                    Node < K, V > loHead = null, loTail = null;
                    Node < K, V > hiHead = null, hiTail = null;
                    Node < K, V > next;
                    do {
                        next = e.next;
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        } else {
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```

参考文献：

无
