# Curso "Subagentes" — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking. Build skill: `formato-curso` (ler `references/MASTER_COMPLETO.md`, `references/SVG-FUTURISTA.md`, `references/DESIGN-SYSTEM.md`, `references/CHECKLIST_REVISAO.md`). Audit: `revisar-curso`.

**Goal:** Publicar um curso INEMA.CLUB profundo sobre subagentes (Claude Code + Codex/outros), 4 trilhas × 6 módulos + 4 índices + landing, em `inematds.github.io/subagentes/`.

**Architecture:** Páginas HTML estáticas self-contained no design system INEMA (Tailwind CDN + CSS/JS inline, light/dark). Cada módulo segue um molde fixo de 10 blocos. Desenhos = SVG futurista inline; telas = SVG monitor-mock + render Remotion (PNG). Construção faseada com gate de validação no módulo-piloto M1.1.

**Tech Stack:** HTML5 + Tailwind (CDN) + SVG inline; Remotion (React, em `_build/remotion/`, não publicado) para PNGs marquee; git + GitHub Pages; portal Next.js para registro.

---

## Política de modelo por fase (embutida — aplica a doutrina do próprio curso)

| Trabalho | Modelo | Raciocínio | Onde no plano |
|---|---|---|---|
| Planejamento + decisões de molde | **Opus 4.8** | alto/máx | Esta sessão (orquestrador) |
| Fase 0 — fundação + piloto M1.1 | **Opus 4.8** | alto | Task 0.1–0.3 |
| Módulos "cabeça" (M1.3, M3.1, M3.2, M4.1, M4.2, M4.3) | **Opus 4.8** | alto | Tasks marcadas `🧠 Opus` |
| Replicação dos demais módulos | **Sonnet 4.6** | médio | Tasks marcadas `⚙️ Sonnet` |
| Índices de trilha | **Sonnet 4.6** | médio | Tasks `idx` |
| Cenas Remotion / SVG novos de estilo | Sonnet (1ºs no Opus) | médio | Fase 5 |
| Scaffolding, nav repetida, wiring de assets, varreduras | **Haiku 4.5** | baixo | Task 0.0, utilitários |
| Revisor imparcial (`revisar-curso`) | Sonnet (final em Opus) | médio→alto | Fim de cada fase |

**Regra de ouro:** tudo em **Opus até o M1.1 ser aprovado no gate**. Só depois virar a chave pra Sonnet/Haiku. Mecanismo: `model:` por subagente; `CLAUDE_CODE_SUBAGENT_MODEL=claude-haiku-4-5` pros utilitários mecânicos. Orquestrador (esta sessão) fica Opus e **delega o build** pra manter o contexto limpo.

---

## File Structure

```
subagentes/
├── index.html                       # Landing (hero + 4 trilhas + resumo "1 tela")
├── docs/superpowers/{specs,plans}/  # spec + este plano
├── _build/remotion/                 # projeto Remotion (NÃO publicado)
└── curso/
    ├── trilha1/ index.html · modulo-1-1.html … modulo-1-6.html · assets/img/
    ├── trilha2/ index.html · modulo-2-1.html … modulo-2-6.html · assets/img/
    ├── trilha3/ index.html · modulo-3-1.html … modulo-3-6.html · assets/img/
    └── trilha4/ index.html · modulo-4-1.html … modulo-4-6.html · assets/img/
```

Responsabilidade: cada `modulo-X-Y.html` é uma página autocontida (uma aula). Cada `trilhaX/index.html` é o índice da trilha (mapa + cards). `index.html` raiz é a porta de entrada do curso.

---

## Convenções de build (PROCEDIMENTO PADRÃO — leia antes de qualquer task)

### P-MOD — Procedimento padrão de MÓDULO de conteúdo
Toda task de módulo executa exatamente isto:
1. Ler `formato-curso`: `SKILL.md`, `references/MASTER_COMPLETO.md` (secs 8/9/10 base, 6.2/7.5/7.6/7.7/7.14 módulo, 1.10 SVG), `references/SVG-FUTURISTA.md`, `references/CHECKLIST_REVISAO.md`. Ler a página-piloto `curso/trilha1/modulo-1-1.html` como referência de molde aprovado.
2. Ler a seção deste plano do módulo-alvo (brief: cor da trilha, objetivo, tópicos, desenho-chave, exemplo `.md`, prompts, exercício).
3. Construir a página seguindo o **molde de 10 blocos**:
   1. Nav completo (Logo + INEMA.CLUB `text-sky-400` + 4 botões de trilha + theme toggle; trilha ativa destacada).
   2. Header: título `text-2xl font-bold` + subtítulo + duração + badge da trilha.
   3. Abertura (1 parágrafo) + **≥1 SVG futurista** (cor da trilha + ciano `#38bdf8`, glow, `role="img"`+`aria-label`, animação sob `prefers-reduced-motion`).
   4. `<h2>Conteúdo detalhado</h2>` simples.
   5. **≥6 tópicos**; cada um com 3 seções "O que é / Por que aprender / Conceitos-chave"; indicador = número em círculo; **variedade** de componentes (≥2 grids ✓/✗, ≥1 timeline, ≥2 tip boxes, ≥1 code box).
   6. Bloco **Exemplo real**: arquivo `.md` de subagente completo (code box) comentado.
   7. Bloco **Prompts prontos**: 2–3 prompts copy-paste.
   8. Bloco **Tela simulada**: SVG monitor-mock (ou `<img>` de `assets/img/…png` quando Remotion já existir).
   9. Bloco **Exercício** + critério de verificação.
   10. Navegação anterior/próximo + INEMA.CLUB.
4. Auto-checar contra `CHECKLIST_REVISAO.md` (todos os itens obrigatórios) e a "Verificação Final Rápida" da SKILL.
5. Reportar: arquivo criado, nº de linhas, nº de tópicos, desenhos usados, e qualquer desvio do molde.

Alvo: **500–800 linhas**, sem template repetido 6×.

### P-IDX — Procedimento padrão de ÍNDICE de trilha
1. Ler MASTER secs 6.1, 7.2, 7.3, 7.4, 7.4b (Mapa da trilha) + SVG-FUTURISTA.
2. Construir: nav completo + hero (com **1 hero SVG**) + **"Mapa da trilha"** (grid de cards-âncora `justify-between`: X.Y esq + duração dir, emoji no título, subtítulo punchy) + cards de módulo (cada um com `<a>…Ver Completo</a>` para `modulo-X-Y.html`) + modal via `<iframe>`.
3. Auto-checar contra checklist (itens 9–16 da Verificação Final).

### Cores
T1 **Emerald** · T2 **Blue** · T3 **Amber** · T4 **Purple**. Secundária de diagrama: ciano `#38bdf8`. Primary `#FACC15`, sky (INEMA.CLUB), red (erros).

### Verificação (substitui "testes")
"Teste" deste projeto = rodar o `CHECKLIST_REVISAO.md`/`revisar-curso` na página + abrir no navegador. Cada task de página só está "verde" quando passa o checklist correspondente.

---

## Fase 0 — Fundação + piloto (🧠 Opus) — TERMINA EM GATE

### Task 0.0: Scaffolding (Haiku ok)
**Files:** Create dirs `curso/trilha{1,2,3,4}/assets/img`, `_build/remotion`.
- [ ] **Step 1:** `mkdir -p curso/trilha1/assets/img curso/trilha2/assets/img curso/trilha3/assets/img curso/trilha4/assets/img _build/remotion`
- [ ] **Step 2:** Commit: `git add -A && git commit -m "chore: scaffold estrutura do curso"`

### Task 0.1: Landing `index.html` (🧠 Opus)
**Files:** Create `index.html`
- [ ] **Step 1:** Executar P-IDX adaptado para landing: nav completo + hero do curso (título "Subagentes", subtítulo, 1 hero SVG fan-out maestro→subagentes) + seção "O que você vai aprender" + **4 cards de trilha** (cor de cada uma, link `curso/trilhaX/index.html`, 1 frase + lista de módulos) + seção "Resumo em uma tela" (recria o slide 18: quando usar subagente, projeto×global, economia, revisor, workflow, a pergunta) + footer com INEMA.CLUB.
- [ ] **Step 2:** Auto-checar checklist (nav, INEMA.CLUB, light/dark, theme toggle, hero SVG).
- [ ] **Step 3:** Commit: `git add index.html && git commit -m "feat(landing): porta de entrada do curso"`

### Task 0.2: Índice T1 `curso/trilha1/index.html` (🧠 Opus)
**Files:** Create `curso/trilha1/index.html` (Emerald)
- [ ] **Step 1:** Executar **P-IDX** para a Trilha 1 (Fundamentos), 6 cards de módulo (M1.1–M1.6), hero SVG, Mapa da trilha.
- [ ] **Step 2:** Auto-checar (itens 9–16).
- [ ] **Step 3:** Commit: `git add curso/trilha1/index.html && git commit -m "feat(t1): índice da trilha Fundamentos"`

### Task 0.3: Módulo-piloto `curso/trilha1/modulo-1-1.html` (🧠 Opus) — DEFINE O MOLDE
**Files:** Create `curso/trilha1/modulo-1-1.html` (Emerald)
**Brief:** "O que é um subagente". Objetivo: assentar o modelo mental (maestro delega; subagente trabalha em janela própria; relatório volta limpo).
Tópicos (≥6): (1) O maestro e o especialista; (2) O ciclo tarefa→janela própria→relatório; (3) A demo das 5 personas (analogia); (4) O que o subagente **não** é (não é o chat, não conversa com você); (5) Relação 1-para-1 com o maestro; (6) Primeiro contato — invocar um built-in.
Desenho-chave: fan-out maestro→subagentes→resumo (slide 2). Tela simulada: monitor-mock do terminal com 5 subagentes em paralelo.
Exemplo `.md`: `code-reviewer` mínimo (name/description/tools/model + corpo de 4 passos).
Prompts prontos: "abra um subagente pra pesquisar X e me traga só o resumo"; "use o general-purpose pra ler estes arquivos e resumir".
Exercício: listar 3 tarefas do seu dia que seriam subagente e justificar com a "pergunta que decide".
- [ ] **Step 1:** Executar **P-MOD** com o brief acima. Esta página é a referência de molde das outras 23.
- [ ] **Step 2:** Auto-checar checklist completo.
- [ ] **Step 3:** Commit: `git add curso/trilha1/modulo-1-1.html && git commit -m "feat(t1): módulo-piloto M1.1 (molde)"`

### ⛔ GATE 0 — validação do usuário
Apresentar landing + índice T1 + M1.1 ao usuário. Só seguir para Fase 1+ após aprovação do molde (densidade, desenhos, exemplo, prompts, exercício, telas). Ajustes pedidos viram o molde definitivo.

---

## Fase 1 — Completar Trilha 1 (Emerald) — após o gate

> Cada task = `P-MOD` com o brief. Modelo: `⚙️ Sonnet` salvo M1.3 (`🧠 Opus`). Commit por módulo. `revisar-curso` ao fim da fase.

### Task 1.2 (⚙️ Sonnet) `curso/trilha1/modulo-1-2.html` — "O verdadeiro motivo: contexto limpo"
Tópicos: (1) Janela de contexto (status line, tokens, %); (2) Poluição: tudo que você lê fica a sessão toda; (3) Fazer você mesmo × enviar subagente; (4) O resumo limpo de volta; (5) Por que é o motivo #1 (não velocidade); (6) Caso 300 páginas → 3 fatos. Desenho: duas janelas (acúmulo × limpo). Exemplo `.md`: `researcher` (Haiku, read-only+web). Prompts: delegar pesquisa pesada. Exercício: estimar tokens poupados num caso real. Commit `feat(t1): M1.2 contexto limpo`.

### Task 1.3 (🧠 Opus) `curso/trilha1/modulo-1-3.html` — "Agente × Subagente × Skill"
Módulo-cabeça (comparação central). Tópicos: (1) As três primitivas em 1 frase cada; (2) Contexto próprio/limpo × compartilhado; (3) Paralelismo; (4) Modelo próprio × herda; (5) Quem chama quem (skill↔agent; agent não chama agent); (6) Tabela de decisão "quando cada um". Desenho: 3 caixas + **tabela viva** (§6.1 do spec). Tela: monitor-mock skill rodando no fluxo vs subagente em sessão paralela. Exemplo `.md`: mesmo job como skill vs como subagente. Prompts: invocar skill × invocar subagente. Exercício: classificar 6 cenários. Commit `feat(t1): M1.3 agente×subagente×skill`.

### Task 1.4 (⚙️ Sonnet) `curso/trilha1/modulo-1-4.html` — "Built-in × Custom"
Tópicos: (1) Built-ins (Explore, Plan, general-purpose, claude); (2) "general-purpose com persona" ≠ custom; (3) Custom = arquivo `.md`; (4) Progressive disclosure (lê só name+description); (5) Por que economiza tokens; (6) `/agents` pra ver o que existe. Desenho: bifurcação + leitura de frontmatter→decisão. Exemplo `.md`: nenhum novo (mostrar saída do `/agents`). Prompts: "liste os subagentes disponíveis". Exercício: achar built-ins na sua máquina. Commit `feat(t1): M1.4 built-in×custom`.

### Task 1.5 (⚙️ Sonnet) `curso/trilha1/modulo-1-5.html` — "A pergunta que decide"
Tópicos: (1) "Isso vai despejar coisa que nunca vou reler?"; (2) Sinais de USAR; (3) Sinais de EVITAR; (4) Regra 10+ arquivos/output que não relê; (5) Escala → workflow dinâmico; (6) Não force subagente. Desenho: fluxo sim/não + duas colunas (§6.3). Tela: — (foco em diagrama). Exemplo `.md`: — . Prompts: 2 prompts (um vira subagente, outro não). Exercício: aplicar a pergunta a 5 tarefas próprias. Commit `feat(t1): M1.5 a pergunta que decide`.

### Task 1.6 (⚙️ Sonnet) `curso/trilha1/modulo-1-6.html` — "Anatomia de um subagente"
Tópicos: (1) O `.md` é o subagente (≈ skill.md); (2) Frontmatter (config) × corpo (cérebro); (3) Os 4 campos-chave; (4) O corpo: papel+passos+saída+não-fazer; (5) Onde vive (.claude/agents) — ponte pra T2; (6) Doc oficial pra todos os campos. Desenho: monitor-mock do `.md` com as 2 zonas destacadas. Exemplo `.md`: um `.md` totalmente anotado. Prompts: "abra meu code-reviewer.md e explique cada campo". Exercício: rotular as partes de um `.md` dado. Commit `feat(t1): M1.6 anatomia`.

### Fim Fase 1: rodar `revisar-curso` na Trilha 1.

---

## Fase 2 — Trilha 2 (Blue) — "Criando na prática"

### Task 2.idx (⚙️ Sonnet) `curso/trilha2/index.html` — P-IDX, Blue, cards M2.1–M2.6.

### Task 2.1 (⚙️ Sonnet) `modulo-2-1.html` — "O frontmatter completo"
Tópicos: (1) `name` (minúsculas/hífens); (2) `description` (o gatilho → M2.2); (3) `tools`; (4) `model` (→ T3); (5) `color`; (6) `memory` (project/user/local/none); (7) avançados (disallowed-tools, MCP). Desenho: frontmatter rotulado. Exemplo `.md`: frontmatter completo comentado. Prompts: gerar frontmatter via `/agents`. Exercício: escrever um frontmatter válido. Commit `feat(t2): M2.1 frontmatter`.

### Task 2.2 (⚙️ Sonnet) `modulo-2-2.html` — "A descrição é o gatilho"
Tópicos: (1) Por que a description dispara; (2) Fraca (exemplos); (3) Forte ("revisa… use proactively após mudanças em auth/pagamentos"); (4) Misfires (a mais/a menos); (5) Colisão skill×agent (caso plan-roaster); (6) Iterar (perguntar por que não disparou); (7) **Fechar aspas no YAML** (o bug real do vídeo). Desenho: duas descrições → roteamento. Exemplo `.md`: antes/depois de uma description. Prompts: "por que o subagente X não disparou nesse prompt? conserte a description". Exercício: reescrever uma description fraca. Commit `feat(t2): M2.2 descrição gatilho`.

### Task 2.3 (⚙️ Sonnet) `modulo-2-3.html` — "O corpo é o cérebro"
Tópicos: (1) Papel ("you are a senior…"); (2) Passos numerados ("when invoked: 1…"); (3) Formato de saída; (4) O que NÃO fazer; (5) Um agente, uma tarefa (teste do "e também"); (6) O corpo invoca skills; (7) Iterar com feedback. Desenho: escada papel→passos→saída. Exemplo `.md`: corpo de `security-reviewer`. Prompts: "transforme esta checklist em corpo de subagente". Exercício: dividir um agente faz-tudo em dois. Commit `feat(t2): M2.3 corpo cérebro`.

### Task 2.4 (⚙️ Sonnet) `modulo-2-4.html` — "Ferramentas e permissões"
Tópicos: (1) Dar só o necessário; (2) Read-only (Read/Grep/Glob); (3) Allowed × disallowed; (4) MCP permitidos; (5) Permissão real > prompt "não faça"; (6) "Se pode ler, assuma que vai"; (7) Granularidade (bash etc). Desenho: camada de permissão vs prompt. Exemplo `.md`: `code-reviewer` só leitura. Prompts: "crie um subagente read-only que…". Exercício: travar um agente como read-only. Commit `feat(t2): M2.4 ferramentas/permissões`.

### Task 2.5 (⚙️ Sonnet) `modulo-2-5.html` — "Projeto × Global"
Tópicos: (1) `.claude/agents` no repo; (2) `~/.claude/agents` global; (3) Compartilhar via git (time); (4) Só você (global); (5) Mover = mover o `.md`; (6) Ter nos dois; (7) Mesma lógica de skills/hooks/MCP. Desenho: duas pastas. Exemplo `.md`: mesmo agente nos 2 escopos. Prompts: "mova este agente de global pro projeto". Exercício: decidir escopo de 4 agentes. Commit `feat(t2): M2.5 projeto×global`.

### Task 2.6 (⚙️ Sonnet) `modulo-2-6.html` — "Criando com /agents + como o Claude escolhe"
Tópicos: (1) `/agents` (ver/criar); (2) generate-with-Claude × manual; (3) escolher tools/model/color/memory no wizard; (4) caso **plan-roaster** (criar + trimar description); (5) seleção (automático/proativo/`@nome`/`--agent`); (6) misfire e conserto; (7) trimar description gerada (progressive disclosure). Desenho/tela: wizard do `/agents` (monitor-mock; candidato a render Remotion na Fase 5). Exemplo `.md`: `plan-roaster.md` enxuto. Prompts: invocar por nome. Exercício: criar e invocar um agente por nome. Commit `feat(t2): M2.6 /agents`.

### Fim Fase 2: `revisar-curso` na Trilha 2.

---

## Fase 3 — Trilha 3 (Amber) — "Modelo, custo e orquestração"

### Task 3.idx (⚙️ Sonnet) `curso/trilha3/index.html` — P-IDX, Amber, cards M3.1–M3.6.

### Task 3.1 (🧠 Opus) `modulo-3-1.html` — "Modelo por tarefa" (cabeça)
Tópicos: (1) Haiku (escanear/buscar/resumir/docs); (2) Sonnet (construir/revisar); (3) Opus (raciocínio/segurança/arquitetura); (4) Raciocínio × custo × latência; (5) Armadilha: subagente começa em branco e reúne contexto; (6) Vale p/ tarefa grande, desperdício p/ 30s; (7) inherit-from-parent. Desenho: escada Haiku→Sonnet→Opus. Tela: status line mostrando modelo do subagente. Exemplo `.md`: 3 agentes, 3 modelos justificados. Prompts: "use Haiku pra escanear, Opus pra decidir". Exercício: atribuir modelo a 6 tarefas (tabela §6.2). Commit `feat(t3): M3.1 modelo por tarefa`.

### Task 3.2 (🧠 Opus) `modulo-3-2.html` — "A economia real" (cabeça)
Tópicos: (1) Chefe Opus/Sonnet + frota Haiku; (2) `CLAUDE_CODE_SUBAGENT_MODEL`; (3) `maxTurns` (evita loop); (4) Multiagente ~15×; (5) Quando vale (grande/ruidoso); (6) Combinação > gastar muito; (7) Medir/observar. Desenho: chefe + trabalhadores baratos. Exemplo `.md`: agente com `model` + `maxTurns`. Prompts: setar env var; limitar turns. Exercício: desenhar a divisão chefe/trabalhador de um job. Commit `feat(t3): M3.2 economia real`.

### Task 3.3 (⚙️ Sonnet) `modulo-3-3.html` — "Encadeando subagentes"
Tópicos: (1) Maestro coordena A→B→C; (2) skills↔agents; (3) agents **não** chamam agents; (4) 1-para-1 (subagentes não conversam); (5) Precisa sequência? maestro; (6) Skill que dispara subagentes; (7) Escala → workflow. Desenho: cadeia A→B→C + setas proibidas (agent⇏agent). Exemplo `.md`: skill que orquestra subagentes. Prompts: "encadeie pesquisa→rascunho→revisão". Exercício: modelar uma sequência de 3 passos. Commit `feat(t3): M3.3 encadeando`.

### Task 3.4 (⚙️ Sonnet) `modulo-3-4.html` — "O revisor imparcial"
Tópicos: (1) Contexto fresco = sem viés; (2) Julga o resultado, não o processo; (3) Quem constrói escreve, quem valida verifica; (4) Aplicar a código; (5) A plano (plan-roaster); (6) A pesquisa; (7) O maior unlock de qualidade. Desenho: builder × reviewer (caixa branca). Exemplo `.md`: `code-reviewer`/`plan-roaster` como validador. Prompts: "revise criticamente este resultado como um engenheiro externo". Exercício: rodar um revisor sobre um trabalho seu. Commit `feat(t3): M3.4 revisor imparcial`.

### Task 3.5 (⚙️ Sonnet) `modulo-3-5.html` — "Workflows dinâmicos"
Tópicos: (1) O que é (orquestra muitos subagentes); (2) Gatilho `ultracode` (era "workflow"); (3) Escala (dezenas/centenas); (4) Custo/limite de sessão; (5) Caso 41/210 subagentes; (6) Quando vale (job paralelo gigante); (7) Cuidado. Desenho: fan-out grande. Tela: workflow rodando N subagentes. Exemplo `.md`: — (conceito). Prompts: pedir um workflow com ressalva de custo. Exercício: decidir se um job pede workflow (justificar). Commit `feat(t3): M3.5 workflows dinâmicos`.

### Task 3.6 (⚙️ Sonnet) `modulo-3-6.html` — "Padrões de orquestração"
Tópicos: (1) Paralelo independente; (2) Pipeline; (3) Fan-out; (4) Subagente × agent teams (conversam, custam +); (5) Regra 10+ arquivos/parede; (6) Jobs independentes (15 capítulos); (7) Escolher topologia. Desenho: 3 topologias lado a lado. Exemplo: tabela padrão→caso. Prompts: "qual topologia pra revisar 20 arquivos?". Exercício: escolher topologia pra 3 problemas. Commit `feat(t3): M3.6 padrões de orquestração`.

### Fim Fase 3: `revisar-curso` na Trilha 3.

---

## Fase 4 — Trilha 4 (Purple) — "Avançado & cross-ambiente"

### Task 4.idx (⚙️ Sonnet) `curso/trilha4/index.html` — P-IDX, Purple, cards M4.1–M4.6.

### Task 4.1 (🧠 Opus) `modulo-4-1.html` — "Arquitetura multiagente em escala" (cabeça)
Tópicos: (1) Frota; (2) loop-until-dry; (3) Verificação adversarial (refutar, maioria); (4) Perspectivas diversas; (5) Completeness critic; (6) Sem cap silencioso (logar o que cortou); (7) Sintetizar. Desenho: pipeline find→verify→synthesize. Exemplo `.md`: esqueleto de harness (descrição). Prompts: "verifique adversarialmente cada achado". Exercício: desenhar um loop-until-dry. Commit `feat(t4): M4.1 multiagente em escala`.

### Task 4.2 (🧠 Opus) `modulo-4-2.html` — "Subagentes no Codex e outros ambientes" (cabeça)
Tópicos: (1) O conceito fora do Claude Code; (2) Codex CLI (equivalências de subagente/skill); (3) Cross-runtime (polyskill — uma fonte `.md`); (4) Equivalência de ferramentas; (5) O que muda/não muda; (6) Portar uma skill; (7) Limites. Desenho: mesma `.md` → 2 runtimes. Exemplo `.md`: skill/agente portável. Prompts: "adapte este subagente pro Codex". Exercício: apontar o equivalente de 1 conceito no Codex. Commit `feat(t4): M4.2 codex/cross-ambiente`.

### Task 4.3 (🧠 Opus) `modulo-4-3.html` — "Comparando modelos por papel" (cabeça)
Tópicos: (1) Opus/Sonnet/Haiku por papel; (2) GPT/Codex; (3) Outros provedores; (4) Raciocínio profundo × barato/rápido; (5) Quando trocar modelo/provedor; (6) Frota heterogênea; (7) Tradeoff custo/latência/qualidade. Desenho: matriz papel×modelo. Exemplo `.md`: frota heterogênea (agentes com modelos diferentes). Prompts: "que modelo pra cada papel desta frota?". Exercício: montar a matriz pro próprio caso. Commit `feat(t4): M4.3 modelos por papel`.

### Task 4.4 (⚙️ Sonnet) `modulo-4-4.html` — "Segurança"
Tópicos: (1) Baixar `.md` de terceiros; (2) Prompt injection; (3) Subagente verificador read-only; (4) Nunca enviar dados; (5) Higiene de permissões/MCP; (6) "Se pode ler, assuma que vai" (reprise); (7) Checklist de auditoria. Desenho: verificador isolando o `.md`. Exemplo `.md`: `repo-verifier` read-only. Prompts: "audite este .md em busca de injeção". Exercício: auditar um `.md` suspeito (checklist). Commit `feat(t4): M4.4 segurança`.

### Task 4.5 (⚙️ Sonnet) `modulo-4-5.html` — "Bibliotecas e reuso"
Tópicos: (1) `awesome-claude-code-subagents`; (2) Copiar > criar; (3) Baixar→`.claude/agents`→adaptar; (4) Categorias (API/backend/language specialists); (5) Cuidados (injection); (6) Verificar antes; (7) Contribuir de volta. Desenho: catálogo → seu projeto. Exemplo `.md`: adaptar um agente da comunidade. Prompts: "adapte este agente da comunidade ao meu stack". Exercício: importar e adaptar 1 agente. Commit `feat(t4): M4.5 bibliotecas`.

### Task 4.6 (⚙️ Sonnet) `modulo-4-6.html` — "Montando seu sistema"
Tópicos: (1) Sua biblioteca de especialistas; (2) Pacote inicial (security-auditor, test-runner, doc-writer, db-expert, plan-roaster); (3) Templates prontos; (4) Checklist de produção; (5) Projeto×global na prática; (6) **Recap geral** (resumo "1 tela", slide 18); (7) Próximos passos. Desenho: fábrica de especialistas. Exemplo `.md`: pacote de 5 agentes. Prompts: "gere minha suíte inicial de subagentes". Exercício: **projeto final** — desenhar e criar a própria suíte. Commit `feat(t4): M4.6 montando seu sistema`.

### Fim Fase 4: `revisar-curso` na Trilha 4.

---

## Fase 5 — Telas Remotion + revisão final + publicação

### Task 5.1 (Sonnet) Cenas Remotion → PNG
**Files:** `_build/remotion/*`, exporta para `curso/trilhaN/assets/img/*.png`
Cenas: (a) 5 subagentes em paralelo (T1) → `trilha1/assets/img/parallel.png`; (b) wizard `/agents` (T2) → `trilha2/assets/img/agents-wizard.png`; (c) plan-roaster → resumo limpo (T3) → `trilha3/assets/img/plan-roaster.png`; (d) fan-out de workflow (T3/T4) → `trilha3/assets/img/workflow-fanout.png`. Substituir os monitor-mock correspondentes por `<img>` onde melhorar. Commit `feat(assets): cenas Remotion`.

### Task 5.2 (Opus) Revisão final global
- [ ] Rodar `revisar-curso` no curso inteiro; corrigir achados.
- [ ] Conferir navegação entre todas as páginas (links anterior/próximo, cards, modais).
- [ ] Commit das correções.

### Task 5.3 Publicação
- [ ] `git remote add origin …inematds/subagentes`; push.
- [ ] Habilitar GitHub Pages → confirmar `inematds.github.io/subagentes/`.
- [ ] Registrar no portal (`/home/nmaldaner/projetos/portal`): `src/data/courses.ts` (`platformsData` alfabético + `updatesData` topo) + trilhas em `Portal.tsx` e `PortalV2.tsx` se aplicável. Commit + push (deploy Vercel).
- [ ] **Confirmar com o usuário** slug do repo e trilha-alvo no portal antes do push (itens abertos do spec §12).

---

## Self-Review (writing-plans)

- **Cobertura do spec:** §3 (24 módulos) → uma task cada ✓; §4 molde → P-MOD ✓; §5 desenhos/telas → SVG no molde + Fase 5 Remotion ✓; §6 tabelas canônicas → M1.3/M3.1/M3.2/M1.5 ✓; §7 formato → Convenções/Cores ✓; §8 publicação → Task 5.3 ✓; §9 fases/gate → Fase 0 + GATE ✓.
- **Placeholders:** sem TODO/TBD; cada módulo tem brief concreto (tópicos+desenho+exemplo+exercício); a expansão para HTML é a entrega verificada do subagente via P-MOD. "Tela —/Exemplo —" indicam ausência intencional, não pendência.
- **Consistência:** nomes de arquivo `modulo-X-Y.html`, cores por trilha, e referências cruzadas (M2.1→M2.2, M3.1→§6.2) batem.

---

## Execution Handoff

Modo escolhido pelo usuário: **execução via subagentes** (subagent-driven). Política de modelo por fase acima. Sequência: Fase 0 (Opus) → **GATE** → Fases 1–5 com modelo por task.
