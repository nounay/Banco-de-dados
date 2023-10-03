# Banco-de-dados
-- comando para seleção
SELECT * FROM produtos ORDER BY id ASC;
-- DDL - Linguagem de definição de dados (Interagir com a tabela inteira)
-- criação de tabela
CREATE TABLE produtos(
id INT PRIMARY KEY NOT NULL,
nome VARCHAR(255),
preco DECIMAL(10,2)
);

-- excluir uma tabela
DROP TABLE produtos;

-- Alterar tabela
ALTER TABLE produtos ADD descricao TEXT;

--inserir dado na tabela
INSERT INTO produtos(id, nome, preco) VALUES
 (1, 'Nescau Radical', 8.00),
 (2, 'Leite Fermentado Yakult ', 11.00),
 (3, 'Lacta Branco', 5.00),
 (4, 'Miojo Monica', 2.00);
 
 -- alterar dado da tela
 UPDATE produtos SET preco = 15.00 WHERE id=1;
 UPDATE produtos SET nome = 'Nescau Fitness' WHERE nome LIKE 'Nescau Radical';
 
 UPDATE produtos SET descricao = 'Armazenar em ambiente gelado'
 WHERE id=2 or id=3;
 
 UPDATE produtos SET descricao = 'Promoção'
 WHERE preco < 5.00;
 
 --Deletar dado de uma tabela
 DELETE FROM produtos WHEN nome LIKE 'Alpino amargo';
 DELETE FROM produtos WHERE ID = 1;
 
 -- Comandos DQL (Linguagem de consulta de dados)
 --selecionar todos(*) produtos, ordene ascendente
 SELECT * FROM produtos ORDER BY ASC;
 --Ordenar pelo preço descrescente(maior para menor)
 SELECT * FROM produtos ORDER BY preco DESC;
 --Selecionar colunas
 SELECT nome, preco FROM produtos ORDER BY id;
 --Selecionar produtos maior que R$4,00
 SELECT preco, nome FROM produtos WHERE preco > 4.00;
 --Selecionar o produto mais caro
 SELECT MAX(preco) AS titulo FROM produtos;
 SELECT preco, nome FROM produto ORDER BY valor DESC limit 1;
 
 --simulando busca por nome exato
 SELECT * FROM produtos WHERE nome LIKE 'Nescau radical';
 
 --busca pelos primeiros caracteres, o resto não importa
 SELECT * FROM produtos WHERE nome LIKE 'ne%'
 
 --A parte anterior não importa
 SELECT * FROM produtos WHERE nome LIKE '%Fit';
 
 --busca por qualquer parte do nome
 SELECT * FROM produtos WHERE nome LIKE '%Nestle%'
 
 INSERT INTO produtos(id, nome, preco) VALUES
 (5, 'Nestle Chocolate branco', 8.00),
 (6, 'Nestle caixa de bombom ', 11.00),
 (7, 'Nestle Prestigio', 5.00),
 (8, 'Nestle sensação', 2.00);
 
 
 --Fase 2 -------------------------------------
 
 --Criando tabela funcionario
 
 CREATE TABLE funcionario(
  id SERIAL PRIMARY KEY,
nome VARCHAR(255) NOT NULL,
data_nasc DATE,
sexo CHAR(1),
salario DECIMAL(10,2),
ativo BOOLEAN
 );
 
 SELECT * FROM funcionario;
 
 INSERT INTO funcionario (nome, data_nasc, sexo, salario, ativo) VALUES
 ('Bob Marley','1945-02-06', 'M', 23000.00, true),
 ('Mare Barrow', '2006-03-31', 'F', 30000.00 , false),
 ('Blair Waldorf', '1990-11-15', 'F', 50000.00, true),
 ('Chuck Bass', '1991-01-19', 'M', 45000.00, true);
 
 SELECT * FROM funcionario;
 
 --Seleciona funcionários que nasceram após 01-01-1992
 SELECT * FROM funcionario WHERE data_nasc > '1991-01-01';
 
 --Seleciona funcionários em um período
 SELECT nome, data_nasc FROM funcionario
 WHERE data_nasc BETWEEN '1940-01-01' AND '1990-01-01';
 
 --Contabilizar
 SELECT sexo,
 COUNT(*) AS total_pessoas,          --Contar
 AVG(salario) AS media_salarial      --Média
 FROM funcionario GROUP BY sexo;
 
 --Tabela clientes
 
 SELECT * FROM clientes;
  --1--
 
 CREATE TABLE clientes (
id SERIAL PRIMARY KEY,
nome VARCHAR(255),
email VARCHAR(255)
);


 
 



--2--
ALTER TABLE clientes ADD telefone VARCHAR(20);


 
 


--3--
ALTER TABLE clientes DROP email;

--Tabela produtos
--4--
CREATE TABLE itens(
id SERIAL PRIMARY KEY,
nome VARCHAR(60),
preço decimal(10,2)
);

SELECT * FROM itens;






--5--
INSERT INTO clientes (nome, telefone) VALUES
 ('Beverly','(44) 99879-5346'),
  ('Talia','(44)99849-7683'),
   ('Gislaine','(44)99989-5716');
   
  --6--
INSERT INTO itens(id, nome, preço) VALUES
 (1, 'Swchenneps','6.70'),
  (2, 'Monster','10.00'),
   (3, 'Cola','12.99');
   
   --7--
   UPDATE clientes SET nome = 'Beverly' WHERE id=1;
   
   --8--
   UPDATE clientes FROM preço '10,00' where id=2;
   --9--
   DELETE FROM clientes where id = '4';
   
   --10--
DELETE FROM itens where id = '1';


SELECT * FROM clientes;
SELECT * FROM itens;

--Entendendo Relacionamentos entre tabelas no banco de dados
--Criação das tabelas
CREATE TABLE estados(
id_estado SERIAL PRIMARY KEY,
nome_estado VARCHAR(50) NOT NULL,
sigla CHAR(2)
);

CREATE  TABLE cidades(
id_cidade SERIAL PRIMARY KEY,   --PK chave primária
nome_cidade VARCHAR(50),
cep VARCHAR(50),
id_estado INT REFERENCES estados(id_estado)  --FK Chave Estrangeira

);

--INSERINDO DADOS NAS TABELAS

INSERT INTO estados(id_estado, nome_estado, sigla) VALUES
(1, 'Paraná', 'PR'),
(2, 'São Paulo', 'SP'),
(3, 'Minas Gerais', 'MG');

INSERT INTO cidades(id_cidade, nome_cidade, cep, id_estado) VALUES
(10, 'Nova Londrina', '87970-000', 1),
(11, 'Marilena', '87960-000', 1),
(12, 'Itaúna', '87980-000', 1),
(13, 'Primavera', '19273-000',2),
(14, 'Rosana', '19274-000',2),
(15, 'Cachoeira da Prata', '35765-000', 3);

SELECT * FROM estados;
SELECT * FROM cidades;


--SELECIONAR TODAS AS CIDADES E SEUS ESTADOS
SELECT cidades.nome_cidade, estados.nome_estado
FROM estados INNER JOIN cidades
ON cidades.id_estado = estados.id_estado;
WHERE estados.sigla LIKE 'SP' OR estados.sigla LIKE 'PR'
ORDER BY cidades.nome_cidade ASC;

--SELECIONA OS ESTADOS COM MAIS CIDADES
SELECT estados.nome_estado, COUNT(cidades.id_cidade) AS total_cidades
FROM estados INNER JOIN cidades
ON estados.id_estado = cidades.id_estado
GROUP BY estados.nome_estado
ORDER BY total_cidades DESC;

--CRIANDO MAIS UMA TABELA PARA O RELACIONAMENTO DE CIDADE-ESTADO
CREATE TABLE pessoa(
	id_pessoa SERIAL PRIMARY KEY,
	nome_pessoa VARCHAR(60),
	data_nascimento DATE,
	sexo CHAR(1),
	estado_civil VARCHAR(60),
	profissao VARCHAR(60),
	id_cidade INT REFERENCES cidades(id_cidade)
	);
	
INSERT INTO pessoa
(id_pessoa, nome_pessoa, data_nascimento, sexo, estado_civil, profissao, id_cidade) VALUES
 (1, 'Gustavo', '2002-08-20', 'm', 'solteiro', 'Contador', 10),
 (2, 'Ana', '2002-01-12', 'f', 'solteira', 'Cozinheira', 10),
 (3, 'Brenda', '2000-04-9', 'f', 'casada', 'Professora', 11),
 (4, 'Heitor', '2001-12-17', 'm', 'solteiro', 'fiscal', 11),
 (5, 'Alexandre', '1997-05-06', 'm', 'casado', 'vendedor', 12),
 (6, 'Kauana', '2005-03-31', 'f', 'casada', 'estilista', 12),
 (7, 'Talia', '2006-06-11', 'f', 'enrolada', 'desing', 13),
 (8, 'João', '2004-04-11', 'm', 'solteiro', 'engenheiro', 13),
 (9, 'Gabriel', '2005-04-20', 'm', 'enroldado', 'mecanico', 14),
 (10, 'Renan', '2007-12-16', 'm', 'solteiro', 'golpista', 14);
SELECT * FROM pessoa;



--selecionar pessoa com sua cidade (relacionando tabela pessoa com cidade)
SELECT pessoa.nome_pessoa, cidades.nome_cidade
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade
INNER JOIN estados
ON cidades.id_estado = estados.id_estado

--Seleciona pessoas de Estado do Paraná
SELECT pessoa.nome_pessoa, estados.nome_estado, cidades.nome_cidade
FROM estados INNER JOIN cidades
ON estados.id_estado = cidades.id_estado
INNER JOIN pessoa
ON cidades.id_cidade = pessoa.id_cidade
WHERE estados.nome_estado LIKE 'Paraná';

--Selecione o nome da pessoa, profissão e qual cidade pertence
SELECT pessoa.nome_pessoa, cidades.nome_cidade, pessoa.profissao
FROM pessoa INNER JOIN cidades

ON pessoa.id_cidade = cidades.id_cidade

--Selecionar cidade com mais mulher/homem
SELECT cidades.nome_cidade, COUNT(*) AS Quantidade
FROM cidades INNER JOIN pessoa 
ON cidades.id_cidade = pessoa.id_cidade
WHERE pessoa.sexo = 'm'
GROUP BY cidades.id_cidade
ORDER BY Quantidade desc;


--1 - Selecione todas as pessoas
SELECT * FROM pessoa;

--2 - Selecine todas as pessoas do sexo Masculino "m"
SELECT pessoa.nome_pessoa, pessoa.sexo FROM pessoa
WHERE pessoa.sexo = 'm';
--3 -Selecione todas pessoas com estado civil solteiro;
SELECT pessoa.nome_pessoa, pessoa.sexo FROM pessoa
WHERE pessoa.estado_civil LIKE 'solteiro'
OR pessoa.estado_civil LIKE 'solteira';

SELECT pessoa.nome_pessoa, pessoa.estado_civil FROM pessoa
WHERE pessoa.estado_civil LIKE 'solteir%';

--4 - Selecione as pessoas e sua profissão
SELECT pessoa.nome_pessoa, pessoa.profissao FROM pessoa;

--5 - Selecione as pessoas que nasceram entre 1990-01-10 e 2003-01-01
SELECT pessoa.nome_pessoa, pessoa.data_nascimento
FROM pessoa WHERE data_nascimento BETWEEN '1990-01-10' AND '2003-01-01';

--6 - Selecione as pessoas do Estado de São Paulo
SELECT pessoa.nome_pessoa, estados.nome_estado
FROM pessoa INNER JOIN cidades
INNER JOIN estados
ON pessoa.id_estado = estados.id_estado
WHERE estados.nome_estado LIKE 'São Paulo'








