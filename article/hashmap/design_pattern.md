# HashMap(jdk8)

**

## **resize()方法（扩容方法）**

```java
final Node < K, V > [] resize() {
    // 把全局的table赋给新变量
    Node < K, V > [] oldTab = table;
    // 如果oldTab==null，说明之前没有旧table，oldCap即旧table的容量，将其赋值零，否则赋值实际旧table容量
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
        } else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
            oldCap >= DEFAULT_INITIAL_CAPACITY)
            newThr = oldThr << 1; // double threshold
    } else if (oldThr > 0) // initial capacity was placed in threshold
        newCap = oldThr;
    else { // zero initial threshold signifies using defaults
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
