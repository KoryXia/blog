## GMP Model Notes Summary

-----

### 1. Locality and G Creation

- **Scenario**: The current processor (P) is running a goroutine (e.g., an **ongoing task**), which creates a new goroutine (**child task**).
- **Mechanism**:
  - The newly created **child task** is first added to the local queue of the current processor to take advantage of locality.
  - Once the **ongoing task** is completed, the scheduler (e.g., an internal scheduling goroutine) assigns the **child task** to the currently executing worker thread, ensuring thread reuse.
- **Note**: Although the **child task** originates from the **parent task**, due to load balancing, it may be assigned to a different processor.

-----

### 2. Load Balancing When Local Queue is Full

- **Scenario**: Suppose a goroutine creates multiple child tasks (e.g., from **Task A** to **Task G**), and the local queue of the current processor has a capacity of 3.
- **Process**:
  1. **First 3 Tasks**: The first three created tasks (**Task A** to **Task C**) are directly added to the local queue (assuming it was initially empty).
  2. **Handling When the Queue is Full**:
     - When a fourth task (**Task D**) is created, the local queue is full, triggering load balancing:
       - The processor moves the first half of its local queue tasks (e.g., **Task A**) along with the newly created **Task D** to the global queue.
       - **Note**: After being transferred, the order of tasks in the global queue may change (e.g., becoming **Task D**, **Task A**).
  3. **Subsequent Task Allocation**:
     - If another task (**Task E**) is created and the queue is full again, the same transfer strategy applies.
     - Later tasks (**Task F**, **Task G**) may enter the local queue directly, depending on the available space after the transfer.

-----

### 3. Waking Idle Processors and Worker Threads

- **Scenario**: When a new G is created, if there are idle M waiting for tasks, the wake-up mechanism is triggered.

- **Mechanism**:

  - An idle M (previously in a sleep state, waiting for G) is awakened and enters a spin state, temporarily occupying the CPU while waiting for tasks.

  - The awakened M fetches Gs from the global queue based on the following formula:

    ```
    n = min(len(global_queue)/GOMAXPROCS + 1, len(global_queue)/2)
    ```

    - **Example**: If there are 3 Gs in the global queue and `GOMAXPROCS` is 4, then `n = 1`, meaning this M will take 1 task from the global queue and bind it to the corresponding P.

-----

### 4. Work Stealing

- **Scenario**: When a P’s local queue and the global queue are both empty.
- **Mechanism**:
  - M will attempt to "steal" tasks from the local queues of other Ps, typically taking half of the available Gs.

-----

### 5. Spinning Threads and Resource Constraints

- **Rules**:
  - The system allows at most `GOMAXPROCS` spinning Ms to avoid excessive CPU usage.
  - Any additional Ms beyond this number will enter a sleep state.
- **Purpose**:
  - Spinning Ms can quickly respond to newly created Gs, ensuring timely scheduling in high-concurrency scenarios.

-----

### 6. System Calls and M/P Unbinding

- **Blocking System Calls (e.g., File IO)**:
  - When a goroutine encounters a blocking operation (**task blocking**), the M executing it will unbind from its associated P.
  - After unbinding, the P will check:
    - If there are runnable Gs in its local queue.
    - If there are Gs in the global queue.
    - If there are idle Ms available to take over.
  - If any of these conditions are met, an idle M will be woken up and bound to the P. Otherwise, the P will enter an idle state.
- **Non-blocking System Calls (e.g., Network Polling)**:
  - When executing a non-blocking call, P and M unbind, and M records its previously bound P.
  - After the non-blocking call completes, M attempts to rebind to its original P:
    - If successful, the original task resumes execution.
    - If unsuccessful, M attempts to acquire an idle P. If none are available, it marks the G as runnable and adds it to the global queue, while M enters a sleep state.



-----

## GMP 模型笔记总结

------

### 1. 局部性与 G 的创建

- 场景：当前处理器（P）正在运行一个 goroutine（比如：**正在运行的任务**），该任务创建了一个新的 goroutine（称为**子任务**）。
- 机制：
  - 新创建的**子任务**会优先加入当前处理器的本地队列，以利用局部性。
  - 当**正在运行的任务**执行完毕后，调度协程（例如：调度器内部的控制 goroutine）会将**子任务**安排到当前正在执行任务的工作线程上，从而实现工作线程的复用。
- 注意：尽管**子任务**来源于**父任务**，但由于负载均衡的机制，**子任务** 也可能会被分配到其他处理器上。

------

### 2. 本地队列满时的负载均衡

- 场景：假设某个 goroutine创建了一系列子任务（比如从**任务 A** 到**任务 G **），而当前处理器的本地队列容量为 3。
- 流程：
  1. **前 3 个任务**：新创建的前三个子任务（比如**任务A**～**任务C**）会直接进入当前处理器的本地队列（假设队列最初为空）。
  2. 队列满时处理：
     - 当创建第四个任务（比如任务D）时，本地队列已满，触发负载均衡：
       - 当前处理器将本地队列中前一半的任务（例如：**任务 A **）和新创建的**任务 D ** 一起转移到全局队列中。
       - 注意：转移后全局队列中任务的顺序可能会发生变化（例如顺序变为**任务 D **、**任务 A **）。
  3. 后续任务的分配：
     - 当下一个任务（比如**任务 E **）创建时，如果再次遇到队列满，也会采用相同的转移策略；
     - 后续的任务（例如**任务 F **、**任务 G **）则可能直接进入本地队列（取决于转移后的队列剩余空间）。

------

### 3. 唤醒空闲处理器与工作线程

- **场景**：当某个 G 新建时，如果有休眠的 M 等待任务，就会触发唤醒机制。

- 机制：

  - 空闲 M（例如之前处于休眠状态、等待 G 的 M ）会被唤醒，并进入自旋状态——即它会暂时占用 CPU 等待任务的到来。

  - 被唤醒的 M 首先会从全局队列中获取 G ，获取 G 数量根据公式计算：

    ```
    n = min(len(global_queue)/GOMAXPROCS + 1, len(global_queue)/2)
    ```

    - **示例**：如果全局队列中有 3 个 G ，而 GOMAXPROCS 为 4，则计算得 n = 1，意味着该 M 会从全局队列中取 1 个任务，并绑定到相应的 P 上。

------

### 4. Work Stealing（任务窃取）

- **场景**：如果某个 P 的本地队列以及全局队列都没有可运行的 G 。
- **机制**：
  - M 会尝试从其他 P 的本地队列中“窃取”部分 G ，通常窃取目标队列中 G 的一半。

------

### 5. 自旋线程与资源限制

- 规则：
  - 系统中最多只能有与 `GOMAXPROCS` 数量相等的自旋状态 M ，以避免过多的 CPU 占用。
  - 超出这一数量的 M 会进入休眠状态。
- 作用：自旋状态的 M 能够快速响应新 G 的创建，保证高并发场景下的及时调度。

------

### 6. 系统调用与 M /P 解绑

- 阻塞系统调用（例如文件 IO）：
  - 当某个 goroutine在执行过程中遇到阻塞（比如**任务阻塞**），负责执行该任务的 M 会与其绑定的 P 解除绑定。
  - 随后，解除绑定的 P 会检查：
    - 本地队列中是否还有待执行 G ，
    - 全局队列中是否存在 G ，
    - 或是否有空闲的 M 可以补上空缺。
  - 如果存在以上任一情况，则会唤醒一个空闲 M 来重新绑定该 P；否则，P 将进入空闲状态。
- 非阻塞系统调用（例如网络轮询）：
  - 在执行非阻塞调用时，P 和 M 解绑，M 会记录下自己当前绑定的 P 。
  - 当非阻塞调用结束后，该 M 会尝试重新绑定到原先的 P ：
    - 若绑定成功，则继续运行原任务；
    - 若绑定失败，该 M 会尝试获取一个空闲 P；如果仍然没有，则将 G 标记为可运行状态，并加入全局队列，同时该 M 进入休眠状态。

