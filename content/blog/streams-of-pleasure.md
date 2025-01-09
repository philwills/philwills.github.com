---
title: "Streams of Pleasure"
date: 2011-08-31 
categories: scala
---
Sometimes you have to work with something that's slow and provides an interface you'd rather not deal with. Hopefully it's a little less dumb and more useful than:

```scala
object SlowErrorProne {
   val MAGIC_NUMBERS = List(3, 42, 666)

  def search(number: Int) = {
    Thread.sleep(500)
    if (MAGIC_NUMBERS contains number) number
    else throw new Exception
  }
}
```

but that will serve as an example.

Coming from Java if you wanted to use this to find a magic number from a list of candidates as efficiently as possible, you might be tempted to write something like:

```scala
object FailingNumberFinder {
  def findMagicNumber(numbers: Seq[Int]) : Option[Int] = {
    for(number <- numbers) {
       try {
         return Some(SlowErrorProne.search(number))
       } catch {
         case _ =>
       }
    }
    return None
  }
}
```

This doesn't work as you might expect:

```shell
 scala> FailingNumberFinder.findMagicNumber(1 to 5)
 res0: Option[Int] = None 
```

Thanks to De La Soul, we all know 3 is a magic number, so what's going on? Scala implements return statements using exceptions and so the catch block swallows the attempt to return early.

We can wrap the attempt to search in a function to avoid this:
```scala
object UglyNumberFinder {
  def findMagicNumber(numbers: Seq[Int]) : Option[Int] = {
    def attemptSearch(number: Int) = {
      try  Some(SlowErrorProne.search(number))
      catch { case _ => None }
    }

    for(number <- numbers) {
       val result = attemptSearch(number)

       result match {
         case Some(magic) => return Some(magic)
         case _ =>
       }
    }
    return None
  }
}
```

```shell
scala> UglyNumberFinder.findMagicNumber(1 to 5)
 res1: Option[Int] = Some(3)
```

but ultimately there are good reasons why using the return keyword in scala is considered bad form.

If we weren't worried how many times we called the troublesome function, we might instead use something more idiomatic, such as:

```scala
object SimpleMagicNumberFinder {
  def findMagicNumber(numbers: Seq[Int]) = {
    def attemptSearch(number: Int) = {
      try  Some(SlowErrorProne.search(number))
      catch { case _ => None }
    }

    val magicNumbers = numbers flatMap (
      attemptSearch(_)
    )
    magicNumbers.headOption
  }
}
```

Fortunately there's a simple way to maintain this style without having to accept the performance penalty:

```scala
object FasterMagicNumberFinder {
  def findMagicNumber(numbers: Seq[Int]) = {
    def attemptSearch(number: Int) = {
      try  Some(SlowErrorProne.search(number))
      catch { case _ => None }
    }

    val magicNumbers = Stream(numbers: _*) flatMap (
      attemptSearch(_)
    )
    magicNumbers.headOption
  }
}
```


Scala's [Stream](http://www.scala-lang.org/api/current/scala/collection/immutable/Stream.html) allows any collection to be wrapped such that their members are lazily evaluated and the standard functions for transforming collections, such as map and flatMap will also be evaluated lazily across the collection, so that we only call the slow function the number of times absolutely necessary.