# LingoHub Android SDK

Master translation and connect with world leading translators. Automate and optimize your translation workflow with [LingoHub](https://lingohub.com).

## Features

- Over-The-Air localization update
- Localization testing using preproduction builds

## Requirements

- minSdk 16

## Installation

```groovy
dependencies {
    ...
    implementation('com.lingohub.sdk:1.0.0-alpha01') {
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

```java

public class MyApplication extends Application {
    ...
    @Override
    public void onCreate() {
        super.onCreate();

        LingoHub.init(this, "<api key>", "<project id>">);
    
        // Add following line only if you want to use pre-production bundles.
        LingoHub.setPreproductionEnabled();
        
        // Fetch the latest translations from LingoHub (can be called anywhere)
        LingoHub.fetchStrings()
    }
    ...
}
```

LingoHub needs to be wrapped around the activity context in order to replace strings.
We recommend implementing a BaseActivity class from which all your other Activity classes extend.

```java
abstract public class BaseActivity extends AppCompatActivity {

    @Override
    protected void attachBaseContext(Context newBase) {
        super.attachBaseContext(LingoHub.INSTANCE.wrap(newBase));
    }
    
}
```

That's it!

In case you are loading strings from parts of your application where LingoHub is not injected (e.g. custom views) you can load strings manually:
```java
LingoHubResources lingoHubResources = new LingoHubResources(context);
String myString = lingoHubResources.getString(R.string.my_string);
```


### Preproduction mode

If you would like to test your localizations before submitting a new package, enable preproduction mode.

```java
LingoHub.setPreproductionEnabled();
```

## Support

For bug reports, please create a new Issue right here on Github. Otherwise have a look at our [Contact options](https://lingohub.com/support)
