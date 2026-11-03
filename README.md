# 🧪 QA-JPetStore

Portfólio de QA: casos de teste manuais, evidências e relatórios de bug do cadastro de usuários da **JPetStore**.

👤 **Autor:** Fabricio S Azeredo — QA Tester Júnior (em transição de carreira, vindo da Engenharia Mecânica e da indústria de petróleo)

---

## 📌 Sobre o projeto

Este projeto documenta testes manuais da funcionalidade **User Registration** (cadastro de usuário) da aplicação de treino [JPetStore — Mate Academy](https://jpetstore.mate.academy/actions/Catalog.action), com foco no campo **User ID**.

**Caminho até o formulário:** `Sign In` → `Register Now!` → campo **User ID**

## 📋 Requisitos testados

| ID | Requisito |
|---|---|
| **PS-REQ-001** | O campo User ID deve aceitar apenas valores únicos. |
| **PS-REQ-002** | O campo User ID deve aceitar apenas caracteres alfanuméricos (letras latinas e números). |

## 🗂️ Estrutura do repositório

```
QA-JPetStore/
├── README.md                          ← esta página
├── REVISOES.md                        ← histórico de revisões
├── PROTOCOLO-EXECUCAO.md              ← como os testes são executados e documentados
├── casos-de-teste/
│   └── casos-de-teste-cadastro.md     ← casos de teste manuais
├── evidencias/                        ← prints da execução
└── bug-reports/                       ← relatórios de bugs (BUG-001 a BUG-005)
    └── modelo-bug-report.md           ← modelo usado nos relatórios
```

## 📊 Resumo da execução

**Ambiente A01:** navegador integrado do app Claude Desktop 4.4212.0 (Chromium 152.0.7977.167) · Windows 10 64-bit · **Data:** 03/11/2026

| ✅ Passou | ❌ Falhou | ⏳ A executar | ⏸️ Pendente | 🐞 Bugs abertos |
|---|---|---|---|---|
| 1 caso (003) | 5 casos (001; 002; 004 e 006 sob premissa; 005 nas variações a e d) | 4 casos (007, 008, 009, E01) | 1 (P01) + 005-b/c (Q-04) | 5 |

## ✅ Resumo dos casos de teste (versão 3.2)

| ID | Resumo | Requisito | Prioridade | Status |
|---|---|---|---|---|
| PS-TC-001 | Não aceita User ID já existente | REQ-001 | 🟠 Alta | ❌ Falhou → [BUG-001](bug-reports/BUG-001.md) |
| PS-TC-002 | Não aceita caracteres especiais (`!` `@` `#`) | REQ-002 | 🟠 Alta | ❌ Falhou em `!`, `@` e `#` → [BUG-002](bug-reports/BUG-002.md) |
| PS-TC-003 | Aceita letras e números (cadastro e login) | REQ-002 | 🔴 Crítica | ✅ Passou |
| PS-TC-004 | Não aceita User ID vazio | Premissa Q-01 | 🟠 Alta | ❌ Falhou sob a premissa → [BUG-004](bug-reports/BUG-004.md) |
| PS-TC-005 | Não aceita espaços | REQ-002 | 🟡 Média | ❌ 005-a falhou → [BUG-002](bug-reports/BUG-002.md) · ⏸️ 005-b/c: espaços das pontas removidos (dúvida Q-04) · ❌ 005-d → [BUG-004](bug-reports/BUG-004.md) |
| PS-TC-006 | Não aceita letras acentuadas | Premissa Q-02 | 🟡 Média | ❌ Falhou sob a premissa → [BUG-002](bug-reports/BUG-002.md) · [BUG-005](bug-reports/BUG-005.md) |
| PS-TC-007 | Não aceita alfabeto não latino | REQ-002 | 🟡 Média | ⏳ A executar |
| PS-TC-008 | Aceita User ID só com letras | REQ-002 | 🟡 Média | ⏳ A executar |
| PS-TC-009 | Aceita User ID só com números | REQ-002 | 🟡 Média | ⏳ A executar |
| PS-TC-P01 | Unicidade com maiúsculas/minúsculas | REQ-001 | 🟡 Média | ⏸️ Pendente de esclarecimento |
| PS-TC-E01 | Exploratório: User ID muito longo | — | 🟢 Baixa | ⏳ A executar |

🔄 **Os casos passaram por revisão por pares.** Veja em [`REVISOES.md`](REVISOES.md) os erros encontrados na versão 1, as correções e o que aprendi.

Detalhes completos em [`casos-de-teste/casos-de-teste-cadastro.md`](casos-de-teste/casos-de-teste-cadastro.md).

## 🐞 Bugs encontrados (todos 🔴 Abertos)

| ID | Título | Severidade | Prioridade | Caso |
|---|---|---|---|---|
| [BUG-001](bug-reports/BUG-001.md) | Cadastro com User ID existente mostra erro 500 com stack trace e SQL expostos | 🟠 Major | 🟠 Alta | PS-TC-001 |
| [BUG-002](bug-reports/BUG-002.md) | Cadastro aceita User ID com caracteres não alfanuméricos (`!`, `@`, `#` e espaço) | 🟠 Major | 🟠 Alta | PS-TC-002 |
| [BUG-003](bug-reports/BUG-003.md) | Campos de senha do cadastro mostram a senha em texto visível | 🟡 Minor | 🟠 Alta | Exploratório |
| [BUG-004](bug-reports/BUG-004.md) | User ID vazio ou só com espaços é recusado sem nenhuma mensagem de erro | 🟡 Minor | 🟡 Média | PS-TC-004, 005-d |
| [BUG-005](bug-reports/BUG-005.md) | User ID com acento é exibido corrompido ("UsuÃ¡rio") na My Account | 🟡 Minor | 🟡 Média | PS-TC-006 |

## 🤝 Como este projeto foi feito

Este portfólio foi desenvolvido com **apoio de IA**, de forma transparente. Os commits marcados com *"fsazeredotech-byte and claude"* foram enviados com a ajuda do Claude.

| Quem | Papel |
|---|---|
| 👤 **Fabricio S Azeredo** | Executou os testes na aplicação (sozinho ou acompanhado pelo Claude; ver a coluna "Execução" de cada caso), digitou todas as senhas, confirmou os bugs, decidiu severidade e prioridade, aprovou cada alteração, enviou parte dos arquivos pelo site do GitHub e conduziu a revisão entre as IAs |
| 🟣 **Claude (Anthropic)** | Redação e organização da documentação, captura e proteção das evidências, envio de parte dos arquivos para o GitHub e, a pedido do testador, preenchimento de campos e cliques em alguns testes (registrado em cada caso) |
| 🟢 **ChatGPT (OpenAI)** | Revisão crítica independente dos casos e dos bugs, e protocolo de execução |

As revisões entre as duas IAs, e o que mudou por causa delas, estão registradas em [`REVISOES.md`](REVISOES.md).

## 🧰 Ferramentas

- Testes manuais no navegador (ambiente A01)
- Google Sheets (planilha de casos de teste do curso)
- GitHub (documentação e versionamento)
- Claude e ChatGPT (apoio na documentação e revisão por pares)

## 🚀 Próximos passos

- [x] Registrar bugs reais encontrados (5 até agora)
- [x] Criar protocolo de execução
- [ ] Concluir a execução dos casos restantes seguindo o protocolo
- [ ] Automatizar os casos com Playwright
- [ ] Testar login e carrinho de compras

---

📚 Projeto desenvolvido durante o curso de Testes de Software da **Mate Academy** (2026).
