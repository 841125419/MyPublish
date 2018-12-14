---
typora-copy-images-to: D:\360Downloads\mybublish\MyPublish\img
---

# wsdl publish and invock

# publish

##代码

```java
package com.kwantler.interchange.wsdl.service.hello;

import javax.jws.WebMethod;
import javax.jws.WebService;
import javax.xml.ws.Endpoint;
import java.util.ArrayList;
import java.util.List;

@WebService
public class JwsServiceHello {
    /** 供客户端调用方法  该方法是非静态的，会被发布
     * @param name  传入参数
     * @return String 返回结果
     * */
    public String getValue(String name){
        return "欢迎你！ "+name;
    }

    public List<User> getUsers(String name){
        List<User> list = new ArrayList<User>();
        for (int i = 0; i < 10; i++){
            User user = new User();
            user.setName(name+i);
            user.setPassword("password"+name+i);
            list.add(user);
        }
        return list;
    }

    /**
     * 方法上加@WebMentod(exclude=true)后，此方法不被发布；
     * @param name
     * @return
     */
    @WebMethod(exclude=true)
    public String getHello(String name){
        return "你好！ "+name;
    }

    /** 静态方法不会被发布
     * @param name
     * @return
     */
    public static String getString(String name){
        return "再见！"+name;
    }


    //通过EndPoint(端点服务)发布一个WebService
    public static void main(String[] args) {
     /*参数:1,本地的服务地址;
           2,提供服务的类;
      */
        Endpoint.publish("http://127.0.0.1:8080/Service/ServiceHello", new JwsServiceHello());
        System.out.println("发布成功!");
        //发布成功后 在浏览器输入 http://192.168.1.105:8080/Service/ServiceHello?wsdl
    }

}
```

@WebService 标注此注释会发布成为一个wsdl服务

##发布测试方法

在浏览器中输入http://127.0.0.1:8080/Service/ServiceHello?wsdl，出现以下图片则为正常

![1543395140809](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1543395140809.png)



#invock

生成调用代码

##打开dos命令窗口

使用快捷键win+r 打开运行窗口，输入cmd，敲击回车

##输入图片中的命令

![1543395424803](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1543395424803.png)

格式：wsimport -s 放置源文件位置 -p 指定包名称 -keep wsdl路径

wsimport  如果不支持请查看jdk环境是否正常

-s  指定放置生成的源文件的位置

-p  指定目标程序包

-keep  如果去掉不会生成.class文件

最后生成的文件会存放在D:\project\java\songtaoguanqu\dataCenter\src\main\java\com\kwantler\interchange\wsdl\service\hello\clientInvoke

##代码

```java
package com.kwantler.interchange.wsdl.client.invoke;


import com.kwantler.interchange.wsdl.service.hello.clientInvoke.JwsServiceHello;
import com.kwantler.interchange.wsdl.service.hello.clientInvoke.JwsServiceHelloService;
import com.kwantler.interchange.wsdl.service.hello.clientInvoke.User;

import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class JwsClientHello {
    public static void main(String[] args) {
        //调用webservice
        JwsServiceHello hello=new JwsServiceHelloService().getJwsServiceHelloPort();
        String name=hello.getValue("panchengming");
        List<User> list = hello.getUsers("chenmingjian");
        System.out.println(Arrays.toString(list.toArray()));

        System.out.println(name);

    }
}
```