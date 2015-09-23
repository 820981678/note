android left menu: <br/>
http://blog.csdn.net/gebitan505/article/details/20390325
<br/>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="match_parent"
    android:background="#FFFFFF"
    >

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:text="服务地址"
        android:id="@+id/textView"
        android:textSize="20dp"
        android:layout_marginTop="30dp"
        android:layout_marginBottom="10dp"
        android:textColor="#000000"
        android:layout_gravity="center_horizontal" />

    <View
        android:layout_width="fill_parent"
        android:layout_height="0.5dp"
        android:background="#999"
        />

    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editText"
        android:layout_marginLeft="10dp"
        android:layout_marginTop="10dp"
        android:layout_marginBottom="10dp"
        android:paddingLeft="5dp"
        android:hint="请输入服务地址"
        android:background="@null"
        android:textCursorDrawable="@null"
        android:textColor="#000000"
        android:layout_gravity="center_horizontal" />

    <View
        android:layout_width="fill_parent"
        android:layout_height="0.5dp"
        android:background="#999"
        />

    <Button
        android:layout_width="300dp"
        android:layout_height="40dp"
        android:text="确 定"
        android:id="@+id/button"
        android:background="@drawable/buttonshape"
        android:layout_marginTop="20dp"
        android:textSize="20dp"
        android:textColor="#000000"
        android:layout_gravity="center_horizontal" />

</LinearLayout>
