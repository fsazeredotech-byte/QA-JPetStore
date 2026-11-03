# 🧭 Protocolo de execução de testes

> **Regra de ouro:** registre o que você **observou** e diga o que **não conseguiu confirmar**. A documentação nunca afirma mais do que as evidências mostram.

Protocolo criado pelo ChatGPT a pedido do testador, revisado e adaptado pelo Claude para um QA iniciante · Decisão final: Fabricio S Azeredo

---

## 1. 🖥️ Ambiente

| ID | Navegador | Sistema | Aplicação |
|---|---|---|---|
| **A01** | Navegador integrado do app Claude Desktop 4.4212.0 (Chromium 152.0.7977.167) | Windows 10 64-bit | https://jpetstore.mate.academy |

**Antes de cada variação:**
- [ ] Confira que aparece **Sign In** no topo, ou seja, que não há sessão aberta. Se aparecer **Sign Out**, clique nele antes de começar.
- [ ] Anote a **data e a hora** (fuso de Dublin).
- [ ] Use os **dados fictícios** do documento de casos de teste.
- [ ] Use uma **senha exclusiva de teste**. Guarde-a só em uma anotação privada; no repositório escreva apenas *"senha de teste não publicada"*.

## 2. 🆔 User ID único por tentativa

- Monte o ID com **data + hora + minuto + segundo**, mantendo só a característica que está sendo testada. Ex.: para testar `#` → `Qa#1103142501`.
- Isso **reduz** a chance de colisão, mas não garante que o ID nunca existiu. Registre: *"ID novo gerado; existência prévia não confirmada"*.
- Se aparecer **erro de duplicidade** (erro 500 do BUG-001), **não** conclua nada sobre a validação. Registre a tentativa e repita com outro ID.
- **Caracteres que o teclado não tem** (ex.: `用户` do PS-TC-007): **copie e cole** do documento de casos de teste.
- **Espaços:** anote em palavras (*"1 espaço no início"*, *"3 espaços"*), porque o print pode não mostrar.

| Caso | Exemplo de User ID |
|---|---|
| 002-c `#` | `Qa#1103142501` |
| 004 vazio | _(deixar vazio)_ |
| 005-a espaço no meio | `Qa 1103142502` |
| 005-b espaço inicial | ` Qa1103142503` |
| 005-c espaço final | `Qa1103142504 ` |
| 005-d só espaços | `   ` (3 espaços) |
| 006 acento | `Usuário1103142505` |
| 007 não latino | `用户1103142506` |
| 008 só letras | `QAFabTesteAbc` (trocar as letras a cada tentativa) |
| 009 só números | `110314250701` |
| E01 50 caracteres | contar exatamente 50 caracteres antes de colar |

## 3. 📸 Prints por etapa

| Etapa | O que capturar | Quando |
|---|---|---|
| **01 · antes de salvar** | O formulário preenchido, com o User ID visível. **Cubra as senhas.** | Sempre |
| **02 · depois de salvar** | A página resultante: mensagem e indicação de sessão (Sign In ou Welcome). | Sempre |
| **03 · antes do login** | A tela de Sign In com o ID digitado. **Cubra a senha.** | Só se o cadastro **parecer aceito** ou houver dúvida |
| **04 · depois do login** | O resultado do login (Welcome ou erro). **Não reutilize o print 02.** | Só se fez a etapa 03 |
| **05 · My Account** | O User ID exibido. **Cubra o telefone e o e-mail** se não forem fictícios. Não clique em salvar nessa página. | Só se o login funcionou |

✅ **Se o sistema recusar claramente** (mensagem de erro no formulário): bastam os prints **01 e 02**. Registre *"cadastro rejeitado na interface"*, sem afirmar nada sobre o banco de dados.

⚠️ **Se aparecer erro 500:** capture o erro e tente o login para verificar se a conta foi criada mesmo assim. **Um login que falha, sozinho, não prova que a conta não existe.**

**Como capturar e proteger:**
- `Windows + Shift + S` para capturar.
- Cubra senhas e dados pessoais com um **retângulo opaco** (preto), nunca com borrão ou marca-texto.
- Salve, **abra de novo para conferir** e só depois envie.
- ⏱️ **Inclua algo que mude a cada captura**, como o **relógio da barra de tarefas** do Windows (capture a tela inteira ou deixe o relógio visível). Telas como o erro 500 ou a página principal deslogada são **idênticas pixel por pixel** em momentos diferentes, e o print sozinho não prova quando foi tirado.
- Se o print for capturado só da página (sem relógio), registre o **horário da captura** no resultado.
- Não capture outras abas, notificações ou o seu perfil.

## 4. 🗂️ Nome dos arquivos

```
CASO_DATA_HORA_TENTATIVA_ETAPA.png   (ou .jpg)
PS-TC-002c_20261103_142501_T01_01-antes-salvar.png
PS-TC-002c_20261103_142501_T01_02-depois-salvar.png
PS-TC-002c_20261103_142501_T01_05-my-account.png
```
- Use o mesmo horário em todos os prints da tentativa. Uma repetição vira **T02**.
- Os formatos `.png` e `.jpg` são aceitos (as capturas feitas pelo navegador do app são `.jpg`).
- Se alguém além do testador preencher campos, clicar ou capturar prints, registre isso no campo **Execução** do caso.
- 💡 Se ficar difícil, mande os prints com qualquer nome e diga a etapa de cada um; eles são renomeados antes de publicar.

## 5. ✍️ Como escrever o resultado

| Campo | Exemplo |
|---|---|
| **Planejado** | `User#123` |
| **Usado** | `Qa#1103142501` |
| **Esperado** | Erro: apenas letras e números (PS-REQ-002) |
| **Observado** | "Após salvar: página principal com Sign In, sem mensagem. Login manual com o ID usado: autenticado. My Account mostra `Qa#1103142501`. Evidências 01–05." |

❌ Evite: *"todos os dados ficaram intactos"*.
✅ Prefira: *"nome e e-mail conferidos, sem alteração observada"*.

## 6. 🚦 Status e quando abrir bug

| Situação | Status |
|---|---|
| Regra confirmada e comportamento igual ao esperado | ✅ **Passou** |
| Regra confirmada e comportamento diferente | ❌ **Falhou** → vincular bug e evidências |
| Site fora do ar, preparação impossível ou interferência | 🚧 **Bloqueado** (com o motivo) |
| Regra ambígua | ⏸️ **Pendente de esclarecimento** |
| Ainda não começou | ⏳ **A executar** |
| Exploratório sem limite conhecido | 🔍 **Exploratório executado** (descrever o observado) |

- Nos casos baseados em **premissa** (004, 005-b/c, 006, 008, 009), escreva *"Passou/Falhou sob a premissa Q-XX"*.
- **Antes de abrir um bug:** confira os dados e a sessão, repita se possível com um ID novo e registre a frequência (*"observado em 2/2 tentativas"*).
- Se for o **mesmo defeito** de um bug existente, acrescente a variação nesse bug em vez de abrir outro.

## 7. 📝 Modelo de registro (copie para cada tentativa)

| Campo | Registro |
|---|---|
| Caso / variação / tentativa | |
| Data, hora e fuso | |
| Ambiente | A01 |
| Execução (quem fez cada parte) | |
| Requisito ou premissa | |
| Dado planejado | |
| Dado usado | User ID exato; espaços e quantidade; senha não publicada |
| Resultado esperado | |
| Depois de salvar | Página, mensagem e indicação de sessão |
| Login manual | Funcionou / falhou / não realizado (motivo) |
| My Account | ID e campos conferidos |
| Status e justificativa | |
| Evidências | |
| Bug / dúvida / limitação | |

✅ **Ao terminar cada variação**, atualize: o detalhe do caso, o resumo dos casos e o README. Deixe visível o que continua pendente.
