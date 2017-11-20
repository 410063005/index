

# how to use auto close resource

[ref](https://stackoverflow.com/questions/26969800/try-with-resources-in-kotli)

```java
OutputStreamWriter(r.getOutputStream()).use {
    // by `it` value you can get your OutputStreamWriter
    it.write('a')
}
```

# how to access package method from java
[ref](https://kotlinlang.org/docs/reference/java-to-kotlin-interop.html)


```kotlin
// example.kt
package demo

class Foo

fun bar() {
}
```

```java
// Java
new demo.Foo();
demo.ExampleKt.bar();
```

# how to use kotlin android extensions

[ref](https://antonioleiva.com/kotlin-android-extensions/)
