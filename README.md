此项目是基于滴滴的VirtualAPK方案从AndroidStudio项目修改成Eclipse项目，并修改了其包名（原项目地址：https://github.com/didi/VirtualAPK）

--注意，使用此项目需要自行解决资源冲突的问题

使用教程
## Host Project

Host projects need to rely on sdk project, then Initialize `PluginManager` in `YourApplication::attachBaseContext()`.
    
``` java
@Override
protected void attachBaseContext(Context base) {
    super.attachBaseContext(base);
    PluginManager.getInstance(base).init();
}
```
    
Modify proguard rules to keep VirtualAPK related files. --> 混淆要求
    
``` java
-keep class com.cooee.virtualapk.internal.VAInstrumentation { *; }
-keep class com.cooee.virtualapk.internal.PluginContentResolver { *; }

-dontwarn com.cooee.virtualapk.**
-dontwarn android.content.pm.**
-keep class android.** { *; }
```


AndroidManifest need to add the following code.

```html
<!-- chenliang add start -->
<activity android:launchMode="standard" android:name=".A$1"/>
<activity android:launchMode="standard" android:name=".A$2" android:theme="@android:style/Theme.Translucent"/>
<activity android:launchMode="singleTop" android:name=".B$1"/>
<activity android:launchMode="singleTop" android:name=".B$2"/>
<activity android:launchMode="singleTop" android:name=".B$3"/>
<activity android:launchMode="singleTop" android:name=".B$4"/>
<activity android:launchMode="singleTop" android:name=".B$5"/>
<activity android:launchMode="singleTop" android:name=".B$6"/>
<activity android:launchMode="singleTop" android:name=".B$7"/>
<activity android:launchMode="singleTop" android:name=".B$8"/>
<activity android:launchMode="singleTask" android:name=".C$1"/>
<activity android:launchMode="singleTask" android:name=".C$2"/>
<activity android:launchMode="singleTask" android:name=".C$3"/>
<activity android:launchMode="singleTask" android:name=".C$4"/>
<activity android:launchMode="singleTask" android:name=".C$5"/>
<activity android:launchMode="singleTask" android:name=".C$6"/>
<activity android:launchMode="singleTask" android:name=".C$7"/>
<activity android:launchMode="singleTask" android:name=".C$8"/>
<activity android:launchMode="singleInstance" android:name=".D$1"/>
<activity android:launchMode="singleInstance" android:name=".D$2"/>
<activity android:launchMode="singleInstance" android:name=".D$3"/>
<activity android:launchMode="singleInstance" android:name=".D$4"/>
<activity android:launchMode="singleInstance" android:name=".D$5"/>
<activity android:launchMode="singleInstance" android:name=".D$6"/>
<activity android:launchMode="singleInstance" android:name=".D$7"/>
<activity android:launchMode="singleInstance" android:name=".D$8"/>
<service android:name="com.cooee.virtualapk.delegate.LocalService"/>
<service android:name="com.cooee.virtualapk.delegate.RemoteService" android:process=":daemon">
    <intent-filter>
        <action android:name="com.cooee.virtualapk.intent.ACTION_DAEMON_SERVICE"/>
    </intent-filter>
</service>

<provider android:authorities=".VirtualAPK.Provider" android:name="com.cooee.virtualapk.delegate.RemoteContentProvider" android:process=":daemon"/><!--name 需要与sdk的包名一致-->
<!-- chenliang add end -->
```
    
    
Finally, load an APK and have fun!
    
``` java
String pluginPath = Environment.getExternalStorageDirectory().getAbsolutePath().concat("/Test.apk");
File plugin = new File(pluginPath);
PluginManager.getInstance(base).loadPlugin(plugin);


// Given "com.cooee.virtualapk.demo" is the package name of plugin APK, 
// and there is an activity called `MainActivity`.
Intent intent = new Intent();
intent.setClassName("com.cooee.virtualapk.demo", "com.cooee.virtualapk.demo.MainActivity");
startActivity(intent);
```