# 🔄 Histórico de revisões

Este arquivo registra como os casos de teste evoluíram depois de revisões, como acontece num time de QA real. A versão anterior de cada arquivo continua disponível no [histórico de commits](../../commits/main).

---

## Versão 2 — 03/11/2026 · revisão por pares
**Revisores:** ChatGPT (revisão inicial) e Claude (análise da revisão) · **Decisão final:** Fabricio S Azeredo

### 🔴 Erros corrigidos
| Caso (v1) | Problema encontrado | Correção (v2) |
|---|---|---|
| PS-TC-006 | Tratava letras **acentuadas** como "não latinas". Isso está errado: `á` e `ç` são letras do alfabeto latino. | O caso foi dividido em **PS-TC-006** (acentuadas, com a dúvida Q-02 registrada) e **PS-TC-007** (alfabeto não latino, `用户123`). |
| PS-TC-002 | O título prometia `! @ #`, mas o teste só usava `!`. | Agora são 3 variações: 002-a, 002-b e 002-c. |
| PS-TC-003 | Só o redirecionamento não comprova que a conta foi criada. | Foi adicionado um login com a conta criada. |

### 🟡 Melhorias
| Item | O que mudou |
|---|---|
| Premissas | Nova seção **Premissas e dúvidas** (Q-01 a Q-06), separando o que é requisito do que é suposição. |
| Prioridade | Foram documentados os **critérios de prioridade**. O PS-TC-003 continua **Crítica**, como definido na tarefa do curso. |
| PS-TC-001 | Passou a usar uma conta de teste controlada (a do PS-TC-003) em vez de depender do usuário `j2ee`, e verifica se a conta original continua intacta. |
| PS-TC-004 | Marcado como premissa: o requisito não diz que o campo é obrigatório. |
| PS-TC-005 | Agora cobre espaço no meio, no início, no fim e só espaços. |
| PS-TC-007 (v1) | Virou **PS-TC-P01, pendente de esclarecimento**, porque sem a regra de maiúsculas/minúsculas não existe resultado esperado. |
| Novos casos | **PS-TC-008** (só letras), **PS-TC-009** (só números) e **PS-TC-E01** (exploratório de tamanho, sem inventar limites). |
| Preparação | O passo a passo agora é o mesmo para todos os casos, começando sem sessão e com um padrão de ID único. |

### 💡 O que aprendi
- **Ler o requisito ao pé da letra:** "letras latinas" não é o mesmo que "sem acento".
- **Um resultado esperado precisa ser verificável:** redirecionar para a página principal não prova que o cadastro deu certo.
- **Não inventar regras:** quando o requisito não diz algo, registro como dúvida em vez de supor um resultado.

---

## Versão 1 — 03/11/2026
Primeira versão: 7 casos de teste para o campo User ID (PS-TC-001 a PS-TC-007).
