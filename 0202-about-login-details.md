# 登录页面流程细节

登录页面是各类网站必备的页面，正好今天实现了一版简单的页面效果，来说一下里面细节的部分吧，顺便把踩过的坑也整理一下：

#### 页面功能概述：
    - 输入手机号码
    - 动态验证码，发送验证码和填写验证码（1分钟后重新发送）

#### 外部引用文件：
    - jQuery库
    - jQuery Validation Plugin  
      表单验证插件  官网地址：http://jqueryvalidation.org/ 
       
### 页面细节部分：

    敲黑板！划重点！！！
    
    - 布置页面的时候，关于input按钮的文字占位符，可用placeholder伪元素？伪类就ok啦。特别注意的是,因为浏览器兼容性不一致，选择器不要用逗号分隔符打包写在一起，否则你把浏览器都刷爆了，样式一点都没生效，开始怀疑人生～参见：参考资料-1。
    
    - 关于填写纯数字的input类型，因为移动端设备有软键盘的存在，所以为了优化用户的体验，最好选择[type=tel]的类型，还有一种type=number的类型也可以输入纯数字，但是两者在表现上还是有细微差异的，比如小数点。选什么用，看具体的业务需求。
    然鹅，逗比用[type=text]用顺手了，电脑上写的时候没觉得什么，真机上测试的时候，被产品骂死了。细节上的体验真的很重要！
    
    - 当点击验证码按钮时，来看看发送验证码的流程吧：
        1. 检查按钮有效性（当验证码正在发送中，或发送成功且还在时效期内时，点击按钮是无效的，不会做任何操作）。我给这个标签元素加了自定义属性值data-disable，值为true代表了不可点击，为false代表可点击，每次点击的时候拿这个自定义属性的值来判断；也可以在代码部分设置一个变量来保存和判断这个值。
        2. 检查手机号码有效性。手机号码传送到后台，也可以检查手机号码的有效性，只不过一方面加长了用户的等待时间，一方面产生了不必要的请求资源浪费，得不偿失。
        3. 接下来就是发送ajax请求了，在发送请求前一步，按钮置灰，data-disable属性设置为true，标示"发送中"的文本信息，防止用户多次点击重复请求，这些都是优化用户体验的小细节。
        4. ajax请求失败。常见的由于网络状态等原因，导致请求失败，这时需要 按钮恢复可点击的状态，data-disable的属性值别忘记设置。
        5. ajax请求成功。按钮进行倒计时操作，此期间data-disable仍然是true，直到倒计时结束，按钮重新恢复可点击状态，颜色、属性和文本信息及时更新。


### 踩坑部分：
    1. 验证码点击之后，按钮置灰，一直处于"发送中"的状态，且部分用户反馈正常、部分用户反馈异常。
        - 处理过程：
        页面报错：初步判断返回的数据类型不是约好的JSON类型，所以使用JSON解析的地方页面报错了。查看请求返回数据，果然是"text/html"格式的。气急败坏地找后台程序哥哥去理论，结果他说这数据不是后台给的，可能是网络哪里出现了问题，返回了非常规数据。
        - 处理方法：
        在这种情况下，ajax依然执行了success函数，只能依靠数据类型的判断来分辨这种网络异常的状态。我在一接收到请求数据的时候，立即做了类型判断：$.type(data) === "object" ，在数据正常的情况下执行后续操作，否则抛出数据异常的提示。按钮的状态也需要做相应的调整。
        - 总结：
        在本地环境和测试环境下基本没有出现过问题，在生产环境不定时地会重现，现在还不太清楚返回text/html的数据是哪里出现了问题，对于前台页面来说，做一下容错处理，不能让用户来承担这种风险。
    
    
#### 可有可无的东西：
    1. 弹窗美化：
       因为做移动端页面，对弹窗的美观性还是比较介意，选用了常用的移动端弹窗插件：layer mobile 地址：http://layer.layui.com/mobile
    2. 用户注册or登录协议
       有些页面会要求同意协议之后才可提交操作，我采用事件监听的方式。首先封装一个函数checkValid，返回操作按钮有效性的判断结果，Boolean值。每次执行相关操作时，如点击"同意协议" 或 填写表单的时候，最后都执行下这个函数，根据结果来调整按钮显示的样式。
        
#### 代码部分：
``` {javascript}
var log = {
     init: function() {
         this.formCheck();
         this.identifyCode();
     },
     // 登录表单验证
     validater: null,
     // 登录表单验证
     formCheck: function() {
         this.validater = $("#login-form").validate({
             // 验证规则
             rules: {
                 mobile: {
                     required: true,
                     checkTel: true
                 },
                 code: {
                     // remote: "",  //远程验证地址
                     required: true
                 }
             },
             // 表单提交事件
             submitHandler: function() {
                 layer && layer.open({
                     type: 2
                     ,content: '登录中'
                 });

                 // 关闭登录提示弹窗
                 setTimeout(function(){
                     layer.closeAll();
                 }, 3000);
                 return false;
             }
         });
     },
     // 发送验证码
     identifyCode: function() {
         var btn = $("#js-sms"); // 发送验证码按钮
         btn.click(function() {
             // 检查按钮是否可发送状态
             if(btn.data("role") && btn.data("role") == "disabled" ) return;

             // 检查手机号是否验证通过
             if(log.validater.check($("#mobile"))) {
                 // 验证通过，发送请求 同时 点击按钮置灰
                 btn.addClass("disabled").data("role", "disabled").text("发送中...");

                 // 发送验证码请求
                 $.ajax("", {
                     type: "post",
                     data: {
                         mobile: $("#mobile").val()
                     },
                     dataType:"json",
                     timeout: 3000,  //请求超时时间
                     // 请求失败
                     error: function() {
                         // 重新设置可点击状态
                         console.log("发送请求失败");
                         btn.removeClass("disabled").data("role", "able").text("重新发送");
                     },
                     success: function(data) {
                         // 发送请求成功
                         console.log(data);
                         // 避免某些时候DNS解析异常，请求成功但是解析数据类型为string
                         if($(data).type() !== "object" ) {
                             console.log("解析数据出错");
                             layer.open({
                                 content: "网络状态异常，稍后再试试吧",
                                 skin: "msg",
                                 time: 2
                             });
                             btn.removeClass("disabled").data("role", "able").text("重新发送");
                             return false;
                         }

                         log.countTime();
                         layer && layer.open({
                             content: "发送成功",
                             skin: "msg",
                             time: 4
                         })

                     }
                 })

             } else {
                 //未验证通过,提示手机号码设置
                 layer && layer.open({
                     content: '请检查手机号码！'
                     ,skin: 'msg'
                     ,time: 2 //2秒后自动关闭
                 });
            }

        });
    },
    // 验证码倒计时
    countTime: function() {
        var btn = $("#js-sms"); // 发送验证码按钮
        var total = 6;
        var timer = null;
        timer = setInterval(function(){
            if(total <=0 ) {
                // 倒计时结束
                btn.removeClass("disabled").data("role", "able").text("重新发送");
                total=60;
                clearInterval(timer);
            } else {
                btn.text(""+total+"s");
            }
            total--;
        }, 1000);
    }

}
log.init();
```
---              
        
#### 参考资料：
    1. [翻译 - 使用CSS修改HTML5 input placeholder颜色 - SegmentFault 思否](https://segmentfault.com/q/1010000000397925)