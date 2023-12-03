# lab1 

## programming homework

1. Add two fields in `TaskControlBlock`:
    -  `task_start_time: usize`
    -  `task_syscall_info: [u32; MAX_SYSCALL_NUM]`
1. Add two functions in `os/src/task`:  
    - `get_current_task_info -> TaskInfo`
    - `syscall_counter`
1. While start running each task, check and note the time in ms.
1. In the entry of `os/src/syscall`, call the `syscall_counter`

## writing homework

1. After applications try to do illegal operations, mechine noticed these actions and trappe by Exceptions.  
    In trap handle functions, kernel printed: 
    ```
    [kernel] PageFault in application, bad addr = 0x0, bad instruction = 0x804003c4, kernel killed it.
    [kernel] IllegalInstruction in application, kernel killed it.
    [kernel] IllegalInstruction in application, kernel killed it.
    ```
    Then kernel killed these tasks and directly ran the next task by call `exit_current_and_run_next()`

    > RustSBI version 0.3.0-alpha.2, adapting to RISC-V SBI v1.0.0
2. About `trap.S`:
    1. `a0` is the first parameter supplied to the `__restore` function. In `goto_restore` function, it point to the top of kernel stack, but the register is not used in `__restore`.
    2. They deal with:
        - `sstatus`: SPP 等字段给出 Trap 发生之前 CPU 处在哪个特权级（S/U）等信息
        - `sepc`: 当 Trap 是一个异常的时候，记录 Trap 发生之前执行的最后一条指令的地址
        - `sscratch`: The stack top before mode change.
    3. These two registers are not used by used tasks, so there is no need to save or restore them.
    4. Now `sp` -> user stack, `sscratch` -> kernel stack
    5. Mode change happened at `sret`. This command means to exit supervisor mode change to user mode.
    6. Now `sp` -> kernel stack, `sscratch` -> user stack
    7. After any command that may cause a trap, such as `ecall` or illegal commands.

## 荣誉准则

1. 在完成本次实验的过程（含此前学习的过程）中，我曾分别与 以下各位 就（与本次实验相关的）以下方面做过交流，还在代码中对应的位置以注释形式记录了具体的交流对象及内容：
    > N/A

1. 此外，我也参考了 以下资料 ，还在代码中对应的位置以注释形式记录了具体的参考来源及内容：
    > [rCore-Tutorial-Guide-2023A 文档](https://learningos.cn/rCore-Tutorial-Guide-2023A/index.html)  
    > [rCore-Tutorial-Book 第三版](https://rcore-os.cn/rCore-Tutorial-Book-v3/)

1. 我独立完成了本次实验除以上方面之外的所有工作，包括代码与文档。 我清楚地知道，从以上方面获得的信息在一定程度上降低了实验难度，可能会影响起评分。

1. 我从未使用过他人的代码，不管是原封不动地复制，还是经过了某些等价转换。 我未曾也不会向他人（含此后各届同学）复制或公开我的实验代码，我有义务妥善保管好它们。 我提交至本实验的评测系统的代码，均无意于破坏或妨碍任何计算机系统的正常运转。 我清楚地知道，以上情况均为本课程纪律所禁止，若违反，对应的实验成绩将按“-100”分计。