---
title: Concurrency
date: 2020-10-16 15:14:32
tags: ['Operating System', "Mutual Exclusion"]
categories: ['class notes']
cover:
summary: Concurrency
img:
---

##### 竞争条件(race conditions): 多个线程或者进程在读写一个共享数据时结果依赖于它们执行的相对时间的情形。

* A **race condition occurs** when multiple threads/processes **access shared data** and the result is dependent on **the order in which the instructions are interleaved**

##### 防止竞争条件: 确保一次只有一个进程可以操作变量，即进程需要进行同步。

#### 1. 临界区(critical section/critical region):

当一个进程在临界区内时，其他进程不允许进入临界区执行。

##### 临界区问题的解决方案需满足三个要求：

1. 互斥访问(Mutual exclusion)：如果一个进程在临界区执行，其他进程不能进入临界区
   * only one process can be in its critical section at any one point in time
2. 空闲让进/进步(Progress)：如果没有进程在临界区，但有些进程需要进入临界区，不能无限期地延长下一个要进入临界区的等待时间。即不在临界区的进程不能阻止另一个进程进入临界区。
   * any process must be able to enter its critical section at some point in time
3. 有限等待(Bounded waiting)：当一个进程提出要进入临界区请求后，只需要等待临界区被使用有上限的次数后，该进程就可以进入临界区。即进程不应该饿死在临界区入口处。
   * processes cannot be made to wait indefinitely

#### 2. 禁止中断(disabling interrupts)

1. 在单处理机中并发进程不能重叠执行，它们只能被插入，而且进程将继续执行直到它调用操作系统服务或被中断，所以，为保证互斥，禁止进程被中断就已足够。
2. 因为临界点不能被中断，互斥就能得到保证。
3. 缺点：
   * 代价高，执行效率降低，因为处理机受到不能插入的限制。
   * 不适用于多处理机系统。

#### 3. 忙等待(busy waiting)

1. 持续测试某个变量直到该变量变为特定值。
2. 缺点：
   * 浪费CPU时间。

#### 4. 自旋锁(spin lock)

1. 利用了忙等待（即自旋）的锁机制称为自旋锁，或者说自旋锁就是忙等待。
2. 下面提到的利用test_and_set方法和compare_and_swap方法的两个例子以及Peterson算法，都可称为自旋锁。
3. 缺点：
   * 造成死锁
   * 过多占用cpu资源
4. 优点：
   * 防止上下文切换
   * 较适用于锁使用者保持锁时间比较短的情况。

#### 5. 互斥锁(mutex)

1. 互斥锁的实现方式之一就是自旋锁。

2. 一个互斥锁就是一个可共享的变量，有两种状态：“锁定”(locked)和“非锁定”(unlocked)

3. 两个原子函数来操作互斥锁：

   * acquire()：在进程进入临界区前调用，将bool值设置为false

     ```c
     acquire() {
       while (!available) ; // busy wait
       available = false;
     }
     ```

   * release()：在某进程退出临界区后调用，将bool值设为true

     ```c
     release() {
       available = true;
     }
     ```

   * available

   * 这两个函数必须是原子操作

   * 这种互斥锁也称为自旋锁，因为调用者会进入忙等待

   * 优缺点同自旋锁

4. 真正的互斥锁与上述不同的是，调用者在锁没释放之前会进入阻塞或睡眠状态，而不是进入忙等待，无限循环地去测试锁是否释放。

```c
acquire() {
  while (!available) {
    // 进入睡眠或阻塞
    // 直到锁释放了再唤醒
  }
}
```

5. 必须是同一个进程上锁和解锁（与之后的信号量比较）

6. 缺点：
   * 需要涉及上下文切换，开销比自旋锁大

#### 6. 死锁(deadlock)

1. 是指两个或两个以上的进程在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象，若无外力作用，它们都将无法推进下去。此时称系统处于死锁状态或系统产生了死锁，这些永远在互相等待的进程称为死锁进程。
2. 规范定义：集合中的每一个进程都在等待只能由本集合中的其他进程才能引发的事件，那么该组进程是死锁的。
3. 必要条件：
   * 互斥条件：指一段时间内某资源只由一个进程占用，即一个资源只能被一个进程使用
   * 请求与保持条件：指进程已经保持至少一个资源，但又提出了新的资源请求，而该资源已被其它进程占有，此时请求进程阻塞，但又对自己已获得的其它资源保持不放。
   * 不剥夺条件：指进程已获得的资源，在未使用完之前，不能被剥夺，只能在使用完时由自己释放。
   * 环路等待条件：指在发生死锁时，必然存在一个进程——资源的环形链，即进程集合{P0，P1，P2，···，Pn}中的P0正在等待一个P1占用的资源；P1正在等待P2占用的资源，……，Pn正在等待已被P0占用的资源。
4. 预防，解决方法，排除方法（还没讲&#x1F605;）

#### 7. Peterson算法(软件实现)

1. 假设LOAD和STORE两个指令都是原子的；也就是说，不能被打断

2. 两个进程Pi, Pj共享两个变量：

* <strong>int</strong> turn: 表明哪个进程是下一个进入临界区的
* <strong>boolean</strong> flag[2]: 表明某个进程准备好进入临界区，<strong>flag[i]</strong> = true表示进程Pi已经准备好进入临界区

3. 进程Pi和Pj的结构

   ```c
   // 进程Pi的结构
   do {
   	flag[i] = true; // i wants to enter critical section
   	turn = j; // allow j to access first
   	while (flag[j] && turn == j) ; // busy waiting
   	// 临界区
   	flag[i] = false;
   	// 剩余区
   }while (...);
   
   // 进程Pj的结构
   do {
   	flag[j] = true; // j wants to enter critical section
   	turn = i; // allow i to access first
   	while (flag[i] && turn == i) ; // busy waiting
   	// 临界区
   	flag[j] = false;
   	// 剩余区
   }while (...);
   ```

4. 该算法满足解决临界区问题的三个必须标准：互斥访问，进入（即不死锁），有限等待（即不饿死）

   * 互斥访问：变量turn只能有一个值

     * flag[i]和flag[j]都为true当Pi和Pj都想进入临界区
     * turn只能等于i或j其中一个值
     * while (flag[i] && turn == i)和while (flag[j] && turn == j)中只有一个为true，为true的那个进程进入忙等待(busy waiting)，而另一个进程可以进入临界区
     * 因此，最多只有一个进程可以进入临界区

   * 空闲让进：

     * <strong>情况一</strong>：

     * 假设Pj正在临界区，Pi正在忙等待
     * 当Pj退出临界区后（即没有进程在临界区），此时flag[j]变为false
     * while (flag[j] && turn == j) 将会停止Pi的忙等待，此时Pi就可以进入临界区
     * <strong>情况二</strong>：
     * 假设Pi和Pj都想进入临界区，即flag[i] = flag[j] = true
     * turn等于i或j => 假设turn = i
     * while (flag[j] && turn == j)终止，Pi进入临界区，Pj进入忙等待
     * Pi退出临界区，flag[i] = false
     * while (flag[i] && turn == i)终止，Pj进入临界区

   * 有限等待：Peterson算法显然让进程等待不超过1次的临界区使用，即可获得权限进入临界区。

     * 由上述情况可知，一个进程最多在另一个进程进入临界区一次后就能进入

5. 分析（“谦让式”）

   * 首先，如果是进程i第一次开始执行，那它可以顺利进入临界区，因为flag[j] = false，进程j还不想进入临界区。
   * 其次，如果进程i和进程j已经在并发执行了，它们的调度顺序是未知的，假设每个进程每次执行一行代码，交替执行。那先执行的进程就“赚了”，比如进程i先执行，那么它会先将turn“谦让”地设置为j，但接下来轮到进程j执行了，它也“谦让”地将turn设置为i。这时又轮到了进程i执行了，而且我们可以发现，while中第二个条件已经不满足了！这时进程i就进入了临界区！然后我们把情况一般化，不再假设每个进程交替地执行一行代码，只要一个进程后执行了turn = i;(或turn = j;)这条语句，那么另一个进程就可以进入临界区。（分析的时候重点关注一点：另一个进程到底想不想进入临界区？）

#### 8. 硬件同步方法

1. test_and_set()

   ```c
   // test and set method
   bool test_and_set(boolean* lock) {
     bool rv = *lock;
     *lock = true;
     return rv;
   }
   
   // Example of using test and set method
   do {
     // while the lock is in use (i.e. true)
     // apply busy waiting
     while (test_and_set(&lock)) ; 
     // lock from false to true, the loop terminates
     
     //critical section
     lock = false;
     //remainder section
   }while (...)
   ```

2. compare_and_swap()

   ```c
   // compare and swap method (version1)
   // 总是返回旧值(expected)，可在cas操作之后对其进行测试，以查看是否匹配旧值
   // 其他version返回bool值，来判断是否成功更新
   int compare_and_swap(int *lock, int expected, int new_value) {
     int temp = *lock;
     if (*lock == expected) {
       *lock = new_value;
     }
     return temp;
   }
   
   // Example of using compare and swap method
   do {
     // while the lock is in use (i.e. true or 1), applying busy waiting
     while (compare_and_swap(&lock, 0, 1) != 0) ;
     //lock from false to true or from 0 to 1, the loop terminates
     
     //critical section
     lock = false; // lock = 0
     //remainder section
   }
   ```

3. 上述两种方法都是由硬件保证同步的，即硬件保证这三条语句必须原子执行，中间不发生任何中断。如果中断，则会有竞争条件发生。

4. 如果上述两种方法被同时调用，则按顺序执行。

5. 缺点：

   * 利用了忙等待(busy waiting)
   * 可能造成死锁

#### 9. 信号量(Semaphores)

1. 实现由操作系统提供

2. 相比互斥量只能取0和1两个值，信号量可以为0-N，用来实现更加复杂的同步，互斥量可看作是信号量取只取0和1时的特殊情况

3. 信号量通过一个计数器控制对共享资源的访问，信号量的值是一个非负整数，所有通过它的线程都会将该整数减一。如果计数器大于0，则访问被允许，计数器减1；如果为0，则访问被禁止，所有试图通过它的线程都将处于等待状态。

4. 信号量定义：

   ```c
   typedef struct {
   	int value;
   	struct process * list;
   } semaphore
   ```

5. 两个原子函数用来操作信号量

   * wait()被调用当一个资源被需要时

     ```
     wait(semaphore * S) {
     	S -> value--;
     	if (S -> value < 0) {
     		add process to S -> list
     		block(); // system call
     	}
     }
     ```

   * signal()/post()被调用当一个资源释放

     ```
     signal(semaphore * S) {
     	S -> value++;
     	if (S -> value <= 0) {
     		remove a process P from S -> list
     		wakeup(P); // system call
     	}
     }
     ```

6. 调用wait()：当计数器小于0时，会阻塞进程

   * 进程进入**阻塞队列**
   * 进程状态由**运行**变为**阻塞**
   * 进程控制交由**进程调度器**

7. 调用signal()/post()：当计数器小于等于0时，从阻塞队列中移除进程

   * 进程状态由**阻塞**变为**就绪**

8. 负信号值代表有几个进程正在等资源
