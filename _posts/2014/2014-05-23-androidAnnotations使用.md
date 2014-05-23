---
layout: post
title: androidAnnotations使用
categories:
- program
tags:
- android
---

发现了一个好玩又好用的库[androidannotations](https://github.com/excilys/androidannotations)。

用处：简化代码，减少累赘的 `findViewById`，`setViewContent`，`setOnClickListen`。简单方便的线程模型。

	@EActivity(R.layout.translate) // Sets content view to R.layout.translate
	public class TranslateActivity extends Activity {

		@ViewById // Injects R.id.textInput
		EditText textInput;

		@ViewById(R.id.myTextView) // Injects R.id.myTextView
		TextView result;

		@AnimationRes // Injects android.R.anim.fade_in
		Animation fadeIn;

		@Click // When R.id.doTranslate button is clicked 
		void doTranslate() {
			translateInBackground(textInput.getText().toString());
		}

		@Background // Executed in a background thread
		void translateInBackground(String textToTranslate) {
			String translatedText = callGoogleTranslate(textToTranslate);
			showResult(translatedText);
		}

		@UiThread // Executed in the ui thread
		void showResult(String translatedText) {
			result.setText(translatedText);
			result.startAnimation(fadeIn);
		}

		// [...]
	}

原理：生成了子类`TranslateActivity_`，所以AndroidManifest.xml文件里面要写成`android:name="com.my.temp3.app.MainActivity_"`。

使用：我是在Android Studio里面集成，照着这个[blog](http://www.jayway.com/2014/02/21/androidannotations-setup-in-android-studio/)的做法，分分钟集成完毕。用中文总结一下就是：

1.在app/build.gradle添加下面的代码，并sync

	buildscript {
		repositories {
			mavenCentral()
		}
		dependencies {
			classpath 'com.neenbedankt.gradle.plugins:android-apt:1.2+'
		}
	}

	apply plugin: 'android'
	apply plugin: 'android-apt'

	android {
		compileSdkVersion 19
			buildToolsVersion "19.0.1"

			defaultConfig {
				minSdkVersion 8
					targetSdkVersion 19
					versionCode 1
					versionName "1.0"
			}
		buildTypes {
			release {
				runProguard false
					proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.txt'
			}
		}
	}

	apt {
		arguments {
			androidManifestFile variant.processResources.manifestFile
				resourcePackageName "com.example.package.name"
		}
	}

	dependencies {
		apt "org.androidannotations:androidannotations:3.0+"
			compile "org.androidannotations:androidannotations-api:3.0+"
			compile 'com.android.support:appcompat-v7:+'
			compile fileTree(dir: 'libs', include: ['*.jar'])
	}

2.修改AndroidManifest.xml

	<activity
	    android:name=".MainActivity_"  // 原来的Activity类加上下划线
	    android:label="@string/app_name" >
	    <intent-filter>
		<action android:name="android.intent.action.MAIN" />
		<category android:name="android.intent.category.LAUNCHER" />
	    </intent-filter>
	</activity>


