# cordova-plugin-mixPush
Cordova 推送，支持设置角标，事件回掉。
此项目代码设计以混合推送为基础，目前支持小米推送，后续会加华为等（此做法在安卓端是业内常规做法，在华为手机启动华为推送，小米手机启动小米，这样达到体验极致），在不改动业务逻辑代码情况下，完全兼容多种推送引擎。

如何使用?
-------------------

use npm:

```npm
 cordova plugin add cordova-plugin-mixpush --variable ANDROID_PACKAGE_NAME=你的安卓项目包名  --variable MI_PUSH_APP_IOS_ID=你IOS推送id --variable MI_PUSH_APP_IOS_KEY=你的IOS推送Key
```
## Example
code:

       //此为代码为最简单用法，详细使用API，请参考www目录下的MixPushPlugin.js 注释
       document.addEventListener('deviceready',push , false);
       function push(){
            var deviceBrand="xiaoMi";//目前只支持小米引擎
            window.plugins.MixPushPlugin.setPushEngine([deviceBrand]);
            var miId = '2882303******08931'; //android id
            var miKey = '57117***2931'; //android key
            //开始启动注册小米推送
            window.plugins.MixPushPlugin.registerPush([miId, miKey]);

            //registerPush事件
           document.addEventListener("MixPushPlugin.onRegisterPush", function onCallBack(data) {
                if (data&&data.code == 200 ) {
                    console.log('注册成功：' + data.regId);
                    setAlias("qq112");//注册成功就设置Alias
                }else{
                    console.log('注册失败：');
                }
            }, false);

            //设置Alias
            function setAlias(alias) {
                window.plugins.MixPushPlugin.setAlias([alias]);
                //onSetAliasPush事件
                document.addEventListener("MixPushPlugin.onSetAliasPush", function onCallBack(data)  {
                    if (data) {
                        data.code == 200 ? "" : setAlias(data.alias);
                        console.log("MixPushPlugin.onSetAliasPush" + data.alias);
                    }
                }, false);
            }

            //来消息后的事件监听
            document.addEventListener("MixPushPlugin.onNotificationArrived",function onCallBack(data) {
                    if (data && data.title) {
                        window.plugins.MixPushPlugin.badgerApplyCount([20]);//来消息后把APP图标上的角标数字改成20
                        console.log('来消息了：' + data.title);
                        alert(data.title);
                    }
             }, false);

            //点消息后的事件监听
             document.addEventListener("MixPushPlugin.onNotificationClicked", function onCallBack(data) {
                         if (data && data.title) {
                             window.plugins.MixPushPlugin.badgerApplyCount([0]);//点消息后清空数字
                             console.log('点消息了：' + data.title);
                             alert(data.title);
                         }
             }, false);

       }












