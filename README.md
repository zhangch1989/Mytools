# Mytools

使用方式：
compile 'com.zhangch1989:mylibrary:1.1.0'

1.新建的tools，测试upload到jcenter的maven中心（包含了pullrefres的各种控件，listview，GridView，ScrollView等）
使用方式：
xml文件
<>
<com.zch.mylibrary.pullload.PullToRefreshLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/refresh_view"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include layout="@layout/refresh_head" />
    <com.zch.mylibrary.pullload.PullableListView
        android:id="@+id/listview"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:dividerHeight="@dimen/list_driver_height"
        android:divider="@android:color/darker_gray"
        />

    <include layout="@layout/load_more"/>
</com.zch.mylibrary.pullload.PullToRefreshLayout>

refreshLayout = (PullToRefreshLayout) view.findViewById(R.id.refresh_view);
refreshLayout.setOnRefreshListener(new PullToRefreshLayout.OnRefreshListener() {
            @Override
            public void onRefresh(PullToRefreshLayout pullToRefreshLayout) {
                page = 1;
                returnpage = -1;
                receiveHandler.sendEmptyMessage(HANDLE_REQ_DATA);
                isloading = true;
            }

            @Override
            public void onLoadMore(PullToRefreshLayout pullToRefreshLayout) {
                if ( returnpage != -1 && !isloading) {
                    ++page;
                    isloading = true;
                    receiveHandler.sendEmptyMessage(HANDLE_REQ_DATA);
                }else {
                    ToastUtils.showToast(context, "已是最底");
                    refreshLayout.loadmoreFinish(PullToRefreshLayout.SUCCEED);
                }
            }
        });

2.新增时间选择控件
调用方式：
  public static void setDatetime(Context context, final EditText et, String nowtime, int type){}//用于EditText点击显示
  public static void setDatetime(Context context, final TextView tv, String nowtime, int type){}//用于TextView点击显示
具体使用方法
[
et_reserve.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                String time = et_reserve.getText().toString();
                if(time.equals("")){
                    time = DateUtils.getNowDatetime();
                }
                DateUtils.setDatetime(context, et_reserve, time, TimeConfig.YEAR_MONTH_DAY_HOUR_MINUTE);
            }
        });

]

