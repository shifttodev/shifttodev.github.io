title= Iniciando com Servets
date=2023-11-18
type=post
tags=web,java
status=published
~~~~~~

O desenvolvimento web utilizando Java envolve a edição Enterprise (Java EE). Essa implementação, construída sobre a plataforma SE, oferece recursos que viabilizam a criação de aplicações em rede, possibilitando, por conseguinte, o desenvolvimento de aplicativos web.

Uma tecnologia fundamental no desenvolvimento Web com Java é o Servlet.

Os Servlets são componentes, que quando instalados em um servidor que implemente um Servlet Container, têm a capacidade de processar requisições e gerar respostas.

Dentre suas características estão:

- Ciclo de vida bem definido
- Tratamento de requisições
- Gerenciamento de Sessão
- Aspectos Segurança

## A API de Servlet do Java

Para desenvolver um servlet simples, é necessário compreender a hierarquia envolvida nas classes dessa API, implementar suas funcionalidades e realizar o deploy em conjunto com um Servlet Container.

Para isso, recorreremos à [documentação](https://javaee.github.io/javaee-spec/javadocs/). 

Já no overview, observamos que, nesta edição da API, os pacotes são organizados sob o pacote principal `javax`.

Um pacote relevante é o `javax.servlet`, que, conforme a própria documentação afirma:

_The javax.servlet package contains a number of classes and interfaces that 
describe and define the contracts between a servlet class and the runtime 
environment provided for an instance of such a class by a conforming servlet 
container._

Em outras palavras, é nesse ponti que se _"inicia"_ a especificação de servlets. Existem outros pacotes mais especializados, mas, por enquanto, adicionaremos somente o pacote `javax.servet.http`:

_The javax.servlet.http package contains a number of classes and interfaces that 
describe and define the contracts between a servlet class running under the 
HTTP protocol and the runtime environment provided for an instance of such a 
class by a conforming servlet container._

Isso é exatamente o que desejamos: criar um componente capaz de processar requisições feitas com o protocolo HTTP.

## Criando um Sevlet de exemplo

Assumindo a existência de um ambiente com Java 8 e Maven configurado, vamos criar um projeto web utilizando Maven.

```bash
$ mkdir simple-servlet
$ cd simple-servlet
$ mvn archetype:generate -DgroupId=io.github.lucianodacunha -DartifactId=simple-servlet -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
```
Utilizando esse archetype, criaremos um projeto web, seguindo a estrutura exigida pela especificação do Servlet.

```
├── pom.xml
└── src
    └── main
        └── webapp
            ├── index.jsp
            └── WEB-INF
                └── web.xml
```

Essa é a estrutura criada com o comando acima ao especificarmos o archetype `mave-archetype-webapp`. 

- `pom.xml`: Esse é o arquivo que descreve todas as configurações do projeto
- `src/main`: Diretório dos códigos-fonte Java.
- src/main/webapp:  
  - `index.jsp`: página Java Server Page, outra tecnologia JEE.
  - `WEB-INF`: Diretório destinada à páginas web e ao descritor dos componentes.
    - `web.xml`: Descreve informações das configurações dos componentes Web (Servlets). A partir da versão 3, dos Servlet, esse xml já não é mais tão necessário, possibilitando a publicação de componentes utilizando annotations.

Com base nessa estrutura, desenvolvemos uma classe que implementará um método capaz de processar requisições GET.

```bash
$ mkdir -p src/main/java/servlet
$ touch src/main/java/servlet/HelloServlet.java
```
Nessa classe, inserimos o seguinte código: 

```java
package servlet;

import java.io.*;
import javax.servlet.*;
import javax.servlet.annotation.*;
import javax.servlet.http.*;

@WebServlet(urlPatterns = "/hello")
public class HelloServlet extends HttpServlet{
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) 
        throws ServletException, IOException {
            PrintWriter out = resp.getWriter();
            out.println("Hello, world!");
    }
}
```
No código acima, estamos utilizando uma extensão da classe `HttpServlet` para implementar um método que responde a uma requisição do tipo GET.

Para isso, importamos os recursos do pacote `java.io.*`, que proporciona  funcionalidades para impressão e envio de mensagens ao navegador.

Fazemos uso de classes dos pacotes `javax.servlet`, que inclui a classe `ServletException`. 

Alé disso, utilizamos classes do pacote `javax.servlet.http`, que oferece recursos como a classe `HttpServlet`. Esta classe abstrata que dever ter pelo menos um de seus métodos sobrecarregados para disponibilizar funcionalidades de requests/responses com HTTP.

Por fim, empregamos recursos do pacote `javax.servlet.annotation`, que disponibiliza anotações existentes desde a versão 3 da API. As annotations forma introduzidas para facilitar o desenvolvimento, reduzindo a necessidade do uso de descritores xml.

Ao acessar o recurso, o usuário receberá uma resposta contendo a expressão `Hello, world!`.

Nesse exemplo, implementamos somente o método doGet, que responde às chamadas GET, feitas à aplicação.

## O arquivo `web.xml`

Localizado na pasta `WEB-INF`, esse arquivo já não é mais tão essencial. A partir da versão 3 da API de Servlet, podemos empregar annotations para realizar configurações, reduzindo consideravelmente o uso de XML no projeto.

Entretanto, se necessário, ainda podemos configurá-lo para realizar o mapeamento dos Servlets. Na prática, optamos por utilizar annotations ou recorremos ao uso do web.xml.

Segue abaixo um exemplo de como o arquivo apresentaria, caso não estivessemos utilizando annotations:

```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee 
         http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
    <!-- Opcional a partir da Servlet 3-->
    <servlet>
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>servlet.HelloServlet</servlet-class>
    </servlet>  
    <servlet-mapping>
        <servlet-name>HelloServlet</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>    
</web-app>
```

## O arquivo `POM.xml`

Esse arquivo descreve todas as configurações do projeto, incluindo dependências e plugins utilizados no Maven.

Para este projeto, utilizaremos uma dependência do Servlet e um plugin do Jetty.

Veja abaixo como ficou o arquivo:

```xml
project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>io.github.lucianodacunha</groupId>
  <artifactId>simple-servlet</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>simple-servlet Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>    
  </dependencies>
  <build>
    <finalName>simple-servlet</finalName>
    <plugins>
			<plugin>
				<groupId>org.eclipse.jetty</groupId>
				<artifactId>jetty-maven-plugin</artifactId>
				<version>9.4.28.v20200408</version>
				<configuration>
					<scanIntervalSeconds>10</scanIntervalSeconds>
					<webApp>
						<contextPath>/</contextPath>
					</webApp>
				</configuration>
			</plugin>
		</plugins>    
  </build>
</project>
```

O plugin do Jetty é o responsável por publicar o projeto utilizando apenas o goal `jetty:run` do Maven. Do contrário, seria necessário instalarmos e configurarmos um Servidor Web.

## Deploy do projeto

Nessa etapa, vamos compilar o projeto para o deploy no servidor:

Na pasta raiz do projeto, execute o comando:

```bash
$ mvn clean package jetty:run

```
Essa sequência de goals irá:
- limpar a pasta do projeto, caso tenha algo compilado, 
- fazer um novo empacotamento, 
- subir o servidor Jetty e publicar o projeto.

Para verificar a saída gerada, no navegador, acesse [http://localhost:8080](http://localhost:8080). 

## Para saber mais:

- [Como instalar e configurar o Java usando SDKMan!](https://sdkman.io/)
- [Installing Apache Maven](https://maven.apache.org/install.html)
- [Jetty : The Definitive Reference](https://eclipse.dev/jetty/documentation/jetty-9/index.html#startup)
- [Maven and Jetty](https://eclipse.dev/jetty/documentation/jetty-9/index.html#maven-and-jetty)
- [JSR-340: Java Servlet 3.1 Specification](https://download.oracle.com/otn-pub/jcp/servlet-3_1-fr-eval-spec/servlet-3_1-final.pdf?AuthParam=1700311911_ed34a23ed1055340619adf3292883904)
- [Web Server/Servlet Container Jetty](https://eclipse.dev/jetty/)