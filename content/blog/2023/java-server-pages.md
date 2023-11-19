title= Java Server Pages
date=2023-11-19
type=post
tags=web,java
status=published
~~~~~~

Serlvets são classes Java que implementam códigos capazes de lidar com requisições HTTP.

Uma vez que toda a requisição HTTP é composta de request/response, a responsabilidade do servlet é tratar esse processamento. 

A resposta gerada pelo Servlet pode se manifestar como uma página web a ser renderizada no cliente. No entanto, a tarefa de escrever páginas HTML diretamente utilizando Servlets pode ser pouco prática. Por esse motivo, surgiram as Java Server Pages (JSP).

As Java Server Pages oferecem uma abordagem mais amigável para a criação de páginas web, suportando a incorporação de código Java em documentos HTML. Essa integração proporciona diversas capacidades à camada de visualização da aplicação web, facilitando o desenvolvimento e melhorando a legibilidade do código.

## Primeira página JSP

Vamos dar uma olhada na estrutura de uma página JSP:

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
  <title>Test JSP</title>
</head>
<body>
  <p>
  <%
    String message = "Código java embedded em um html.";
    out.write(message);
  %>
  </p>
</body>
</html>
```

Apesar de inclusão de código Java facilitar a criação de views, essa prática nunca foi bem aceita. Pois dificulta a manutenção.

Já faz algum tempo que existem outras soluções, como Expressions Language, tagblis e engine de template que oferecem abordagens mais elegantes.

## Principais características de uma `JSP`

- **diretiva `@page`**: informa ao container que esse documento aceitará código Java.
- **código html**: apesar da extensão ser JSP o documento é escrito utilizando tags HTML.
- **código java**: o código Java deve ser inserido entre as tags `<% %>`.

As páginas JSP geralmente são criadas no raiz do diretório WEB-INF/, podendo ficar organizadas em algum sub-diretório, conforme preferir.

```
├── pom.xml
└── src
    └── main
        └── webapp
            ├── index.jsp
            └── WEB-INF
                └── web.xml
```

Na diretíva `@page`, podemos declarar também eventuais `imports` necessários. Exemplo:

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java"
         import="com.github.javafaker.Faker"
%>
...
<%
String message = new Faker().name().fullName();
out.write("Olá "  +message);
%>
  </p>
```

Perceba que diretivas são formas de adicionar funcionalidades às páginas HTML. Assim, temos ainda mais dois tipos de diretivas:

- **include:** permite que um conteúdo de um arquivo seja incluído no código
- **taglib:** permite importar bibliotecas de tags para ser utilizada no código JSP.

## Utilizando a diretiva include

O código abaixo mostra um exemplo de utilização da diretiva `include`:

```jsp
  <%@ include file="header.jsp" %>
  <p>
  <%
    String message = new Faker().name().fullName();
    out.write("Olá "  +message);
  %>
  </p>
  <%@ include file="footer.jsp" %>
```

O exemplo acima, mostra uma solução que pode ser utilizada quando temos que repetir um trecho de código em diversas páginas. 

Desta forma, podemos importar o conteúdo de arquivo para uma página, sem ficar repetindo o código.

## Utilizando a diretiva taglib

O código abaixo demonstra um exemplo de utilização da diretiva `taglib`:

```jsp
...
<%@ taglib uri = "http://java.sun.com/jsp/jstl/core" prefix = "c" %>
<%@ taglib uri = "http://java.sun.com/jsp/jstl/functions" prefix = "fn" %>
<html>
...
```

Esse trecho de código, declara duas taglibs: **core** e **functions**. Essas libs definem um conjunto de funções que podem ser utilizadas nas páginas JSP.

Para utilizar essas funções, utilizamos os prefixos declarados, seguidos das funções. Abaixo um exemplo de utilização.

```jsp
  <c:set value="This is first String." var="string1" />
  <c:set value="${fn:toUpperCase(string1)}" var="string2" />

  <p>Final string : <c:out value="${string1}" /></p>
```

O código acima:

- declara duas variáveis: `string1` e `string2`. 
- Atribui o texto `This is first String.` à variável `string1`.
- converte o texto da variável `string1` para maiúsculas e atribui à variável `string2`.

## Expression Language

Expression Language - EL, como o próprio nome diz, é essa linguagem de expressões que podem ser processadas pelo servidor, permitindo habilitar recursos dinamicamente como acessos a outros componentes da aplicação.

Em sua forma mais básica, as EL possuem operadores, que juntamente com libs de tags mais avançadas permitem construir páginas dinâmicas.

Abaixo, um exemplo de utilização de um EL:

```
<p>Soma de 1 + 1: ${1 + 1}</p>
```

Saída: 

```
Soma de 1 + 1: 2
```

## Resumindo

Ao final das contas, aquilo que podemos realizar com Servlet também pode ser feito com JSP. Isso se deve ao fato de que, após a compilação, a JSP é convertida em um Servlet.

Nos exemplos apresentados anteriormente, observamos que é possível mesclar código Java com HTML. Embora isso tenha sido amplamente aceito no passado, há algum tempo dispomos de soluções mais avançadas.

As Expression Language - EL, surgem como uma maneira de mitigar esse problema. Por meio delas, torna-se possível remover todo o código Java da camada de visualização, utilizando tags que facilitam a manutenção do código.

Em conjunto com as taglibs, as EL oferecem recursos que permitem construir a camada de visualização por meio de páginas robustas e dinâmicas.

## Repositório dos código utilizados

- [simple-jsp](https://github.com/shifttodev/simple-jsp)

## Para saber mais

- [Expression Language](https://docs.oracle.com/javaee/6/tutorial/doc/gjddd.html)
- [Java Server Pages](https://www.oracle.com/java/technologies/jspt.html)