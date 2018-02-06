[![Release](https://jitpack.io/v/adrielcafe/KBus.svg)](https://jitpack.io/#adrielcafe/KBus) [![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-KBus-green.svg?style=flat)]( https://android-arsenal.com/details/1/6583)

# KBus
Super lightweight (13 LOC) and minimalistic (`post()`, `subscribe()`, `unsubscribe()`) EventBus written with idiomatic Kotlin and RxJava 2

## KBus in 3 steps

### 1. Define your events
```kotlin
// With data
data class ShowMessageEvent(val message: String)

// Without data
class OtherEvent
```

### 2. Add subscribers
```kotlin
override fun onStart() {
    super.onStart()
    KBus.subscribe<ShowMessageEvent>(this) {
        showMessage(it.message)
    }
    KBus.subscribe<OtherEvent>(this) {
        doSomething()
    }
}

override fun onStop() {
    super.onStop()
    KBus.unsubscribe(this)
}
```

Where:
* **ShowMessageEvent** - the event you're subscribing
* **this** - the subscriber (by default the current Activity/Fragment)
* **{ showMessage() }** - the subscription observer
* **it** - the event instance

#### Sticky events
If you want to post events between Activities/Fragments just subscribe in `onCreate()` (or `onPostCreate()`) and unsubscribe in `onDestroy()`
```kotlin
override fun onPostCreate(savedInstanceState: Bundle?) {
    super.onPostCreate(savedInstanceState)
    KBus.subscribe<ShowMessageEvent>(this) {
        showMessage(it.message)
    }
}

override fun onDestroy() {
    super.onDestroy()
    KBus.unsubscribe(this)
}
```

### 3. Post events
```kotlin
KBus.post(ShowMessageEvent("Hello KBus!"))
```

## Add KBus to your project
```gradle
repositories {
    maven { url "https://jitpack.io" }
}

dependencies {
    implementation 'io.reactivex.rxjava2:rxjava:2.1.7'
    implementation 'com.github.adrielcafe:KBus:1.0'
}
```