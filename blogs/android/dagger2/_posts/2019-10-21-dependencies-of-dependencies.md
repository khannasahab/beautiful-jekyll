---
layout: post
title: Dependencies of Dependencies
subtitle: Dagger 2, A better management
categories: [android]
tags: [android, dagger2, app development]
share_request: true
subscribe_email: true
---

### `This post is for Android engineers who are familiar with Dagger2`

{: .box-plain}
There are always multiple ways to do anything in software development. Dagger2 is no exception. Every way has its own pro and con. A better understanding of the tool helps us in taking an informed decision. This post is about `dependencies of dependencies (D.o.D)`.

Before we proceed, let's see the basic terminologies we use in Dagger2:

1. `@Module`: The class which offers bare bones objects/dependencies.
2. `@Component`: The interface which exposes the top-level dependencies. 

A Component uses modules to offer the top-level dependencies to app. The scope of the component is dependent upon the scope of the module offerings.

Module is where we create `provideXXX` functions and we create the objects (dependency) needed for top-level dependencies. A few examples of a modules are, `NetworkModule`, `ContextModule`, `ImageLoadModule` etc.

One way is to create an app level module and have all the objects/dependencies of the app created in that but this solution does not scale as app level components are supposed to outlive `Activities`, It is always better to create scoped components whose life is same as the area of the app using them. Let's see this example:

{: .text-center}
![Network Module Dagger 2](/img/dagger2/networkmodule.png)

### and we have AppComponent which is using Network module to offer a dependency `Retrofit` (singleton app scoped)

```
@Singleton
@Component(modules = [NetworkModule::class])
interface AppComponent {
    fun getRetrofit(): Retrofit
}
```

### Now assume we have an Activity level component and a module:

```
@Module
class MainActivityModule(private val activity: MainActivity) {
    @Provides
    @MainActivityScope
    fun provideViewModel(retrofit:Retrofit): MainViewModel {...)
    }
}
```
### provideViewModel needs a dependency `retrofit`, below is the component and scope

```
@MainActivityScope
@Component(modules = [MainActivityModule::class])
interface MainActivityComponent {
    fun inject(activity: MainActivity)
}

@Scope
@Retention(AnnotationRetention.RUNTIME)
annotation class MainActivityScope
```

## Problem statement
How do I organise my code when a dependency has a dependency which is defined in other module? In our case, `MainActivityModule` needs a dependency from `NetworkModule`

## Solutions

### Solution 1

{: .box-plain}
`Solution 1`: Merge all the dependencies in 1 module and use the module in multiple components. It is a dirty solution. We will have `duplicate code` all around. ❌ ❌

### Solution 2

{: .box-plain}
`Solution 2`: Use `includes` property of the `@module` annotation. In our case, the `MainActivityModule` will include `NetworkModule` using `includes`, 

```
@Module(includes = [NetworkModule::class])
class MainActivityModule(private val activity: MainActivity) {...}
```

{: .box-bordered}
This solution does not have duplicate code but it has a serious flaw. Even though the dependency is marked `Singleton/Scoped` in network module, the `MainActivity Component` will generate its own object graph and we will end up in multiple objects of the singleton dependencies. Remember `Scope applies to the component level`. In our case, everytime the MainActivity will create MainActivityComponent, a new Retrofit and corresponding dependencies will be created. ❌

{: .box-plain}
`Solution 2.1`: Use `multiple-modules` with a component. In our case, we could do following:

```
@Component(modules = [MainActivityModule::class, NetworkModule::class])
interface MainActivityComponent {...}
```

{: .box-bordered}
Our `MainActivityComponent` is using 2 modules this time. We have the same problem here, everytime the MainActivity will create MainActivityComponent, a new Retrofit and corresponding dependencies will be created. ❌

### Solution 3

{: .box-plain}
`Solution 3`: Use the `dependencies` property in `@Component`. Dagger 2 understands the concept of management and `D.o.D`, We can mark a component dependent upon other component with a simple syntax:

```
@Component(dependencies = [AppComponent::class], modules = [MainActivityModule::class])
interface MainActivityComponent {...}
```

{: .box-plain}
Here, we marked our `MainActivityComponent` is using the module `MainActivityModule` but it is `dependent` upon another component `AppComponent`. In this case, `AppComponent` is using the NetworkModule and Application class is responsible to create the instance of `AppComponent` (The app level dependencies)<br/><br/>The catch here is we when the Activity will create the instane of `MainActivityComponent` it need to pass instance of `AppComponent` as a dependency. Activity can get it from the application class:

#### Inside Activity

```
val appComponent = (application as MyApp).appComponent
val activityComponent = DaggerMainActivityComponent.builder()
            .mainActivityModule(MainActivityModule())
            .appComponent(appComponent)
            .build()
```

Here is the code of your application class which is responsible for app level dependencies:

```
lateinit var appComponent: AppComponent

    override fun onCreate() {
        super.onCreate()
        appComponent = DaggerAppComponent.builder().appModule(AppModule(this)).build()
    }
```

{: .box-bordered}
Now, we are explicitly passing `appComponent` in the `MainActivityComponent` and dagger is intelligent enough to use the requried dependencies from here. ✅

### Solution 4

{: .box-plain}
`Solution 4`: The last solution is to use `subcomponent`, As we know our ScopedComponents like `MainActivityComponent` in this example is dependent on the `App-Level-Dependencies`, we can also think them as subcomponent in the entire system. Instead of marking them component, we can mark them subcomponent:

```
@Subcomponent(modules = [MainActivityModule::class])
interface MainActivityComponent {...}
```
{: .box-plain}
In this case, Dagger will not generate a class for our `MainActivityComponent` but we have to create a new function in our `AppComponent` interface:

```
@Component(modules = [AppModule::class])
interface AppComponent {
    fun getRetrofit(): Retrofit
    fun mainActivityComponent(mainModule:MainActivityModule):MainActivityComponent
}
```

Notice, we have another function offering `MainActivityComponent` in our `AppComponent`, All we need is to use the `appComponent` instance everytime we need the `MainActivityComponent` instance inside our activity:

```
val appComponent = (application as MyApp).appComponent
val activityComponent  = appComponent.mainActivityComponent(MainActivityModule(this))
```

{: .box-bordered}
Now, we are explicitly marking  `MainActivityComponent` as `SubComponent` and offering a function in the `MAppComponent` and dagger is intelligent enough to use the requried dependencies from here. ✅ ✅ No duplicacy, full readibility and Scoped dependencies.

#### Note: Dagger has a restriction of Scopes. The dependent and subcomponent, Components can not use same scope. A scope is nothing but a way to tell dagger: What is lifespan of a dependency.

## Which solution is better?

Clearly solution 1 and 2 do not fit the bill. In first solution with `includes` it leads to duplicate dependencies and same goes to `multi-module` with component. 

Solution 3 and 4 respect the scope defined and they help you to re-use the app-level dependencies in the Activity level dependencies. I do not have a clear winner but If I have to choose, I will opt the solution 4 which is more readable in my eyes. You are free to choose any.


