# 项目规范

[TOC]

## 一、Android项目中APP目录的结构

**注意：一定要把git仓库建在app目录下！！切记切记！！**

```bash
.
│  .gitignore
│  app.iml
│  build.gradle # 构建脚本，类似于Makefile（注意，这是APP目录下的build.gradle，不是根目录下的）
│  proguard-rules.pro
│  README.md
├─build # 二进制文件
├─libs # 第三方包/库
└─src # 源文件
    ├─androidTest # AndroidTest
    │  └─java
    ├─main # 源码
    │  │  AndroidManifest.xml # 清单文件，包括app和activity的配置
    │  ├─java # Java代码
    │  └─res # 可引用的resources
    │      ├─drawable # 图片
    │      │      ic_launcher_background.xml
    │      │      
    │      ├─drawable-v24 # 图片
    │      │      ic_launcher_foreground.xml
    │      │      
    │      ├─layout # 纵屏布局
    │      │      activity_main.xml
    │      │      
    │      ├─layout-land # 横屏布局
    │      │      activity_main.xml
    │      │      
    │      ├─mipmap-anydpi-v26 # 图标
    │      │      ic_launcher.xml
    │      │      ic_launcher_round.xml
    │      │      
    │      ├─mipmap-hdpi # 图标
    │      │      ic_launcher.png
    │      │      ic_launcher_round.png
    │      │      
    │      ├─mipmap-mdpi # 图标
    │      │      ic_launcher.png
    │      │      ic_launcher_round.png
    │      │      
    │      ├─mipmap-xhdpi # 图标
    │      │      ic_launcher.png
    │      │      ic_launcher_round.png
    │      │      
    │      ├─mipmap-xxhdpi # 图标
    │      │      ic_launcher.png
    │      │      ic_launcher_round.png
    │      │      
    │      ├─mipmap-xxxhdpi # 图标
    │      │      ic_launcher.png
    │      │      ic_launcher_round.png
    │      │      
    │      ├─values # 引用值（map），包括颜色、字符串和样式，用于resource复用
    │      │      colors.xml
    │      │      strings.xml
    │      │      styles.xml
    │      │      
    │      └─values-zh-rCN # 汉化配置
    │             strings.xml
    │              
    └─test # junit
        └─java
```

## 二、根目录下build.gradle的规范（示例）

```go
buildscript {
    
    repositories {
        google()
        jcenter()
    }
    dependencies {
        //noinspection GradleDependency
        classpath 'com.android.tools.build:gradle:3.2.0' // 必须是3.2.0，不可更改
    }
}

allprojects {
    repositories {
        google()
        // maven { url "https://maven.google.com" }
        // 二者均可，哪个能编译成功就选哪个
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

## 三、app目录下build.gradle的规范（示例）

```go
apply plugin: 'com.android.application'

android { // 这个块中为必须遵守项，不能改动任何版本号。另外applicationId必须和GitHub中统一
    compileSdkVersion 28
    defaultConfig {
        applicationId "com.example.mytest"
        minSdkVersion 23
        targetSdkVersion 28
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    // 以下5个依赖必须丝毫不差，防止出现版本冲突。其他依赖，和GitHub保持一致。
    implementation 'com.android.support:appcompat-v7:28.0.0'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
    // 其他依赖...
}

```

## 四、.gitignore的内容

```
/build
app.iml
proguard-rules.pro
```

**有且只能有这三项**

## 五、AndroidManifest.xml中的注意事项

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.example.mytest">

    <application
        android:allowBackup="false"
        android:resizeableActivity="false"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme"
        tools:ignore="GoogleAppIndexingWarning"
        tools:targetApi="n">
        <!-- application标签中的注意事项：
             一、allowBackup必须为false
             二、必须有 android:resizeableActivity="false"
             三、必须有 tools:targetApi="n"
        -->
        <activity android:name=".MainActivity" android:screenOrientation="portrait">
            <!-- 每一个activity标签必须有 android:screenOrientation="portrait"，只支持竖屏 -->
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

