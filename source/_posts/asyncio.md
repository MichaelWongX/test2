---
layout: post
title: asyncio
date: 2017-08-20 11:30:21
tags: python
---


**asyncio 学习笔记**



## coroutines ##
coroutines used with asyncio may be implemented using the **asyn def** statement or by using **generators** . The **async def** type of coroutine was added in Python 3.5.
Generator-based corontines should be decorated with **@asyncio.coroutine** , although this is not strictly enfored.

The word "corontine", like the word "generator", is used for two different concepts:
* the function that defines a coroutine, using **async def** or decorated with **@asyncio.coroutine**.
* the object obtained by calling a coroutine function.

Things a coroutine can do:

* `result = await future` or `result = yield from futur`, suspends the coroutine  until the future is done, then return the future's result, or raises an exception, which will be porpagated. (if the future is cancdelled, it will raise a `CancelledError` exception. the yielf from can automatically deal with StopIteration.
* `result = await coroutine` or `result = yield from coroutine`, wait for another coroutine or produce a result(or raise an exception, which will be propagated). The coroutine expression must be a call to another coroutine.
* `return expression` produce a result to the coroutine that is waiting for this one using await or yield from
* `raise exception` raise an exception in the coroutine that is waiting for this one using await or yield from.

## run an event loop ##
* AbstractEventLoop.run_forever()
	run until stop() si called. if stop() is called before run_forever() is called, this polls the I/O selector once with a timeout of zero. run all callbacks scheduled in response to I/O events (and those that were already scheduled) and then exists. If stop（） is called while run_forever() is running, this will run the current batch of callbacks and then exits.
* .run_until_complete(future)
	run untile the Future is done. If the argument is a coroutine object, it is wrapped by ensure_future(). Return the Future's result, or raise its exception.
* .stop()
* .close()
* .run_in_executor(f)

## calls ##
most asyncio functions don't accept keywords. use functools.partical() to pass keywords.
* AbstractEventLoop.call_soon（callback, *args).
* .call_soon_threadsafe(callback, *args)
	like call_soon(), but thread safe.
* .call_later(delay, callback, *args)
	callback after the given delay seconds
* .call_at(when, callback, *args)

## Future ##
class asyncio.Future(*, loop=None)
This class is almost compatible with  concurrent.
* .result()
	Return the result this future represents.If the future has been cancelled, raises CancelledError. If the future’s result isn’t yet available, raises InvalidStateError. If the future is done and has an exception set, this exception is raised.
* .exception()
	Return the exception that was set on this future.
* .add_done_callback(fn)
	Add a callback to be run when the future becomes done.

futures.Future.
AbstractEventLoop.create_future() creat Future object attached to the loop

## Tasks ##
AbstractEventLoop.create_task(coro),Schedule the execution of a coroutine object: wrap it in a future. Return a Task object.
**Don’t directly create Task instances: use the ensure_future() function or the AbstractEventLoop.create_task() method.**

asyncio.as_completed(fs, *, loop=None, timeout=None)
    Return an iterator whose values, when waited for, are Future instances.
asyncio.ensure_future(coro_or_future, *, loop=None)

    Schedule the execution of a coroutine object: wrap it in a future. Return a Task object.If the argument is a Future, it is returned directly.
coroutine asyncio.wait(futures, *, loop=None, timeout=None, return_when=ALL_COMPLETED)
	Wait for the Futures and coroutine objects given by the sequence futures to complete. Coroutines will be wrapped in Tasks. Returns two sets of Future: (done, pending).

## Executor ##
Call a function in an Executor (pool of threads or pool of processes). By default, an event loop uses a thread pool executor (ThreadPoolExecutor). coroutine AbstractEventLoop.run_in_executor(executor, func, *args) Arrange for a func to be called in the specified executor.

## 参考 ##
[https://docs.python.org/3/library/asyncio-eventloop.html](https://docs.python.org/3/library/asyncio-eventloop.html "asyncio-eventloop")

