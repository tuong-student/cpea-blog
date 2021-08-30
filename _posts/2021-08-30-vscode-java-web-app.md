---
title: Hướng dẫn tạo project Java Web App với VSCode
date: '2021-08-30'
published: true
---


## Thiết lập môi trường 101
Trước khi vào bài học, cần tải và cài những thứ cần thiết sau:

| Cellar | Window | Mac | Linux |
|--|--|--|--|
|Openjdk | `Liên hệ Trí Lạc Đà` | `brew install openjdk`|`sudo apt install default-jdk`
|Tomcat9|`Liên hệ Trí Lạc Đà`| `brew install tomcat@9`| [wget][tomcatwget]
|Maven|`Liên hệ Trí Lạc Đà`|`brew install maven`|`sudo apt install maven`

- Brain! 🧠
- Some Magic  ✨
- Cầu nguyện ông bà 🙏

Sau khi có tất cả những thứ kể trên, để thuận tiện cho công việc setup, bật vscode và tải những extension hỗ trợ sau:

 1. [Extension Pack for Java][pack]
 2. [Tomcat for Java][tomcat]
 3. [XML][xml]

## Vào việc

### 1. Tạo project
Đầu tiên ta sẽ tạo một project mới.
Ấn `Create Java Project` ngay thanh `Explorer` bên trái (Chỉ xuất hiện sau khi đã cài extension số 1 ở trên).

![img](https://i.imgur.com/DahPzkV.png?1)

Một chiếc bảng xuất hiện để lựa chọn loại project, chọn `Maven`

![img](https://i.imgur.com/0o9m0IZ.png?)

Một chiếc bảng khác lại hiện ra bắt ta lựa chọn mẫu project cho `Maven`, chọn `maven-archetype-webapp`

![img](https://i.imgur.com/VbthooT.png?)

Tiếp theo ta sẽ chọn phiên bản cho project. Ở đây ta sẽ chọn `1.4` (cho nó mới 😀)
>Lưu ý: khi làm việc nhóm cả team phải thống nhất với nhau phiên bản để tránh trường hợp **"Ông chạy được, bà tắt tiếng"**

![img](https://i.imgur.com/bp7AcQi.png?)

Đặt group Id và artifact Id cho project.  Vì lười nên thôi lấy ví dụ có sẵn luôn cho tiện 🥲.  Có thể tìm hiểu thêm [tại đây][idmeaning] về ý nghĩa và cách đặt tên cho 2 loại id này. `Enter` cái rụp và chọn vị trí để lưu project.

![img](https://i.imgur.com/ovB620j.png?)

![img](https://i.imgur.com/ZDLOmIZ.png?)

Lúc này `terminal` hiện lên chạy như một vị thần. Có hai chỗ `terminal` sẽ đứng im và bắt chúng ta điền thông tin, tạm thời đừng quan tâm, cứ `Enter` là được.
```js
...
1. Define value for property 'version' 1.0-SNAPSHOT:
...
2. Y:
...
 ```

![img](https://i.imgur.com/J5ZXY0v.gif?)

Bùm, folder project đã xong nhờ khả năng của hacker Pepe mới thuê. Một chiếc bảng hiện ra thông báo folder project đã được tạo, ấn `Open` (Hoặc ai rảnh thì vào folder của nó mở nhé 😏)

### 2. Chuẩn bị bên trong project
#### a. Cấu trúc project 

>Ghi chú nhẹ: Loz nào éo ra như này thì xác định ăn loz cả sải. Không nhấ thi phả có folder project.

![img](https://i.imgur.com/XpZbBMz.png?)

#### b. Thêm nội dung và chỉnh sửa cấu trúc:

_Đoạn này là đoạn sướng nhất. Ông nào thích code khoái lắm._

1. Việc đầu tiên cũng là việc thứ nhất, thêm dòng này vào bên trong tag `<dependencies>` của file `pom.xml` (Không có là có chiện nha Lâm 😏)

```xml
<dependencies>
...
<dependency>
<groupId>javax.servlet</groupId>
<artifactId>javax.servlet-api</artifactId>
<version>4.0.1</version>
<scope>provided</scope>
</dependency>
...
</dependencies>
```

2. Việc thứ hai cũng là việc thứ nhì, xóa hết tất cả và nhét bên trong file `web.xml` nằm ở folder `webapp/WEB-INF` đoạn code thử nghiệm sau:

```xml
<!DOCTYPE  web-app PUBLIC "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN" "http://java.sun.com/dtd/web-app_2_3.dtd">
<web-app>
<display-name>Archetype Created Web Application</display-name>
<servlet>
<servlet-name>servlet</servlet-name>
<servlet-class>com.example.servlet</servlet-class>
</servlet>
<servlet-mapping>
<servlet-name>servlet</servlet-name>
<url-pattern>/api</url-pattern>
</servlet-mapping>
<welcome-file-list>
<welcome-file>index.jsp</welcome-file>
</welcome-file-list>
</web-app>
```

3. Việc thứ ba không phải việc của má, tạo folder theo cấu trúc sau `java/com/example` trong folder  `src/main`. Sau đó ở folder `example` tạo file `servlet.java` thêm vào đoạn code sau:

```java
package  com.example;
import  java.io.PrintWriter;
import  javax.servlet.http.HttpServlet;
import  javax.servlet.http.HttpServletRequest;
import  javax.servlet.http.HttpServletResponse;

public  class  servlet  extends  HttpServlet {
	public  void  processRequest(HttpServletRequest  request,  HttpServletResponse  response) {
		response.setContentType("text/html;charset=UTF-8");
		try (PrintWriter  out  =  response.getWriter()) {
			out.println("<html>");
			out.println("<body>");
			out.println("<h1>Hello all from servlet</h1>");
			out.println("<h1>Servlet NewServlet at "  +  request.getContextPath() +  "</h1>");
			String  user  =  request.getParameter("user");
			out.println("<h2>Welcome "  + user +  "</h2>");
			out.println("</body>");
			out.println("</html>");
		} catch (Exception  e) {
			e.printStackTrace();
		}
	}

	@Override
	public  void  doGet(HttpServletRequest  request,  HttpServletResponse  response) {
		processRequest(request, response);
	}

	@Override
	public  void  doPost(HttpServletRequest  request, HttpServletResponse  response) {
		processRequest(request, response);
	}
}
```

4. Việc thứ tư cũng là việc cuối cùng, trong file `index.jsp` ở folder `webapp`,  xóa hết và thêm các của nợ sau:

```html
<html lang="en">
	<head>
		<meta  charset="utf-8">
		<title>Servlet Home Page</title>
	</head>
	<body>
		<h1>Hello World!</h1>
		<form  action="./api"  method="get, post">
			<label>Name of you: </label>
			<input  type="text"  name  ="user">
			<input  type="submit"  value="Click here">
		</form>
	</body>
</html>
```

Nhìn cú pháp quen không? 😌

#### c. Tomcat server:
|[Window][window-tomcat]|[Mac][mac-tomcat]|Linux|
|--|--|--|

Thành quả: 
>Ông nào làm thành công quả này coi như có được 70% sức mạnh trong tay.

Biểu tượng server stop cùng tên xuất hiện!

![img](https://i.imgur.com/V1NFauf.png?)

#### d. Một kỷ nguyên mới khởi đầu:
Kéo được con mồn lèo xong rồi (hơi quằn) thì chạy qua thanh `Explorer` bên trái tìm `Maven`. Bật nó lên kiếm `Lifecycle`  và thực hiện:
1. `clean`
2. `package` 

![img](https://i.imgur.com/XaljUws.png?)

Đợi một khoảng để chương trình xóa rồi tạo lại folder `target` mới dựa trên những gì mình mới thêm vào.


Nếu folder `target` chưa xuất hiện thì nhìn qua thanh `Explorer` bên trái tìm mục `Java Project`, chuột phải vào tên của project chọn `Update Project`

![img](https://i.imgur.com/yNke80v.png?)

Lúc này cấu trúc của project có thể được nhìn như sau:

![img](https://i.imgur.com/SMZ77nI.png?)

#### e. Đến giờ chạy code rồi - Mammy said:

>Hê hê, công đoạn làm mấy ông đạt cực khoái nè. Lên đỉnh luôn. 🤏

Ở folder `target`, Chuột phải vào folder `demo` (folder trùng tên với `artifact Id` mình tạo) chọn `Run on Tomcat Server`

![img](https://i.imgur.com/8sIpLPJ.png?)

Lúc này nhìn vào mục `Tomcat Server` ở thanh `Explorer` bên trái sẽ thấy màu server chuyển từ đỏ sang xanh (stop to start) và xuất hiện biểu tượng `Web demo`

![img](https://i.imgur.com/K34HHqv.png?)

Chuột phải vào biểu tượng đó và chọn `Open in Browser`

![img](https://i.imgur.com/4wqQhEX.png?)

Cùng làm tách trà ngồi dựa lưng vào ghế thư giãn xem công sức nãy giờ của mình được hiển thị như thế nào. Salys ủng hộ bạn! 

![img](https://i.imgur.com/CrdJEDi.png?)

Đếm từ 1 đến 10... 

![img](https://i.imgur.com/82cJFwH.png?)

Quá đã, không uổng công ngồi đọc nãy giờ. Nếu thất bại, thằng Viên nó sẽ bị chửi nên là tới nước này tốt nhất là nên lên hình. Pls! 🤬

Gòi! Điền tên rồi ấn `Click here` để coi `servlet` làm ăn ra sao 

![img](https://i.imgur.com/mj4Ix1B.png?)

Một sinh viên Bách Khoa chia sẻ:
>Úi giời ơi, dễ vãiiiii loz!  

![img](https://c.tenor.com/OKn9PA2QSM8AAAAd/bomman-gay.gif)

# Thế nhá, mình thăng đây. Hẹn gặp lại mina ở bài post khác. 🎆

[//]: # (These are reference links used in the body of this note)
[pack]: <https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack>
[xml]: <https://marketplace.visualstudio.com/items?itemName=redhat.vscode-xml>
[idmeaning]: <http://maven.apache.org/guides/mini/guide-naming-conventions.html>
[tomcat]: <https://marketplace.visualstudio.com/items?itemName=adashen.vscode-tomcat>
[tomcatwget]: <https://downloads.apache.org/tomcat/tomcat-8/v8.5.70/bin/apache-tomcat-8.5.70.zip>
[window-tomcat]: <https://i.imgur.com/cSa5KBn.mp4>
[mac-tomcat]: <https://i.imgur.com/JliA2pO.mp4>
