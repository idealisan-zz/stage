## 安卓ListView上下留白正确做法

[https://stackoverflow.com/questions/3953647/adding-empty-space-to-end-of-listview](https://stackoverflow.com/questions/3953647/adding-empty-space-to-end-of-listview?newreg=462d66e5712f42ef8e62c88d5186240d)

The easiest way to add space is to add padding in xml and set `clipToPadding:"false"`.

### For `RecyclerView`

```
<android.support.v7.widget.RecyclerView
android:layout_width="match_parent"
android:layout_height="match_parent"
android:paddingTop="20dp"
android:paddingBottom="20dp"
android:clipToPadding="false"/>
```

### For `ListView`

```
<ListView
android:layout_width="match_parent"
android:layout_height="match_parent"
android:paddingTop="20dp"
android:paddingBottom="20dp"
android:clipToPadding="false"/>
```

### And same goes for the `ScrollView`

```
<ScrollView
android:layout_width="match_parent"
android:layout_height="match_parent"
android:paddingTop="20dp"
android:paddingBottom="20dp"
android:clipToPadding="false"/>
```

This specifically adds the blank space to top and bottom but hide the space when you scroll the view.