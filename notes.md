# BANCOS DE DADOS SQL NO NODEJS


mapa mental da aula:

- https://mm.tt/app/map/2671538433?t=XQZwTwkIX4



# CRIANDO E REMOVENDO UM BANCO DE DADOS


Para criar um banco de dados:


- CREATE DATABASE universe_API_development;


Para listar as tabelas que você possui:


- \l


Para se conectar ao banco de dados criado:


- \c universe_API_development


Para apagar o banco de dados criado:


- DROP DATABASE universe_API_development;



# TIPOS DE DADOS


Documentação do PostgreSQL com os tipos de dados:

- https://www.postgresql.org/docs/8.4/datatype.html?_gl=1*14tom99*_ga*MTkzOTUyNDU4MS4xNjkzODU3MTA2*_ga_37GXT4VGQK*MTY5NTY3MTY3NS42LjEuMTY5NTY3NDY0NS4wLjAuMA..



# CRIANDO E REMOVENDO UMA TABELA


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



# ALTERANDO UMA TABELA


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



# INSERINDO E CONSULTANDO LINHAS NA TABELA


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



# CHAVE PRIMÁRIA

 
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



# ATUALIZANDO E REMOVENDO LINHAS


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



# EXERCÍCIO: (RESOLUÇÃO)


Para criar a tabela:


    CREATE TABLE users (
        id SERIAL PRIMARY KEY,
        name VARCHAR (50) NOT NULL,
        profession VARCHAR (50) NOT NULL,
        birthday DATE NOT NULL
    );


Para inserir os valores:


- INSERT INTO users (name, profession, birthday)
    VALUES ('João', 'Programador', '1991-11-20');

- INSERT INTO users (name, profession, birthday)
    VALUES ('Marta', 'Astronauta', '1987-11-20');


Para listar todas as linhas:


- SELECT * FROM users;


Para incluir a coluna 'vegan':


- ALTER TABLE users ADD COLUMN vegan boolean;


Para atualizar as linhas:


- UPDATE users SET vegan = true WHERE id = 1;
- UPDATE users SET vegan = false WHERE id = 2;


Para apaguar todas as linhas:


- DELETE FROM users;


Para apagar a tabela:


- DROP TABLE users;



# ASSOCIAÇÃO DE TABELAS


- Existem alguns tipos de associações entre tabelas que indicam que elas podem utilizar dados de outras. 

- Ex: Um astronauta tem várias naves, e para identificarmos de qual astronauta é aquela nave, usamos uma foreign key(chave estrangeira) e fazemos essa conexão!


Primeiro vamos criar um banco de dados dentro do postgres: 
    
    
- $ CREATE DATABASE foreignkey
    

Vamos nos conectar a esse banco de dados para criarmos nossas tabelas:
    
    
- $ \c foreignkey
    

Iniciamos criando a tabela “astronauta”: 
    
    
    $ CREATE TABLE astronauta(
        id SERIAL PRIMARY KEY NOT NULL,
        nome VARCHAR(50) NOT NULL,
        idade INT NOT NULL	
    );

    
Vamos agora criar a tabela “naves”, e colocar a foreignkey, para criar o primeiro relacionamento:
    
    
    $ CREATE TABLE nave(
        id SERIAL PRIMARY KEY NOT NULL,
        nome VARCHAR(50) NOT NULL,
        capacidade INT NOT NULL, 
        astronautaId INT,
        CONSTRAINT fkAstronauta FOREIGN KEY(astronautaId) REFERENCES astronauta(id)
    );

    
Agora vamos em “astronauta”, inserir um registro com nome e ID:
    

- $ INSERT INTO astronauta(nome, idade) VALUES (Armstrong, 44)
    

1Dentro da nave, vamos inserir o id do astronauta, para que tenhamos a conexão:
    
    
- $ INSERT INTO nave(nome, capacidade, astronautaid) VALUES (Apollo11, 6, 1)
    

Agora que temos a associação entre tabelas, você não pode criar primeiro a nave, por que o valor id do astronauta não permite ser nulo, então sempre pé obrigatório ter um astronauta dono daquela nave.



# INSTALANDO O SEQUELIZE E SE CONECTANDO AO BANCO DE DADOS


Abrir o terminal do vs code e dar um comando de init:
    
    
- $ npm init -y

    
Instalar as dependências que iremos precisar (sequelize e banco de dados) Estamos instalando o express para usar mais na frente.
    

- $ npm install sequelize pg pg-hstore express

    
Agora iremos fazer a instalação do sequelize-cli, que fará uma organização básica:
    

- $ sudo npm install -g sequelize-cli
    
- $ sudo npm install sequelize-cli -D

    
Agora iremos dar início as pastas do nosso projeto.
    

- $ sequelize-cli init

    
Agora vamos na pasta config, iremos excluir o arquivo dentro dela e criar um novo arquivo, com o mesmo nome só que JS “config.js” e vamos inserir o código:
    
    
    module.exports = {
        dialect: "postgres",
        host: "localhost",
        username: "postgres",
        password: "123456",
        database: "curso_sequelize",
        define: {
            timestamps: true,
        },
    };

    
Agora iremos criar o nosso DB com um comando vindo do cli também. Você entra no terminal:


- $ sequelize db:create



# CRIANDO SEU PRIMEIRO MIGRATE


Vamos iniciar a criação da migrate, que será por onde criaremos nossas tabelas. Vamos começar com um comando no terminal:
    
    
    
- $ sequelize migration:create --name=planets

    
Ele cria um up e um down já com um exemplo aqui em comentário.

Up, serve para fazer fazer alguma ação, nesse caso criar tabelas, o down para você desfazer comentários, por isso eles precisam ser alterados juntos.


Vamos primeiro alterar o nome da tabela, que está entre “” de users para planets, depois vamos adicionar mais configurações dentro dos {} onde está inicialmente o id.
        

    up: async (queryInterface, Sequelize) => {
        await queryInterface.createTable("planets", { id: Sequelize.INTEGER });
    },
    
    down: async (queryInterface, Sequelize) => {
        await queryInterface.dropTable("planets");
    },
    

Vamos agora adicionar informações para a nossa tabela, já temos o ID, mas vamos adicionar informações nesse id e também outras informações que queremos ter na tabela.
    
    
    "use strict";
    
    module.exports = {
      up: async (queryInterface, Sequelize) => {
        await queryInterface.createTable("planets", {
          id: {
            type: Sequelize.INTEGER,
            autoIncrement: true,
            allowNull: false,
            primaryKey: true,
          },
          name: {
            type: Sequelize.STRING,
            allowNull: false,
          },
          position: {
            type: Sequelize.INTEGER,
            allowNull: false,
          },
          createdAt: {
            type: Sequelize.DATE,
          },
          updatedAt: {
            type: Sequelize.DATE,
          },
        });
      },
    
      down: async (queryInterface, Sequelize) => {
        await queryInterface.dropTable("planets");
      },
    };
    

Agora, iremos dar um comando de terminal para fazer a criação do nosso migrate:


- $ sequelize db:migrate


E agora você pode verificar no terminal se foi criado com sucesso. Pode também ir ver através do terminal a criação dele, mas caso queira, temos também alternativas visuais, uma delas é o pgAdmin, que você pode baixar e configurar por esse link: https://www.pgadmin.org/

Você vendo através do pgadmin, pode verificar que tem duas tabelas, uma que você criou e outra que se chama SequelizeMeta. Nela estarão os comandos que você deu através do “db:migrate”. Isso serve para controle e também para o comando para desfazer, que caso você queira testar agora, basta digitar
    
    
- $ sequelize db:migrate:undo

    
E agora a sua tabela sumiu, mas dando “db:migrate” ela vai voltar ao normal. Isso é útil caso você tenha colocado um item errado. Mas não pode fazer isso depois que já foi para produção.



# INSERINDO VALORES VIA SEQUELIZE


Vamos inserir valores via sequelize e também vamos fazer a leitura desses valores inseridos.

Vamos criar dentro da pasta config um arquivo chamado sequelize.js, ele terá as configurações que iremos usar no topo dos nossos models:
    
    
    const Sequelize = require("sequelize");
    const database = require("./config");
    
    const sequelize = new Sequelize(database);
    
    module.exports = sequelize;
    
    
Vamos criar um arquivo chamado de “Planet.js” dentro de models e também vamos criar um arquivo chamado de “index.js” na raiz do projeto. Primeiro vamos fazer a config de Planet.js:
    
    
    const { DataTypes } = require("sequelize");
    const sequelize = require("../config/sequelize");
    
    const Planet = sequelize.define("planets", {
      name: DataTypes.STRING,
      position: DataTypes.INTEGER,
    });
    
    module.exports = Planet

    
Agora iremos fazer as configurações dentro de “index.js”:
    
    
    (async () => {
      const Planet = require("./models/Planet");
    
      const newPlanet = await Planet.create({
        name: "Terra",
        position: 3,
      });
      console.log(newPlanet);
    })();

    
Agora, rodando um comando no terminal, teremos a confirmação no próprio terminal se está correto ou não e se foi criado ou não!
    
    
- $ node index

    

# CONSULTANDO VALORES VIA SEQUELIZE


Vamos agora fazer dentro do próprio index um novo comando único para visualização dos nossos dados, aqui mesmo no console.
    

    const seePlanets = await Planet.findAll();
    
    console.log(seePlanets);

    
2Caso você queira pegar um planeta específico, podemos usar também um outro método para achar findbyPk(primary key) E nesse caso vai retornar apenas o planeta com ID = 1
    
    
    const seePlanets = await Planet.findByPk(1);
    
      console.log(seePlanets);

    
Podemos também ter um método que acha planetas através do seu nome, para pegarmos outras informações dele. Digamos que você não sabe o ID dele, então você pode recorrer ao nome, então você vai usar um novo estilo de método
    
    
    const seePlanets = await Planet.findAll({
        where: {
          name: "Terra",
        },
      });

    
Com isso, já temos os métodos de criar e de ler.


# ATUALIZANDO VALORES VIA SEQUELIZE
