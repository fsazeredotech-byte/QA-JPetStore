# 📝 Casos de teste — Cadastro de usuário (campo User ID)

**Aplicação:** [JPetStore — Mate Academy](https://jpetstore.mate.academy/actions/Catalog.action)
**Funcionalidade:** User Registration — **escopo: somente o campo User ID** (não cobre o cadastro completo)
**Requisitos:** PS-REQ-001 (User ID único) · PS-REQ-002 (User ID apenas alfanumérico, letras latinas) — fonte: tarefa do curso Mate Academy
**Versão do documento:** 2 (revisada — ver [`REVISOES.md`](../REVISOES.md))
**Ambiente:** Google Chrome (versão: ____) · Sistema operacional: ____
**Data da execução:** ____ · **Executado por:** Fabricio S Azeredo

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
3. **Dados válidos para os outros campos:** Password `Senha123` · Repeat password `Senha123` · First name `Teste` · Last name `QA` · Email `teste.qa@example.com` · Phone `21999999999` · Address 1 `Rua Exemplo, 100` · City `Rio de Janeiro` · State `RJ` · Zip `20000000` · Country `Brasil`.
4. Para IDs que precisam ser novos, usar o padrão `QAFab` + data e hora (ex.: `QAFab0311261205`) e **anotar o valor usado**.

⚠️ Usar apenas dados fictícios — este repositório é público.

**Legenda de status:** ✅ Passou · ❌ Falhou · ⏳ A executar · ⏸️ Bloqueado/pendente

---

## 📋 Resumo
| ID | Resumo | Req. | Prioridade | Status |
|---|---|---|---|---|
| PS-TC-001 | Não aceita User ID já existente | REQ-001 | 🟠 Alta | ⏳ |
| PS-TC-002 | Não aceita caracteres especiais (`!` `@` `#`) | REQ-002 | 🟠 Alta | ⏳ |
| PS-TC-003 | Aceita letras e números — cadastro e login | REQ-002 | 🔴 Crítica | ✅ Passou |
| PS-TC-004 | Não aceita User ID vazio | Q-01 | 🟠 Alta | ⏳ |
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
| **Dado usado / Resultado atual** | |
| **Status / Evidência** | ⏳ · `evidencias/PS-TC-001.png` |

## PS-TC-002 — Não aceita caracteres especiais
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 · 🟠 Alta |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Inserir o User ID da tabela abaixo.<br>3. Preencher os demais campos com dados válidos.<br>4. Clicar em **Save Account Information**.<br>5. Repetir para cada dado. |
| **Resultado esperado** | O sistema exibe um erro informando que o User ID deve conter apenas letras e números, e a conta não é criada. |

| Variação | Dado | Resultado atual | Status | Evidência |
|---|---|---|---|---|
| 002-a | `User!123` | | ⏳ | `PS-TC-002a.png` |
| 002-b | `User@123` | | ⏳ | `PS-TC-002b.png` |
| 002-c | `User#123` | | ⏳ | `PS-TC-002c.png` |

## PS-TC-003 — Aceita letras e números (cadastro e login)
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 · 🔴 Crítica (teste de fumaça) |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Inserir um User ID novo com letras e números (padrão `QAFab` + data e hora).<br>3. Preencher os demais campos com dados válidos.<br>4. Clicar em **Save Account Information**.<br>5. Clicar em **Sign Out**.<br>6. Fazer login com o User ID e a senha `Senha123`. |
| **Resultado esperado** | Passo 4: cadastro bem-sucedido e redirecionamento para a página principal (resultado definido na tarefa do curso).<br>Passo 6: o login funciona e o nome da conta aparece na página. |
| **Dado usado** | User ID `QAFab0311261210` · First/Last name `teste1` · senha de teste (não publicada) |
| **Resultado atual** | Passo 4: cadastro concluído e redirecionamento para a página principal com a mensagem "Welcome teste1!". A página **My Account** mostra o User ID e os dados salvos.<br>Passo 6: login com o novo User ID funcionou ("Welcome teste1!"). |
| **Data da execução** | 03/11/2026 |
| **Status / Evidência** | ✅ **Passou** · [`PS-TC-003-passo4.jpg`](../evidencias/PS-TC-003-passo4.jpg) · [`PS-TC-003-passo6-login.jpg`](../evidencias/PS-TC-003-passo6-login.jpg) |

## PS-TC-004 — Não aceita User ID vazio *(premissa Q-01)*
| Campo | Descrição |
|---|---|
| **Requisito** | Premissa Q-01 · 🟠 Alta |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Deixar o User ID em branco.<br>3. Preencher os demais campos com dados válidos.<br>4. Clicar em **Save Account Information**. |
| **Resultado esperado** | O sistema exibe um erro informando que o User ID é obrigatório, e a conta não é criada. |
| **Resultado atual** | |
| **Status / Evidência** | ⏳ · `evidencias/PS-TC-004.png` |

## PS-TC-005 — Não aceita espaços *(premissa Q-04)*
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 / Q-04 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-002, com os dados abaixo. |
| **Resultado esperado** | O sistema exibe um erro informando que o User ID deve conter apenas letras e números, e a conta não é criada. |

| Variação | Dado | Resultado atual | Status | Evidência |
|---|---|---|---|---|
| 005-a | `Test User` (espaço no meio) | | ⏳ | `PS-TC-005a.png` |
| 005-b | ` Teste123` (espaço no início) | | ⏳ | `PS-TC-005b.png` |
| 005-c | `Teste123 ` (espaço no fim) | | ⏳ | `PS-TC-005c.png` |
| 005-d | `   ` (só espaços) | | ⏳ | `PS-TC-005d.png` |

## PS-TC-006 — Não aceita letras acentuadas *(premissa Q-02)*
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 / Q-02 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-002, com o dado `Usuário1`. |
| **Resultado esperado** | O sistema exibe um erro informando que o User ID deve conter apenas letras (A–Z) e números, e a conta não é criada. |
| **Resultado atual** | |
| **Status / Evidência** | ⏳ · `evidencias/PS-TC-006.png` |

## PS-TC-007 — Não aceita alfabeto não latino
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-002, com o dado `用户123`. |
| **Resultado esperado** | O sistema exibe um erro informando que o User ID deve conter apenas letras latinas e números, e a conta não é criada. |
| **Resultado atual** | |
| **Status / Evidência** | ⏳ · `evidencias/PS-TC-007.png` |

## PS-TC-008 — Aceita User ID só com letras *(premissa Q-06)*
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 / Q-06 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-003 (passos 1–4), com um ID novo só com letras (ex.: `QAFabTeste`). |
| **Resultado esperado** | Cadastro bem-sucedido. |
| **Dado usado / Resultado atual** | |
| **Status / Evidência** | ⏳ · `evidencias/PS-TC-008.png` |

## PS-TC-009 — Aceita User ID só com números *(premissa Q-06)*
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 / Q-06 · 🟡 Média |
| **Passos** | Os mesmos do PS-TC-003 (passos 1–4), com um ID novo só com números (ex.: a data e hora `0311261205`). |
| **Resultado esperado** | Cadastro bem-sucedido. |
| **Dado usado / Resultado atual** | |
| **Status / Evidência** | ⏳ · `evidencias/PS-TC-009.png` |

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
