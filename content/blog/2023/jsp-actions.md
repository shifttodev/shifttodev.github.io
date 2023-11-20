title= JSP Actions
date=2023-11-20
type=post
tags=web,java, jsp, servlet
status=published
~~~~~~

Actions são outro conjunto de tags que são processadas pelo servidor, auxiliando na criação de página dinâmicas, funcionais e que permitam o acesso a objetos Java.

Tipos de actions são:

- Standard actions
- Custom Actions
- JSTL actions

Na sequência, dois exemplos de utilização de actions em JSP.

## 1. include:

- `menu.jsp`

```html 
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<div>
  <a href="/index.jsp">Index</a>
</div>
```

- `form.jsp`

```html
<jsp:include page="menu.jsp" />
<br>
<form action="/redirect" method="post">
  Username: <input type="text" name="user" id="user">
  <input type="submit" value="Enviar">
</form>
```

Esse exemplo demonstra a utilização da action `include`. Como o próprio nome diz, essa action inclui um trecho de código em outra página.

Em nosso exemplo, estamos utilizando include para adiconar o menu nas páginas de nossa app.

## 2. forward

- `index.jsp`
```htm
<h2>Exemplos de Actions</h2>
<ul>
  <li><a href="/form.jsp">Form</a></li>
  <li><a href="/forward01.jsp">Forwards</a></li>
</ul>
```

- `forward01.jsp`
```html
   <jsp:forward page="forward02.jsp">
    <jsp:param name="page01" value="Passei pela pagina 1"/>
  </jsp:forward>
```

- `forward02.jsp`
```html
<jsp:forward page="forward03.jsp">
  <jsp:param name="page02" value="Passei pela pagina 2"/>
</jsp:forward>
```

- `forward03.jsp`
```html
<jsp:forward page="goal.jsp">
  <jsp:param name="page03" value="Passei pela pagina 3"/>
</jsp:forward>
```

- `goal.jsp`
```html
  <jsp:include page="menu.jsp"/>
  <%
     String param1 = request.getParameter("page01");
     String param2 = request.getParameter("page02");
     String param3 = request.getParameter("page03");
  %>
  <ul>
    <li><%= param1%></li>
    <li><%= param2%></li>
    <li><%= param3%></li>
  </ul>
```

Esse exemplo, demonstra a utilização da action `forward`.

Importante destacar que existe uma diferença entre redirect e forward:

- redirect: direcionamento feito pelo servidor, mais utilizado após o processamento de um post.
- forward: direcionamento feito pelo cliente, mantém a requisição e todo seu contexto.

No exemplo acima, temos 5 arquivos jsp, sendo eles:

- index: a página inicial, com os links para as jsp's e para o primeiro forward.
- forward01: primeira página que encaminha para a página 2.
- forward02: primeira página que encaminha para a página 3.
- forward03: primeira página que encaminha para a página goal.
- goal: que existe todas os parâmetros coletados durante os encaminhamentos.

Essa sequência de jsp's demonstra como funciona o forward, utilizando a actions jsp:forward. 

Perceba que ao chegar na página destino, temos três parâmetros, coletados durante os encaminhamentos.

```
- Passei pela pagina 1
- Passei pela pagina 2
- Passei pela pagina 3
```

Isso se deve ao fato de que o forward só redireciona a requisição, mantendo todo o seu contexto. O que não aconteceria com o redirect.

## Para saber mais

- [Overview of JSP Syntax Elements](https://docs.oracle.com/cd/A97336_01/buslog.102/a83726/genlovw3.htm#1008181)

- [JSP Actions: projeto do código utilizado](https://github.com/shifttodev/jsp-actions)