# 📝 Casos de teste — Cadastro de usuário (campo User ID)

**Aplicação:** [JPetStore — Mate Academy](https://jpetstore.mate.academy/actions/Catalog.action)
**Funcionalidade:** User Registration — **escopo: somente o campo User ID** (não cobre o cadastro completo)
**Requisitos:** PS-REQ-001 (User ID único) · PS-REQ-002 (User ID apenas alfanumérico, letras latinas) — fonte: tarefa do curso Mate Academy
**Versão do documento:** 3.2 (revisada — ver [`REVISOES.md`](../REVISOES.md)) · **Como executar:** [`PROTOCOLO-EXECUCAO.md`](../PROTOCOLO-EXECUCAO.md)
**Ambiente A01:** navegador integrado do app Claude Desktop 4.4212.0 (Chromium 152.0.7977.167) · Windows 10 64-bit
**Execuções:** a partir de 03/11/2026 · **Executado por:** Fabricio S Azeredo, com apoio do Claude em algumas etapas (detalhado em **Execução** em cada caso)

---

## 🎯 Critérios de prioridade
| Prioridade | Critério |
|---|---|
| 🔴 **Crítica** | Se falhar, o fluxo principal fica bloqueado (ninguém consegue se cadastrar). Faz parte do teste de fumaça. |
| 🟠 **Alta** | Regra explícita do requisito; falha permite dados inválidos ou duplicados no sistema. |
| 🟡 **Média** | Variação ou caso de borda de uma regra; impacto menor ou regra ainda não confirmada. |
| 🟢 **Baixa** | Exploratório ou cosmético. |

> Prioridade do **teste** = importância de executá-lo. Severidade de um **bug** = impacto do defeito encontrado.

## ❓ Premissas e dúvidas sobre os requisitos
| ID | Dúvida | Premissa usada até ser esclarecida |
|---|---|---|
| Q-01 | O User ID é obrigatório? (o requisito não diz) | Sim, é obrigatório (PS-TC-004). |
| Q-02 | "Letras latinas" inclui letras acentuadas (á, ç)? | Não: só `A–Z`, `a–z` e `0–9`. **Observado no PS-TC-006: a letra acentuada foi aceita** (e exibida corrompida, BUG-005). |
| Q-03 | `j2ee` e `J2EE` são o mesmo ID? | Sem premissa — cenário pendente (PS-TC-P01). |
| Q-04 | Espaços no início/fim são removidos ou rejeitados? | Rejeitados. **Observado no 005-b e no 005-c (03/11/2026): os espaços do início e do fim foram removidos (trim)**; aguardando definição da regra. |
| Q-05 | Existe tamanho mínimo/máximo para o User ID? | Não documentado. **E01: 50 caracteres geraram erro 500 (BUG-006)**; o limite exato não foi determinado. |
| Q-06 | O ID pode ter só letras ou só números? | Sim, ambos são alfanuméricos. |

## 🧪 Preparação (vale para todos os casos)
1. Abrir a [Pet Store](https://jpetstore.mate.academy/actions/Catalog.action) **sem estar logado** (se estiver, clicar em **Sign Out**).
2. Caminho até o formulário: **Sign In** → **Register Now!**
3. **Dados válidos para os outros campos** *(planejados; o valor realmente usado é registrado em cada execução)*: senha de teste exclusiva, não publicada (exemplo de formato: `Senha123`) · First name `Teste` · Last name `QA` · Email `teste.qa@example.com` · Phone `21999999999` · Address 1 `Rua Exemplo, 100` · City `Rio de Janeiro` · State `RJ` · Zip `20000000` · Country `Brasil`.
4. Para IDs que precisam ser novos, usar o padrão `QAFab` + data e hora (ex.: `QAFab0311261205`) e **anotar o valor usado**.

⚠️ Usar apenas dados fictícios — este repositório é público.

**Legenda de status:** ✅ Passou · ❌ Falhou · ⏳ A executar · ⏸️ Bloqueado/pendente

---

## 📋 Resumo
| ID | Resumo | Req. | Prioridade | Status |
|---|---|---|---|---|
| PS-TC-001 | Não aceita User ID já existente | REQ-001 | 🟠 Alta | ❌ Falhou (BUG-001) |
| PS-TC-002 | Não aceita caracteres especiais (`!` `@` `#`) | REQ-002 | 🟠 Alta | ❌ Falhou nas 3 variações (BUG-002) |
| PS-TC-003 | Aceita letras e números — cadastro e login | REQ-002 | 🔴 Crítica | ✅ Passou |
| PS-TC-004 | Não aceita User ID vazio | Q-01 | 🟠 Alta | ❌ Falhou sob Q-01 (BUG-004) |
| PS-TC-005 | Não aceita espaços (meio, início, fim, só espaços) | REQ-002 / Q-04 | 🟡 Média | ❌ 005-a falhou (BUG-002) · ⏸️ 005-b/c espaços das pontas removidos (Q-04) · ❌ 005-d recusa silenciosa (BUG-004) |
| PS-TC-006 | Não aceita letras acentuadas | REQ-002 / Q-02 | 🟡 Média | ❌ Falhou sob Q-02 (BUG-002) · BUG-005 |
| PS-TC-007 | Não aceita alfabeto não latino | REQ-002 | 🟡 Média | ❌ Falhou (BUG-002) · BUG-005 |
| PS-TC-008 | Aceita User ID só com letras | REQ-002 / Q-06 | 🟡 Média | ✅ Passou sob Q-06 |
| PS-TC-009 | Aceita User ID só com números | REQ-002 / Q-06 | 🟡 Média | ✅ Passou sob Q-06 |
| PS-TC-P01 | Unicidade com maiúsculas/minúsculas | REQ-001 / Q-03 | 🟡 Média | ⏸️ Pendente |
| PS-TC-E01 | Exploratório: User ID muito longo | Q-05 | 🟢 Baixa | 🔍 Executado → BUG-006 |

---

## PS-TC-001 — Não aceita um User ID já existente
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-001 · 🟠 Alta |
| **Pré-condição** | Conta de teste criada no PS-TC-003, com o User ID anotado. |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Inserir o User ID da conta criada no PS-TC-003.<br>3. Preencher os outros campos com dados válidos.<br>4. Clicar em **Save Account Information**.<br>5. Fazer login com a conta original do PS-TC-003. |
| **Resultado esperado** | Passo 4: o sistema exibe um erro informando que o User ID já existe, e nenhuma conta nova é criada.<br>Passo 5: a conta original continua acessível, com os dados intactos. |
| **Dado usado** | User ID `QAFab0311261210` (o mesmo do PS-TC-003) |
| **Resultado atual** | Passo 4: em vez de uma mensagem de erro, apareceu **"HTTP Status 500 – Internal Server Error"** com stack trace e SQL (`DuplicateKeyException`, `PK_ACCOUNT`). A conta duplicada não foi criada.<br>Passo 5: o login com a conta original funcionou ("Welcome teste1!"). Na página My Account, User ID, nome e e-mail foram conferidos, sem alteração observada (sem print publicado, porque a página contém dados não fictícios). |
| **Data da execução** | 03/11/2026 |
| **Execução** | Tentativa registrada aqui (`QAFab0311261210`): **testador** preencheu, salvou e fez o login de verificação; **Claude** capturou o print do erro. A segunda ocorrência, citada no BUG-001, também foi feita pelo testador. |
| **Status / Evidência** | ❌ **Falhou** → [BUG-001](../bug-reports/BUG-001.md) · [`BUG-001-erro-500.jpg`](../evidencias/BUG-001-erro-500.jpg) |

## PS-TC-002 — Não aceita caracteres especiais
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 · 🟠 Alta |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Inserir o User ID da tabela abaixo.<br>3. Preencher os demais campos com dados válidos.<br>4. Clicar em **Save Account Information**.<br>5. Repetir para cada dado. |
| **Resultado esperado** | O sistema exibe um erro informando que o User ID deve conter apenas letras e números, e a conta não é criada. |
| **Execução** | 002-a: todos os campos, inclusive o User ID, preenchidos por script do Claude; senha pelo testador; clique em Save pelo Claude; login pelo testador. 002-b: campos fictícios digitados pelo Claude; User ID, senha, clique em Save e login pelo testador. 002-c T01: formulário preenchido e salvo pelo testador; prints pelo Claude. 002-c T02: **100% pelo testador**; prints capturados pelo Claude. |

| Variação | Dado | Resultado atual | Status | Evidência |
|---|---|---|---|---|
| 002-a | `User!123` | Nenhum erro: a conta foi criada e o login funcionou; My Account mostra `User!123` (03/11/2026) | ❌ Falhou → [BUG-002](../bug-reports/BUG-002.md) | [`BUG-002-conta-criada.jpg`](../evidencias/BUG-002-conta-criada.jpg) |
| 002-b | `User@123` | Nenhum erro; a conta foi criada (o login manual funcionou e a repetição gerou o erro 500 de duplicidade). Não houve login automático. | ❌ Falhou → [BUG-002](../bug-reports/BUG-002.md) | [`PS-TC-002b-apos-salvar.jpg`](../evidencias/PS-TC-002b-apos-salvar.jpg) |
| 002-c | Planejado `User#123` · **T01** usado `Qa#1103125501` | ⚠️ **Inconclusiva**: erro 500 de duplicidade ao salvar (possível envio duplicado). Nada concluído; repetida como T02. ⚠️ O print 02 é idêntico, byte a byte, ao `BUG-001-erro-500.jpg`: é uma captura nova (12:57), mas a página de erro 500 é visualmente idêntica. | — | [01](../evidencias/PS-TC-002c_20261103_125501_T01_01-antes-salvar.jpg) · [02](../evidencias/PS-TC-002c_20261103_125501_T01_02-depois-salvar.jpg) |
| 002-c | **T02** usado `Qa#1103125930` (03/11/2026 12:59, A01) | Após salvar: página principal com Sign In, sem mensagem de erro. 1º login feito sem o print 03; refeitos o logout e o login para capturar as etapas 03 e 04. Login autenticado ("Welcome TESTE!"). My Account mostra **User ID `Qa#1103125930`**. Criação com `#` confirmada pela interface. ⚠️ O print 02 é idêntico, byte a byte, ao `PS-TC-002b-apos-salvar.jpg`: é uma captura nova (12:59) da página principal deslogada, que é visualmente idêntica. A prova da criação está nos prints 03 a 05. | ❌ Falhou → [BUG-002](../bug-reports/BUG-002.md) | [01](../evidencias/PS-TC-002c_20261103_125930_T02_01-antes-salvar.jpg) · [02](../evidencias/PS-TC-002c_20261103_125930_T02_02-depois-salvar.jpg) · [03](../evidencias/PS-TC-002c_20261103_125930_T02_03-antes-login.jpg) · [04](../evidencias/PS-TC-002c_20261103_125930_T02_04-depois-login.jpg) · [05](../evidencias/PS-TC-002c_20261103_125930_T02_05-my-account.jpg) |

## PS-TC-003 — Aceita letras e números (cadastro e login)
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 · 🔴 Crítica (teste de fumaça) |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Inserir um User ID novo com letras e números (padrão `QAFab` + data e hora).<br>3. Preencher os demais campos com dados válidos.<br>4. Clicar em **Save Account Information**.<br>5. Clicar em **Sign Out**.<br>6. Fazer login com o User ID e a senha de teste usada no cadastro. |
| **Resultado esperado** | Passo 4: cadastro bem-sucedido e redirecionamento para a página principal (resultado definido na tarefa do curso).<br>Passo 6: o login funciona e o nome da conta aparece na página. |
| **Dado planejado** | `QAFab` + data e hora · dados fictícios do documento |
| **Dado usado** | User ID `QAFab0311261210` · First/Last name `teste1` · senha de teste (não publicada) · ⚠️ os demais campos **não** usaram os dados fictícios planejados |
| **Resultado atual** | Passo 4: cadastro concluído e redirecionamento para a página principal com a mensagem "Welcome teste1!". A página **My Account** mostra o User ID e os dados salvos.<br>Passo 6: login com o novo User ID funcionou ("Welcome teste1!"). |
| **Data da execução** | 03/11/2026 |
| **Execução** | 100% pelo testador. Claude: capturou o print. |
| **Status / Evidência** | ✅ **Passou** · [`PS-TC-003_welcome-teste1.jpg`](../evidencias/PS-TC-003_welcome-teste1.jpg) |
| **Limitação da evidência** | A tela "Welcome teste1!" é idêntica após o cadastro (passo 4) e após o login (passo 6), por isso **um único print** é publicado e ele **não distingue** as duas etapas. A confirmação do passo 6 e do User ID na My Account foi feita pelo testador, sem print publicado. Próximas execuções seguem o protocolo, com prints distintos por etapa. |

## PS-TC-004 — Não aceita User ID vazio *(premissa Q-01)*
| Campo | Descrição |
|---|---|
| **Requisito** | Premissa Q-01 · 🟠 Alta |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Deixar o User ID em branco.<br>3. Preencher os demais campos com dados válidos.<br>4. Clicar em **Save Account Information**. |
| **Resultado esperado** | O sistema exibe um erro informando que o User ID é obrigatório, e a conta não é criada. |
| **Dado usado** | User ID vazio (conferido: nenhum caractere) · dados fictícios do documento · senha de teste (não publicada) |
| **Resultado atual** | O formulário voltou com os dados preenchidos e o cadastro não foi concluído na interface, mas **nenhuma mensagem de erro apareceu**. No código da página, o campo User ID recebeu `class="error"`, sem destaque visível. |
| **Data da execução** | 03/11/2026 14:38 · A01 |
| **Execução** | **Testador:** digitou a senha, First name, Last name e um e-mail. **Claude** (a pedido do testador): trocou o e-mail para o fictício, digitou os demais campos, conferiu no código da página que o User ID estava vazio, capturou os prints e clicou em Save. |
| **Status / Evidência** | ❌ **Falhou sob a premissa Q-01** → [BUG-004](../bug-reports/BUG-004.md) · [01](../evidencias/PS-TC-004_20261103_143800_T01_01-antes-salvar.jpg) · [02](../evidencias/PS-TC-004_20261103_143800_T01_02-depois-salvar.jpg) |

## PS-TC-005 — Não aceita espaços *(premissa Q-04)*
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 / Q-04 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-002, com os dados abaixo. |
| **Resultado esperado** | O sistema exibe um erro informando que o User ID deve conter apenas letras e números, e a conta não é criada. |

| Variação | Dado | Resultado atual | Status | Evidência |
|---|---|---|---|---|
| 005-a | Planejado `Test User` · **usado `Qa 11031515`** (1 espaço no meio) · 03/11/2026 15:15, A01 | Após salvar: página principal com Sign In, sem mensagem de erro. Login com `Qa 11031515` autenticado ("Welcome Teste!"). My Account mostra **User ID `Qa 11031515`**, com o espaço. Print 03 não capturado (o login foi feito antes da captura). **Execução:** campos fictícios digitados pelo Claude; User ID, senha, Save e login pelo testador; prints pelo Claude, com data e hora. | ❌ Falhou → [BUG-002](../bug-reports/BUG-002.md) | [01](../evidencias/PS-TC-005a_20261103_151559_T01_01-antes-salvar.jpg) · [02](../evidencias/PS-TC-005a_20261103_151559_T01_02-depois-salvar.jpg) · [04](../evidencias/PS-TC-005a_20261103_151559_T01_04-depois-login.jpg) · [05](../evidencias/PS-TC-005a_20261103_151559_T01_05-my-account.jpg) |
| 005-b | Planejado ` Teste123` · **usado `␣Qa11031520`** (1 espaço inicial, 11 caracteres, conferido no código da página) · 03/11/2026 15:23, A01 | Após salvar: página principal com Sign In, sem mensagem. Login com `␣Qa11031520` (11 caracteres) autenticado. My Account exibe **`Qa11031520` sem o espaço** (10 caracteres): o espaço inicial foi **removido** em algum ponto (ao salvar, ao logar ou na exibição — não determinado). O ID exibido é alfanumérico, então **não viola o PS-REQ-002**; a premissa Q-04 ("rejeitar") não se confirmou. **Execução:** campos fictícios pelo Claude; User ID, senha, Save e login pelo testador; prints com data e hora pelo Claude. | ⏸️ **Pendente de esclarecimento (Q-04)**: observado *trim* em vez de rejeição; não é bug sem a regra definida | [01](../evidencias/PS-TC-005b_20261103_152342_T01_01-antes-salvar.jpg) · [02](../evidencias/PS-TC-005b_20261103_152342_T01_02-depois-salvar.jpg) · [03](../evidencias/PS-TC-005b_20261103_152342_T01_03-antes-login.jpg) · [04](../evidencias/PS-TC-005b_20261103_152342_T01_04-depois-login.jpg) · [05](../evidencias/PS-TC-005b_20261103_152342_T01_05-my-account.jpg) |
| 005-c | Planejado `Teste123 ` · **usado `Qa11031530␣`** (1 espaço final, 11 caracteres, conferido no código da página) · 03/11/2026 15:30, A01 | Após salvar: página principal com Sign In, sem mensagem. Login com `Qa11031530␣` (11 caracteres) autenticado. My Account exibe **`Qa11031530` sem o espaço** (10 caracteres): o espaço final foi **removido** (ponto exato não determinado). Mesmo comportamento do 005-b. Não viola o PS-REQ-002; a premissa Q-04 ("rejeitar") não se confirmou. **Execução:** campos fictícios pelo Claude; User ID, senha, Save e login pelo testador; prints com data e hora pelo Claude. | ⏸️ **Pendente de esclarecimento (Q-04)**: *trim* observado em 2/2 (início e fim) | [01](../evidencias/PS-TC-005c_20261103_153050_T01_01-antes-salvar.jpg) · [02](../evidencias/PS-TC-005c_20261103_153050_T01_02-depois-salvar.jpg) · [03](../evidencias/PS-TC-005c_20261103_153050_T01_03-antes-login.jpg) · [04](../evidencias/PS-TC-005c_20261103_153050_T01_04-depois-login.jpg) · [05](../evidencias/PS-TC-005c_20261103_153050_T01_05-my-account.jpg) |
| 005-d | Planejado 3 espaços · **usado 28 espaços e nada mais** (conferido no código da página; a tecla ficou pressionada) · 03/11/2026 15:36, A01 | Após salvar: o formulário voltou com os dados e o cadastro não foi concluído na interface, **sem nenhuma mensagem de erro**. No código, o campo User ID recebeu `class="error"` e ficou selecionado (destaque cinza no print 02), com os 28 espaços ainda no campo. Mesmo comportamento do PS-TC-004. Login e My Account não realizados (não houve indicação de cadastro). **Execução:** campos fictícios pelo Claude; User ID, senha e Save pelo testador; prints com data e hora pelo Claude. | ❌ Falhou → [BUG-004](../bug-reports/BUG-004.md) | [01](../evidencias/PS-TC-005d_20261103_153631_T01_01-antes-salvar.jpg) · [02](../evidencias/PS-TC-005d_20261103_153631_T01_02-depois-salvar.jpg) |

## PS-TC-006 — Não aceita letras acentuadas *(premissa Q-02)*
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 / Q-02 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-002, com o dado `Usuário1`. |
| **Resultado esperado** | O sistema exibe um erro informando que o User ID deve conter apenas letras (A–Z) e números, e a conta não é criada. |
| **Dado usado** | `Usuário11031540` (15 caracteres, com `á`, conferido no código da página) · dados fictícios · senha de teste (não publicada) |
| **Resultado atual** | Após salvar: página principal com Sign In, **sem mensagem de erro**. Login com `Usuário11031540` autenticado ("Welcome Teste!"). My Account exibe **`UsuÃ¡rio11031540`**: a conta foi criada com a letra acentuada, mas o `á` aparece corrompido (ver [BUG-005](../bug-reports/BUG-005.md)). |
| **Data da execução** | 03/11/2026 15:41 · A01 |
| **Execução** | Campos fictícios pelo Claude; User ID, senha, Save e login pelo testador; prints com data e hora pelo Claude. |
| **Status / Evidência** | ❌ **Falhou sob a premissa Q-02** (a letra acentuada foi aceita) → [BUG-002](../bug-reports/BUG-002.md) · achado adicional → [BUG-005](../bug-reports/BUG-005.md) · [01](../evidencias/PS-TC-006_20261103_154115_T01_01-antes-salvar.jpg) · [02](../evidencias/PS-TC-006_20261103_154115_T01_02-depois-salvar.jpg) · [03](../evidencias/PS-TC-006_20261103_154115_T01_03-antes-login.jpg) · [04](../evidencias/PS-TC-006_20261103_154115_T01_04-depois-login.jpg) · [05](../evidencias/PS-TC-006_20261103_154115_T01_05-my-account.jpg) |

## PS-TC-007 — Não aceita alfabeto não latino
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-002, com o dado `用户123`. |
| **Resultado esperado** | O sistema exibe um erro informando que o User ID deve conter apenas letras latinas e números, e a conta não é criada. |
| **Dado usado** | `用户11031645` (10 caracteres; 2 chineses, códigos 29992 e 25143, conferidos no código da página; colado pelo testador) · dados fictícios · senha de teste (não publicada) |
| **Resultado atual** | Após salvar: página principal com Sign In, **sem mensagem de erro**. Login com `用户11031645` autenticado ("Welcome Teste!"). My Account exibe **`ç¨æ·11031645`** (14 caracteres): a conta foi criada com caracteres não latinos, que aparecem corrompidos (ver [BUG-005](../bug-reports/BUG-005.md)). Na 1ª tentativa de login os campos Username e Password foram preenchidos trocados; isso foi corrigido antes do clique em Login e não foi publicado print com a senha. |
| **Data da execução** | 03/11/2026 16:47 · A01 |
| **Execução** | Campos fictícios pelo Claude (preenchimento direto nos campos, porque os cliques no painel falharam nesta rodada); User ID colado, senha, Save e login pelo testador; prints com data e hora pelo Claude. |
| **Status / Evidência** | ❌ **Falhou** (o PS-REQ-002 exige letras latinas) → [BUG-002](../bug-reports/BUG-002.md) · achado adicional → [BUG-005](../bug-reports/BUG-005.md) · [01](../evidencias/PS-TC-007_20261103_164715_T01_01-antes-salvar.jpg) · [02](../evidencias/PS-TC-007_20261103_164715_T01_02-depois-salvar.jpg) · [03](../evidencias/PS-TC-007_20261103_164715_T01_03-antes-login.jpg) · [04](../evidencias/PS-TC-007_20261103_164715_T01_04-depois-login.jpg) · [05](../evidencias/PS-TC-007_20261103_164715_T01_05-my-account.jpg) |

## PS-TC-008 — Aceita User ID só com letras *(premissa Q-06)*
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 / Q-06 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-003 (passos 1–4), com um ID novo só com letras (ex.: `QAFabTeste`). |
| **Resultado esperado** | Cadastro bem-sucedido. |
| **Dado usado** | `QaFabTesteAbcd` (só letras; o planejado era `QAFabTeste`, trocado para um ID novo) · dados fictícios · senha de teste (não publicada) |
| **Resultado atual** | Após salvar: página principal com Sign In, sem mensagem de erro (sem login automático). Login com `QaFabTesteAbcd` autenticado ("Welcome Teste!"). My Account exibe **`QaFabTesteAbcd`**. |
| **Data da execução** | 03/11/2026 16:51 · A01 |
| **Execução** | **Executado pelo Claude** a pedido do testador (preenchimento dos campos, cliques em Save e Login e prints com data e hora); **senhas digitadas pelo testador**. |
| **Status / Evidência** | ✅ **Passou sob a premissa Q-06** · [01](../evidencias/PS-TC-008_20261103_165145_T01_01-antes-salvar.jpg) · [02](../evidencias/PS-TC-008_20261103_165145_T01_02-depois-salvar.jpg) · [03](../evidencias/PS-TC-008_20261103_165145_T01_03-antes-login.jpg) · [04](../evidencias/PS-TC-008_20261103_165145_T01_04-depois-login.jpg) · [05](../evidencias/PS-TC-008_20261103_165145_T01_05-my-account.jpg) |

## PS-TC-009 — Aceita User ID só com números *(premissa Q-06)*
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 / Q-06 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-003 (passos 1–4), com um ID novo só com números (ex.: a data e hora `0311261205`). |
| **Resultado esperado** | Cadastro bem-sucedido. |
| **Dado usado** | `11031655` (só números) · dados fictícios · senha de teste (não publicada) |
| **Resultado atual** | Após salvar: página principal com Sign In, sem mensagem de erro (sem login automático). Login com `11031655` autenticado ("Welcome Teste!"). My Account exibe **`11031655`**. |
| **Data da execução** | 03/11/2026 16:54 · A01 |
| **Execução** | **Executado pelo Claude** a pedido do testador (preenchimento dos campos, cliques em Save e Login e prints com data e hora); **senhas digitadas pelo testador**. |
| **Status / Evidência** | ✅ **Passou sob a premissa Q-06** · [01](../evidencias/PS-TC-009_20261103_165405_T01_01-antes-salvar.jpg) · [02](../evidencias/PS-TC-009_20261103_165405_T01_02-depois-salvar.jpg) · [03](../evidencias/PS-TC-009_20261103_165405_T01_03-antes-login.jpg) · [04](../evidencias/PS-TC-009_20261103_165405_T01_04-depois-login.jpg) · [05](../evidencias/PS-TC-009_20261103_165405_T01_05-my-account.jpg) |

---

## ⏸️ Cenário pendente de esclarecimento

### PS-TC-P01 — Unicidade com maiúsculas/minúsculas *(dúvida Q-03)*
O requisito PS-REQ-001 não diz se `j2ee` e `J2EE` são o mesmo ID. **Sem essa regra, não existe resultado esperado.** Por isso este caso não será executado como aprovado/reprovado até a dúvida ser respondida.
- **Pergunta ao time:** "O User ID deve ser único ignorando maiúsculas e minúsculas?"
- **Depois da resposta:** inserir o ID do PS-TC-003 com as letras trocadas entre maiúsculas e minúsculas e comparar com a regra definida.

## 🔍 Teste exploratório

### PS-TC-E01 — User ID muito longo *(dúvida Q-05)*
Os limites de tamanho não estão documentados, por isso **não foram inventados valores**. Objetivo: observar como o sistema reage.
- **Dado:** um ID alfanumérico de 50 caracteres (ex.: `QAFab` + 45 letras/números).
- **O que observar:** o sistema aceita, recusa com uma mensagem amigável ou mostra um erro técnico (ex.: erro de servidor, mensagem em inglês técnico)?
- **Dado usado:** `QaLongo11031700abcdefghijklmnopqrstuvwxyzABCDEFGHI` (50 caracteres, conferidos no código da página; o campo não tem `maxlength`) · 03/11/2026 16:56 · A01
- **Resultado atual:** **erro 500** ao salvar: `string data, right truncation; table: SIGNON column: USERNAME`, com SQL e stack trace expostos. O login seguinte com o mesmo ID foi recusado ("Invalid username or password"), o que **não prova** ausência de gravação parcial.
- **Execução:** executado pelo Claude a pedido do testador; senhas digitadas pelo testador.
- **Status:** 🔍 **Exploratório executado** → [BUG-006](../bug-reports/BUG-006.md) · [01](../evidencias/PS-TC-E01_20261103_165636_T01_01-antes-salvar.jpg) · [02](../evidencias/PS-TC-E01_20261103_165636_T01_02-depois-salvar.jpg) · [03](../evidencias/PS-TC-E01_20261103_165636_T01_03-antes-login.jpg) · [04](../evidencias/PS-TC-E01_20261103_165636_T01_04-depois-login.jpg)
- **Limite exato:** não determinado (só 50 caracteres foi testado). Os IDs de 14 a 15 caracteres usados nos outros testes foram aceitos.
