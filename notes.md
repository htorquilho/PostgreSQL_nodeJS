BANCOS DE DADOS SQL NO NODEJS


mapa mental da aula:

- https://mm.tt/app/map/2671538433?t=XQZwTwkIX4



CRIANDO E REMOVENDO UM BANCO DE DADOS


Para criar um banco de dados:


- CREATE DATABASE universe_API_development;


Para listar as tabelas que você possui:


- \l


Para se conectar ao banco de dados criado:


- \c universe_API_development


Para apagar o banco de dados criado:


- DROP DATABASE universe_API_development;




TIPOS DE DADOS


Documentação do PostgreSQL com os tipos de dados:

- https://www.postgresql.org/docs/8.4/datatype.html?_gl=1*14tom99*_ga*MTkzOTUyNDU4MS4xNjkzODU3MTA2*_ga_37GXT4VGQK*MTY5NTY3MTY3NS42LjEuMTY5NTY3NDY0NS4wLjAuMA..



CRIANDO E REMOVENDO UMA TABELA


Modelo do comando para criar uma tabela:


    CREATE TABLE table_name (
        column1 datatype(length) column_contraint,
        column2 datatype(length) column_contraint,
        columnX datatype(length) column_contraint,
        table_constraints
    );

- CREATE TABLE = Comando para criar a tabela


- table_name = Nome da tabela


- column1...X = Nome da coluna


- datatype = Tipo do dado (numérico, texto e etc)


- length = Tamanho da coluna


- column_contraint = São regras específicas aplicadas nas colunas de uma tabela, ou na tabela em si. É possível definir restrições em colunas e em tabelas, permitindo um grande controle sobre os dados armazenados.


- table_constraints = Definições extras sobre a tabela (Chave primária e etc)


Para criar o seu banco de dados:


- CREATE DATABASE universe_api_development;


Para se conectar ao banco:


- \c universe_api_development;


Para ver as tabelas existentes:


- \dt ou \dt+ (para ver mais informações, quando houver)


Para criar a tabela:


    CREATE TABLE planets (
        name VARCHAR (50) NOT NULL,
        star_name VARCHAR (50) NOT NULL,
        code VARCHAR (20) UNIQUE NOT NULL,
        discovery_date DATE,
        satellites INT
    );


Para ver a tabela criada:


- \dt+


Para ver o schema da tabela:


- \d planets


Para remover a tabela:


- DROP TABLE planets;


Para conferir se a tabela foi removida:


- \d planets


Leituras extras:

- https://www.postgresql.org/docs/9.1/sql-createtable.html

- https://www.techonthenet.com/postgresql/primary_keys.php

- https://www.postgresql.org/docs/8.1/ddl-constraints.html



ALTERANDO UMA TABELA


Para criar uma tabela:


    CREATE TABLE planets (
        name VARCHAR (50) NOT NULL,
        sun_name VARCHAR (50) NOT NULL,
        code VARCHAR (20) UNIQUE NOT NULL,
        discovery_date DATE,
        satellites INT,
        has_life BOOLEAN
    );


Verificando como estão as colunas da tabela:


- \d planets


Para alterar a tabela, adicionando uma coluna a ela:


- ALTER TABLE planets ADD COLUMN description text;


Verificando como estão as colunas da tabela:


- \d planets


Para alterar a tabela, removendo uma coluna:


- ALTER TABLE planets DROP COLUMN description;


Verificando como estão as colunas da tabela:


- \d planets


Para alterar a tabela, incluindo uma Constraint em uma coluna:


- ALTER TABLE planets ADD CHECK (satellites > 1);


Para alterar a tabela, incluindo um valor default para uma coluna:


- ALTER TABLE planets ALTER COLUMN has_life SET DEFAULT true;


Para renomear uma coluna:


- ALTER TABLE planets RENAME COLUMN sun_name TO star_name;


Verifique como estão as colunas da tabela:


- \d planets


Leitura recomendada:

- http://pgdocptbr.sourceforge.net/pg80/ddl-alter.html



INSERINDO E CONSULTANDO LINHAS NA TABELA


Para inserir uma nova linha na tabela:


- INSERT INTO planets (name, star_name, code, discovery_date, satellites)
    VALUES ('X45', 'Rimuru', 'SDF54FD8SX', '1961-06-16', 10);


Para consultar todos os valores da tabela:


- SELECT * FROM planets;


Para testar a constraint de satélites que adicionamos na aula anterior (um planeta tem que ter mais que um satélite):


- INSERT INTO planets (name, star_name, code, discovery_date, satellites, has_life)
    VALUES ('X45', 'Rimuru', 'SDF54FD8SX', '1961-06-16', 0, false);


inserindo mais alguns dados para podermos fazer uma consulta mais seletiva dos dados:


- INSERT INTO planets (name, star_name, code, discovery_date, satellites, has_life)
    VALUES ('Marte', 'Sol', 'SDSD767', '1672-06-16', 2, false);

- INSERT INTO planets (name, star_name, code, discovery_date, satellites, has_life)
    VALUES ('Saturno', 'Sol', 'VSD8SD', '1610-06-16', 82, false);


Consultando apenas os valores da coluna name:


- SELECT name FROM planets;


Consultando apenas os valores das colunas name e star_name:


- SELECT name, star_name FROM planets;


Selecionando do banco apenas planetas onde a estrela é o sol:


- SELECT * FROM planets WHERE star_name = 'Sol';


Selecionando do banco apenas planetas que tem vida:


- SELECT * FROM planets WHERE has_life = 'true';


Selecionando do banco apenas planetas que foram descobertos a partir de 1600:


- SELECT * FROM planets WHERE discovery_date > '1600-01-01';


Selecionando planetas com mais de 4 satélites:


- SELECT * FROM planets WHERE satellites > 4;



Leituras recomendadas:

- https://www.postgresqltutorial.com/postgresql-where



CHAVE PRIMÁRIA

 
- A chave primária, ou Primary Key (PK) é o identificador único de um registro na tabela. Pode ser constituída de uma coluna (chave simples) ou pela combinação de dois ou mais colunas (chave composta), de tal maneira que não existam dois registros com o mesmo valor de chave primária.

- A constraint Primary Key (chave primaria), não permite valores nulos e impõe a exclusividade de linhas.

- O principal objetivo da primary key é nos ajudar a selecionar as linhas do banco de dados de forma mais simples, por exemplo, para pegar o planeta X, basta fazer um WHERE pelo ID dele.


Para adicionar uma chave primária a nossa tabela existente:


- ALTER TABLE planets ADD COLUMN ID SERIAL PRIMARY KEY;


Para ver a estrutura da tabela:


- \d planet


Para ver como o id foi inserido em todas as linhas:


- SELECT * FROM planets;


Para apagar a tabela para ver como inserir o id desde o inicio:


- DROP TABLE planets;


Criando novamente a tabela, agora com o id:


    CREATE TABLE planets (
        id SERIAL PRIMARY KEY,
        name VARCHAR (50) NOT NULL,
        star_name VARCHAR (50) NOT NULL,
        code VARCHAR (20) UNIQUE NOT NULL,
        discovery_date DATE,
        satellites INT,
        has_life BOOLEAN
    );


Inserindo alguns dados:


- INSERT INTO planets (name, star_name, code, discovery_date, satellites, has_life)
    VALUES ('X45', 'Rimuru', 'SDF54FD8SX', '1961-06-16', 10, true);

- INSERT INTO planets (name, star_name, code, discovery_date, satellites, has_life)
    VALUES ('Marte', 'Sol', 'SDSD767', '1672-06-16', 2, false);

- INSERT INTO planets (name, star_name, code, discovery_date, satellites, has_life)
    VALUES ('Saturno', 'Sol', 'VSD8SD', '1610-06-16', 82, false);


Verificando a presença do id selecionando tudo:


- SELECT * FROM planets;


Para pegar os dados apenas de um Planeta usando o ID:


- SELECT * FROM planets WHERE id = 1;



ATUALIZANDO E REMOVENDO LINHAS


Antes de atualizarmos uma linha na tabela, vamos resgatar todos os valores para vermos os IDs (que vão nos auxiliar na hora de selecionar a linha para atualizar):


- SELECT * FROM planets;


Agora para atualizar o planeta 'X45', vamos resgatar os dados dele pelo id (1) e passar o comando para alterar o nome da estrela dele:


- UPDATE planets SET star_name = 'Tempest' WHERE id = 1;


Para vermos a mudança, selecionando apenas o planeta afetado:


- SELECT * FROM planets WHERE id = 1;


Você também pode rodar o comando abaixo para atualizar o campo e retornar a linha ao final do processo:


- UPDATE planets SET star_name = 'Tempest' WHERE id = 1 RETURNING *;


Para apagar uma linha:


- DELETE FROM planets WHERE id = 3;


Verifique que você não consegue mais selecionar aquela linha pelo id:


- SELECT * FROM planets WHERE id = 3;



Leituras recomendadas:

- https://www.postgresqltutorial.com/postgresql-update

- https://www.postgresqltutorial.com/postgresql-delete


Exercício: criando uma tabela e consultando valores.
