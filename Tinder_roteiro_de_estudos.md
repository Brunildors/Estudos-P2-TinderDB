# Roteiro de estudos: defesa do Desafio P2

**Banco TinderDB · SQL Server · defesa na segunda, 16/11/2026 · trio**

Este roteiro é para o trio que vai defender o banco. Ele cobre a noite de sábado (14/11), o domingo (15/11) e a manhã de segunda. Use junto com o **Resumo** (o que estudar e como responder) e com o **Questionário** (30 perguntas com correção na hora).

> **Como usar.** Marque as caixas conforme avança. Cada bloco diz o que ler, onde ver no script, o que praticar e quando você pode dar o bloco por pronto. Os exercícios (A1, B3, G2 e assim por diante) estão no Resumo, na seção "Treino do desafio ao vivo". Os números de linha do script valem para o arquivo atual; se editarem o script, procurem pelos títulos das seções.

## Regras do jogo

- **Todo mundo estuda tudo.** O professor escolhe quem responde, e o regulamento exige que todos os integrantes expliquem o banco (§6 e §7, item 9).
- A divisão de partes, no Resumo, serve só para ensaiar a ordem da apresentação.
- Se o tempo apertar, pule para a **versão mínima** no fim deste roteiro.
- **Antes de tudo, confirmem com o professor que ninguém do trio fará a P2.** Quem faz o desafio e a P2 perde as duas notas (§8).

## Visão geral

| Quando | Bloco | Duração | Quem | Meta |
|---|---|---|---|---|
| Sáb 18h00 | 0. Preparar o ambiente | 45 min | Todos | Script rodando de ponta a ponta |
| Sáb 19h00 | 1. Regulamento, divisão e mapa do banco | 30 min | Todos | Cada um sabe a sua parte |
| Sáb 19h30 | 2. Aulas 01 e 02: tipos de SQL e constraints | 90 min | Todos | Explicar cada constraint com exemplo |
| Sáb 21h00 | 3. Aulas 02 e 03: INSERT, SELECT, WHERE, ORDER BY | 60 min | Todos | Escrever consultas de memória |
| Dom 09h00 | 4. Aula 04: UPDATE e DELETE | 60 min | Todos | Nunca esquecer o WHERE |
| Dom 10h00 | 5. Aula 05: JOIN, agregação, GROUP BY, HAVING | 105 min | Todos | Explicar as 13 consultas |
| Dom 11h45 | 6. Transações e integridade | 75 min | Todos | Demonstrar COMMIT, ROLLBACK e um erro |
| Dom 14h00 | 7. Comandos fora dos slides, decisões e pontos fracos | 60 min | Todos | Responder "por quê?" |
| Dom 15h00 | 8. Ensaio do trio | 90 min | Todos | Apresentar em até 20 min |
| Dom 16h30 | 9. Refazer o que falhou | 60 min | Todos | 27 de 30 no questionário |
| Seg, antes da aula | 10. Checklist do dia | 45 min | Todos | Tudo pronto e testado |

## Os blocos

### Bloco 0. Preparar o ambiente
**Sábado, 18h00 · 45 min · todos, cada um no seu computador**

- [ ] Instalar o SQL Server 2022 Express e conectar no SSMS (Apêndice A, no fim deste roteiro).
- [ ] Abrir o `Tinder_exemplo.sql` e apertar **F5**. Esperar 9 mensagens vermelhas no fim: são os testes de integridade da seção 7.
- [ ] Conferir as duas últimas linhas de resultado: **12** usuários e **2** swipes da Ana.
- [ ] Apertar F5 de novo. O script apaga e recria o banco, então volta ao estado inicial.
- [ ] Anotar qualquer erro que não seja da seção 7 e resolver pelo Apêndice A.

**Pronto quando:** o script roda de ponta a ponta no seu computador e você sabe voltar ao estado inicial com F5.

### Bloco 1. Regulamento, divisão e mapa do banco
**Sábado, 19h00 · 30 min · todos**

- [ ] Ler o regulamento (4 páginas). Marcar §3 (escopo), §4 (requisitos), §5 (entregáveis), §6 (defesa) e §8 (P2).
- [ ] Ler no Resumo as seções 1 a 4: o desafio, o que o professor avalia, a defesa dividida no trio e o mapa do banco.
- [ ] Combinar quem é o Integrante 1, 2 e 3.
- [ ] Fazer o **questionário completo sem estudar**, como diagnóstico. Anotar a nota: ela é a linha de base.

**Pronto quando:** cada um sabe qual parte apresenta e qual foi a nota inicial.

### Bloco 2. Aulas 01 e 02: tipos de SQL, constraints e criação
**Sábado, 19h30 · 90 min · todos**

- [ ] **Ler:** Aula 01, slides 9 a 15 (DDL, DML, TCL, DCL, DQL). Aula 02, slides 3 a 10 (as seis constraints).
- [ ] **Ver no script:** seção 1 (linhas 26 a 235) e seção 2 (linhas 238 a 260). Comece pelas tabelas `Usuario` e `Swipe`.
- [ ] **Resumo:** seção 5 (matéria e projeto) nas linhas da Aula 01 e da Aula 02.
- [ ] **Praticar:** exercícios E1, E2, E3 (ALTER TABLE) e G1 a G4 (erros de integridade).
- [ ] **Questionário:** temas "Tipos de SQL" e "Constraints".
- [ ] Preencher de cabeça uma tabela com as seis constraints, dando um exemplo do TinderDB para cada: PK, FK, UNIQUE, CHECK, NOT NULL e DEFAULT.

**Pronto quando:** você explica as seis constraints em voz alta com um exemplo real do script e diz em que seção está cada uma.

### Bloco 3. Aulas 02 e 03: INSERT, SELECT, WHERE e ORDER BY
**Sábado, 21h00 · 60 min · todos**

- [ ] **Ler:** Aula 02, slides 16 a 18 e 20 a 23. Aula 03, slides 3 a 10 (operadores do `WHERE` e `ORDER BY`).
- [ ] **Ver no script:** seção 3 (linhas 267 a 397) e as consultas 5.1 e 5.4 (linhas 467 a 509).
- [ ] **Praticar:** exercícios A1 a A6 e D5. Escreva primeiro, abra a resposta depois.
- [ ] **Questionário:** tema "INSERT, SELECT, WHERE e ORDER BY".
- [ ] Fechar a noite: refazer só as erradas do questionário e dormir.

**Pronto quando:** você escreve, sem olhar, uma consulta com `WHERE`, `AND`/`OR`, `IN`, `LIKE`, `IS NULL` e `ORDER BY`.

### Bloco 4. Aula 04: UPDATE e DELETE
**Domingo, 09h00 · 60 min · todos**

- [ ] **Ler:** Aula 04, slides 3 a 10, incluindo o aviso em vermelho sobre `UPDATE` e `DELETE` sem `WHERE`.
- [ ] **Ver no script:** seção 4 (linhas 400 a 460). Reparar na ordem do unmatch: mensagens primeiro, match depois.
- [ ] **Praticar:** exercícios D1 a D4. Depois refaça de cabeça as 6 questões do TP04 (livros da biblioteca): reajuste, endereço, data de devolução, exclusão de categoria, exclusão de um empréstimo e exclusão de todos os empréstimos de um usuário.
- [ ] **Questionário:** tema "UPDATE e DELETE".

**Pronto quando:** você nunca escreve `UPDATE` ou `DELETE` sem `WHERE` e explica por que filhos saem antes dos pais.

### Bloco 5. Aula 05: JOIN, agregação, GROUP BY e HAVING
**Domingo, 10h00 · 105 min · todos**

- [ ] **Ler:** Aula 05, slides 3 a 7 (os quatro tipos de JOIN), 12 (funções de agregação), 14 (`GROUP BY`) e 16 (`HAVING`).
- [ ] **Ver no script:** consultas 5.2 a 5.13 (linhas 473 a 585). Para cada uma, diga em uma frase qual pergunta real ela responde.
- [ ] **Praticar:** exercícios B1 a B8 e C1 a C4.
- [ ] **Questionário:** tema "JOIN e agregação".
- [ ] Desenhar de memória os quatro `JOIN` com dois círculos, como no slide 6.

**Pronto quando:** você explica as 13 consultas como perguntas de negócio e diferencia `INNER`, `LEFT`, `RIGHT` e `FULL JOIN` sem olhar.

### Bloco 6. Transações e integridade
**Domingo, 11h45 · 75 min · todos**

- [ ] **Ler:** Aula 04, slides 14 a 18 (`COMMIT` e `ROLLBACK`) e Aula 01, slide 6 (suporte a transações).
- [ ] **Ver no script:** seção 6 (linhas 589 a 662) e seção 7 (linhas 665 a 713).
- [ ] **Praticar:** exercícios F1 a F3. Demonstrar a 6.2 passo a passo na tela, mostrando o `SELECT` de dentro e o de fora da transação.
- [ ] **Praticar:** rodar três testes da seção 7 (7.1, 7.2 e 7.6) e dizer qual regra barrou cada um e qual é o número do erro.
- [ ] **Questionário:** tema "Transações".

**Pronto quando:** você demonstra `COMMIT`, `ROLLBACK` e um erro de integridade ao vivo, sem travar, e explica por que a 6.3 usa `TRY/CATCH`.

> **Almoço e pausa, das 13h às 14h.**

### Bloco 7. Comandos fora dos slides, decisões técnicas e pontos fracos
**Domingo, 14h00 · 60 min · todos**

- [ ] **Ler no Resumo:** seção 6 (o que o script usa e os slides não mostram), seção 8 (perguntas prováveis) e seção 9 (pontos fracos conhecidos).
- [ ] **Praticar em voz alta:** cada um explica aos outros cinco decisões técnicas e três pontos fracos, em dois minutos cada. Os outros dois perguntam "por quê?".
- [ ] **Questionário:** tema "Modelagem do TinderDB".

**Pronto quando:** você responde "por quê?" para `DECIMAL`, `NVARCHAR`, `IDENTITY`, `DATE` e `DATETIME`, ausência de cascata, `ic_ativo` e `qt_idade` calculada.

### Bloco 8. Ensaio do trio
**Domingo, 15h00 · 90 min · todos**

- [ ] **Ensaio cronometrado.** Cada integrante apresenta a sua parte no SSMS, na ordem do Resumo (seção 3). Meta: até 20 minutos no total, sem travar.
- [ ] **Desafio ao vivo simulado.** Um do trio sorteia 6 exercícios do Treino, um de cada bloco (consulta, alterar consulta, operação, ALTER TABLE, transação e erro). Os outros dois resolvem em 3 minutos cada, sem olhar as respostas.
- [ ] Anotar o que falhou. Isso vira a lista do bloco seguinte.

**Pronto quando:** o ensaio fecha em até 20 minutos e o trio resolve ao menos 4 dos 6 exercícios sorteados.

### Bloco 9. Refazer o que falhou
**Domingo, 16h30 · 60 min · todos**

- [ ] **Questionário:** usar "Refazer só as erradas" até zerar.
- [ ] Refazer os exercícios que falharam no ensaio.
- [ ] Segundo ensaio, em versão curta (10 minutos): criação, uma consulta, uma transação e um erro.

**Pronto quando:** o questionário completo dá 27 de 30 ou mais.

### Bloco 10. Checklist do dia
**Segunda, antes da aula · 45 min · todos**

- [ ] Percorrer o checklist da véspera e do dia, na seção 13 do Resumo.
- [ ] Apertar F5 uma vez no script e conferir **12** usuários e **2** swipes da Ana.
- [ ] Rodar `SELECT @@TRANCOUNT;`. O resultado deve ser 0.
- [ ] Revisar as dez perguntas mais prováveis (Resumo, seção 8).
- [ ] Questionário completo, meta de 27 de 30.

**Pronto quando:** o script roda sem surpresa, as duas últimas consultas dão 12 e 2, e o trio sabe quem começa a falar.

## Versão mínima (cerca de 4 horas)

Se o tempo estiver curto, faça só isto, nesta ordem:

1. [ ] Bloco 0: script rodando (45 min).
2. [ ] Resumo, seções 1 a 3: o desafio, o que o professor avalia e a defesa dividida (30 min).
3. [ ] Bloco 6: transações e integridade (60 min). São itens obrigatórios do regulamento (§4.4 e §4.5).
4. [ ] Exercícios A1 a A6 e B1 a B5 (60 min).
5. [ ] Um ensaio do trio com 3 exercícios sorteados (45 min).

## Apêndice A. Instalação e problemas comuns

### A.1 Instalar o SQL Server 2022 Express

O SSMS já abre bancos, mas o servidor é outro programa. A documentação da Microsoft lista o Windows 10 como suportado pelo Express 2022 (exige o Windows 10 versão 1607 ou mais nova). Escolhemos a versão 2022 por ser a 16.0 que aparece nos slides do professor.

**Sem janelas**, no PowerShell **como administrador**:

```powershell
winget install --id Microsoft.SQLServer.2022.Express --exact --accept-source-agreements --accept-package-agreements
```

A instalação leva de 5 a 10 minutos sem mostrar tela. **Com o assistente**, acrescente `--interactive` ao comando e, quando o instalador abrir, escolha **Básica**.

### A.2 Conectar no SSMS

- **Nome do servidor:** `localhost\SQLEXPRESS`
- **Autenticação:** Autenticação do Windows
- **Criptografia:** Obrigatória, com **Confiar no certificado do servidor** marcada

### A.3 Rodar o script

Abra o `Tinder_exemplo.sql`, confira que a conexão aparece na barra do editor e aperte **F5**. Para rodar só um trecho, selecione-o antes de apertar F5. Os resultados aparecem na aba **Resultados**, e os `PRINT` na aba **Mensagens**.

### A.4 Problemas comuns

| O que aparece | Causa provável | O que fazer |
|---|---|---|
| Não conecta, "servidor não encontrado" | Serviço parado ou nome errado | No PowerShell como administrador: `Start-Service 'MSSQL$SQLEXPRESS'`. Ou abra o SQL Server Configuration Manager e inicie o serviço. Confira o nome `localhost\SQLEXPRESS` |
| Erro de certificado ("a cadeia de certificados ... não é confiável") ao conectar | O servidor local usa um certificado próprio | Marcar **Confiar no certificado do servidor** na janela de conexão |
| `Invalid object name 'Usuario'` (erro 208) | A janela está conectada ao `master` | Escolher `TinderDB` na lista de bancos da barra ou rodar `USE TinderDB;` |
| `Invalid column name` (erro 207) logo depois de um `ALTER TABLE` | Faltou o `GO` entre o `ALTER` e o uso da coluna | O SQL Server só enxerga a coluna nova no lote seguinte: coloque `GO` |
| `Must declare the scalar variable '@nova'` (erro 137) | A variável vive só até o `GO` | Selecione o trecho inteiro, do `DECLARE` até o fim do lote |
| `TinderDB` não aparece no Pesquisador de Objetos | A lista não foi atualizada | Botão direito em **Bancos de Dados**, **Atualizar** |
| `Cannot drop database 'TinderDB' because it is currently in use` (erro 3702) | Outra aba está usando o banco | Fechar as outras abas e rodar de novo |
| A aba "trava" depois de um teste de transação | Transação ficou aberta | `SELECT @@TRANCOUNT;`. Se for maior que 0, rode `ROLLBACK;` |
| Mensagens vermelhas no fim do script | São os 9 testes da seção 7 | É o esperado |
| Números diferentes dos do `.txt` | Algumas datas do script são fixas e outras dependem do dia da execução | Ver Resumo, seção 10 |
| Resultado aparece como texto | O SSMS está em "Resultados em texto" | `Ctrl+D` volta para grade; `Ctrl+T` vai para texto |
| O painel de resultados sumiu | Foi recolhido | `Ctrl+R` mostra e esconde |

As mensagens do servidor saem em inglês ou em português, conforme o idioma da instalação. O número do erro é o mesmo nos dois casos.
