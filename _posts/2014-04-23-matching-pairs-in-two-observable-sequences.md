---
layout: post
title: Matching pairs in two observable sequences
comments: true
---

For one of my projects that makes heavy use of [Reactive Extensions](http://msdn.microsoft.com/en-us/data/gg577609.aspx), 
I needed to pair equal values in two observable sequences.

An example:

```csharp
var sequence1 = new[]{3, 1, 4, 2}.ToObservable();
var sequence2 = new[]{1, 2, 3, 4}.ToObservable();

var result = sequence1.MatchPair(sequence2).Subscribe(x => Console.WritleLine("{0} {1}", x.Left, x.Right));
```

This should print

```
1 1
3 3
2 2
4 4
```

The distinction between the left and right element must be made, as I want this to work on any class and base the equality on a key selector.

My end result looked was this (you can also find this code [here](https://github.com/flagbug/ReactiveMarrow/blob/master/ReactiveMarrow/ReactiveMarrow/ObservableExtensions.cs#L18)):

```csharp
public static IObservable<Pair<T>> MatchPair<T, TKey>(this IObservable<T> left, IObservable<T> right, Func<T, TKey> keySelector)
{
    return Observable.Create<Pair<T>>(o =>
    {
        var leftCache = new List<T>();
        var rightCache = new List<T>();
        var gate = new object();
        var disposable = new CompositeDisposable();
        bool leftDone = false;
        bool rightDone = false;

        left.Subscribe(l =>
        {
            lock (gate)
            {
                // Look for the last element, as we want FIFO
                int lastIndex = rightCache.FindLastIndex(x => EqualityComparer<TKey>.Default.Equals(keySelector(x), keySelector(l)));

                if (lastIndex > -1)
                {
                    T element = rightCache[lastIndex];
                    rightCache.RemoveAt(lastIndex);

                    o.OnNext(new Pair<T>(l, element));
                }

                else
                {
                    leftCache.Add(l);
                }
            }
        }, ex =>
        {
            o.OnError(ex);
            disposable.Dispose();
        }, () =>
        {
            lock (gate)
            {
                leftDone = true;

                if (rightDone)
                {
                    o.OnCompleted();
                }
            }
        }).DisposeWith(disposable);

        right.Subscribe(r =>
        {
            lock (gate)
            {
                // Look for the last element, as we want FIFO
                int lastIndex = leftCache.FindLastIndex(x => EqualityComparer<TKey>.Default.Equals(keySelector(x), keySelector(r)));

                if (lastIndex > -1)
                {
                    T element = leftCache[lastIndex];
                    leftCache.RemoveAt(lastIndex);

                    o.OnNext(new Pair<T>(element, r));
                }

                else
                {
                    rightCache.Add(r);
                }
            }
        }, ex =>
        {
            o.OnError(ex);
            disposable.Dispose();
        }, () =>
        {
            lock (gate)
            {
                rightDone = true;

                if (leftDone)
                {
                    o.OnCompleted();
                }
            }
        }).DisposeWith(disposable);

        return disposable;
    });
}

```

In addition we need the `Pair` structure that stores the left and right value

```csharp
public struct Pair<T>
{
    public Pair(T left, T right)
        : this()
    {
        this.Left = left;
        this.Right = right;
    }

    public T Left { get; private set; }

    public T Right { get; private set; }
}
```

and a small helper methid for the `CompositeDisposable` class
(Alternatively you can wrap the subscribe methods in a `disposable.Add`)

```csharp
public static T DisposeWith<T>(this T disposable, CompositeDisposable with) where T : class, IDisposable
{
    if(disposable == null)
        throw new ArgumentNullException("disposable");
    
    if(with == null)
        throw new ArgumentNullException("with");

    with.Add(disposable);

    return disposable;
}
```
