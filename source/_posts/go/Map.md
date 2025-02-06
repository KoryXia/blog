---
title: Map
tags: [Learn, Go]
category: [Learn, Go]
cover: /img/Go_Logo.png
description: Map
data: 2025-02-06
---

# Map

`tophash` only store high 8 bits of the hash result.

## Optimization of `bmap` in Go's `map` Implementation

### **Why Mark `bmap` as Pointer-Free?**
In Go's `map` implementation, the `bmap` structure represents a bucket in the hash table. If both the `key` and `value` types stored in the map are non-pointer types, the `bmap` can be marked as **not containing pointers**. This is beneficial because it allows the GC to skip scanning these buckets during its mark phase, reducing GC overhead and improving performance.

### **Issue: `bmap.overflow` is a Pointer**
`bmap` has an `overflow` field, which is a pointer to additional overflow buckets. This breaks the "pointer-free" assumption.

### **Solution: Using `hmap.extra`**
Instead of keeping overflow pointers inside `bmap`, they are stored in `hmap.extra`:
- `hmap.extra.overflow` holds overflow bucket pointers for `hmap.buckets`.
- `hmap.extra.oldoverflow` holds overflow pointers for `hmap.oldbuckets` (before resizing).

This keeps `bmap` pointer-free while ensuring overflow buckets remain reachable.

### **Conclusion**
GC no longer scans the entire `map`, only `extra.overflow`. This optimization improves GC efficiency while maintaining correctness.