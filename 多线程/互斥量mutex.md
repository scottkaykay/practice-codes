# C++ 互斥量Mutex

## Mutex头文件包含的内容

Mutex系列类： std::mutex, std::recursive_mutex,  std::time_mutex,  std::recursive_timed_mutex

Lock类：  std::lock_guard,  std::unique_lock  它们都是锁管理工具

函数： std::try_lock **尝试**给多个互斥量上锁  std::lock 同时给多个互斥量上锁   std::call_once如果多个线程要调用同一个函数，此函数可以保证多个线程\
对该函数只调用一次。

## std::mutex

最基本的互斥量，提供了独占所有权的特性，即不支持递归地对std::mutex对象上锁（就是加好几层锁，std::recursive_lock可以）。

### 成员函数

1. 构造函数：std::mutex不允许拷贝构造，也不允许 move 拷贝，最初产生的 mutex 对象是处于 unlocked 状态的。\
2. lock(),调用线程将锁住该互斥量。线程调用该函数会发生下面3种情况：\
i.互斥量还没上锁，调用该线程会将该互斥量锁住，直到调用unlock之前，该线程一直拥有这个锁。\
ii.如果当前互斥量被其他线程锁住，则当前的调用线程被阻塞住;
iii.如果当前互斥量被当前调用线程锁住，则会产生死锁(deadlock).

3.unlock() 解锁，释放对互斥量的所有权。

4. try_lock() 尝试锁住互斥量。如果互斥量被其他线程占有，则当前线程**也不会被阻塞**。线程调用该函数也会出现下面 3 种情况，\
i. 如果当前互斥量没有被其他线程占有，则该线程锁住互斥量，直到该线程调用 unlock 释放互斥量。\
ii.如果当前互斥量被其他线程锁住，则当前调用线程返回 false，而并不会被阻塞掉。\
iii. 如果当前互斥量被当前调用线程锁住，则会产生死锁(deadlock)。

## std::recursive_mutex

std::recursive_mutex 与 std::mutex 一样，也是一种可以被上锁的对象，但是和 std::mutex 不同的是，std::recursive_mutex 允许同一个线程\
对互斥量多次上锁（即递归上锁），来获得对互斥量对象的多层所有权，std::recursive_mutex 释放互斥量时需要调用与该锁层次深度相同次数的 unlock()，\
可理解为 lock() 次数和 unlock() 次数相同，除此之外，std::recursive_mutex 的特性和 std::mutex 大致相同。

## std::time_mutex

std::time_mutex 比 std::mutex 多了两个成员函数，try_lock_for()，try_lock_until()。

try_lock_for 函数接受一个**时间范围**，表示在这一段时间范围之内线程如果没有获得锁则被阻塞住（与 std::mutex 的 try_lock() 不同，try_lock 如果被调用时没有获得锁则直接返回 false），\
如果在此期间其他线程释放了锁，则该线程可以获得对互斥量的锁，如果超时（即在指定时间内还是没有获得锁），则返回 false。

try_lock_until 函数则接受一个**时间点**作为参数，在指定时间点未到来之前线程如果没有获得锁则被阻塞住，如果在此期间其他线程释放了锁，则该线程可以获得对互斥量的锁，\
如果超时（即在指定时间内还是没有获得锁），则返回 false。
