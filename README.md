# Respostas da Momento

Respostas das perguntas trabalhadas no repositorio [Momento](https://github.com/gabaugusto/sample-databases/blob/main/Tabelas/7_Exemplos/Momento/README.md), produzido pelo professor Gabriel para o PROA.

--- 

### Departamento de Tecnologia 

* Inclua suas próprias informações no departamento de Tecnologia da empresa.

R:
```sql
insert into momento.funcionarios(funcionario_id, primeiro_nome, sobrenome, email, senha, telefone, data_contratacao, cargo_id, salario, gerente_id, departamento_id)
 values (208, "Gustavo", "Faustino", "gustavo.gfoliveira@momento.org", "4@8@15@14", "11940028922", '2024-01-11', 9, 4500.00, 103, 6);
```

* Agora diga, quantos funcionários temos ao total na empresa?

R: 42
```sql
SELECT count(*) FROM momento.funcionarios;
```


* E quanto ao Departamento de Tecnologia?

R: 6
```sql
SELECT count(*) FROM momento.funcionarios where departamento_id = 6;
```
### Departamento de Vendas 

* **Quantos funcionários trabalham no Departamento de Vendas?**
Use uma consulta para descobrir o número total de funcionários alocados nesse departamento.

R: 5
```sql
SELECT count(*) FROM funcionarios where departamento_id = (select departamento_id from departamentos where departamento_nome = "vendas");
```
* **Salários no Departamento de Vendas**

R:
```sql
-- Média salarial:
select round(avg(salario),2) as 'Media salarial dos Vendedores' from funcionarios where departamento_id = (select departamento_id from departamentos where departamento_nome = "vendas");

-- Todos os salários
SELECT concat(primeiro_nome, sobrenome) as "Nome Completo", salario as "Salários" FROM funcionarios where departamento_id = (select departamento_id from departamentos where departamento_nome = "vendas");
```

* Qual é o custo total dos salários do pessoal de Vendas? Isso nos ajuda a entender o orçamento do departamento!

R:
```sql
-- Soma de todos os salários
select sum(salario) as 'Soma dos salários' from funcionarios where departamento_id = (select departamento_id from departamentos where departamento_nome = "vendas");
```
* Quanto o departamento de Vendas gasta em salários?

R:
```sql
-- Soma de todos os salários
select sum(salario) as 'Soma dos salários' from funcionarios where departamento_id = (select departamento_id from departamentos where departamento_nome = "vendas");
```

* Quais são os produtos mais vendidos e quais têm pouca ou nenhuma saída?

R:
```sql
SELECT 
    produtos.produto_nome AS "Nome do produto",
    MIN(quantidade) AS "Mais vendido",
    MAX(quantidade) AS "Menos vendidos"
FROM 
    vendas 
INNER JOIN 
    produtos ON produtos.produto_id = vendas.produto_id
GROUP BY 
    produtos.produto_nome;
```

* Qual é o produto mais caro no inventário da empresa?

R: Computador do escritório Umbrella Corp
```sql
select suprimento_nome, quantidade, custo, round((custo / quantidade),2) as "preço unitário",
suprimentos.escritorio_id from suprimentos order by (custo / quantidade) desc limit 1;
```
### Departamento de Inovações 

* **Um novo departamento foi criado. O departamento de Inovações.** 
Ele será locado no Brasil. Por favor, adicione-o no banco de dados da empresa colocando quaisquer informações que você achar relevantes.

R:
```sql
insert into momento.escritorios(escritorio_id, escritorio_nome, endereco, cep, cidade, estado_provincia,pais_id)
 values (4200, "New Londo Ruins", "2230 Avenida Paulista", "01311-000", "São Paulo", "São Paulo", "BR");

 insert into momento.departamentos (departamento_id, departamento_nome, escritorio_id)
values (14, "Inovações", 4200);
```

* O departamento de Inovações está sem funcionários. Inclua alguns colegas de turma nesse departamento.  

R:
```sql
insert into momento.funcionarios 
(funcionario_id, primeiro_nome, sobrenome, email, senha, telefone, data_contratacao, cargo_id, salario, gerente_id, departamento_id)
values 
(2001, "Lucas", "Ferreira", "lucas.ferreira@example.com", "senha123", "+5511999990001", "2024-11-01", 20, 21000.00, NULL, 14),
(2002, "Ana", "Silva", "ana.silva@example.com", "senha123", "+5511999990002", "2024-11-01", 14, 9000.00, NULL, 14),
(2003, "Marcos", "Lima", "marcos.lima@example.com", "senha123", "+5511999990003", "2024-11-01", 21, 8000.00, NULL, 14),
(2004, "Beatriz", "Santos", "beatriz.santos@example.com", "senha123", "+5511999990004", "2024-11-01", 15, 12000.00, NULL, 14),
(2005, "Ricardo", "Oliveira", "ricardo.oliveira@example.com", "senha123", "+5511999990005", "2024-11-01", 10, 9500.00, NULL, 14);
```

### Funcionários

* Quantos funcionários da empresa Momento possuem conjuges?

R: 4
```sql
select count(*) from momento.dependentes where dependentes.relacionamento like "Conjuge%";
```

* Qual o funcionário contratado há mais tempo na empresa?

R: Steven Wayne
```sql
select concat(primeiro_nome, " ", sobrenome) as "Nome", data_contratacao as "Data do contrato" from momento.funcionarios order by data_contratacao asc limit 1;
```

* Qual o funcionário contratado há menos tempo na empresa?

R: Antes das adições era Zatanna Zatara, porém agora é Ricardo Oliveira.
```sql
select concat(primeiro_nome, " ", sobrenome) as "Nome", data_contratacao as "Data do contrato" from momento.funcionarios order by data_contratacao desc limit 1;
```

* Quem são os funcionários com mais tempo na empresa, considerando a `data_contratacao`?

R: Os 3 mais velhos na empresa são: Steven Wayne, Jennifer Whalen, Neena Kochhar.
```sql
select concat(primeiro_nome, " ", sobrenome) as "Nome", data_contratacao as "Data do contrato" from momento.funcionarios order by data_contratacao asc limit 3;
```

* Como a média salarial dos funcionários da "Momento" evoluiu nos últimos anos?
Dica: utilize a função `AVG()` para calcular a média salarial dos funcionários. e `GROUP BY` para agrupar os resultados por ano.

R:
```sql
select 
 extract(year from data_contratacao) as year,
 round(avg(funcionarios.salario),2) as "Média salarial no ano"
 from funcionarios 
 group by extract(year from data_contratacao)
 order by extract(year from data_contratacao);
 ```

### Médias salariais

* Qual a média salarial dos funcionários da empresa Momento, excluindo-se o CEO, CMO e CFO?

R: 8728.60
```sql
select round(avg (salario),2) as media_salarial from momento.funcionarios where cargo_id not in (select cargo_id from momento.cargos where cargo_nome in ("CEO","CFO","CMO"));
```

* Qual a média salarial do departamento de tecnologia? 

R: 9769.02
```sql
 select round(avg (salario),2) as media_salarial_tecnologia from momento.funcionarios where departamento_id not in (select departamento_id from momento.departamentos where departamento_nome like "Tecnologia");
 ```
* Qual o departamento com a maior média salarial?

R: Tecnologias Avançadas
```sql
select funcionarios.departamento_id, round(avg (salario),2) as media_salarial, departamentos.departamento_nome from momento.funcionarios 
inner join momento.departamentos
on departamentos.departamento_id = funcionarios.departamento_id
group by departamento_id order by media_salarial desc limit 1;
```

* Qual o departamento com o menor número de funcionários?

R: Os três com menos funcionários são: Administração, Relações Públicas e Recursos Humanos
```sql
select funcionarios.departamento_id, count(*) as quant_funcionarios, departamentos.departamento_nome from funcionarios 
inner join momento.departamentos on departamentos.departamento_id = funcionarios.departamento_id
group by departamento_id order by quant_funcionarios asc limit 3;
```

### Produtos

* Pensando na relação quantidade e valor unitario, qual o produto mais valioso da empresa?

R: Sabre de luz do Mace Windu
```sql
select * from momento.produtos order by produto_price desc limit 1;
```
* Qual o produto mais vendido da empresa?

R: Uniforme do Superman
```sql
SELECT 
	produtos.produto_id,
    produtos.produto_nome AS "Nome do produto",
    MAX(quantidade) AS qnt_vendido
FROM 
    vendas 
INNER JOIN 
    produtos ON produtos.produto_id = vendas.produto_id
GROUP BY 
    produtos.produto_id
order by qnt_vendido desc limit 1;
```

* Qual o produto menos vendido da empresa?

R: Batarangs oficiais
```sql
SELECT 
	produtos.produto_id,
    produtos.produto_nome AS "Nome do produto",
    MAX(quantidade) AS qnt_vendido
FROM 
    vendas 
INNER JOIN 
    produtos ON produtos.produto_id = vendas.produto_id
GROUP BY 
    produtos.produto_id
order by qnt_vendido asc limit 1;
```

### Escritórios

* Quantos escritórios a "Momento" possui em cada região? (Dica: relacione as tabelas regioes e escritorios).

R:
Americas	5,
Europa	4,
Sul da Asia ou Africa	1

```sql
    SELECT regioes.regiao_nome, COUNT(escritorios.escritorio_id) AS total_escritorios
FROM momento.escritorios
INNER JOIN momento.paises ON paises.pais_id = escritorios.pais_id
INNER JOIN momento.regioes ON regioes.regiao_id = paises.regiao_id
GROUP BY regioes.regiao_nome;
```

* Qual é o custo total de suprimentos em cada escritório? Que tal ordenar os resultados para ver qual escritório possui os suprimentos mais caros?

R: O escritório mais caro é a Umbrella Corp
```sql
-- Custo total em cada escritorio
select suprimentos.escritorio_id, escritorios.escritorio_nome, sum(custo) as soma_custos from momento.suprimentos
inner join escritorios on escritorios.escritorio_id = suprimentos.escritorio_id
group by escritorio_id;

-- Agora, ordenado
select suprimentos.escritorio_id, escritorios.escritorio_nome, sum(custo) as soma_custos from momento.suprimentos
inner join escritorios on escritorios.escritorio_id = suprimentos.escritorio_id
group by escritorio_id
order by soma_custos desc limit 1;
```