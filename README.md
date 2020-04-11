    # TP6 : Les servlets(chat)
## Le noms des auteurs : 
Shahrokh Abolhassani Bavili

Seyed Saeid Hosseini
## Résumé du TP :
Dans ce TP, nous voulons créer une application Web qui permet à deux personnes de discuter entre elles.
Pour cela, nous utilisons Java EE et écrivons des codes avec des servlets et des fichiers jsp.
## Les codes et leurs explications :
### Chat.java:
c'est le code principal qui permet à "StringBuffer" de discuter.
```java=
package pr.tp.web.servlet;

import java.io.IOException;
import java.util.Date;
import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@WebServlet("/chat")
public class Chat extends HttpServlet {
	private static final long serialVersionUID = 197811968639586823L;
	private StringBuffer chatContent;

	@Override
	public void init() throws ServletException {

		chatContent = new StringBuffer();

		chatContent.append("Bienvenue sur le chat.  ");

		chatContent.append("Soyez gentils.").append("\n");
	}

	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		RequestDispatcher rd = request.getRequestDispatcher("chat.jsp");
		RequestDispatcher rd2 = request.getRequestDispatcher("chat2.jsp");

		request.setAttribute("content", chatContent.toString());
		request.setAttribute("content2", chatContent.toString());
		rd.include(request, response);
		rd2.include(request, response);
	}

	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		String a = req.getParameter("ligne");
		String b = req.getParameter("ligne2");
		HttpSession session3 = req.getSession();
		HttpSession session4 = req.getSession();
		if (a != null) {

			Date time = new Date();

			String Name1 = (String) session3.getAttribute("UserName1");
			chatContent.append("(" + time.toString() + "): ").append(Name1 + " >>").append(req.getParameter("ligne"))
					.append("\n");
		} else if (b != null) {
			Date time = new Date();

			String Name2 = (String) session4.getAttribute("UserName2");
			chatContent.append("(" + time.toString() + "): ").append(Name2 + " >>").append(req.getParameter("ligne2"))
					.append("\n");

		}
		String refresh = req.getParameter("refresh");
		if (refresh != null) {
			int start = 0;
			int end = 1000;
			chatContent.delete(start, end);

		}

		doGet(req, resp);
	}

}




```
### chat.jsp:
Comme nous l'avons vu dans le code précédent, celui ci appelle deux codes JSP(chat.jsp et chat2.jsp),avec l'utilisation de "action" dans la méthode <form>.

Donc pour chat.jsp nous avons:
```java=
<%@ page language="java" contentType="text/html; charset=windows-1256"
	pageEncoding="windows-1256"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="windows-1256">
<title>Chat</title>
<link rel="stylesheet" href="styles/chat.css">
</head>
<body>

	<div id="name">${UserName1 }:</div>
	<pre>
		<div id="user">
${content}

	
	</pre>
	</div>

	<form name="chatForm" action="chat" method="post">

		<input type="text" name="ligne" value="" id="text" /> <input
			type="submit" name="action" value="Submit" id="submit" /> <input
			type="submit" name="refresh" value="Refresh" id="refresh" />


	</form>


	---------------------------------------------------
	<br>
</body>
</html>

```
### chat2.jsp:
Pour chat2, nous avons également:
```java=

<%@ page language="java" contentType="text/html; charset=windows-1256"
	pageEncoding="windows-1256"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="windows-1256">
<title>Chat</title>
<link rel="stylesheet" href="styles/chat.css">
</head>
<body>
	<div id="name">${UserName2 } :</div>
	<pre>
<div id="user">
<%=request.getAttribute("content2")%>

	</pre>
	</div>
	<form name="chatForm" action="chat" method="post">

		<input type="text" name="ligne2" value="" /> <input type="submit"
			name="action" value="Submit" /> <input type="submit" name="refresh"
			value="Refresh" />

	</form>

	---------------------------------------------------
	<br>



	<form action="logoutpage" method="post">
		<input type="submit" name="logout" value="Logout" />


	</form>
</body>
</html>



```

### LoginServlet.java:
Pour saisir le nom de la première personne, nous créons cette servlet:
```java=
package pr.tp.web.servlet;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

/**
 * Servlet implementation class LogoutServlet
 */
@WebServlet("/logout")
public class LogoutServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse
	 *      response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		// TODO Auto-generated method stub
		HttpSession session1 = request.getSession();
		HttpSession session2 = request.getSession();
		session1.removeAttribute("UserName1");
		session2.removeAttribute("UserName2");
		response.sendRedirect("login.jsp");

	}

}


```
### login.jsp:
Comme nous l'avons montré, LoginServlet utilise un fichier JSP pour saisir le nom de l'utilisateur,et ainsi nous créons login.jsp:
```java=
<%@ page language="java" contentType="text/html; charset=windows-1256"
	pageEncoding="windows-1256"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="windows-1256">
<title>Login Of User 1</title>
<link rel="stylesheet" href="styles/login.css">
<link rel="icon" href="images/log.ico" type="image/gif" sizes="16*16">
</head>
<body>
	<div id="main">

		<img alt="log1" src="images/log1.jpg">

		<form action="login" method="post">

			Hello and Welcome What is Your Name User1?: <input type="text"
				name="uname1" id="text"><br> <input type="submit"
				value="Login" id="login">

		</form>
	</div>


</body>
</html>
```
#### Page Web de sortie:
![](https://i.imgur.com/GdaK8R9.jpg)


### LoginServlet2.java:
Pour la deuxième personne nous nous servons de ce code:
```java=
package pr.tp.web.servlet;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

/**
 * Servlet implementation class LoginServlet
 */
@WebServlet("/login2")
public class LoginServlet2 extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse
	 *      response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		// TODO Auto-generated method stub
		String UserName2 = request.getParameter("uname2");

		if (UserName2.equals("")) {

			response.sendRedirect("login2.jsp");
		} else {

			HttpSession session2 = request.getSession();
			session2.setAttribute("UserName2", UserName2);
			response.sendRedirect("welcome.jsp");
		}

	}

}


```
### login2.jsp
Le JSP du LoginServlet2 est:
```java=
<%@ page language="java" contentType="text/html; charset=windows-1256"
	pageEncoding="windows-1256"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="windows-1256">
<title>Login Of User 2</title>
<link rel="stylesheet" href="styles/login.css">
<link rel="icon" href="images/log.ico" type="image/gif" sizes="16*16">
</head>
<body>
	<div id="main">

		<img alt="log2" src="images/log1.jpg">

		<form action="login2" method="post">

			Hello and Welcome What is Your Name User2?: <input type="text"
				name="uname2" id="text"><br> <input type="submit"
				value="Login" id="login">

		</form>
	</div>


</body>
</html>
```
#### page Web de sortie:
![](https://i.imgur.com/x0Hsv6U.jpg)


Il est à noter que si l'entrée de l'utilisateur 1 ou de l'utilisateur 2 est vide, le code reviendra au début pour reprendre les entrées.

### welcome.jsp:
Comme nous l'avons vu précédemment dans LoginServlet2.java,le code utilise la méthode "response.sendRedirect (" welcome.jsp ")" pour saluer les utilisateurs, ainsi le welcome.jsp dont nous disposons:
```java=
<%@ page language="java" contentType="text/html; charset=windows-1256"
	pageEncoding="windows-1256"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="windows-1256">
<title>Welcome!</title>
<link rel="stylesheet" href="styles/welcome.css">
</head>
<body>

	<%
		if ((session.getAttribute("UserName1") == null) && (session.getAttribute("UserName2")) == null) {
			response.sendRedirect("login.jsp");
		}
	%>
	<div id="main">
		<div id="text">
			<h2>
				Welcome ${UserName1 } and ${UserName2 } !<br> You Are Going To
				Chat With Each Other, Have A Fun :)
			</h2>
		</div>
		<form action="logout">
			<input type="submit" value="Logout">
		</form>
		<form action="chat">
			<input type="submit" value="Continue To Chat">
		</form>


	</div>
</body>
</html>
```
#### Page Web de sortie:
Pour tester l'application dans ce TP, nous utilisons notre nom en tant qu'utilisateur 1 et utilisateur 2.


![](https://i.imgur.com/Ak8Z8a7.jpg)


Dans cette page la touche "Continue To Chat", nous permet d'accéder au chat et à la page principale, avec les données qui incluent les noms des utilisateurs 1 et 2.

### LogoutServlet.java
Dans la page "Welcome", nous avons également le bouton "Logout" que nous utilisons pour supprimer les "cash" et "sessions" et revenir à la page de départ.

```java=
package pr.tp.web.servlet;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

/**
 * Servlet implementation class LogoutServlet
 */
@WebServlet("/logout")
public class LogoutServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse
	 *      response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		// TODO Auto-generated method stub
		HttpSession session1 = request.getSession();
		HttpSession session2 = request.getSession();
		session1.removeAttribute("UserName1");
		session2.removeAttribute("UserName2");
		response.sendRedirect("login.jsp");

	}

}


```

### LogoutPageServlet.java:
A la fin, nous utilisons le bouton "Logout" pour clore le chat qui appelle la page "GoodBye"

```java=
package pr.tp.web.servlet;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

/**
 * Servlet implementation class LogoutServlet
 */
@WebServlet("/logoutpage")
public class LogoutPageServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse
	 *      response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		// TODO Auto-generated method stub
		HttpSession session1 = request.getSession();
		HttpSession session2 = request.getSession();
		session1.removeAttribute("UserName1");
		session2.removeAttribute("UserName2");

		response.sendRedirect("logout.jsp");
	}
}
```

### logout.jsp:
Dans le servlet précédent, on utilise également le "logout.jsp" pour réaliser la fonction du Logout.

```java=
<%@ page language="java" contentType="text/html; charset=windows-1256"
	pageEncoding="windows-1256"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="windows-1256">
<title>GoodBye</title>
<link rel="stylesheet" href="styles/logout.css">
</head>
<body>


	<h1>
		<b>Goodbye, We Hope That You Had A Good Time Here!</b>
	</h1>

	<br>

	<div id="titre">This application has been created by:</div>
	<div id="autors">
		<ul>
			<li><a href="https://www.linkedin.com/in/shahrokh-abolhassani/">Shahrokh
					Abolhassani Bavili</a></li>

			<li><a
				href="https://www.linkedin.com/in/seyed-saeid-hosseini-272809175/">Seyyed
					Saeid Hosseini</a></li>
		</ul>
	</div>
</body>
</html>
```
#### Page Web de sortie:

![](https://i.imgur.com/w3Opnww.jpg)


## Le résultat:
En définitive, nous avons obtenu la réalisation de cette page où deux personnes discutent entre elles


![](https://i.imgur.com/BRqGTPy.jpg)















    
