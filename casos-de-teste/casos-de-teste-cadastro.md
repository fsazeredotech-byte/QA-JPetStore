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
| Q-02 | "Letras latinas" inclui letras acentuadas (á, ç)? | Não: só `A–Z`, `a–z` e `0–9`. |
| Q-03 | `j2ee` e `J2EE` são o mesmo ID? | Sem premissa — cenário pendente (PS-TC-P01). |
| Q-04 | Espaços no início/fim são removidos ou rejeitados? | Rejeitados. |
| Q-05 | Existe tamanho mínimo/máximo para o User ID? | Não documentado — investigar com teste exploratório (PS-TC-E01). |
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
| PS-TC-005 | Não aceita espaços (meio, início, fim, só espaços) | REQ-002 / Q-04 | 🟡 Média | ⏳ |
| PS-TC-006 | Não aceita letras acentuadas | REQ-002 / Q-02 | 🟡 Média | ⏳ |
| PS-TC-007 | Não aceita alfabeto não latino | REQ-002 | 🟡 Média | ⏳ |
| PS-TC-008 | Aceita User ID só com letras | REQ-002 / Q-06 | 🟡 Média | ⏳ |
| PS-TC-009 | Aceita User ID só com números | REQ-002 / Q-06 | 🟡 Média | ⏳ |
| PS-TC-P01 | Unicidade com maiúsculas/minúsculas | REQ-001 / Q-03 | 🟡 Média | ⏸️ Pendente |
| PS-TC-E01 | Exploratório: User ID muito longo | Q-05 | 🟢 Baixa | ⏳ |

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
| 005-a | `Test User` (espaço no meio) | | ⏳ | conforme o protocolo |
| 005-b | ` Teste123` (espaço no início) | | ⏳ | conforme o protocolo |
| 005-c | `Teste123 ` (espaço no fim) | | ⏳ | conforme o protocolo |
| 005-d | `   ` (só espaços) | | ⏳ | conforme o protocolo |

## PS-TC-006 — Não aceita letras acentuadas *(premissa Q-02)*
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 / Q-02 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-002, com o dado `Usuário1`. |
| **Resultado esperado** | O sistema exibe um erro informando que o User ID deve conter apenas letras (A–Z) e números, e a conta não é criada. |
| **Resultado atual** | |
| **Status / Evidência** | ⏳ · conforme o protocolo |

## PS-TC-007 — Não aceita alfabeto não latino
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-002, com o dado `用户123`. |
| **Resultado esperado** | O sistema exibe um erro informando que o User ID deve conter apenas letras latinas e números, e a conta não é criada. |
| **Resultado atual** | |
| **Status / Evidência** | ⏳ · conforme o protocolo |

## PS-TC-008 — Aceita User ID só com letras *(premissa Q-06)*
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 / Q-06 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-003 (passos 1–4), com um ID novo só com letras (ex.: `QAFabTeste`). |
| **Resultado esperado** | Cadastro bem-sucedido. |
| **Dado usado / Resultado atual** | |
| **Status / Evidência** | ⏳ · conforme o protocolo |

## PS-TC-009 — Aceita User ID só com números *(premissa Q-06)*
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 / Q-06 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-003 (passos 1–4), com um ID novo só com números (ex.: a data e hora `0311261205`). |
| **Resultado esperado** | Cadastro bem-sucedido. |
| **Dado usado / Resultado atual** | |
| **Status / Evidência** | ⏳ · conforme o protocolo |

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
- **Resultado atual:** ____
- **Se aparecer um erro técnico**, registrar um bug em `bug-reports/`.
