# Resumo da matéria: SQL Server

**Banco de Dados · Aulas 01 a 05 do 2º bimestre**

Este é o resumo do conteúdo das aulas, em ordem, com a explicação de cada conceito, a sintaxe e um exemplo. Não é um plano de estudo: é o texto para ler e consultar. Os exemplos usam as tabelas do TinderDB e, quando ajuda, as tabelas dos slides (Cliente, Livro, Funcionário).

> **Como ler.** Cada aula termina com uma caixa "Para fixar" com o essencial. A última seção traz uma ficha de consulta rápida com a ordem das cláusulas e os erros mais comuns.

## Aula 01. Introdução ao SQL

### O que é SQL

SQL (Structured Query Language) é a linguagem usada para criar, consultar e modificar dados em **bancos de dados relacionais**. Nasceu nos anos 1970 e é usada em MySQL, PostgreSQL, Oracle, SQL Server e outros. No SQL Server, a variante se chama **T-SQL** (Transact-SQL).

### Características

- **Declarativa.** Você diz *o que* quer (quais linhas, com qual critério), e o banco decide *como* buscar. Não se escreve laço nem passo a passo.
- **Independente de plataforma.** Os comandos básicos funcionam em qualquer banco relacional. Mas cada banco tem extensões próprias: `TOP` e `IIF` são do SQL Server, `LIMIT` é do MySQL e do PostgreSQL.
- **Suporte a transações.** Várias operações podem formar uma unidade: ou todas acontecem, ou nenhuma. Isso protege a integridade dos dados.
- **Ampla aplicabilidade.** Serve de uma consulta simples até operações complexas, em sistemas de conteúdo e financeiros.

### Os grupos de comandos

| Grupo | Nome | Para que serve | Comandos |
|---|---|---|---|
| DDL | Data Definition Language | Criar, modificar e excluir **objetos** (tabelas, índices, visões) | `CREATE`, `ALTER`, `DROP` |
| DML | Data Manipulation Language | Inserir, atualizar e excluir **registros** | `INSERT`, `UPDATE`, `DELETE` |
| DQL | Data Query Language | Consultar dados com critérios | `SELECT` |
| TCL | Transaction Control Language | Confirmar ou desfazer o que o DML fez; marcar pontos de salvamento | `COMMIT`, `ROLLBACK`, `SAVE TRANSACTION` |
| DCL | Data Control Language | Controlar o acesso aos dados | `GRANT`, `REVOKE` |

Os slides escrevem "TLC" no título do TCL, mas a sigla comum é TCL.

### Padrão de nomes usado nas aulas

Prefixos que indicam o tipo da coluna: `cd_` código, `nm_` nome, `ds_` descrição, `dt_` data, `ic_` indicador (sim ou não), `qt_` quantidade, `sg_` sigla, `vl_` valor em dinheiro. Exemplos: `cd_matricula`, `nm_funcionario`, `dt_admissao`.

> **Para fixar.** SQL é declarativa. DDL mexe na estrutura, DML nos registros, DQL consulta, TCL controla transações e DCL controla permissões. `ALTER TABLE` é DDL, mesmo mudando a tabela inteira.

## Aula 02. Constraints, INSERT e SELECT

### O que são constraints

Uma constraint (restrição) é uma **regra** aplicada a uma ou mais colunas para garantir a integridade dos dados. Ela impõe regras de negócio e impede operações que violem a consistência do banco. Quando uma operação viola a regra, o SQL Server cancela o comando e mostra um erro.

### As seis constraints

**PRIMARY KEY.** Garante valores **únicos e não nulos**, identificando cada linha. Só existe uma por tabela, mas pode ser composta por várias colunas.

```sql
CREATE TABLE Tabela1 (
    cd_tabela1 INT PRIMARY KEY,
    nm_tabela1 VARCHAR(50)
)
```

**FOREIGN KEY.** Mantém a **integridade referencial**: o valor da coluna precisa existir na coluna apontada da outra tabela. Também impede apagar o registro "pai" enquanto houver "filhos" apontando para ele.

```sql
CREATE TABLE Tabela3 (
    cd_tabela3 INT PRIMARY KEY,
    ds_tabela3 VARCHAR(200),
    cd_tabela2 INT FOREIGN KEY REFERENCES Tabela2
)
```

**UNIQUE.** Impede valores repetidos na coluna (ou na combinação de colunas). Diferente da PK, pode haver várias por tabela e, no SQL Server, aceita **um** NULL.

```sql
ds_email VARCHAR(100) UNIQUE
```

**CHECK.** Define uma condição que os valores precisam cumprir. É avaliada a cada modificação, e o comando é barrado se a condição for falsa. Atenção: se o valor for NULL, a condição é "desconhecida" e o CHECK **deixa passar**. Combine com `NOT NULL` quando o campo for obrigatório.

```sql
qt_idade INT CHECK (qt_idade >= 18)
```

**NOT NULL.** Obriga a coluna a ter valor.

```sql
ds_email VARCHAR(100) NOT NULL
```

**DEFAULT.** Define o valor usado quando o `INSERT` não informa a coluna.

```sql
pc_comissao INT DEFAULT 30
```

| Constraint | Aceita NULL? | Aceita repetição? | Quantas por tabela |
|---|---|---|---|
| PRIMARY KEY | Não | Não | Uma |
| UNIQUE | Um único NULL | Não | Várias |
| FOREIGN KEY | Sim, se a coluna aceitar | Sim | Várias |
| CHECK | Passa com NULL | Depende da regra | Várias |

### Tipos de dados mais usados

| Tipo | Guarda | Exemplo |
|---|---|---|
| `INT`, `SMALLINT`, `TINYINT` | Inteiros de tamanhos diferentes | código, quantidade |
| `DECIMAL(10,2)` | Número exato com 2 casas | valor em reais |
| `VARCHAR(n)` | Texto de até n caracteres | nome, e-mail |
| `NVARCHAR(n)` | Texto Unicode (aceita emoji) | bio, mensagem |
| `CHAR(n)` | Texto de tamanho fixo | sigla de 1 ou 2 letras |
| `DATE` | Só a data | nascimento |
| `DATETIME` | Data e hora | envio de mensagem |
| `BIT` | 0 ou 1 | indicador sim ou não |

Escolha o tipo pelo que o dado representa: dinheiro em `DECIMAL`, telefone em `VARCHAR` (não se faz conta com ele e pode começar com zero), data em `DATE`.

### Dar nome às constraints

Nos slides a regra vem escrita direto na coluna. Também é possível dar **nome**, o que torna a mensagem de erro clara e permite removê-la depois:

```sql
CREATE TABLE Plano (
    cd_plano  INT IDENTITY(1,1) NOT NULL,
    nm_plano  VARCHAR(30) NOT NULL,
    vl_mensal DECIMAL(10,2) NOT NULL,
    CONSTRAINT PK_Plano PRIMARY KEY (cd_plano),
    CONSTRAINT UQ_Plano_nome UNIQUE (nm_plano),
    CONSTRAINT CK_Plano_valor CHECK (vl_mensal >= 0)
);
```

### INSERT

O `INSERT` adiciona registros. Há duas formas:

```sql
-- declarando as colunas (preferida)
INSERT INTO Plano (nm_plano, vl_mensal)
VALUES ('Gold', 49.90);

-- sem declarar as colunas: os valores seguem a ordem exata da tabela
INSERT INTO Plano
VALUES ('Gold', 49.90, NULL, 5, 1);
```

Declarar as colunas é mais seguro: se a tabela mudar, o comando continua certo. Textos e datas vão entre aspas simples. Várias linhas podem vir juntas, separadas por vírgula.

### SELECT básico

```sql
SELECT nm_usuario, ds_email   -- colunas escolhidas
FROM Usuario;

SELECT *                      -- todas as colunas
FROM Usuario;
```

> **Para fixar.** PK é única e não nula. FK exige que o valor exista no pai. UNIQUE aceita um NULL. CHECK passa com NULL. NOT NULL obriga valor. DEFAULT preenche quando falta. Prefira `INSERT` com a lista de colunas.

## Aula 03. WHERE e ORDER BY

### WHERE: filtrar linhas

O `WHERE` mantém só as linhas que cumprem a condição.

```sql
SELECT nm_usuario, nm_cidade
FROM Usuario
WHERE nm_cidade = 'Santos';
```

### Operadores

| Operador | Significado | Exemplo |
|---|---|---|
| `=` | Igual | `WHERE sg_genero = 'F'` |
| `<>` | Diferente | `WHERE nm_cidade <> 'Santos'` |
| `>`, `>=` | Maior, maior ou igual | `WHERE qt_idade >= 18` |
| `<`, `<=` | Menor, menor ou igual | `WHERE vl_mensal < 50` |
| `AND` | As duas condições | `WHERE ic_ativo = 1 AND qt_idade > 25` |
| `OR` | Pelo menos uma | `WHERE nm_cidade = 'Santos' OR nm_cidade = 'Cubatão'` |
| `IN (...)` | Está em uma lista | `WHERE nm_cidade IN ('Santos', 'Cubatão')` |
| `BETWEEN a AND b` | Entre dois valores, **inclusive** | `WHERE qt_idade BETWEEN 25 AND 35` |
| `LIKE` | Comparação parcial de texto | `WHERE nm_usuario LIKE 'A%'` |
| `IS NULL` | Sem valor | `WHERE ds_bio IS NULL` |

### Pontos que costumam confundir

- **LIKE.** `%` casa qualquer sequência (inclusive vazia) e `_` casa exatamente um caractere. `'A%'` começa com A, `'%A'` termina com A, `'%A%'` contém A.
- **NULL.** NULL significa "sem valor" e não é igual a nada, nem a ele mesmo. `= NULL` nunca devolve linha. Use `IS NULL` e `IS NOT NULL`.
- **AND antes de OR.** O `AND` é resolvido antes do `OR`. Em `A OR B AND C`, o `B AND C` é avaliado primeiro. Use parênteses ou `IN` para deixar a intenção clara.
- **Texto e data entre aspas simples.** Números não levam aspas.

### ORDER BY: ordenar o resultado

```sql
SELECT nm_usuario, qt_idade
FROM Usuario
WHERE nm_cidade = 'Santos'
ORDER BY qt_idade DESC, nm_usuario;
```

- `ASC` (crescente) é o padrão. `DESC` é decrescente.
- Dá para ordenar por várias colunas: a segunda desempata a primeira.
- Sem `ORDER BY`, o SQL Server **não garante** a ordem das linhas.

> **Para fixar.** `WHERE` filtra linhas. `IS NULL` para ausência de valor. `AND` vem antes de `OR`. `ORDER BY` é o último e é crescente por padrão.

## Aula 04. UPDATE, DELETE e transações

### UPDATE: alterar registros

```sql
UPDATE Usuario
SET nm_cidade = 'Guarujá', ic_ativo = 1
WHERE cd_usuario = 3;
```

Pode alterar várias colunas de uma vez, separadas por vírgula, e usar o valor atual da coluna na conta: `SET vl_mensal = vl_mensal * 1.10`.

### DELETE: remover registros

```sql
DELETE FROM Foto
WHERE cd_usuario = 7 AND qt_ordem = 2;
```

> **Atenção.** `UPDATE` e `DELETE` **sem `WHERE`** atingem a tabela inteira, e o SQL Server executa sem avisar. Antes de rodar, escreva um `SELECT` com o mesmo `WHERE` para ver quais linhas serão atingidas.

### Apagar com chave estrangeira

Se existem registros "filhos" apontando para o "pai", o `DELETE` do pai é barrado pela FK (erro 547). A ordem certa é **apagar primeiro os filhos e depois o pai**. Exemplo: apagar um match exige apagar antes as mensagens dele.

```sql
DELETE FROM Mensagem   WHERE cd_combinacao = 3;   -- filhas
DELETE FROM Combinacao WHERE cd_combinacao = 3;   -- pai
```

### Transações

Uma transação agrupa comandos em uma unidade: **todos acontecem ou nenhum acontece**. É a propriedade de atomicidade.

```sql
BEGIN TRANSACTION;
    UPDATE cliente SET saldo = saldo - 100 WHERE id = 1;
    UPDATE cliente SET saldo = saldo + 100 WHERE id = 2;
COMMIT;      -- confirma e torna permanente
```

```sql
BEGIN TRANSACTION;
    UPDATE cliente SET saldo = saldo - 100 WHERE id = 1;
    UPDATE cliente SET saldo = saldo + 100 WHERE id = 2;
ROLLBACK;    -- desfaz tudo desde o BEGIN
```

| Comando | O que faz |
|---|---|
| `BEGIN TRANSACTION` | Abre a transação |
| `COMMIT` | Confirma todas as alterações, que passam a ser permanentes |
| `ROLLBACK` | Desfaz todas as alterações e volta ao estado anterior |
| `SAVE TRANSACTION nome` | Cria um ponto de salvamento |
| `ROLLBACK TRANSACTION nome` | Volta só até o ponto de salvamento; a transação continua aberta |
| `@@TRANCOUNT` | Mostra quantas transações estão abertas |

Dentro da transação, os próprios comandos já enxergam as mudanças, mesmo antes do `COMMIT`. Depois do `ROLLBACK`, elas somem.

Duas ressalvas importantes:

- Violar uma regra (CHECK, FK, UNIQUE) cancela **só o comando que falhou**, e a transação continua aberta. Para garantir o "tudo ou nada" diante de erros, use `TRY ... CATCH` com `ROLLBACK` no `CATCH`, ou `SET XACT_ABORT ON`.
- Uma transação aberta e esquecida deixa a conexão presa. Confira com `SELECT @@TRANCOUNT;`.

> **Para fixar.** Sempre `WHERE` no `UPDATE` e no `DELETE`. Filhos antes do pai. `COMMIT` confirma, `ROLLBACK` desfaz. Erro de regra não desfaz a transação sozinho.

## Aula 05. JOIN, agregação, GROUP BY e HAVING

### JOIN: combinar tabelas

O `JOIN` junta linhas de duas ou mais tabelas por uma condição de ligação, geralmente a chave estrangeira.

```sql
SELECT u.nm_usuario, m.ds_texto
FROM Mensagem m
INNER JOIN Usuario u ON u.cd_usuario = m.cd_remetente;
```

Apelidos (`u`, `m`) encurtam o texto e são obrigatórios quando a mesma tabela aparece duas vezes.

| Tipo | O que devolve |
|---|---|
| `INNER JOIN` | Só as linhas que têm correspondência **nas duas** tabelas |
| `LEFT JOIN` | Todas as linhas da tabela da **esquerda** e as correspondentes da direita; sem par, as colunas da direita vêm NULL |
| `RIGHT JOIN` | Todas as linhas da tabela da **direita** e as correspondentes da esquerda |
| `FULL JOIN` | Todas as linhas dos **dois** lados, com NULL onde não há par |

Exemplo prático: com `Usuario LEFT JOIN Mensagem`, quem nunca enviou mensagem aparece com as colunas de `Mensagem` em NULL. Filtrar `WHERE m.cd_mensagem IS NULL` mostra só essas pessoas. Essa é a técnica para achar "quem não tem".

Em `LEFT` e `RIGHT JOIN`, uma condição sobre a tabela opcional deve ficar no `ON`. No `WHERE`, ela elimina as linhas com NULL e o resultado vira um `INNER JOIN`.

### Funções de agregação

Calculam um resultado a partir de várias linhas.

| Função | Resultado | Exemplo |
|---|---|---|
| `COUNT(*)` | Número de linhas | `SELECT COUNT(*) FROM Mensagem` |
| `COUNT(coluna)` | Número de valores **não nulos** | `COUNT(m.cd_mensagem)` |
| `SUM(coluna)` | Soma | `SUM(vl_pagamento)` |
| `AVG(coluna)` | Média | `AVG(vl_preco)` |
| `MAX(coluna)` | Maior valor | `MAX(vl_mensal)` |
| `MIN(coluna)` | Menor valor | `MIN(vl_mensal)` |

Cuidados:

- Funções de agregação **ignoram NULL** (exceto `COUNT(*)`).
- `AVG` de uma coluna inteira devolve inteiro e **trunca** a parte decimal (a média de 31 e 32 vira 31). Converta antes: `AVG(CAST(qt_idade AS DECIMAL(5,1)))`.

### GROUP BY: agrupar

Organiza as linhas em grupos e aplica a função a cada grupo.

```sql
SELECT ds_forma, COUNT(*) AS qt_pagamentos, SUM(vl_pagamento) AS vl_total
FROM Pagamento
GROUP BY ds_forma;
```

**Regra de ouro:** toda coluna do `SELECT` que não está dentro de uma função de agregação precisa estar no `GROUP BY`. Sem isso, o SQL Server dá o erro 8120. Se dois registros podem ter o mesmo nome, agrupe também pelo código.

### HAVING: filtrar grupos

Funciona como o `WHERE`, mas atua **depois** do agrupamento, e por isso aceita funções de agregação.

```sql
SELECT cd_denunciado, COUNT(*) AS qt_denuncias
FROM Denuncia
GROUP BY cd_denunciado
HAVING COUNT(*) >= 2;
```

| | `WHERE` | `HAVING` |
|---|---|---|
| Atua | Antes de agrupar | Depois de agrupar |
| Filtra | Linhas | Grupos |
| Aceita `COUNT`, `SUM`... | Não | Sim |

> **Para fixar.** `INNER` só o que casa; `LEFT` preserva a esquerda; `RIGHT` a direita; `FULL` os dois lados. `COUNT(coluna)` ignora NULL. Tudo que não é agregado vai no `GROUP BY`. Filtro de linha no `WHERE`, filtro de grupo no `HAVING`.

## Ficha de consulta rápida

### A ordem das cláusulas

Como se **escreve**:

```sql
SELECT   colunas e funções
FROM     tabela
JOIN     outra_tabela ON condição
WHERE    filtro de linhas
GROUP BY colunas de agrupamento
HAVING   filtro de grupos
ORDER BY ordenação;
```

Como o banco **executa** (por isso o alias do `SELECT` só aparece no `ORDER BY`): `FROM` e `JOIN`, depois `WHERE`, `GROUP BY`, `HAVING`, `SELECT` e, por último, `ORDER BY`.

### Comandos em uma linha

| Quero... | Uso |
|---|---|
| Criar o banco ou a tabela | `CREATE DATABASE`, `CREATE TABLE` |
| Mudar a estrutura | `ALTER TABLE t ADD coluna tipo` |
| Inserir | `INSERT INTO t (c1, c2) VALUES (v1, v2)` |
| Alterar | `UPDATE t SET c = v WHERE ...` |
| Apagar | `DELETE FROM t WHERE ...` |
| Consultar | `SELECT ... FROM ... WHERE ... ORDER BY ...` |
| Juntar tabelas | `... JOIN outra ON ...` |
| Contar, somar, tirar média | `COUNT`, `SUM`, `AVG` com `GROUP BY` |
| Confirmar ou desfazer | `COMMIT`, `ROLLBACK` |

### Erros comuns e o que significam

| Erro | Causa mais provável |
|---|---|
| 547 | Violou um `CHECK` ou uma `FOREIGN KEY` |
| 2627 | Valor repetido em `PRIMARY KEY` ou `UNIQUE` |
| 515 | `NULL` em coluna `NOT NULL` |
| 8120 | Coluna do `SELECT` fora do `GROUP BY` |
| 208 | Nome de tabela inexistente, ou conectado ao banco errado |
| 207 | Nome de coluna inexistente, ou `ALTER TABLE` sem `GO` antes do uso |
| 102 | Erro de sintaxe: vírgula, aspas ou parêntese faltando |
