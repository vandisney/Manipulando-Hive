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

### ✍️ 4- Visualizar a descrição da tabela pop.
Para isso podemos executar o `desc pop` que retorna as colunas com os tipos de dados da tabela e o `desc formatted pop` que retorna todas as informações da tabela de forma detalhada,como a estrutura e a localização (diretório) de uma tabela qualquer (interna ou externa).
![imagem4](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem4.png)

### ✍️ 5- Selecionar os 10 primeiros registros da tabela pop.
Como é possível observar não foi encontrado nenhum registro. `select * from pop limit 10;`
![imagem5](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem5.png)
>Inserção de dados na tabela pop `load data inpath '/user/aluno/vandisney/data/populacao' overwrite into table pop;`
![imagem6](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem6.png)
>Validação da ação realizada
![imagem7](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem7.png)

### ✍️ 6- Contar a quantidade de registros da tabela pop.
Agora podemos aplicar a consulta podemos contar a quantidade de registros da tabela pop com a seguinte consulta `select count (*) from pop;`
![imagem8](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem8.png)

### ✍️ 7- Criar o diretório '/user/aluno/<nome>/data/nascimento' no HDFS
Para realização da questão, é necessário criar uma pasta 'nascimento'no namenode através do comando `docker exec -it namenode hdfs dfs -mkdir /user/aluno/vandisney/data/nascimento` e posteriormente listar o diretório para observar os diretórios já criados! 
![imagem9](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem9.png)

```Quando trabalhamos com hive é possível criar dois tipos de tabelas: Tabelas Internas e Tabelas Externas. Quando trabalhamos com Tabelas Externas, o Hive não move os dados para seu diretório de armazém. Se a tabela externa for descartada, os metadados da tabela serão excluídos, mas não os dados. Agora, se trabalhamos com tabelas internas, o Hive move os dados para seu diretório de armazém. Se a tabela for eliminada, os metadados da tabela e os dados serão excluídos.```
>Para criar a tabela externa nascimento foi necessario aplicar o seguinte comando create external table nascimento(nome string, sexo string, frequencia int) partitioned by (ano int) row format delimited fields terminared by ',' lines terminated by '\n' stored as textfile location '/user/aluno/vandisney/data/nascimento'; 
![imagem10](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem10.png)

### ✍️ 8- Adicionar partição ano=2015.
Após a criação da tabela externa, podemos adicionar na partição a tabela nascimento do ano de 2015 `alter table nascimento add partition(ano=2015)`.
![imagem11](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem11.png)

### ✍️ 9- Enviar o arquivo local '/input/exercises-data/names/yob2015.txt' para o HDfs no diretorio '/user/aluno/vandisney/data/nascimento/ano=2015
Para isso foi usado o comando: `docker exec -it namenode hdfs dfs -put /input/exercises-data/names/yob2015.txt /user/aluno/vandisney/data/nascimento/ano=2015', observando-se o resultado da ação no Hive `select * from nascimento limit 10;`
![imagem12](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem12.png)

### ✍️ 10- Adicionar partição ano=2016 e ano=2017 e enviar os arquivos 'yob2017.txt' e o 'yob2017.txt' para suas respectivas pastas.
>Para a criação da partição ano=2016 `alter table nascimento add partition(ano=2016)` e para o envio do arquivo `docker exec -it namenode hdfs dfs -put /input/exercises-data/names/yob2016.txt /user/aluno/vandisney/data/nascimento/ano=2016;`
>Para a criação da partição ano=2017 `alter table nascimento add partition(ano=2017);` e para o envio do arquivo `docker exec -it namenode hdfs dfs -put /input/exercises-data/names/yob2017.txt /user/aluno/vandisney/data/nascimento/ano=2017`.
![imagem13](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem13.png)

### ✍️ 11- Selecionar os 5 primeiros registros da tabela nascimento para as partições criadas.
Para a seleção `select * from nascimento where ano=2015 limit 5;` para cada partição.
![imagem14](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem14.png)

### ✍️ 12- Contar a quantidade de nomes de crianças nascidas em 2017 e contar a quantidade de crianças nascidas em 2017.
>Para verificação da quantidade de nomes com a consulta `select count(nome) as qtd from nascimento where ano=2017;` e o sistema irá retornar a quantidade de nomes de crianças do ano de 2017! Vale lembrar que, quando aplicamos consultas sobre uma base de dados ou partição, o Hive realiza a consulta como um MapReduce.
>Depois de saber a quantidade de nomes no ano de 2017 vamos contar a quantidade de crianças nascidas em 2017 com o comando `select sum(frequencia) as qtd from nascimento where ano=2017;`
![imagem15](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem15.png)
![imagem16](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem16.png)

### ✍️ 13- Contar a quantidade de crianças nascidas por sexo no ano de 2015 e mostrar por ordem de ano decrescente a quantidade de crianças nascidas por sexo.
>Para realizar esse processo vamos contar a quantidade de crianças nascidas por sexo no ano de 2015 com a consulta `select sexo, sum(frequencia) as qtd from nascimento where ano=2015 group by sexo;` e assim o sistema irá retornar a quantidade de crianças que nasceram em 2015 filtrado por sexo.
>Agora vamos mostrar por ordem de ano descrecente a quantidade de crianças nascidas por sexo, e para isso vamos consultar com o comando `select ano, sexo, sum(frequencia) as qtd from nascimento group by ano, sexo order by ano desc;`
![imagem17](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem17.png)
![imagem18](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem18.png)

### ✍️ 14- Mostrar por ordem de ano decrescente a quantidade de crianças nascidas por sexo com o nome iniciado com ‘A’ e qual nome e quantidade das 5 crianças mais nascidas em 2016.
>Para resolver a primeira questão vamos mostrar por ordem de ano decrecente, a quantidade de crianças por sexo com o nome iniciado com 'A'. Para isso vamos aplicar a seguinte consulta `select ano, sexo, sum(frequencia) as qtd from nascimento where nome like 'A%' group by ano, sexo order by ano desc;` e só assim o sistema irá retornar a quantidade de crianças com a letra 'A' nascidas de acordo com cada ano.
>Na segunda questão, vamos verificar qual o nome e quantidade das 5 crianças mais nascidas em 2016 com o comando `select nome, max (frequencia) as qtd from nascimento where ano=2016 group by nome order by qtd desc limit 5;`
![imagem19](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem19.png)
![imagem20](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem20.png)

### ✍️ 15- Qual nome e quantidade das 5 crianças mais nascidas em 2016 do sexo masculino e feminino.
Para isso vamos utilizar a seguinte consulta `select nome, max(frequencia) as qtd, sexo from nascimento where ano=2016 group by nome, sexo order by qtd desc limit 5;`
![imagem21](https://github.com/vandisney/Manipulando-Hive/blob/main/imagens/imagem21.png)
