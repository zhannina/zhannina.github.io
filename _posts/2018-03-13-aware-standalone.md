---
layout: post
title: How to create a standalone Android application with Aware Applications sensor
---

I recently had to create a standalone android application with Aware applications sensor.
I summed up the steps in this post for a reference.

<li> Add the following variable to build.gradle (Project: Application) (it should be added just before buildscript{})

project.ext {
    aware_libs = (System.getenv("aware_libs") as String ?: "development-SNAPSHOT")
}
</li>

<li> Add Aware in build.gradle (Module: app). In order to do that inside the dependencies curly braces put:

api "com.github.denzilferreira:aware-client:$aware_libs" 
</li>

<li> Add jitpack repository in build.gradle (Module: app). For that inside repositories curly braces add:

 maven { url 'https://jitpack.io' } 
</li>
 
<li> Inside values folder create bools.xml file containing the following: (please remember these are all tags, they are not being displayed in my blog, but I will fix it later once I have)

< ?xml version="1.0" encoding="utf-8"? >

< resources>

    < item name="accessibility_access" format="boolean" type="bool">true</ item>
    < item name="standalone" format="boolean" type="bool">true</ item>
	
</ resources>
</li>

<li>
5. Override < service> tag in the Manifest file < application> tag:

< service

     android:name="com.aware.Applications"
     android:enabled="true"
     android:exported="true"
     android:permission="android.permission.BIND_ACCESSIBILITY_SERVICE"
     tools:replace="android:enabled">
	 
     < intent-filter>
	 
         < action android:name="android.accessibilityservice.AccessibilityService" />
         < category android:name="android.accessibilityservice.category.FEEDBACK_GENERIC" />
		 
	 </ intent-filter>
	 
     < meta-data
         android:name="android.accessibilityservice"
         android:resource="@xml/aware_accessibility_config" />
		 
</ service>
</li>


<li> Add permissions inside the Manifest file before the < application> tag:
< permission

        android:name="com.aware.READ_CONTEXT_DATA"
        android:description="@string/read_permission"
        android:icon="@drawable/ic_launcher_settings"
        android:label="Read AWARE&apos;s Context data"
        android:protectionLevel="normal" />
		
< permission

    android:name="com.aware.WRITE_CONTEXT_DATA"
    android:description="@string/write_permission"
    android:icon="@drawable/ic_launcher_settings"
    android:label="Write to AWARE&apos;s Context data"
    android:protectionLevel="normal" />
</li>

<li> Within the MainActivity (or service) where you want to use the sensor, start Aware:
either using the intent:

 Intent aware = new Intent(this, Aware.class);
 
 startService(aware);
		
or simply calling: 
 Aware.startAWARE(this);
 
</li>

<li> Start the Applications sensor:

 Aware.setSetting(this, Aware_Preferences.STATUS_APPLICATIONS, true);

 Applications.isAccessibilityServiceActive(getApplicationContext());
</li>

<li> Set Applications sensor observer:

Applications.setSensorObserver(new Applications.AWARESensorObserver() {
      @Override
      public void onForeground(ContentValues data) {
          Log.d(TAG, data.toString());
      }

      @Override
      public void onNotification(ContentValues data) {

      }

      @Override
      public void onCrash(ContentValues data) {

      }

      @Override
      public void onKeyboard(ContentValues data) {

      }

      @Override
      public void onBackground(ContentValues data) {

      }
	  
      @Override
      public void onTouch(ContentValues data) {

      }
});
</li>

Your application should be working fine now.
