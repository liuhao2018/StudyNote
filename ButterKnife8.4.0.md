### butterKnife 8.4.0集成

project build.gradle

```
buildscript{
	repositories{
		mavenCentral()
	}
}
```

```
classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'
```

module build.gradle

```
apply plugin:'com.neenbedankt.android-apt'
```

```
 compile 'com.jakewharton:butterknife:8.4.0'
 apt'com.jakewharton:butterknife-compiler:8.4.0'
```

