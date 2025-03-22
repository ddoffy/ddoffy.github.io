---
layout: post
title: How redis works
date: 2025-03-22 14:35
category: technology
author: D. Doffy
tags: [redis]
summary: A beginner-friendly explanation of how Redis works, including skip lists and their role in making Redis fast.
---

Redis is an open-source, in-memory data structure store that is widely used as a database, cache, and message broker. Its speed and efficiency make it a popular choice for applications requiring low-latency data access. In this post, we will explore how Redis works, with a focus on skip lists and their implementation.

## What is Redis?

Redis stands for **Remote Dictionary Server**. It stores data in key-value pairs and supports various data structures such as strings, hashes, lists, sets, and sorted sets. Redis operates entirely in memory, which allows it to achieve extremely fast read and write operations.

## How Skip Lists Work

A **skip list** is a data structure that allows fast search, insertion, and deletion operations. It is an alternative to balanced trees and is particularly useful in scenarios where maintaining balance is complex or costly.

### Structure of a Skip List

A skip list consists of multiple levels of linked lists:
- The bottom level contains all the elements in sorted order.
- Higher levels act as "express lanes" to skip over multiple elements, reducing the number of comparisons needed during search operations.

Each element in the skip list has a random chance of being promoted to a higher level. This randomness ensures that the skip list remains balanced on average.

### Operations in a Skip List

1. **Search**: Start at the topmost level and move forward until the target element is found or the next element is greater than the target. If the target is not found, move down a level and repeat.
2. **Insertion**: Insert the element at the bottom level and randomly decide whether to promote it to higher levels.
3. **Deletion**: Remove the element from all levels where it exists.

The time complexity for search, insertion, and deletion in a skip list is O(log n) on average.

## Skip Lists in Redis

Redis uses skip lists to implement **sorted sets** (data type `zset`). A sorted set is a collection of unique elements, each associated with a score. The elements are stored in order of their scores, allowing efficient range queries and rank-based operations.

### Why Skip Lists?

Skip lists are chosen for sorted sets in Redis because:
- They provide predictable performance with O(log n) complexity for search, insertion, and deletion.
- They are simple to implement and maintain compared to balanced trees.
- They allow efficient range queries, which are essential for many Redis use cases.

### Implementation in Redis

In Redis, a sorted set is implemented using two data structures:
1. **Hash Table**: Maps elements to their scores for O(1) lookups.
2. **Skip List**: Maintains the elements in sorted order for efficient range queries and rank-based operations.

When you perform an operation on a sorted set, Redis uses the hash table for direct lookups and the skip list for range-based queries. This combination ensures both speed and flexibility.

## Why is Redis Extremely Fast?

Redis achieves its speed through several design choices:
- **In-Memory Storage**: All data is stored in memory, eliminating the need for disk I/O during normal operations.
- **Efficient Data Structures**: Redis uses optimized data structures like skip lists, hash tables, and compressed lists to minimize overhead.
- **Single-Threaded Model**: Redis processes commands sequentially in a single thread, avoiding the complexity of thread synchronization.
- **Pipelining**: Redis supports pipelining, allowing multiple commands to be sent in a single request, reducing network latency.

## Conclusion

Redis is a powerful tool for building high-performance applications. Its use of skip lists for sorted sets is a prime example of how thoughtful design choices can lead to exceptional performance. By understanding the underlying data structures like skip lists, you can better appreciate the efficiency and versatility of Redis.

If you're new to Redis, start by experimenting with basic commands and data structures. As you gain experience, explore advanced features like sorted sets and pipelines to unlock the full potential of Redis.

