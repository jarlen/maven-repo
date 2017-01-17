# maven-repo

Common tools

#Usage

##for Gradle

```
dependencies {
    ......
    compile 'cn.jarlen.maven:richcommon:1.2.4'
    compile 'cn.jarlen.maven:okhttppatch:3.4.2'
    ......
}
```
## Using SNAPSHOT

add this to repositories section in build.gradle

```
repositories {
        maven { url 'https://raw.githubusercontent.com/jarlen/maven-repo/master/' }
        jcenter()

    }
```

# ChangeLog

####发布richcommon1.2.7    2016-1-15

**1. 调整基于RecycleView列表多类型扩展**

编码每个类型的View，需继承**BaseRvMultiItemView**

```
public class MultiItemView1 extends BaseRvMultiItemView<Bean> {
    public MultiItemView1(Context context) {
        super(context);
    }

    @Override
    protected int getLayoutResId() {
        return R.layout.layout_rv_item_one;
    }

    @Override
    protected void onBindView(RvViewHolder viewHolder, Bean item) {
        Log.e("jarlen","onBindView--->"+viewHolder);
        TextView name = viewHolder.getView(R.id.name);
        name.setText(item.getName());
    }

    @Override
    protected boolean isForViewType(@NonNull Bean item,int position) {
        return 1 == item.getType();
    }
}
```

编码数据适配器Adapter，需继承**RvMultiAdapter**

```
private class MultiTestAdapter extends RvMultiAdapter<Bean>{

    public MultiTestAdapter(Context context) {
            super(context);
        }

        @Override
        protected void preMultiItemView(RvMultiItemManager itemManager) {
            itemManager.addMultiItemView(new MultiItemView1(mContext));
            itemManager.addMultiItemView(new MultiItemView2(mContext));
            itemManager.addMultiItemView(new MultiItemView3(mContext));
            itemManager.addMultiItemView(new MultiItemView4(mContext));
            itemManager.addMultiItemView(new MultiItemView0(mContext));
        }
    }
```

**Sample**

```
public class RvMultiActivity extends BaseActivity {

    private RecyclerView recyclerView;
    private MultiTestAdapter testAdapter;
    @Override
    protected int getLayoutId() {
        return R.layout.activity_rv_adapter;
    }

    @Override
    protected void onBindView() {
        recyclerView = (RecyclerView) findViewById(R.id.recyclerView);
        recyclerView.setLayoutManager(new LinearLayoutManager(this, LinearLayoutManager.VERTICAL, false));
        testAdapter = new MultiTestAdapter(this);
        recyclerView.setAdapter(testAdapter);
        List<Bean> datas = new ArrayList<Bean>();
        for (int index = 0; index < 1000; index++) {
            Bean bean = new Bean();
            bean.setType(index % 3);
            bean.setName("test " + index);

            datas.add(bean);
        }
        testAdapter.addDataList(datas);
    }

    @Override
    protected void preBindView() {

    }
}
```

PS: 补充两点:

1、对于大多数多类型列表数据，每条数据格式不同，那么应该为JSONArray类型，每个item为JSONObject，可以将JSONArray分解成JSONObject数组使用。

2、很少相同的数据，那么item数据格式相同。那么无需转成JSONObject数组，直接转换成Bean List即可。

**2. 调整MVP模式框架,分离组合接口**，以下例子:

编码View层接口，需继承**IBaseView**

```
public interface IAddView extends IBaseView {
    void showAdd(String sum);
}
```

编码Presenter层接口，需继承**IBasePresenter**

```
public interface IAdd extends IBasePresenter {
    void add(String a, String b);
}
```

编码Presenter层业务，需继承**BaseActivityPresenter**，补充泛型参数(**IAddView**为View层接口)，并实现**IAdd**接口

```
public class AddPresenter extends BaseActivityPresenter<IAddView> implements IAdd{
    @Override
    public void add(String a, String b) {
        int sum = Integer.valueOf(a) + Integer.valueOf(b);
        getProxyView().showAdd(""+sum);
    }
}
```

实现View层接口及界面所需参数，需继承**BaseMvpActivity**，补充泛型参数(**IAdd**为Presenter层接口，**IAddView**为View层接口),并实现**IAddView**接口

```
public class AddActivity extends BaseMvpActivity<IAdd, IAddView> implements IAddView, View.OnClickListener {
    
    private TextView result;
    private Button testBtn;
    private EditText inputOne;
    private EditText inputTwo;

    @Override
    public Class<AddPresenter> getPresenter() {
        return AddPresenter.class;
    }

    @Override
    public IAddView getProxyView() {
        return this;
    }

    @Override
    public int getLayoutId() {
        return R.layout.layout_mvp;
    }

    @Override
    public void onBindView(Bundle savedInstanceState) {
        result = (TextView) findViewById(R.id.result);
        testBtn = (Button) findViewById(R.id.testBtn);
        testBtn.setOnClickListener(this);
        inputOne = (EditText) findViewById(R.id.input_one);
        inputTwo = (EditText) findViewById(R.id.input_two);
    }

    @Override
    public void preBindView() {
    }

    @Override
    public void showAdd(String sum) {
        result.setText(sum);
    }

    @Override
    public void onClick(View view) {
        switch (view.getId()) {
            case R.id.testBtn:
                Intent intent = new Intent(AddActivity.this, RvMultiActivity.class);
                startActivity(intent);
                getProxyPresenter().add(inputOne.getText().toString(), inputTwo.getText().toString());
                break;
            default:
                break;
        }
    }
}
```


**3. 调整基础Activity/Fragment基础类，不需要一定实现preBindView()接口**



####发布 richcommon1.2.4        2016-12-13

**RvCommonAdapter的扩展,支持多类型ItemView的列表 **

```
public int getLayoutResId(int viewType) {

            switch (viewType) {
                case 0:
                    return R.layout.layout_rv_item_one;
                case 1:
                    return R.layout.layout_rv_item_two;
                case 2:
                    return R.layout.layout_rv_item_three;
                case 3:
                    return R.layout.layout_rv_item_four;

                default:

                    return R.layout.layout_rv_item_one;

            }
        }

```

**CommonAdapter的扩展,支持多类型ItemView的列表 **

```
public int getLayoutResId(int position) {
                return R.layout.layout_list_item;
            }
```

**实现MVP基础架构**

**View : **基于activity、fragment

1. 支持泛型
2. Code量较少
3. 不用关心MVP之间的流程 

--- 基于 Activity

1. 继承IBaseView, 编写View层显示接口

```
public interface AddView extends IBaseView {
    void showAdd(String sum);
}
```

2. 继承BaseMvpActivity, 实现VIew层接口

类头:
AddActivity extends BaseMvpActivity<AddPresenter, AddView> implements AddView

```
 @Override
    public void showAdd(String sum) {
        result.setText(sum);
    }
```
3. 继承 BaseActivityPresenter,实现业务层逻辑

```
public class AddPresenter extends BaseActivityPresenter<AddView> {

    public void add(String a, String b) {
        int sum = Integer.valueOf(a) + Integer.valueOf(b);
        getView().showAdd("" + sum);
    }
}
```

--- 基于 Fragment

1. 继承IBaseView, 编写View层显示接口
2. 继承BaseMvpFragment, 实现VIew层接口
3. 继承 BaseFragmentPresenter,实现业务层逻辑

####发布  richcommon1.2.3        2016-11-15  

 进一步优化可复用的Adapter
 
 1.**支持泛型**
 
 2.**Code量更少**


* 基于**ListView**的Adapter的**CommonAdapter**

```
commonAdapter = new CommonAdapter<String>(this) {
            @Override
            public void onBindView(ViewHolder viewHolder, String item) {
                TextView tv = viewHolder.getView(R.id.tv);
                tv.setText(item);
            }

            @Override
            public int getLayoutResId() {
                return R.layout.layout_list_item;
            }
        };
mListView.setAdapter(commonAdapter);
```

* 基于**RecycleView.Adapter**的**RvCommonAdapter**

```
    @Override
    public void onBindView(RvViewHolder viewHolder, String item) {
        ImageView image = viewHolder.getView(R.id.iv_image);
        Bitmap bitmap = ImageUtils.getBitmapByPath(item);
        image.setImageBitmap(bitmap);
    }
```

* 基于**RecycleView.Adapter**的**RvViewHolder**

		与ViewHolder相似

####发布 richcommon1.2.2     2016-8-30
* 添加水印view、可循环回收的Imageview


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
