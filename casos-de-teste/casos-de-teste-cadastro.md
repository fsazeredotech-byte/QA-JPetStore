# 📝 Casos de teste — Cadastro de usuário (campo User ID)

**Aplicação:** [JPetStore — Mate Academy](https://jpetstore.mate.academy/actions/Catalog.action)
**Funcionalidade:** User Registration
**Requisitos:** PS-REQ-001 (User ID único) · PS-REQ-002 (User ID apenas alfanumérico, letras latinas)
**Ambiente:** Google Chrome (versão: ____) · Sistema operacional: ____
**Data da execução:** ____ · **Executado por:** Fabricio S Azeredo

**Legenda de prioridade:** 🔴 Crítica · 🟠 Alta · 🟡 Média · 🟢 Baixa
**Legenda de status:** ✅ Passou · ❌ Falhou · ⏳ A executar

> **Dados válidos para os outros campos** (usar em todos os casos): Password `Senha123` · Repeat password `Senha123` · First name `Teste` · Last name `QA` · Email `teste.qa@example.com` · Phone `21999999999` · Address 1 `Rua Exemplo, 100` · City `Rio de Janeiro` · State `RJ` · Zip `20000000` · Country `Brasil`.
> ⚠️ Usar apenas dados fictícios — este repositório é público.

---

## PS-TC-001 — O campo User ID não deve aceitar um ID de usuário já existente
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-001 |
| **Prioridade** | 🟠 Alta |
| **Pré-condição** | Existe um usuário cadastrado com o User ID usado no teste (ex.: `j2ee`, usuário padrão da JPetStore, ou um ID criado no PS-TC-003). |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Inserir um User ID que já exista no sistema.<br>3. Preencher os outros campos com dados válidos.<br>4. Clicar em **Save Account Information**. |
| **Dado de teste** | `j2ee` |
| **Resultado esperado** | Uma mensagem de erro informa que o User ID deve ser único. A conta não é criada. |
| **Resultado atual** | |
| **Status** | ⏳ A executar |
| **Evidência** | `evidencias/PS-TC-001.png` |

## PS-TC-002 — O campo User ID não deve aceitar caracteres especiais (! @ #)
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 |
| **Prioridade** | 🟠 Alta |
| **Pré-condição** | Usuário não logado. |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Inserir um User ID com símbolo especial.<br>3. Preencher os demais campos com dados válidos.<br>4. Clicar em **Save Account Information**. |
| **Dado de teste** | `User!123` |
| **Resultado esperado** | Uma mensagem de erro informa que o User ID deve conter apenas letras e números. A conta não é criada. |
| **Resultado atual** | |
| **Status** | ⏳ A executar |
| **Evidência** | `evidencias/PS-TC-002.png` |

## PS-TC-003 — O campo User ID deve aceitar letras e números
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 |
| **Prioridade** | 🔴 Crítica |
| **Pré-condição** | Usuário não logado. O User ID usado ainda não existe. |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Inserir um User ID válido com letras e números.<br>3. Preencher os demais campos com dados válidos.<br>4. Clicar em **Save Account Information**. |
| **Dado de teste** | `TestUser2026` (se já existir, usar outro, ex.: `TestUser2026b`) |
| **Resultado esperado** | Cadastro bem-sucedido e redirecionamento para a página principal. |
| **Resultado atual** | |
| **Status** | ⏳ A executar |
| **Evidência** | `evidencias/PS-TC-003.png` |

## PS-TC-004 — O campo User ID não deve aceitar valor vazio
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 |
| **Prioridade** | 🟠 Alta |
| **Pré-condição** | Usuário não logado. |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Deixar o campo User ID em branco.<br>3. Preencher os demais campos com dados válidos.<br>4. Clicar em **Save Account Information**. |
| **Dado de teste** | _(campo vazio)_ |
| **Resultado esperado** | Uma mensagem de erro informa que o User ID é obrigatório. A conta não é criada. |
| **Resultado atual** | |
| **Status** | ⏳ A executar |
| **Evidência** | `evidencias/PS-TC-004.png` |

## PS-TC-005 — O campo User ID não deve aceitar espaço
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 |
| **Prioridade** | 🟡 Média |
| **Pré-condição** | Usuário não logado. |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Inserir um User ID com espaço no meio.<br>3. Preencher os demais campos com dados válidos.<br>4. Clicar em **Save Account Information**. |
| **Dado de teste** | `Test User` |
| **Resultado esperado** | Uma mensagem de erro informa que o User ID deve conter apenas letras e números. A conta não é criada. |
| **Resultado atual** | |
| **Status** | ⏳ A executar |
| **Evidência** | `evidencias/PS-TC-005.png` |

## PS-TC-006 — O campo User ID não deve aceitar letras não latinas ou acentuadas
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-002 |
| **Prioridade** | 🟡 Média |
| **Pré-condição** | Usuário não logado. |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Inserir um User ID com letra acentuada.<br>3. Preencher os demais campos com dados válidos.<br>4. Clicar em **Save Account Information**. |
| **Dado de teste** | `Usuário1` |
| **Resultado esperado** | Uma mensagem de erro informa que o User ID deve conter apenas letras latinas e números. A conta não é criada. |
| **Resultado atual** | |
| **Status** | ⏳ A executar |
| **Evidência** | `evidencias/PS-TC-006.png` |

## PS-TC-007 — A unicidade do User ID não deve depender de maiúsculas/minúsculas
| Campo | Descrição |
|---|---|
| **Requisito** | PS-REQ-001 |
| **Prioridade** | 🟡 Média |
| **Pré-condição** | Existe um usuário `j2ee` cadastrado. |
| **Passos** | 1. Abrir o formulário de cadastro.<br>2. Inserir o mesmo User ID existente com letras maiúsculas.<br>3. Preencher os demais campos com dados válidos.<br>4. Clicar em **Save Account Information**. |
| **Dado de teste** | `J2EE` |
| **Resultado esperado** | Uma mensagem de erro informa que o User ID já existe. A conta não é criada. |
| **Resultado atual** | |
| **Status** | ⏳ A executar |
| **Evidência** | `evidencias/PS-TC-007.png` |

> 💬 **Observação:** o PS-TC-007 é uma interpretação do requisito PS-REQ-001 (o requisito não diz se `j2ee` e `J2EE` são o mesmo ID). Se o sistema aceitar, registrar como **dúvida sobre o requisito** para o time, e não necessariamente como bug.
