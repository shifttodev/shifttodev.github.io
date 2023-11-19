title= Mais sobre JSP's
date=2023-11-19
type=post
tags=web,java
status=published
~~~~~~

## Enviando e recebendo dados com `JSP`

Em JavaServer Pages (JSP), os objetos implícitos são referências a objetos Java que estão disponíveis automaticamente em cada página JSP, sem a necessidade de serem explicitamente declarados ou inicializados. Esses objetos fornecem funcionalidades específicas que facilitam o desenvolvimento de aplicações web Java.  

Vejamos um exemplo:

```html
<h2>Dados enviados</h2>

<form action="/index.jsp" method="post">
    Nome: <input type="text" id="user" name="user">
    <input type="submit" value="Enviar">
</form>
<hr>
  <h2>Dados recebidos</h2>
  <% String name = request.getParameter("user");
     if (name != null && !(name.equals(""))){
  %>
    <p><%=name  %></p>
  <% } else {%>
  <p>Nenhum dado recebido</p>
  <%}%>
```

Aqui temos uma página JSP divida em duas partes. 

Na parte superior, temos um formulário HTML que possui um campo texto e um botão submit. Esta parte é responsável por enviar os dados.

Na parte inferior, temos um parágrafo. Esta parte é responsável por receber os dados.

Inserido entre as tags HTML, temos um pouco de código Java, responsável por recuperar os dados enviados na requisição e exibí-los na página.

Caso nenhum dado seja enviado, a frase _Nenhum dados recebido_ será exibido.

Notamos que o código fica pouco legível, já que tem tudo misturado, HTML e Java, o que dificulta identificar onde começa e termina a sintaxe de cada código.

Porém, apesar da baixa legibilidade do código, ele está funcional.

Isso foi possível graças aos objetos implícitos que existem nas páginas JSP. Perceba que em nenhum momento instanciamos o `request`. Ou seja, esse objeto já existe por padrão em uma JSP.


## Tratando erros 

Abaixo, como configurar o a aplicação para exibir uma página de erro 404 personalizada.

- Crie sua página de erro com as informações desejadas
- Nesse exemplo, utilizaremos o erro 404. Portanto, nomeie a página com o código do erro, nesse caso `404.jsp`.
- Define o conteúdo da página. Exemplo:

```html
<h2 style="color: red;">Endereço/recurso não encontrado</h2>
``` 

- Mapeie no `web.xml` a nova página:

```xml
<error-page>
    <error-code>404</error-code>
    <location>/404.jsp</location>
</error-page>
```

Acesse um recurso inexiste no sua aplicação e verifique a nova página de erro, [http://localhost:8080/nao-existe](http://localhost:8080/nao-)

É possível também definir páginas para outros tipos de erros, por exemplo, erros em tempo de execução.

No código abaixo, um exemplo de uma página que lança uma NPE em razão da tentativa de acessar uma variável que não receber os parâmetros.

```html
%@ page contentType="text/html;charset=UTF-8"
         language="java"
         errorPage="error-page.jsp"
%>
<html>
<head>
  <title>Lança Erro</title>
</head>
<body>
  <% String naoExiste = request.getParameter("naoexiste");  %>
  <p><%= naoExiste.toLowerCase() %></p>
  <%@include file="menu.jsp"%>
</body>
</html>
```

Na diretiva `@page` definimos que caso ocorra algum erro, o servidor irá chamar a página `error-page.jsp`.

Abaixo o código da página `error-page.jsp`:

```html
<%@ page contentType="text/html;charset=UTF-8"
         language="java"
         isErrorPage="true"
%>
<html>
<head>
  <title>Página de Erro</title>
</head>
<body>
  <p style="font-weight: bolder;color: red">Erro na execução de sua JSP</p>
  <p style="font-style: italic;font-size: 1.5rem;">
    Exception: <%= exception.toString() %></p>
  <%@include file="menu.jsp"%>
</body>
</html>
```

Nessa página, indicamos na diretiva `@page` que é uma página de erro. Assim, logo que o erro acontecer, será capturado por ela. 

Na sequência, acessará o objeto implícito exception, exibindo o toString().

Mais um exemplo de objeto Java implícito na página JSP, nesse caso, uma Exception.

Existem muitos outros recursos implícitos nas páginas JSP que iremos tratar em outro momento oportuno.

Para melhor organização dos exemplos, criei uma página de índice, que traz links para todas as funcionalidades.

Abaixo, o repositório com o código completo dos exemplos:

- [jsp-outros-recursos](https://github.com/shifttodev/jsp-outros-recursos)


## Para saber mais

- [Implicit Objects](https://docs.oracle.com/cd/E19316-01/819-3669/bnaij/index.html)
