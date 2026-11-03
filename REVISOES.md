# 🔄 Histórico de revisões

Este arquivo registra como os casos de teste evoluíram depois de revisões, como acontece num time de QA real. A versão anterior de cada arquivo continua disponível no [histórico de commits](https://github.com/fsazeredotech-byte/QA-JPetStore/commits/main).

---

## Versão 3 — 03/11/2026 · revisão das evidências e dos bug reports
**Revisores:** ChatGPT (revisão e protocolo de execução) e Claude (análise e ajustes) · **Decisão final:** Fabricio S Azeredo

**Lição principal:** *a documentação não pode afirmar mais do que as evidências mostram.*

### 🔴 Erros corrigidos
| Onde | Problema encontrado | Correção |
|---|---|---|
| Evidências | `PS-TC-003-passo4`, `PS-TC-003-passo6-login` e `PS-TC-001-passo5-login` eram **o mesmo arquivo** (a tela "Welcome teste1!" é idêntica nos três momentos). As imagens não provavam etapas diferentes. | Foi mantido um único print (`PS-TC-003_welcome-teste1.jpg`) e a limitação foi registrada nos casos |
| BUG-002 | "Afeta todo novo cadastro" generalizava além do observado. | Trocado por "observado em 2/2 variações (`!` e `@`)" |
| BUG-002 | "A validação acontece no servidor" não foi demonstrado. | Removido |
| BUG-002 | A falta de login automático era atribuída ao símbolo. | Movida para uma observação separada, **sem atribuir a causa** |
| BUG-001 | Afirmava que o usuário "perde os dados digitados" sem evidência, e misturava requisito com usabilidade. | O texto agora descreve só o observado e separa o REQ-001 (unicidade) da expectativa de usabilidade |
| BUG-003 | Imagem com borrão (pode ser revertido). | Trocado por tarja preta opaca, e o HTML dos campos foi incluído como evidência |

### 🟡 Melhorias
- **Status** (Aberto), **ambiente A01** (navegador e versão, sistema) e **frequência** em todos os bugs.
- **PROTOCOLO-EXECUCAO.md:** como executar e documentar cada teste (prints por etapa, nomes, dado planejado × usado, status).
- **README:** resumo da execução, status parcial do PS-TC-002 e checklist atualizado.
- **Casos de teste:** separação entre o dado planejado e o usado; a senha não é mais publicada como dado.

### 💡 O que aprendi
- **Uma imagem só prova o que ela mostra.** Três prints da mesma tela não provam três etapas.
- **Frequência e escopo:** escrever "observado em 2/2", e não "sempre" ou "todos".
- **Separar fato, hipótese e expectativa:** o que vi, o que suspeito e o que acho que deveria acontecer são coisas diferentes.

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
