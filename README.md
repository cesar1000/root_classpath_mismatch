# Mismatched class loader hierarchy restored with CC causes build perf regression

The root project class loader configured when the configuration is restored from cache is mismatched with the class loader configuration without configuration caching. As a result, tasks and actions declared in the root project classpath are found to be out of date.

Running `./gradlew :lib:configure` shows the mismatched class loader hierarchy, and the resulting mismatched hash.

```
$ ./gradlew :lib:configure --no-configuration-cache

> Task :lib:configure
VisitableURLClassLoader(ClassLoaderScopeIdentifier.Id{coreAndPlugins:settings[:]:buildSrc[:]:root-project[:](export)})
CachingClassLoader(org.gradle.internal.classloader.MultiParentClassLoader@68766a54)
org.gradle.internal.classloader.MultiParentClassLoader@68766a54
VisitableURLClassLoader(ClassLoaderScopeIdentifier.Id{coreAndPlugins:settings[:](export)})
CachingClassLoader(FilteringClassLoader(VisitableURLClassLoader(legacy-mixin-loader)))
FilteringClassLoader(VisitableURLClassLoader(legacy-mixin-loader))
VisitableURLClassLoader(legacy-mixin-loader)
VisitableURLClassLoader(ant-and-gradle-loader)
VisitableURLClassLoader(ant-loader)
jdk.internal.loader.ClassLoaders$PlatformClassLoader@69474b88
VisitableURLClassLoader(ClassLoaderScopeIdentifier.Id{coreAndPlugins:settings[:]:buildSrc[:](export)})
VisitableURLClassLoader(ClassLoaderScopeIdentifier.Id{coreAndPlugins:settings[:](export)})
CachingClassLoader(FilteringClassLoader(VisitableURLClassLoader(legacy-mixin-loader)))
FilteringClassLoader(VisitableURLClassLoader(legacy-mixin-loader))
VisitableURLClassLoader(legacy-mixin-loader)
VisitableURLClassLoader(ant-and-gradle-loader)
VisitableURLClassLoader(ant-loader)
jdk.internal.loader.ClassLoaders$PlatformClassLoader@69474b88
CLASS LOADER HASH: 596fa144c4a61e0b0ed0df7e6d4893dd

BUILD SUCCESSFUL in 1s
1 actionable task: 1 executed

$ ./gradlew :lib:configure
Configuration cache is an incubating feature.
Reusing configuration cache.

> Task :lib:configure
VisitableURLClassLoader(ClassLoaderScopeIdentifier.Id{coreAndPlugins:settings[:]:buildSrc[:]:root-project[:](export)})
VisitableURLClassLoader(ClassLoaderScopeIdentifier.Id{coreAndPlugins:settings[:]:buildSrc[:](export)})
VisitableURLClassLoader(ClassLoaderScopeIdentifier.Id{coreAndPlugins:settings[:](export)})
CachingClassLoader(FilteringClassLoader(VisitableURLClassLoader(legacy-mixin-loader)))
FilteringClassLoader(VisitableURLClassLoader(legacy-mixin-loader))
VisitableURLClassLoader(legacy-mixin-loader)
VisitableURLClassLoader(ant-and-gradle-loader)
VisitableURLClassLoader(ant-loader)
jdk.internal.loader.ClassLoaders$PlatformClassLoader@69474b88
CLASS LOADER HASH: 521290de1513fd8c22827ca721cd644c

BUILD SUCCESSFUL in 1s
1 actionable task: 1 executed
Configuration cache entry reused.
```
