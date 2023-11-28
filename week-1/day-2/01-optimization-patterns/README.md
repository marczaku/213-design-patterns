# Optimization Patterns

Optimization patterns in programming are design patterns that focus on improving the performance, efficiency, and resource utilization of a software system. These patterns address concerns related to speed, memory usage, and overall optimization.

## Covered

- Object Pool
- Flyweight
- Dirty Flag
- Spatial Partition

## Left Out

- Caching
- Prefetching
- Hashing
- Indexing
- Batching
- Cache Optimization Patterns
  - Spatial Locality
  - Temporal Locality
  - Cache Blocking
  - Loop Unrolling
  - Prefetching
  - Data Alignment

## Exercise!

Your Turn! Solve the exercises handed you as Class Material.

- Groups of 4
- It's barely about writing code!
- And more about analyzing the problem
- Reading up on the solution
- Adapting the solution to the given codebase
- Coding is only the last step

## Hints

Think that you're done? Check my hints (one by one) to see if you covered everything!

### Object Pooling

<details>
  <summary>First Hint</summary>
  
  Have you noticed that the `EnemySpawner` spawns a lot of `Enemy` instances? And they take a long time setting up on `Start`. This can be avoided using Object Pooling.
  
</details>

<details>
  <summary>Second Hint</summary>
  
  There are many ways of implementing Object Pooling, but Unity actually has provided a really useful implementation.
  
</details>

<details>
  <summary>Third Hint</summary>
  
  When pooling, it is important to know, how many instances of an object you'll need. But the spawners increasingly spawn more enemies. Do you have an idea, of how you could solve this problem? Where technically, there could be close to an unlimited amount of enemies?
  
</details>

<details>
  <summary>Fourth Hint</summary>
  
  You could make sure to gradually increase the pool size. So, if you know that soon, 10 enemies are supposed to spawn, you could add one enemy per frame to the pool instead of 10 at once. This reduces lag spikes.
  
</details>

<details>
  <summary>Fifth Hint</summary>
  
  Have you noticed already, that the castle will continue shooting at dead enemies? Can you fix this?
  
</details>

<details>
  <summary>Sixth Hint</summary>
  
  The castle's projectiles also should be pooled!
  
</details>

<details>
  <summary>Seventh Hint</summary>
  
  If you pool them, have you noticed that they now always despawn as soon as they spawn?
  
</details>

<details>
  <summary>Eighth Hint</summary>
  
  You need to reset the Bullet Script's fields when putting them back into or taking them out of the pool.
  
</details>

### Flyweight

<details>
  <summary>First Hint</summary>
  
  Most of the memory load comes from the `Tree` Script's `TreeSeasonColors` reference which each trees load. Can you use the Flyweight pattern to share the information between trees instead?
  
</details>

<details>
  <summary>Second Hint</summary>
  
  Do your trees now always all have the same colors? This was not intended and not the base before. You need to change the `TreeSeasonColors` class somehow to make sure that only the memory-heavy part is shared.
  
</details>

<details>
  <summary>Third Hint</summary>
  
  The `private ColorInfo[] colors` Field is the part that consumes most memory.
  
</details>

<details>
  <summary>Fourth Hint</summary>
  
  Avoid using `static`. It can actually lead to additional performance benefits, but from an architectural / clean code point of view, it'd be best if the `TreeSpawner` were responsible of holding and sharing the Flyweight Data.
  
</details>

### Dirty Flag

<details>
  <summary>First Hint</summary>
  
  Check the Debug Logs in Unity's Console. They will show you, how often the field was updated and how often updates were actually required. e.g. `Updated: 1034/50 times.` means that the computer parameter was updated 1034 times even though only 50 updates would've been necessary if done right.
  
</details>

<details>
  <summary>Second Hint</summary>
  
  In `Console` class, `SetRed` `SetGreen` and `SetBlue` are functions that change a parameter. `CalculateComputedDataFromParameters` is the function that then calculates new information based on when any of the previous information changes. It is currently invoked as soon as any of the previous parameters changed. The computed parameter is utilized in function `UtilizeComputedParameter`. Think about if you can reduce, how often `CalculateComputedDataFromParameters` is invoked.
  
</details>

<details>
  <summary>Third Hint</summary>
  
  One problem is that it can happen, that multiple parameters change before the parameter is used. e.g.: `SetRed()` then `SetGreen()`. Each invoke `CalculateComputedDataFromParameters()`, so two times. But the console only uses the updated parameter (`UtilizeComputedParameter`) after both updates have already happened. Can you make sure that the parameter is only calculated once as soon as it's needed instead of twice?
  
</details>

<details>
  <summary>Fourth Hint</summary>
  
  Another problem is that it can happen, that the calculated parameter is used multiple times even though the base parameters haven't changed. e.g.: `SetRed()`, then two times `UtilizeComputedParameter`, `UtilizeComputedParameter`. Do you make sure that you don't recalculate the parameter when it hasn't changed?
  
</details>

### Spatial Partitioning

<details>
  <summary>First Hint</summary>
  
  In `Update`, the Player finds the closest car using `FindClosestCar` and then rotates towards it. The problem is that there's many cars and the more cars spawn, the slower this algorithm will get. How can you make sure to find the closest of all cars faster?
  
</details>

<details>
  <summary>Second Hint</summary>
  
  Data Structures like a Quad Tree allow objects to be grouped by sectors. That way, you can more easily get access to all cars of a certain sector that the player is in. e.g. if the player is in Stockholm, you don't need to check for all cars of the world. You can build your own data structure or download one from the web.
  
</details>

<details>
  <summary>Third Hint</summary>
  
  This one is really tricky, so don't worry if you get stuck, I don't expect you to complete this task.
  
</details>
