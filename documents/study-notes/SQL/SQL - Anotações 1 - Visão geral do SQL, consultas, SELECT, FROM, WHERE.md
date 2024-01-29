---
tags:
  - computer-science/databases
  - "#computer-science/sql"
doctype: study-note
desc: "Anotações dos estudos em SQL (Structured Query Language): Visão geral do SQL, consultas, SELECT, FROM e WHERE"
name: SQL - Anotações 1 - SELECT, FROM, WHERE
id: 202401281716
date: 2024-01-28
---
# `= this.name`
`= this.desc`

## Visão geral do SQL
Nos anos 1970, a IBM começou a trabalhar em um modelo de Sistema de Gerenciamento de Bancos de Dados (SGBD) capaz de implementar o modelo relacional e a algebra relacional sobre bancos de dados. O [Sistema R]() foi desenvolvido com este fim.

Em 1986, é inaugurado o SQL1, proposto pelo ANSI. Outros marcos temporais de destaque são
- 1992 - SQL2
- 1999 - SQL3 (ou SQL99)
- 2016 - última versão

Podemos dividir o SQL em duas partes:
- SQL enquanto DDL (Data Definition Language)
	- Criação, atualização, remoção de tabelas e índices
	- Visões, mecanismos de segurança
	- Restrições de integridade
- SQL enquanto DML (Data Manipulation Language)
	- Inserção, atualização e remoção de tuplas (linhas de dados)
	- Consultas aos dados
	- Controle de transações
	- DML embutida dentro de uma linguagem de programação $\times$ DML interativa


## $\S 2$ SQL enquanto uma Data Manipulation Language (DML)
### $\S 2.1$ Consulta simples (SQL1)
A sintaxe básica de uma **consulta** em SQL1 é
```SQL
SELECT <atributos>
FROM <tabelas>
[WHERE <condições>]
[ORDER BY <atributos e critério de seleção]
```
Devemos notar que uma tabela é construída a partir de uma série de *atributos*; desta forma, cada **linha** será uma tupla **ordenada**, na qual cada valor está relacionado a um dos atributos (campos, colunas da tabela). Assim, queremos selecionar determinado *atributo* de determinada *tabela.*

O comando `SELECT` em SQL é correspondente à **projeção** da Algebra Relacional. De maneira análoga, quando estamos selecionando mais de uma tabela, devemos levar em consideração que estamos selecionado de um **produto cartesiano** sobre estas tabelas.

Podemos definir *condições* para aplicar sobre as linhas da tabela; para tanto, usamos o (opcional) `WHERE` e aplicamos as condições. Isto é equivalente à **seleção** da Algebra Relacional. Por fim, podemos *ordenar* os resultados tanto *em relação aos atributos selecionados* quanto a um *critério de ordenação.* A ordenação também é parte opcional da sintaxe de uma consulta simples.

Em Algebra Relacional, temos

$$\pi ~ a_{1}, a_{2}, ~...~, a_{n} (\sigma ~ \text{cond}(R_{1} ~|X|~ \text{cond} ~ R_{2} ~|X|~ ... ~\text{cond} ~ R_{m}))$$
onde $a_{i}, i=0..n$ são os atributos que queremos selecionar (projetar, em Algebra Relacional), $R_{1} |X| ~ ... ~ |X| ~ R_{n}$ é o produto cartesiano das tabelas (correspondente ao `WHERE` do SQL).


### $\S 2.2$ Cláusula `SELECT`
Corresponde à **projeção** da Álgebra Relacional e é onde definimos quais atributos ou campos desejamos selecionar de uma tabela. Assim, selecionamos as colunas para a *tabela-resultado* que obteremos com a consulta.

- Podemos *renomear* os atributos ou colunas para exibição;
- O operador $*$ permite que *todos* os atributos sejam selecionados;
- É possível realizar operações algébricas envolvendo os atributos (somas duas colunas, por exemplo);
- O operador `DISTINCT` é responsável pela **eliminação de tuplas duplicadas**, quando o desenvolvedor julgar necessário
- As **funções de agregação** são cálculos que realizamos sobre conjuntos de tuplas.

#### $\S 2.2.1$ Cláusula `AS`
- A cláusula `AS` é uma cláusula **opcional** utilizada quando queremos *estabelecer um **alias*** para determinado **atributo** ou para uma **tabela** inteira. É possível também *deixar **implícita** a cláusula* `AS`.

`SELECT nomee AS nome FROM EMPREGADOS` é utilizado para retornar a projeção de todos os nomes de uma tabela chamada `EMPREGADOS`. Entretanto, nossa tabela-resultado irá apresentar o campo `nomee` como `nome`. O mesmo pode ser feito de formar **implícita**, apenas **colocando o alias imediatamente após o atributo**: `SELECT nomee nome FROM EMPREGADOS`.

- A cláusula `AS` também pode ser utilizada para *nomear* uma nova relação. 
- 
Se quisermos estabelecer uma relação **comissão** para aplicar sobre nossos empregados, de forma que cada empregado beneficiado por aquela receba dez por cento do seu salário, fazemos: `SELECT nomee nome, salario*0.1 AS comissao FROM EMPREGADOS`

#### $\S 2.2.2$ Cláusula `DISTINCT`
Utilizado em projeções (`SELECT`) quando queremos **remover valores duplicados da tabela-resultado**.

Projetar os salários dos funcionários de uma tabela EMPREGADOS, de modo que valores duplicados sejam excluídos da tabela-resultado: `SELECT DISTINCT salario FROM EMPREGADOS`

### $\S 2.3$ Cláusula `FROM`
- Define quais serão as **tabelas necessárias e utilizadas na consulta**
	- Atributos a serem projetados na tabela-resultado;
	- Atributos usados em condições;
	- Tabelas que formam um *caminho*

Todos os atributos que desejamos projetar na tabela-resultado precisam vir de alguma ou de algumas tabelas. De forma análoga, os atributos para definir nossas condições precisam estar nas tabelas. Também, quando queremos fazer junções, é necessário que utilizemos as tabelas corretas para chegar a um caminho até as junções que queremos.

- Podem ser utilizados *aliases* (apelidos, outras formas de nomear uma mesma entidade) para as tabelas

Desde o SQL99, sempre utilizamos *tabelas de junção* na cláusula `FROM`.

- A sintaxe `FROM <tabelas de junção` permite todas as junções já encontradas em Álgebra Relacional
	- inner join: junção interna;
	- outer join: junção externa;
	- natural join: junção interna e externa;
	- cross join: produto cartesiano


### $\S 2.4$ Exemplos primários
Seja `EMPREGADOS` uma tabela composta pelos atributos `(code, nomee, salario, ingresso)`. Então, definimos as seguintes consultas:

```SQL
/* Projetar os dados de todos os empregados */
SELECT *
FROM EMPREGADOS

/* Projetar o nome e o salário de todos os empregados */
SELECT nomee, salario
FROM EMPREGADOS

/* Utilização de um alias para renomear um campo
 Novamente, projetando o nome e o salário */
SELECT nomee AS nome, salario
FROM EMPREGADOS

/* Utilização de um alias sem a cláusula AS */
SELECT nomee nome, salario
FROM EMPREGADOS

/* Utilização da cláusula AS para estabelecer uma nova relação (comissão) */
SELECT nomee nome, salario*0.1 AS comissao 
FROM EMPREGADOS

/* Projetar os salários dos funcionários, de modo que valores duplicados sejam excluídos da tabela-resultado */
SELECT DISTINCT salario
FROM EMPREGADOS
```

### $\S 3$ Cláusula `WHERE`: condições
Através da cláusula opcional `WHERE`, podemos definir **condições** a serem testas sobre os valores de retorno. Desta forma, projetamos uma tabela-resultado a partir de uma consulta `SELECT <att> FROM <table>`, e aplicamos sobre ela uma ou mais condições: `SELECT <att> FROM <table> WHERE <cond>.
- Possibilidade de utilização de parênteses para especificar o escopo da condição
- Operadores lógicos: `AND`, `OR`, `NOT`
- Operadores comparativos: $>$, $<$, $<>$, $=$
- Constantes, atributos, operadores aritméticos também podem fazer parte de condições
- Cláusula `BETWEEN`, que é **equivalente a** `>= AND <=`
- Cláusula `LIKE`
	- `%`: operador que substitui cadeia de caracteres
	- `_`: operador de substituição de caracteres
- Cláusulas `IS NULL` e `IS NOT NULL`
- Cláusula `IN <conjunto de valores>`
- **Subconsultas** podem fazer parte de uma condição


#### $\S 3.1$ Cláusula `BETWEEN`
Verdadeiro se o valor está entre um valor inicial e um valor final.
#### $\S 3.2$ Cláusula `LIKE`
Busca em substrings.
#### $\S 3.3$ Cláusula `IS NULL` (`IS NOT NULL`)
Checa se um valor é nulo (`NULL`)
#### $\S 3.4$ Cláusula `IN`
Checa se um valor pertence a uma lista de valores.

#### $\S 3.5$ Exemplos primários
```SQL
 /* Seja EMPREGADOS uma tabela cujas colunas são compostas pela tupla (codee, nomee, salario, ingresso). Então, seguem-se alguma consultas para demonstrar possibilidades da clásula WHERE associada, ou não, a outras cláusulas específicas, vistas acima. */

/* Todos dados dos empregados com ingresso posterior a 2015 */
SELECT *
FROM EMPREGADOS
WHERE ingresso > 2015

/* O nome e salário dos empregados que recebem entre 2.000 e 5.000 reais */
SELECT nomee, salario
FROM EMPREGADOS
WHERE salario >= 2000 and salario <= 5000

/* O nome e o salário dos empregados que recebem entre 2.000 e 5.000 reais, utilizando a cláusula BETWEEN */
SELECT nomee, salario
FROM EMPREGADOS
WHERE salario between 2000 and 5000


/* Seja PROJETOS uma tabela constituída pela tupla (codp, nomep, orcamento, gerente), então temos as seguintes consultas */

/* O nome dos projetos sem gerente */
SELECT nomep
FROM PROJETOS
WHERE gerente IS NULL


/* O nome dos projetos cujo nome inicia com “proj” */
SELECT nomep
FROM PROJETOS
WHERE nomep LIKE ‘proj%’
```

### $\S 4$ Tabelas de Junção
- **Junção *theta*** (interna, inner join, $\theta$-join)
	- `FROM tabela1 JOIN tabela2 ON <condição de junção>`
- **Junção natural** (natural join)
	- `FROM tabela1 JOIN tabela2 USING <atributo de mesmo nome>
	- `FROM tabela1 NATURAL JOIN tabela2`
- **Observação**: a coluna com **nome duplicada** é **removida**.

#### $\S 4.1$ Cláusula `ON`

#### $\S 4.2$ Cláusula `USING`

#### $\S 4.3$ Exemplos primários em tabelas de junção
```SQL
/* Sejam as tabelas EMPREGADOS e PROJETOS conforme definidas acima, e defina-se a tabela ALOCACOES através da tupla (codp, code, funcao) */

/* Projetar o nome dos empregados e as funções que eles desempenham em projetos 
 Veja como usamos uma junção theta (interna, inner join) para criar uma tabela intermediária que contém tanto as entradas de EMPREGADOS quanto as entradas de ALOCACOES. A partir desta tabela, queremos retornar aquelas tuplas nas quais o código do empregado é o mesmo código da alocacão.

!! Notar que não estamos colocando nenhuma restrição sobre os resultados, por isto não utilizamos uma cláusula WHERE nestas três consultas

*/
SELECT DISTINCT nomee, funcao
FROM EMPREGADOS JOIN ALOCACOES
	ON (EMPREGADOS.code = ALOCACOES.code)

/* O mesmo pode ser feito através de junção natural mas especificamos o atributo através de uma cláusula USING <atr>

!! Interessante para evitar efeitos colaterais
*/
SELECT DISTINCT nomee, funcao
FROM EMPREGADOS JOIN ALOCACOES
	USING (code)

/* Outra maneira de utilizar a junção natural */
SELECT DISTINCT nomee, funcao
FROM EMPREGADOS NATURAL JOIN ALOCACOES
```

#### $\S 4.4$ Junções em SQL
- Junção interna (inner join)
	- Utiliza-se somente a palavra `JOIN` em SQL, ou
	- `ON <condição>` 
	- `USING <atributo>`
	- `NATURAL JOIN`
- Junção externa (outer join)
	- Não há correspondência entre colunas de mesmo nome;
	- Concatena com tuplas de valores NULL
	- Existem os três tipos de junções externas: **left outer join**, **right outer join** e **full outer join**, conforme a Álgebra Relacional as define.
		`table1 LEFT [OUTER] JOIN table2`: tuplas de `table1` vão para o resultado, combinada com tuplas de `table2` ou concatenadas com NULL
		`table1 RIGHT [OUTER] JOIN table2`: tuplas de `table2` vão para o resultado, combinada com tuplas de `table1` ou concatenadas com NULL
		`table1 FULL [OUTER] JOIN table2`: tuplas de `table1` e `table2` vão para o resultado, ambas podendo ser concatenadas com NULL.
- Produto cartesiano (cross join)
	- Retorna todos os valores do produto cartesiano de $n$ tabelas.

```SQL
/* Novamente lançando mão das tabelas EMPREGADOS, PROJETOS e ALOCACOES definidas acima, definem-se as seguintes pesquisas */

/* O nome dos empregados, e de suas funções (incluir no resultado todos os empregados) */
SELECT distinct nomee, funcao
FROM empregados left join alocacoes
on (empregados.code = alocações.code)

SELECT distinct nomee, funcao
FROM empregados left join alocacoes using (code)

SELECT distinct nomee, funcao
FROM empregados natural left join alocacoes
```

### $\S 5$ Cláusula `ORDER BY`: ordenamento da tabela-resultado
A cláusula `ORDER BY` é uma cláusula opcional utilizada para **ordenar os valores da tabela-resultado** de acordo com os *valores dos atributos* e de acordo com *os próprios atributos escolhidos*. As opções de sintaxe para a cláusula `ORDER BY` são:
- `ORDER BY <atr> [<ord>]
- `ORDER BY [, <atr> [<ord> ...]

Onde `<atr>` será um (ou mais) atributos presentes na tabela-resultado e `<ord>` pode ser um dos dois valores:
- `ASC` (default: ordem crescente, ascendente)
- `DESC` (ordem decrescente)


Os resultados de uma tabela-resultado são, na maioria das vezes, montados em uma ordem inesperada e não necessariamente acessível para uma boa visualização dos dados; portanto, torna-se interessante, senão necessário, ordená-los de forma coerente.

#### $\S 5.1$ Exemplos primários da cláusula `ORDER BY` 
```SQL
/* Fazendo uso da tabela EMPREGADOS, confrome definimos anteriormente, apresentamos as seguintes consultas focadas na ordenação da tabela-resultado */

/* O nome e salario dos empregados */
Select nomee, salario
From empregados

/* O nome e salario dos empregados, ordenada alfabeticamente */
Select nomee, salario
From empregados
Order by nomee

/*O  nome e salario dos empregados, ordenada por salario em ordem decrescente, e por ordem alfabética de nome como segundo critério */
Select nomee, salario
From empregados
Order by salario DESC, nomee
```