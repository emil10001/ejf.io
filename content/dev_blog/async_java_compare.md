+++
date = "2015-09-08T07:04:08-07:00"
title = "Comparison of Asynchronous Data Loading in Java"

+++

<img alt="threads" width="75%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/threads.png">

Recently, I was working on a problem at work that had some blocking calls that I thought I may not want to block on. My first instinct was to throw those requests into a thread-pool, but I also needed to get the values out of them. There are some obvious ways of doing this, but they all involve different trade-offs.

I'm going to compare a few different methods of doing asynchronous work in Java, and try to think about the pros and cons of each. What I will not say is what you _should_ use, since that's going to be application-specific. This is also not intended to be an exhaustive discussion, since I simply don't have that kind of time, and I'm sure that there are many ways of doing this. Instead, I will try to focus on the most common methods. This is also going to be about Java, not Android, Android has some fancy-pants thread handling stuff that is not present in Java.

## Futures

If you need to have the value returned to the calling thread, you'll probably need to use `Future`. This may involve some amount of blocking I/O. Here's the basic flow for a Future:

1. create a reference to your future, and give it a type
1. pass a `Callable` to a thread-pool, and make the return type of the callable match your future
1. get the value out of your future

Here's a code example:

    Future<String> future = threadpool.submit((Callable<String>)() -> getString());
    // blocks until the callable returns
    String myString = future.get();

Here's the rub with that, `future.get()` is a blocking call, so while you've run `getString()` on another thread, you're still blocking the calling thread.

If you don't need the value right away, you can avoid calling `get()`, and periodically check `future.isDone()`. Here is an example of that:

    Future<String> future = threadpool.submit((Callable<String>)() -> getString());
    // do some work while the task is running
    doSomeWork();
    anotherMethodCall();
    if (!future.isDone())
        System.out.println("still need a blocking call");
    // blocks until the callable returns
    String myString = future.get();


There are cases where this is acceptable, or even preferred. For example, if the threadpool represents some resource that you'd like to tightly bound (e.g. a set of 10 threads for doing network requests), and you actually need the response before you can do any more work on the calling thread, then this pattern makes sense.

### pros

* returns your value back to the calling thread
* fairly straightforward threading model
* you know exactly when in your flow you will have your value

### cons

* very likely to block the calling thread

## Callbacks and Wrapper Classes

A second possible way to handle this problem is to create a callback or wrapper class (or wrapper interface), which provides some known method that can be called from within the thread-pool after the initial work is done.

High-level view of this method:

1. create a method or runnable to call
1. add a call back to the callback in the task submitted to the thread-pool

The biggest problem with this method is that you may not be able to, or want to modify the task running in the thread-pool to call your callback. It ties the two together, making the task more tailored for one specific application.

Let's take a look at how this might work:

    // first, define our callback class
    static class Callback implements Runnable {
        private String result;

        public void setResult(String s) {
            result = s;
        }

        @Override
        public void run() {
            handleResponse(result);
        }
    }

    public static void handleResponse(String result) {
        System.out.println("got result: " + result);
    }

    void doAsyncWork(){
      Callback callback = new Callback();
      threadpool.submit(() -> {
          callback.setsetResult(getString());
          callback.run();
      });
    }

This is quite a bit more verbose than the `Future` solution, however it does avoid the issue of blocking the caller thread. While this is non-blocking, it is not terribly flexible, as you need to know something about the callback in the task that's running on the thread-pool. Ideally, you should be able to separate these things out more.

### pros

* non-blocking

### cons

* does not return to the calling thread
* verbose
* messy
* brittle

## Observers

`Observers` are a little more verbose than Callbacks, but can be much more flexible. The key to Observers that makes them more flexible is that they separate out the retrieval of a value from the logic of handling that data. You can also have multiple Observers watching an Observable.

Observers provide more separation, and may be able to be made more generic with less effort than a callback. However, you still need to create a class and a method that you'll use for setting the data and kicking off the Observer.

High-level view of this method:

1. create an `Observable` class
1. implement `Observer`
1. set the value in the Observable in the task submitted to the thread-pool

Let's look at an implementation.

    protected static class ContentObserver implements Observer {

        @Override
        public void update(Observable o, Object arg) {
            if (!(o instanceof ObservableContent))
                return;

            System.out.println("update result" + ((ObservableContent) o).getContent());
        }
    }

    protected static class ObservableContent extends Observable {
        private String content = null;

        public ObservableContent() {
        }

        public void setContent(String content) {
            this.content = content;
            setChanged();
            notifyObservers();
        }

        public String getContent() {
            return content;
        }

    }

    public static void doAsyncWork(ObservableContent content) {
        threadpool.submit(() -> {
            content.setContent(getString());
        });
    }


### pros

* non-blocking
* flexible

### cons

* does not return to the calling thread
* verbose

## RxJava

While [RxJava](https://github.com/ReactiveX/RxJava) is not a part of the Java language, I think that it is interesting, and seems to be quickly gaining popularity. RxJava takes the observer pattern a step further, and greatly simplifies it.

RxJava is nearly its own DSL (Domain Specific Language), so there's a lot available with RxJava.

High-level view of this method:

1. create a RxJava `Observable`
1. tell it what to do
1. tell it what thread to do the work on
1. give it a `Subscriber`
1. tell the Subscriber which thread to work on

It's easier than it sounds, let's take a look.

    public static void handleResponse(String result) {
        System.out.println("got result: " + result);
    }

    Observable.just(0)
        .map(i -> request())
        .subscribeOn(Schedulers.from(threadpool))
        .observeOn(Schedulers.io())
        .subscribe(c -> handleResponse(c));

I'm not going to explain how to use RxJava here, but I will say that [Dan Lew has a great series on RxJava on his blog](http://blog.danlew.net/2014/09/15/grokking-rxjava-part-1/).

### pros

* non-blocking
* able to return to different threads
* very flexible
* compact
* simple

### cons

* does not return to the calling thread
* adds a dependency
* requires learning how to use RxJava

## Conclusion

I think that I'm going to give RxJava a try next time I need a non-blocking asynchronous data loading in Java. It still depends on the application, without the Java 8 lambdas, RxJava is still fairly verbose, similar to Java Observers. There are also considerations around whether or not you want to take dependencies in your application, and which specific dependencies you want to take. If I didn't want the dependency, I would probably use Java's built-in Observers. However, if I need that return value on the same thread, it's still going to be Futures, and blocking the calling thread.

If you have a better way of doing this, especially getting the value back on the same thread without blocking, I'd be interested to hear about it. (Again, for Java, not Android, as Android does not have the same constraints.)

[I wrote up a full set of examples of all the above here.](https://github.com/emil10001/async-java-futures)
