title=EL & JSTL
date=2023-11-29
type=post
tags=java, web, servlet, jstl, el
status=published
~~~~~~

## Expression Language - EL

EL é uma linguagem utilizada para simplificar o desenvolvimento Java Web, que permite o acesso a dados em ambiente JSP.

Exemplo de utilização:

```
<jsp:useBean id="pessoa" class="app.model.Pessoa">
${pessoa.nome}
```

O código acima, acessa a propriedade `nome` da classe Pessoa instanciada na declaração `jsp:useBean`.

A EL aceita outros recursos como operadores, conversões, boxing, etc.

```
${1 + 1}
${empty pessoa}
```

## Jakarta Standard Tag Library - JSTL

JSTL é um conjunto de libs que estendes as funcionalidades das JSP's, possibilitando o processamento, desenvolvimento lógico e acesso a banco de dados, sem utilizar código scriptlet Java.

Exemplo:

```
<!-- utilizando a instrução if -->
<c:if test="${valor > 10}">
    <p>Exibe a mensagem</p>
</c:if>

<!-- utilizando a instrução choose -->
<c:choose>
    <c:when test="${valor > 10}">
        <p>Mensagem 1</p>
    </c:when>
    <c:otherwise>
        <p>Mensagem 2</p>
    </c:otherwise>
</c:choose>

<!-- utilizando a instrução forEach -->
<c:forEach var="pessoa" items="${pessoas}">
    <p>${pessoa}</p>
</c:forEach>
```

Essas são algumas das tags que podemos utilizar no desenvolvimento de algoritmos dentro de páginas JSP.

## Instalação

Adicione a seguinte dependência no `pom.xml`.

```
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
```

## Projeto utilizando EL e JSTL

- [el-jstl](https://github.com/shifttodev/el-jstl)

## Para saber mais

- [The Java EE 5 Tutorial](https://docs.oracle.com/javaee/5/tutorial/doc/bnake.html)