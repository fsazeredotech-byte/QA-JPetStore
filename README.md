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
├── casos-de-teste/
│   └── casos-de-teste-cadastro.md     ← casos de teste manuais
├── evidencias/                        ← prints da execução (um por caso)
└── bug-reports/                       ← relatórios de bugs encontrados
    └── modelo-bug-report.md           ← modelo usado nos relatórios
```

## ✅ Resumo dos casos de teste

| ID | Resumo | Requisito | Prioridade | Status |
|---|---|---|---|---|
| PS-TC-001 | Não aceita User ID já existente | REQ-001 | 🟠 Alta | ⏳ A executar |
| PS-TC-002 | Não aceita caracteres especiais | REQ-002 | 🟠 Alta | ⏳ A executar |
| PS-TC-003 | Aceita letras e números | REQ-002 | 🔴 Crítica | ⏳ A executar |
| PS-TC-004 | Não aceita User ID vazio | REQ-002 | 🟠 Alta | ⏳ A executar |
| PS-TC-005 | Não aceita espaço no User ID | REQ-002 | 🟡 Média | ⏳ A executar |
| PS-TC-006 | Não aceita letras não latinas | REQ-002 | 🟡 Média | ⏳ A executar |
| PS-TC-007 | Unicidade não depende de maiúsculas/minúsculas | REQ-001 | 🟡 Média | ⏳ A executar |

Detalhes completos em [`casos-de-teste/casos-de-teste-cadastro.md`](casos-de-teste/casos-de-teste-cadastro.md).

## 🐞 Bugs encontrados

_Nenhum bug registrado ainda. Os relatórios serão adicionados em `bug-reports/` somente se um defeito real for encontrado na execução._

## 🧰 Ferramentas

- Testes manuais em navegador (Google Chrome)
- Google Sheets (planilha de casos de teste do curso)
- GitHub (documentação e versionamento)

## 🚀 Próximos passos

- [ ] Executar os casos de teste e registrar evidências
- [ ] Registrar bugs reais encontrados
- [ ] Automatizar os casos com Playwright
- [ ] Testar login e carrinho de compras

---

📚 Projeto desenvolvido durante o curso de Testes de Software da **Mate Academy** (2026).
