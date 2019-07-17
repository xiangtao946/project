---
title: 最简单的HtmlUnit抓取JS渲染网址数据
date: 2019-07-16
---
## JAVA抓取通过JS渲染的网站（动态）网页数据（HtmlUnit）

### 1.先添加所需的依赖

```java
<dependencies>
            <dependency>
				<groupId>net.sourceforge.htmlunit</groupId>
				<artifactId>htmlunit</artifactId>
				<version>2.27</version>
			</dependency>

		<dependency>
			<groupId>xml-apis</groupId>
			<artifactId>xml-apis</artifactId>
			<version>1.4.01</version>
		</dependency>
		
    <!-- 这里要2.4版本，2.2会缺少东西 -->
		<dependency>
			<groupId>commons-io</groupId>
			<artifactId>commons-io</artifactId>
			<version>2.4</version>
		</dependency>
		
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.11</version>
			<scope>test</scope>
		</dependency>
  </dependencies>
```

### 2.编写测试类

```java
package htmlunit;

import com.gargoylesoftware.htmlunit.NicelyResynchronizingAjaxController;
import com.gargoylesoftware.htmlunit.WebClient;
import com.gargoylesoftware.htmlunit.html.HtmlPage;

public class Test {
	    @org.junit.Test
        public void test() {
		WebClient webClient = new WebClient();
        webClient.getOptions().setJavaScriptEnabled(true);//运行js脚本执行
        webClient.setAjaxController(new NicelyResynchronizingAjaxController());//设置支持AJAX
        webClient.getOptions().setCssEnabled(false);//忽略css
        webClient.getOptions().setUseInsecureSSL(true);//ssl安全访问
        webClient.getOptions().setThrowExceptionOnScriptError(false);  //解析js出错时不抛异常
        webClient.getOptions().setTimeout(100);  //超时时间  ms

        //获取页面
        String url = "https://www.toutiao.com/";
        HtmlPage page = null;
		try {
			page = webClient.getPage(url);
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

        System.out.println("页面title文本:" + page.getTitleText());
        System.out.println("-------------------执行js脚本之前");
        System.out.println(page.asXml());//页面文档结构字符串

        webClient.waitForBackgroundJavaScript(500);   //等待js脚本执行完成
        System.out.println("-------------------执行js脚本后");
        System.out.println(page.asXml());//页面文档结构字符串
        }
}
```

### 3.完毕

忽略报错，打印出头条html

![0716](/img/0716.png)

