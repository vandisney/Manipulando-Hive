# Manipulando-Hive
Esse repositório tem como objetivo demonstrar uma prática com o framework Hive em conjunto com HDFS, realizado em docker!

### ✍️ 1- Enviar o arquivo local “/input/exercises-data/populacaoLA/populacaoLA.csv” para o diretório no HDFS “/user/aluno/data/populacao”.
  * Acessar o namenode através do comando: `docker exec -it namenode bash`
  * Verificação da pastas contidas no diretório data pelo comando: `hdfs dfs -ls /user/aluno/vandisney/data` , é possível observar que a pasta população não exite, para isso devemos cria-lá com `hdfs dfs -mkdir -p /user/aluno/vandisney/data/populacao`, após estas ações é possível realizar o que a questão solicita com o comando: `hdfs dfs -put /input/exercises-data/populacaoLA/populacaoLA.csv /user/aluno/vandisney/data/populacao`, conforme demonstração das ações na imagem.
 ![imagem1](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem1.png)

### ✍️ 2- Listar os bancos de dados no hive.
  * Acessar o hive `docker exec -it hive-server bash`
  * Acessar o cliente beeline `beeline -u jdbc:hive2://localhost:10000`
  * Listar bancos de dados `show databases;`
![imagem2](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem2.png)

### ✍️ 3- Criar o banco de dados <nome>, em seguida criar a table interna pop 
  A tabela pop terá os campos (zip_code int, total_population int, median_age float, total_males int, total_females int, total_households int, average_house, hold_size float)
row format delimited fields terminated by ','
lines terminated by '\n'
stored as textfile
tblproperties("skip.header.line.count"="1");

> Para criação do banco de dados com o comando `create database vandisney;` em seguinte pode-se listar o banco com `show databases;`, para criação de objetos no banco criado, deve estar 'logado nele' com o comando `use vandisney;`, para a criação da tabela pop solicitada com o comando: 
<p>
create table pop <br/>
(<br/>
zip_code int,<br/>
total_population int,<br/>
 median_age float,<br/>
total_males int,<br/>
total_females int,<br/>
total_households int,<br/>
average_household_size float<br/>
)<br/>
row format delimited<br/>
fields terminated by ','<br/>
lines terminated by '\n'<br/>
stored as textfile<br/>
tblproperties("skip.header.line.count"="1");
</p>

![imagem3](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem3.png)


