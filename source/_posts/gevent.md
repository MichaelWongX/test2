---
layout: post
title: gevent
date: 2017-08-20 11:30:21
tags: python
---

一些关于gevent module的常见用法

## Greenlet objects ##
Greenlet is a light-weight cooperatively-scheduled execution unit. pass the target function and its arguments to Greenlet constructor and call start():

	g = Greenlet(myfunction, 'arg1', 'arg2', kwarg1=1)
	g.start()
	g = Greenlet.spawn(myfunction, 'arg1', 'arg2', kwarg1=1) # the other 	way to create and run greenlet


**class Greenlet**


Greenlet.__init__(run=None, *args, **kwargs)

    Greenlet constructor.
    Parameters:	
        args – The arguments passed to the run function.
        kwargs – The keyword arguments passed to the run function.
        run – The callable object to run. If not given, this object’s _run method will be invoked (typically defined by subclasses).
* Greenlet.value:hold the value returned by the function if the greenlet has finished successfully. or if it fnished in error,None
* Greenlet.exception:hold the exception instance raise by the function, or None
* Greenlet.start():schedule the greenlet to run in this loop iteration
* start_later(seconds)
* join(timeout=None):wait until the greenlet finishes or timeout expires
* get(block=True, timeout=None):return the result of greenlet has returned or re-raise exception
* kill(exception=GreenletExit, block=True, timeout=None)
* link(callback)
* link_value(callback):Like link() but callback is only notified when the greenlet has completed successfully.
## Spawn helpers ##
* spawn(function, *args, **kwargs):Create a new Greenlet object and schedule it to run function(*args, **kwargs). This can be used as gevent.spawn or Greenlet.spawn.
*  spawn_later(seconds, function, *args, **kwargs)
*  spawn_raw(function, *args, **kwargs):This returns a raw greenlet which does not have all the useful methods that gevent.Greenlet has. Typically, applications should prefer spawn(), but this method may occasionally be useful as an optimization if there are many greenlets involved.
## Stopping Greenlets ##
* kill(greenlet, exception=GreenletExit)
* killall(greenlets, exception=GreenletExit, block=True, timeout=None)
## Waiting ##
* wait(objects=None, timeout=None, count=None):Wait for objects to become ready or for event loop to finish.the objects can be  gevent.Greenlet,gevent.event.Event,gevent.lock.Semaphore,gevent.subprocess.Popen.
* iwait(objects, timeout=None, count=None):Iteratively yield objects as they are ready, until all (or count) are ready or timeout expired.
* joinall(greenlets, timeout=None, raise_error=False, count=None):return A sequence of the greenlets that finished before the timeout (if any) expired.
## gevent.pool or group ##
The Group class in this module abstracts a group of running greenlets. When a greenlet dies, it’s automatically removed from the group. All running greenlets in a group can be waited on with Group.join(), or all running greenlets can be killed with Group.kill().The Pool class, which is a subclass of Group, provides a way to limit concurrency.

## 参考 ##
[http://www.gevent.org/](http://www.gevent.org/ "gevent")