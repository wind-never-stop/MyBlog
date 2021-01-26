---
title: Facebook 登录集成
categories: 
- 第三方配置
tags:
- Facebook配置
---

### 1.添加依赖
在对应 Module 的 __build.gradle__ 文件下添加以下代码：
```
 implementation 'com.facebook.android:facebook-login:[5,6)'
```
### 2.配置资源和清单
1.在 __strings.xml__ 文件中添加如下配置：
>
```
<string name="facebook_app_id">[APP_ID]</string>
<string name="fb_login_protocol_scheme">fb[APP_ID]</string>

```
<!--more-->
2.在 __AndroidManifest.xml__ 清单文件中添加以下配置：

```
<meta-data 
android:name="com.facebook.sdk.ApplicationId"
android:value="@string/facebook_app_id"/> 

<activity 
android:name="com.facebook.FacebookActivity"
android:configChanges= "keyboard|keyboardHidden|screenLayout|screenSize|orientation" 
android:label="@string/app_name" />

<activity 
android:name="com.facebook.CustomTabActivity"
android:exported="true"> 
<intent-filter> 
<action android:name="android.intent.action.VIEW" />
<category android:name="android.intent.category.DEFAULT" /> 
<category android:name="android.intent.category.BROWSABLE" />
<data android:scheme="@string/fb_login_protocol_scheme" /> 
</intent-filter>
</activity>

```
### 3.添加集成代码
1.在 __onCreate()__ 或 __onCreateView()__ 方法中对facebook SDK进行初始化并注册回调 代码如下：

```
 /**
     * 初始化Facebook
     */
    private fun initFacebook(){
        callbackManager = CallbackManager.Factory.create()
        if (Constants.DEBUG) {
            //打印facebook 识别应用的 key
            KLog.e("facebook key=${FacebookSdk.getApplicationSignature(this)}")
        }
        //添加回调
        LoginManager.getInstance().registerCallback(callbackManager,
            object : FacebookCallback<LoginResult> {
                override fun onSuccess(loginResult: LoginResult) {
                    // App code
                    KLog.e("login", "token: " + loginResult.accessToken.token)
                    val code = loginResult.accessToken.token
                    //调用后台接口进行登录
                    viewModel?.facebookLogin(code)
                }

                override fun onCancel() {
                    // App code
                }

                override fun onError(exception: FacebookException) {
                    // App code
                    KLog.e("login", "e: $exception")
                }
            })
    }

```
2.在 __onActivityResult()__ 方法中调用 __callbackManager.onActivityResult()__ 将登录结果传递给 __LoginManager__ , 代码如下：


```
   @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
     super.onActivityResult(requestCode, resultCode, data);
     // Facebook 登录结果传递给LoginManager
        callbackManager.onActivityResult(requestCode, resultCode, data);
       
    }
```

3.调用Facebook SDK进行登录，代码如下：

```
 /**
     * 调用facebook 登录
     */
    private fun toFacebook(){
        val accessToken = AccessToken.getCurrentAccessToken()
        val isLoggedIn = accessToken != null && !accessToken.isExpired
        if (isLoggedIn) {
            //判断Facebook是否登录  如果登录先退出
            try {
                LoginManager.getInstance().logOut()
                AccessToken.setCurrentAccessToken(null)
            } catch (e: Exception) {
                e.printStackTrace()
            }
        }
        //调用facebook登录
        LoginManager.getInstance().logInWithReadPermissions(
            this,
            listOf("public_profile", "email", "user_friends")
        )
    }
```

5.由于一般情况下我们只是需要Facebook 返回的token识别用户，所以在退出该页面时在__onDestroy()__调用Facebook的退出API,代码如下：

```
/**
     * 退出Facebook登录
     */
    private fun outFacebook(){
        //退出facebook
        val accessToken = AccessToken.getCurrentAccessToken()
        val isLoggedIn = accessToken != null && !accessToken.isExpired
        if (isLoggedIn) {
            //判断Facebook是否登录  如果登录先退出
            try {
                LoginManager.getInstance().logOut()
                AccessToken.setCurrentAccessToken(null)
            } catch (e: Exception) {
                e.printStackTrace()
            }
        }
    }

```

### 注意：
虽然在一般情况下使用Facebook登录就会显示 facebook 识别应用的key,但是有些情况下会出现直接通过登录而不会显示key,这个时候我们可以调用 Facebook 的API进行获取，代码如下：

```
FacebookSdk.getApplicationSignature(context)
```

到这里就集成完毕了
