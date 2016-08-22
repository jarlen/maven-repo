# How to use

Simply add the repository to your build.gradle file:

```
repositories {
        maven { url 'https://raw.githubusercontent.com/jarlen/maven-repo/master/' }
        jcenter()

    }
```

And you can use the artifacts like this:

```
dependencies {
    ......
    compile 'cn.jarlen.maven:androidmvp:1.0.1'
    ......
}
```

# ChangeLog

* richcommon1.2.1      2016-8-1


#richcommon
##一、包:Adapter 
###1、extends SimpleBaseAdapter 实现getView();
   
   * 支持泛型
   * 代码量少
   * 可结合ViewHolder使用

###2、ViewHolder 超级复用(getView)

   * 可复用
   * 代码量少
   * 与Adapter相结合
   

```
ViewHolder viewholder = ViewHolder.getViewHolder(Context context, ViewGroup parent,View convertView, int position, int layoutId);
TextView tv = viewholder.getView(resId);
return viewholder.getConvertView();
```
   
##二、utils

###1、AppUtil

* public static String getAppVersion(Context context)
* public static void uninstallApp(Context context, String packageName)
* public static boolean isAppInstalled(Context context, String packageName)
* public static ApplicationInfo getApplicationInfo(Context context, String packageName)
* public static List<PackageInfo> getPackageInfos(Context context)
* public static void startApp(Context ctx, String packageName)

###2、FileUtil

* public static boolean deleteFile(File file)
* public static boolean deleteFile(String filePath)
* public static boolean createFile(File file)
* public static boolean createFile(String filePath)
* public static boolean createDirectory(String dirPath)
* public static boolean moveFile(String srcPath, String dstPath, boolean isForce)
* public static boolean isGifImage(File srcPath)
* public static long getsize(File file)
* public static String getFileSHA1(String path)
* public static String getFileMD5(String path)
* public static String getFileSize(long length)

###3、PreferenceUtils

* public static PreferenceUtils newInstance(Context context, String name)

###4、SystemUtil

* public final static boolean isMainThread()
* public static long getMemoryUnused(Context mContext)
* public static int getUid(Context context)
* public static void reStartApp(Context context, Class<?> clazz)
* public static boolean isRooted()
* public static void lockScreen(Context context)
* public static String getIMEI(Context context)
* public static String getBluetoothMac()
* public static String getWlanMac(Context context)
* public static float getAndroidVersion()
* public static int dp2px(Context context, float dpValue)
* public static int px2dp(Context context, int px)
* public static int sp2px(Context context, float sp)
* public static int getStatusBarHeight(Context context)
* public static int getStatusBarHeight2(Context context)
* public static float getScreenInches(Context context)
* public static int getDensity(Context context)
* public static String getRomTotalSize(Context context)
* public static String getRomAvailableSize(Context context)
* public static void scanSdCard(Context context)
* public static void scanSdCard(Context context, String filePath, MediaScannerConnection.OnScanCompletedListener listener)

###5、TimeUtil

* public static int getToday()
* public static String getToday2()

###6、ToastUtil

* public static ToastUtil makeToast(Context ctx)
* public ToastUtil setText(String message)
* public ToastUtil setGravity(int gravity)
* public ToastUtil setImage(int picId)
* public ToastUtil setDuration(int duration)
* public void show()


#License:
        
        Copyright (C) 2016 jarlen
  
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at
  
  http://www.apache.org/licenses/LICENSE-2.0
  
  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
