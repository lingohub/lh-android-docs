# LingoHub Android SDK

[![](https://jitpack.io/v/com.lingohub/lh-android-framework.svg)](https://jitpack.io/#com.lingohub/lh-android-framework)

## Features

- Over-The-Air localization update
- Localization testing using preproduction builds

## Requirements

- minSdk 16

## Installation

Add the jitpack maven repository to your top level .gradle file:

```groovy
allprojects {
    repositories {
        ...
        maven {
            url 'https://jitpack.io'
        }
    }
}
```

Now, add the LingoHub dependeny to your application level .gradle file:
```groovy
dependencies {
    ...
    implementation('com.lingohub.sdk:1.0.0') {
        transitive = true
    }
}
```

## Usage

### Configuration

Configure the LingoHub SDK in your `AppDelegate` with:

- *Application Context*
- *API Key* 
- *Project Id* 

```kotlin
class App : Application() {

    override fun onCreate() {
        super.onCreate()
        
        LingoHub.configure(this, "<api key>", "<project id>")
        
        // Add following line only if you want to use pre-production packages.
        LingoHub.setPreproductionEnabled()
        
        // Fetch the latest translations from LingoHub
        LingoHub.fetchStrings()
}
```

LingoHub needs to be wrapped around the activity context in order to replace strings.
We recommend implementing a BaseActivity class from which all your other Activity classes extend.

```kotlin
abstract class BaseActivity : AppCompatActivity() {

    override fun attachBaseContext(newBase: Context) {
        super.attachBaseContext(LingoHub.wrap(newBase))
    }
}
```

That's it!

In case you are loading strings from parts of your application where LingoHub is not injected (e.g. custom views) you can load strings manually:
```kotlin
val lingoHubResources = LingoHubResources(this)
val myString = lingoHubResources.getString(R.string.my_string)
```


### Preproduction mode

If you would like to test your localizations before submitting a new package, enable preproduction mode.

```kotlin
LingoHub.setPreproductionEnabled()
```

## Support

For bug reports, please create a new Issue right here on Github. Otherwise have a look at our [Contact options](https://lingohub.com/support)
