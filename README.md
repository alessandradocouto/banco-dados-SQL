# Exercicios-Banco-Dados
## Banco de Dados Relacionais SQL

### Questão 1: Construa um esquema relacional equivalente a este diagrama ER, indicando chaves primárias e estrangeiras.

![imagem do diagrama da Questão 1](https://raw.githubusercontent.com/alessandradocouto/Exercicios-Banco-Dados/master/Q_BD.png)


1. Vamos separar todas as tabelas com seus atributos e indicando sua respectiva chave primária(pk):

        cliente(**cpf**, nome,senha,data_nasc)

        reserva(**codigo**, cpf, check-in,check-out,num_hospede,tipo,preco)
            cpf referencia cliente

        quarto(**codigo**, descricao,capacidade,diaria)


2. Vamos analisar o relacionamento entre essas tabelas:

	a. Relacionamento entre *cliente* e *reserva*:
     - (1, 1): Cada reserva é feita por um único cliente.
	 - (0, n): Cada cliente possui nenhuma ou muitas reservas. 	
		
	Identificamos que reserva tem uma chave estrangeira(fk) vinda de cliente, o **n** da entidade Cliente indo para a entidade Reserva.
	
    Cliente não tem chave estrangeira(fk), apenas sua chave primária(pk) mesmo.
        
        cliente(**cpf**, nome,senha,data_nasc)

        reserva(**codigo**, cpf, check-in,check-out,num_hospede,tipo,preco)
            cpf referencia cliente

	b. Relacionamento entre *reserva* e *quarto*:
	 - 1, n): Cada reserva é obrigatória ter 1 quarto ou vários quartos.
	 - (0, n): Cada quarto pode ter nenhuma ou várias reservas.
		
	Repare que nesse tipo de relação que tem muitos quartos(n) e muitas reservas(n), é necessário criar uma outra tabela com os códigos exclusivos(chaves primarias, pk's) de cada tabela, pois esses 2 códigos são chaves primária e estrangeira ao mesmo tempo(serão codigos únicos, mas também vindas de outra tabela).
	
        reservaquarto(**codigoreserva**, **codigoquarto**)
            codigoreserva referencia reserva
            codigoquarto referencia quarto

3. A nova tabela do diagrama ER fica assim:

        cliente(**cpf**,nome,senha,data_nasc)

        reserva(**codigo**,cpf,check-in,check-out,num_hospede,tipo,preco)
            cpf referencia cliente

        quarto(**codigo**,descricao,capacidade,diaria)

        reservaquarto(**codigoreserva**, **codigoquarto**)
            codigoreserva referencia reserva
            codigoquarto referencia quarto





### Questão 2: Observe a tabela a seguir com as chaves primárias indicadas. 

    EMP(**eid**: int, ename: string, idade: int, salario: real)

    DEPT (**did**: int, dnome: string, orçamento: real, gerenteid: int)
        gerenteid referencia EMP(eid)

    TRABALHA (**eid**: int, did: int, cargahoraria: int)
        eid referencia EMP(eid)
        did referencia DEPT(did)


1. Apresente o diagrama ER referente e esse esquema.
2. Dê um exemplo de uma chave estrangeira que envolva (referencie) a tabela DEPT.
3. Escreva os comandos SQL necessários para criar as relações do esquema apresentado, incluindo as restrições de chave primária e chave estrangeira. Considere que a tabela TRABALHA relaciona empregados com departamentos e a tabela DEPT se relaciona com EMP a partir do atributo gerenteid. 
4. Escreva uma instrução SQL para adicionar “Daniel de Oliveira” como um empregado com eid = 125, idade = 40 e salário = 15000. 
5. Escreva uma consulta SQL que retorne a média salarial dos empregados, por
departamento. 
6. Escreva uma consulta SQL que retorna os nomes dos empregados que trabalham no departamento que possui o maior orçamento. 
7. Escreva uma consulta em SQL que retorne os nomes dos departamentos que não
possuem funcionários alocados a eles. A lista deve estar ordenada de forma
decrescente em relação ao nome do departamento. 


### Respostas:

1. A tabela TRABALHA é um caso de n x n, resultando em uma tabela com 1 chave primária e 1 chave estrangeira relativa à EMP e 1 chave primária e 1 chave estrangeira relativa DEPT.
Além disso, há uma relação especial entre EMP e DEPT com a chave estrangeira gerenteid. É permitido que os empregados atue como gerente(gerenteid).

![imagem do diagrama ER da Questão 2 item 1](https://raw.githubusercontent.com/alessandradocouto/Exercicios-Banco-Dados/master/Q2-1.png)


2. Podemos ver que a tabela TRABALHA tem chave estrangeira e também chave primária **did** que referencia a tabela DEPT.

        TRABALHA(eid:int, did:int, cargahoraria: int)
            id referencia EMP(eid)
            did referencia DEPT(did)


3. Vamos mostrar os bancos de dados disponiveis(show), em seguida criaremos (create) e entraremos(use) no banco para inserir os dados:

        SHOW databases;
        CREATE DATABASE Empresa_TechX;
        USE Empresa_TechX;


a. Criando todas as tabelas do esquema relacional com comando sql:

    CREATE TABLE EMP (
        eid INTEGER NOT NULL AUTO_INCREMENT,
        ename VARCHAR(45),
        idade INTEGER,
        salario DECIMAL(15,2),
        CONSTRAINT EMP_pk PRIMARY KEY(eid)
    );

    CREATE TABLE DEPT (
        did INTEGER NOT NULL AUTO_INCREMENT,
        dnome VARCHAR(45),
    orcamento DECIMAL(15,2),
    gerenteid INTEGER,
        PRIMARY KEY(did),
        CONSTRAINT DEPT_EMP_fk FOREIGN KEY(gerenteid) REFERENCES EMP(eid) ON DELETE NO ACTION
    );

    CREATE TABLE TRABALHA (
        eid INTEGER NOT NULL AUTO_INCREMENT,
        did INTEGER NOT NULL,
        cargahoraria INTEGER,
        PRIMARY KEY(eid, did),
        CONSTRAINT TRABALHA_EMP_fk FOREIGN KEY(eid) REFERENCES EMP(eid) ON DELETE CASCADE,
        CONSTRAINT TRABALHA_DEPT_fk FOREIGN KEY(did) REFERENCES DEPT(did) ON DELETE CASCADE
    );


b. Inserindo os dados no banco para teste, há diversas maneiras de fazer isso, mas vamos exercitar os comandos básicos de SQL:

 entrando no banco criado 

`USE Empresa_TechX;`

 para mostrar as tabelas(entidades) criadas 

`SHOW TABLES;`

 inserindo 9 empregados no sql: 

    INSERT INTO EMP (eid,ename,idade,salario) VALUES (117, 'Fabiana Silva Lima', 32, 3000);
    INSERT INTO EMP (eid,ename,idade,salario) VALUES (118, 'Jorginho do Sul', 65, 2500);
    INSERT INTO EMP (eid,ename,idade,salario) VALUES (119, 'Marco Dias', 28, 1980);
    INSERT INTO EMP (eid,ename,idade,salario) VALUES (120, 'Julia Costa da Serra', 33, 4500);
    INSERT INTO EMP (eid,ename,idade,salario) VALUES (121, 'Andre Verissimo', 23, 4400);
    INSERT INTO EMP (eid,ename,idade,salario) VALUES (122, 'Sophia Fernandes', 47, 5900);
    INSERT INTO EMP (eid,ename,idade,salario) VALUES (123, 'Mario Xavier', 58, 3300);
    INSERT INTO EMP (eid,ename,idade,salario) VALUES (124, 'Xanna Alves', 29, 5000);


erro, vamos consertar o salario do empregado de codigo(eid) 122 para 2900 
 
faremos isso atualizando a tabela  EMP com o dado certo passando um filtro desse empregado que é o eid especifico(where)

`UPDATE EMP SET salario = 2900 WHERE eid = 122;`

inserindo os DEPT 

    INSERT TO DEPT(did,dnome,orcamento,gerenteid) VALUES (1,'InfraEstrutura', 45400700, 118);
    INSERT TO DEPT(did,dnome,orcamento,gerenteid) VALUES (2,'Saude', 58900200, 119);
    INSERT TO DEPT(did,dnome,orcamento,gerenteid) VALUES (3,'Transporte', 13600100, 121);
    INSERT TO DEPT(did,dnome,orcamento,gerenteid) VALUES (4,'Comunicacao', 8200990,null);
    INSERT TO DEPT(did,dnome,orcamento,gerenteid) VALUES (5,'Minas_Energia', 18340690,null);

 inserindo na tabela TRABALHA 

    INSERT INTO TRABALHA (eid, did,cargahoraria) VALUES (117, 1, 40);
    INSERT INTO TRABALHA (eid,ename,idade,salario) VALUES (118, 1, 40);
    INSERT INTO TRABALHA (eid,ename,idade,salario) VALUES (122, 1, 44);
    INSERT INTO TRABALHA (eid,ename,idade,salario) VALUES (119, 2, 44);
    INSERT INTO TRABALHA (eid,ename,idade,salario) VALUES (120, 2, 44);
    INSERT INTO TRABALHA (eid,ename,idade,salario) VALUES (123, 2, 40);
    INSERT INTO TRABALHA (eid,ename,idade,salario) VALUES (121, 3, 40);



4. Inserindo na tabela EMP com determinados campos os valores relacionados aos campos correspondentes da tabela:

`INSERT INTO EMP (eid,ename,idade,salario) VALUES (125,'Daniel de Olievira', 40, 1500);`



5. avg é uma função que envolve grupos, então precisaremos usar o **group by** para agrupar os departamentos por nome. Usaremos também as 3 tabelas(EMP,TRABALHA,DEPT) para enconrar tanto os empregados quanto os departamentos que eles trabalham.

        SELECT 
            d.dnome,  AVG(salario) as media_salarial_empregados
        FROM 
            EMP e, DEPT d, TRABALHA t
        WHERE 
            e.eid = t.eid
        AND 
            d.did = t.did
        GROUP BY 
            (d.dnome);



6. Queremos saber os nomes dos empregados que trabalham no departamento(usaremos as 3 tabelas para assim como no item 5, acharmos a relação de qual empregado trabalha em qual departamento).
E faremos uma subquery(subconsulta) do departamento que tem o maior orcamento.


    SELECT 
        e.ename
    FROM 
        EMP e, TRABALHA t, DEPT d
    WHERE 
        t.eid = e.eid 
    AND 
        t.did = d.did
    AND 
        d.orcamento =  (SELECT MAX(d.orcamento)  as maior_orcamento_dept  FROM DEPT d);


7. Para saber se um departamento tem alguém que trabalha nele, pesquisaremos se o código exclusivo da tabela DEPT(**chave primária**) está na tabela TRABALHA, fazendo uma ordenação decrescente pelo nome do departamento.

        SELECT
            d.dnome
        FROM
            DEPT d
        WHERE
            d.did NOT IN (select t.did from TRABALHA t)
        ORDER BY d.dnome DESC;
