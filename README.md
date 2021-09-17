# LingoHub Android SDK

[![](https://jitpack.io/v/com.lingohub/lh-android-sdk.svg)](https://jitpack.io/#com.lingohub/lh-android-sdk)

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
    implementation 'com.lingohub:lh-android-sdk:x.y.z'
}
```
(Please replace ```x.y.z``` with the latest version numbers: [![](https://jitpack.io/v/com.lingohub/lh-android-sdk.svg)](https://jitpack.io/#com.lingohub/lh-android-sdk))

## Usage

### Configuration

Configure the LingoHub SDK in your `Application` class with:

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

    private val lingohubDelegate: AppCompatDelegate by lazy {
        LingoHub.getAppCompatDelegate(this, super.getDelegate())
    }
    
    override fun getDelegate() = lingohubDelegate

}
```

That's it!

In case you are loading strings from parts of your application where LingoHub is not injected (e.g. custom views) you can load strings manually:
```kotlin
val lingoHubResources = LingoHubResources(this)
val myString = lingoHubResources.getString(R.string.my_string)
```
or if you need to load strings provided via custom attributes:
```kotlin
val lingoHubResources = LingoHubResources(context)
val typedArray = context.theme.obtainStyledAttributes(attrs, R.styleable.PieChart, 0, 0)
val stringId = typedArray.getResourceId(R.styleable.PieChart_myString, -1)
if(stringId != -1){
    textView.text = lingoHubResources.getString(stringId)
}
```

### Language switching

If you would like to change the language of your app at runtime, you can use the LingoHub SDK for it.

```kotlin
LingoHub.setLocale(locale)
```

### Preproduction mode

If you would like to test your localizations before submitting a new package, enable preproduction mode.

```kotlin
LingoHub.setPreproductionEnabled()
```

### Troubleshooting

If you can't get the SDK to work, use the LingoHubLogger class to find out the cause of the problem.

```kotlin
LingoHub.setLogger(object : LingoHubLogger {

    override fun onInfo(message: String) {
        Log.d("LH SDK", message)
    }
    
    override fun onError(error: String, cause : Throwable?) {
        Log.e("LH SDK", error, cause)
    }
   
})
```

## Support

For bug reports, please create a new Issue right here on Github. Otherwise have a look at our [Contact options](https://lingohub.com/support)
