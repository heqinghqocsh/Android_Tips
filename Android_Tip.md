# Android小提示

### 安装apk

```java
Intent i = new Intent(Intent.ACTION_VIEW);
        i.setDataAndType(Uri.parse("file://" + apkfile.toString()), "application/vnd.android.package-archive");
        mContext.startActivity(i);
```

### 验证引用是否有某项权限

```java
private static boolean hasExternalStoragePermission(Context context) {
        int perm = 			context.checkCallingOrSelfPermission("android.permission.WRITE_EXTERNAL_STORAGE");
        return perm == 0;
    }
```

### 将字符串编码成16进制字符串（适用于所有包括中文），并反编码

```java
public String toHexString(String str) {
        //根据默认编码获取字节数组
        byte[] bytes = null;
        try {
            bytes = str.getBytes("UTF-8");
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        if (bytes == null) return null;
        StringBuilder sb = new StringBuilder(bytes.length * 2);
        //将字节数组中每个字节拆解成2位16进制整数
        for (int i = 0; i < bytes.length; i++) {
            sb.append(hexString.charAt((bytes[i] & 0xf0) >> 4));
            sb.append(hexString.charAt(bytes[i] & 0x0f));
        }
        return sb.toString();
    }

public static String hexToString(String s) {
        if ("0x".equals(s.substring(0, 2))) {
            s = s.substring(2);
        }
        byte[] baKeyword = new byte[s.length() / 2];
        try {
            for (int i = 0; i < baKeyword.length; i++) {
                baKeyword[i] = (byte) (Integer.parseInt(s.substring(i * 2, i * 2 + 2), 16));
            }
        } catch (NumberFormatException e) {
            e.printStackTrace();
        }
        try {
            s = new String(baKeyword, "utf-8");//UTF-16le:Not
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
        }
        return s;
    }
```

### 加载assets下的HTML文件

```java
webView.loadUrl("file:///android_asset/about.html");
```

### Java与JavaScript互相调用

#### Java端代码

```java
//如果访问的页面中有Javascript，则WebView必须设置支持Javascript
mWebView.getSettings().setJavaScriptEnabled(true);
mWebView.addJavascriptInterface(new JavascriptInterface(), "demo");
mWebView.loadUrl("file:///android_asset/xxx.html");
//调用JavaScript方法
mWebView.loadUrl("javascript:xxxMethod()");
mWebView.loadUrl("javascript:bindData(' " + jsonStr + " ')");

//JavascriptInterface
final class JavascriptInterface {
        @android.webkit.JavascriptInterface
        public void print(String html) {
            //....
        }
    }
```

#### JavaScript端代码

```javascript
function bindData(json) {
	    $.each($.parseJSON(json),function(i,item){
	        //....
	    });
	}
//JavaScript端调用Java中的方法
window.demo.print(str);
```

### 判断是否在主线程中

```java
public static boolean isOnMainThread() {
        return Looper.myLooper() == Looper.getMainLooper();
    }
```



