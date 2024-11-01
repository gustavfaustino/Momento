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

* O departamento de Inovações está sem funcionários. Inclua alguns colegas de turma nesse departamento.  

### Funcionários

* Quantos funcionários da empresa Momento possuem conjuges?

* Qual o funcionário contratado há mais tempo na empresa?

* Qual o funcionário contratado há menos tempo na empresa?

* Quem são os funcionários com mais tempo na empresa, considerando a `data_contratacao`?

* Como a média salarial dos funcionários da "Momento" evoluiu nos últimos anos?
Dica: utilize a função `AVG()` para calcular a média salarial dos funcionários. e `GROUP BY` para agrupar os resultados por ano.

### Médias salariais

* Qual a média salarial dos funcionários da empresa Momento, excluindo-se o CEO, CMO e CFO?

* Qual a média salarial do departamento de tecnologia? 

* Qual o departamento com a maior média salarial?

* Qual o departamento com o menor número de funcionários?

### Produtos

* Pensando na relação quantidade e valor unitario, qual o produto mais valioso da empresa?

* Qual o produto mais vendido da empresa?

* Qual o produto menos vendido da empresa?

### Escritórios

* Quantos escritórios a "Momento" possui em cada região? (Dica: relacione as tabelas regioes e escritorios).

* Qual é o custo total de suprimentos em cada escritório? Que tal ordenar os resultados para ver qual escritório possui os suprimentos mais caros?