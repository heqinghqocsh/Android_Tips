# Android小提示

## 安装apk

```java
Intent i = new Intent(Intent.ACTION_VIEW);
        i.setDataAndType(Uri.parse("file://" + apkfile.toString()), "application/vnd.android.package-archive");
        mContext.startActivity(i);
```

## 验证引用是否有某项权限

```java
private static boolean hasExternalStoragePermission(Context context) {
        int perm = 			context.checkCallingOrSelfPermission("android.permission.WRITE_EXTERNAL_STORAGE");
        return perm == 0;
    }
```
