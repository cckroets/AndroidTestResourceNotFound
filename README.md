# Android Test Plugin Resource Not Found Demo

Demonstration of `Resources$NotFoundException` exception when using `com.android.test` gradle plugin.

- `app` is a `com.android.app` module, which contains a `ExampleInstrumentedTest` in the `src/androidTest` source set.

- `android-tests` is a `com.android.test` module which contains a copy of the same test above.

- `mylibrary` is a `com.android.library` module. It is included in:
    - `app` via `implementation` and `androidTest`
    - `android-tests` via `implementation`

The dependencies from the test variants aren't necessary, but it demonstrates the issue.


The test attempts to get a string declared in the library module from the application's context using
`Resources::getString`. When this is executed in the `android-tests` module, an exception is thrown:

```
E/TestRunner: android.content.res.Resources$NotFoundException: String resource ID #0x7f0b001b
        at android.content.res.Resources.getText(Resources.java:348)
        at android.content.res.Resources.getString(Resources.java:441)
        at android.content.Context.getString(Context.java:578)
        at com.github.cckroets.android.tests.ExampleInstrumentedTest.useAppContext(ExampleInstrumentedTest.java:28)
        at java.lang.reflect.Method.invoke(Native Method)
        at org.junit.runners.model.FrameworkMethod$1.runReflectiveCall(FrameworkMethod.java:50)
        at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:12)
        at org.junit.runners.model.FrameworkMethod.invokeExplosively(FrameworkMethod.java:47)
        at org.junit.internal.runners.statements.InvokeMethod.evaluate(InvokeMethod.java:17)
        at org.junit.runners.ParentRunner.runLeaf(ParentRunner.java:325)
        at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:78)
        at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:57)
        at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
        at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
        at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
        at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
        at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
        at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
        at androidx.test.ext.junit.runners.AndroidJUnit4.run(AndroidJUnit4.java:104)
        at org.junit.runners.Suite.runChild(Suite.java:128)
        at org.junit.runners.Suite.runChild(Suite.java:27)
        at org.junit.runners.ParentRunner$3.run(ParentRunner.java:290)
        at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:71)
        at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:288)
        at org.junit.runners.ParentRunner.access$000(ParentRunner.java:58)
        at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:268)
        at org.junit.runners.ParentRunner.run(ParentRunner.java:363)
        at org.junit.runner.JUnitCore.run(JUnitCore.java:137)
        at org.junit.runner.JUnitCore.run(JUnitCore.java:115)
        at androidx.test.internal.runner.TestExecutor.execute(TestExecutor.java:56)
        at androidx.test.runner.AndroidJUnitRunner.onStart(AndroidJUnitRunner.java:392)
        at android.app.Instrumentation$InstrumentationThread.run(Instrumentation.java:2145)
```

