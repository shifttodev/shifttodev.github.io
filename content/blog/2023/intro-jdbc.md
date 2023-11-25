title=Introdução ao JDBC
date=2023-11-21
type=post
tags=web, java, jdbc
status=published
~~~~~~

O JDBC, acrônimo para Java Database Connectivity, é um conjunto de pacotes que viabiliza o desenvolvimento de aplicações Java capazes de interagir com bancos de dados relacionais.

Os principais pacotes incluem:

1. java.sql: Este pacote disponibiliza recursos essenciais para interação com bancos de dados, tais como:
        - DriverManager
        - Statement
        - PreparedStatement
        - Connection
        - SQLException, entre outros.

2. javax.sql: Este pacote oferece funcionalidades adicionais, incluindo:
        - DataSource
        - DriverManager, entre outros.

Esses pacotes fornecem uma base sólida para a criação de aplicações Java que necessitam estabelecer conexões com bancos de dados relacionais, realizar consultas e atualizações, além de lidar com exceções associadas a operações de banco de dados. O uso combinado desses recursos capacita os desenvolvedores a criar aplicações robustas e eficientes no ambiente Java.

## Operações CRUD - create, read, update e delete

Para realizar todas essas operações, é necessário estabelecer uma conexão com o banco de dados. Portanto, o código para essa conexão será exibido apenas na primeira operação.

### Read

Essa classe cria e retorna uma conexão com o banco de dados, a ser utilizada nas demais operações.

```java
public class ConnectionFactory {
    public static Connection getConnection(){

        try {
            Class.forName("com.mysql.cj.jdbc.Driver").newInstance();
            return DriverManager.getConnection("jdbc:mysql://url-do-banco/nome-do-banco",
                    "user", "passwd");
        } catch (SQLException | IllegalAccessException | InstantiationException | ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
    }
}
```

Na sequência, estamos efetivamente fazenda leitura dos dados. 

Essa classe, recebe uma conexão com o banco e através dela acessa os dados. Por fim, exibe o valor de dois campos de cada registro.

```java
public class Main {
    public static void main(String[] args) {
        Connection conn = ConnectionFactory.getConnection();
        Statement st = null;
        ResultSet rs = null;
        try {
            st = conn.createStatement();
            rs = st.executeQuery("SELECT titulo, isbn, edicao, ano, descricao FROM livros");

            while(rs.next()) {
                System.out.printf("%-30s %s\n",
                        rs.getString("titulo"), rs.getString("isbn"));

            }
        } catch(SQLException e){
            throw new RuntimeException(e);
        } finally {
            try {
                rs.close();
                st.close();
                conn.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
'
```

Para isso, estamos utilizando:

- `Statement:` instrução a ser enviado ao banco de dados
- `ResultSet:` conjunto de dados retornados do banco de dados, o qual, podemos iterar sobre ele.

#### Utilizando `try-with-resources`

A utilização dessas classes pode lançar algumas exceptions, entre elas a SQLException, por isso da necessário do uso do try.

Além disso, como estamos abrindo a conexão com recursos, precisamos ao final, fechá-los. Nesse caso, temos que utilizar outro try para os métodos close.

Existem outras maneiras de lidar com essa complexidade, mas a partir do Java 8, temos o recurso do `try-with-resources`, que possíbilita simplificar um pouco o código:

```java
public class Main {
    public static void main(String[] args) {
        try(
            Connection conn = ConnectionFactory.getConnection();
            Statement st = conn.createStatement();
            ResultSet rs = st.executeQuery(
                "SELECT titulo, isbn FROM livros");

        ) {
            while(rs.next()) {
                System.out.printf("%-30s %s\n",
                        rs.getString("titulo"), rs.getString("isbn"));
            }
        } catch(SQLException e){
            throw new RuntimeException(e);
        }
    }
}
```

Dessa forma, não precisamos fechar explicitamente os recursos abertos, pois objetos que implementam a interface `AutoCloseable` serão fechados automaticamente pelo try-with-resources.

### Create

Nesta operação estamos criando/inserindo um novo regitro no banco de dados. 

```java
    static void insert(){
        Livro livro = getNewBook();
        try(
            Connection conn = ConnectionFactory.getConnection();
            PreparedStatement ps = conn.prepareStatement(
                "INSERT INTO livros (titulo, isbn, edicao, ano)" +
                        "VALUES (?,?,?,?)");
        ){
            ps.setString(1, livro.getTitulo());
            ps.setString(2, livro.getIsbn());
            ps.setInt(3, livro.getEdicao());
            ps.setInt(4, livro.getAno());

            int row = ps.executeUpdate();

            if (row > 0){
                System.out.printf("Livro %s inserido com sucesso!\n", livro);
            } else {
                System.out.println("Falha ao inserir o registro.");
            }

            ps.clearParameters();

        }catch (SQLException e){
            throw new RuntimeException(e);
        }
    }
```

Para inserção precisamos dos seguintes elementos:

- a conexão
- a instrução a ser executada no banco, juntamente com os dados

Com esses elementos definidos, executamos a instrução e verificamos o retorno. Caso o retorno seja maior que 0, a operação foi bem sucedida.

A implementação da interface PreparedStatement possibilita criar uma instrução capaz de receber parâmetros, ou seja, os dados que desejamos inserir no banco.

Após isso, utilizamos o método `executeQuery` para enviar a instrução com os dados para o banco.

### Delete

Assim como na instrução de insert, para remover remover registros com JDBC também devemos criar uma instrução, parametrizá-la e enviarmos para o banco de dados.

```java
    static void delete(){
        String isbnToDel = "9781886701939";

        try(
            Connection conn = ConnectionFactory.getConnection();
            PreparedStatement ps = conn.prepareStatement(
                    "DELETE FROM livros WHERE isbn = ?"
            );
        ){
            ps.setString(1, isbnToDel);
            int row = ps.executeUpdate();

            if (row > 0){
                System.out.printf("Registro  %s removido com sucesso.\n", isbnToDel);
                return;
            }

            System.out.println("Falha ao remover o registro.");

        } catch (SQLException e){
            throw new RuntimeException(e);
        }
    }
```

No exemplo acima, estamos utilizando um campo chave da tabela livros, para localizar o registro desejado. 

Caso exista esse registro no banco de dados, o comando retornará um valor maior que 0.

### Update

Nessa operação, atualizamos um ou mais registros no banco de dados.

```java
    static void update(){
        Livro livro = getNewBook();
        String isbn = "9790060776267";

        try(
            Connection conn = ConnectionFactory.getConnection();
            PreparedStatement ps = conn.prepareStatement(
                    "UPDATE livros SET " +
                            "titulo = ?, " +
                            "edicao = ?, " +
                            "ano = ? " +
                            "WHERE isbn = ?")
        ) {
            ps.setString(1, livro.getTitulo());
            ps.setInt(2, livro.getEdicao());
            ps.setInt(3, livro.getAno());
            ps.setString(4, isbn);

            int row = ps.executeUpdate();
            if (row > 0) {
                System.out.printf("Registro %s atualizado com sucesso.\n", 
                        isbn);
            } else {
                System.out.println("Falha ao atualizar o registro.");
            }

            ps.clearParameters();

        } catch (SQLException e){
            throw new RuntimeException(e);
        }
    }
```

O código de update é muito parecido com o código de insert. A maior diferença acontece no SQL.

## Projeto utilizando JDBC

- [intro-jdbc](git@github.com:shifttodev/intro-jdbc.git)