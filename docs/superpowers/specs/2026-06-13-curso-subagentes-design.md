# Curso "Subagentes" — Design Doc / Spec

- **Data:** 2026-06-13
- **Status:** Aprovado para planejamento (aguardando review final do usuário)
- **Formato:** INEMA.CLUB (skill `formato-curso`)
- **Hospedagem:** repo `subagentes` → `inematds.github.io/subagentes/` → registrar no portal `inema.club`
- **Idioma:** PT-BR
- **Fonte de conteúdo:** transcrição do vídeo + apresentação "Claude Code Subagentes" (18 slides), expandidas com profundidade própria.

---

## 1. Visão geral e objetivos

Curso completo, profundo e **único** sobre **subagentes** no Claude Code (e ambientes correlatos como Codex), cobrindo também **skills** e **agentes built-in** para que o aluno entenda as três primitivas e saiba **quando usar cada uma**.

**Objetivo central:** levar a pessoa de "nunca criei um subagente" até "monto e orquestro meu próprio sistema de especialistas, escolhendo modelo por tarefa e economizando dinheiro", passando por um módulo de referência avançada.

**Promessa de qualidade (o que torna o curso "único"):** cada módulo entrega, além da teoria:
- **≥1 desenho** (SVG futurista conceitual no estilo INEMA);
- **Exemplo real** — um arquivo `.md` de subagente completo e comentado;
- **Prompts prontos** — copy-paste pra criar/invocar/iterar;
- **Tela simulada** — SVG monitor-mock (ou render Remotion) do fluxo no terminal/UI;
- **Exercício** prático com critério de verificação.

**Não-objetivos (YAGNI):** ver §11.

---

## 2. Público e resultados de aprendizagem

**Público:** trilhas 1–3 = iniciante → praticante (perfil B). Trilha 4 = referência avançada (perfil C), para quem já usa Claude Code no dia a dia.

**Ao final, o aluno consegue:**
1. Explicar com clareza a diferença entre **agente, subagente e skill** e escolher a primitiva certa.
2. Articular o **motivo real** dos subagentes (proteção de contexto) e aplicar a "pergunta que decide".
3. **Criar** um subagente custom do zero: frontmatter + corpo, com descrição que dispara sozinha.
4. **Restringir ferramentas** (read-only) e escolher **projeto × global**.
5. **Casar modelo com tarefa** (Haiku/Sonnet/Opus) e montar o padrão "chefe inteligente + trabalhadores baratos" pra economizar.
6. **Orquestrar** encadeamentos, revisor imparcial e workflows dinâmicos — com noção de custo.
7. Levar o conceito pra **outros ambientes/modelos** (Codex, cross-runtime) e montar a própria biblioteca de especialistas com segurança.

---

## 3. Arquitetura do curso (4 trilhas × 6 módulos = 24 páginas + landing)

| Trilha | Cor INEMA | Tema | Frase-âncora |
|---|---|---|---|
| **T1 Fundamentos** | Emerald | Modelo mental + comparação central | "Entender antes de criar" |
| **T2 Criando na prática** | Blue | Construir o `.md` que dispara sozinho | "Do `.md` ao especialista" |
| **T3 Modelo, custo e orquestração** | Amber | Modelo por tarefa, economia, orquestração | "Chefe inteligente, trabalhadores baratos" |
| **T4 Avançado & cross-ambiente** | Purple | Escala, Codex/outros modelos, segurança | "Além do Claude Code" |

### Trilha 1 — Fundamentos (Emerald)

- **M1.1 — O que é um subagente.** Especialista que você delega; chat = maestro; o relatório volta limpo. _Desenho:_ fan-out maestro→subagentes→resumo. _Exemplo:_ `code-reviewer.md` mínimo. _Exercício:_ identificar 3 tarefas do dia que seriam subagente.
- **M1.2 — O verdadeiro motivo: contexto limpo.** Janela de contexto, poluição, "fazer você mesmo × enviar subagente". _Desenho:_ duas janelas (acúmulo × resumo). _Exemplo:_ pesquisa de produto delegada. _Exercício:_ estimar tokens poupados num caso.
- **M1.3 — Agente × Subagente × Skill (comparação central).** Contexto próprio vs compartilhado, paralelismo, modelo próprio, quem chama quem. _Desenho:_ tabela viva + SVG de 3 caixas. _Exemplo:_ o mesmo job feito como skill vs subagente. _Exercício:_ classificar 6 cenários.
- **M1.4 — Built-in × Custom.** Explore/Plan/general-purpose vs os que você cria; **progressive disclosure** (só lê name+description até decidir). _Desenho:_ leitura do frontmatter → decisão. _Exemplo:_ invocação automática do general-purpose. _Exercício:_ achar built-ins disponíveis com `/agents`.
- **M1.5 — A pergunta que decide.** "Isso vai despejar coisa que nunca vou reler?"; sinais de usar/evitar. _Desenho:_ fluxo de decisão sim/não. _Exemplo:_ 2 prompts (um vira subagente, outro não). _Exercício:_ aplicar a pergunta a 5 tarefas próprias.
- **M1.6 — Anatomia de um subagente.** O arquivo `.md`: frontmatter (config) + corpo (cérebro). Panorama, aprofundado na T2. _Desenho:_ monitor-mock do arquivo com as duas zonas destacadas. _Exemplo:_ `.md` anatomizado. _Exercício:_ rotular as partes de um `.md` dado.

### Trilha 2 — Criando na prática (Blue)

- **M2.1 — O frontmatter completo.** `name`, `description`, `tools`, `model`, `color`, `memory` (+ ponteiros pra disallowed-tools, MCP). _Desenho:_ frontmatter rotulado. _Exemplo:_ frontmatter completo comentado. _Exercício:_ escrever um frontmatter válido.
- **M2.2 — A descrição é o gatilho.** Fraca × forte; "use proactively"; misfires (disparo a mais/a menos); como iterar. _Desenho:_ duas descrições → roteamento. _Exemplo:_ antes/depois de uma description. _Exercício:_ reescrever uma description fraca.
- **M2.3 — O corpo é o cérebro.** Papel → passos numerados → formato de saída → o que **não** fazer; **um agente, uma tarefa** (o teste do "e também"). _Desenho:_ escada papel→passos→saída. _Exemplo:_ corpo de `security-reviewer`. _Exercício:_ dividir um agente "faz-tudo" em dois.
- **M2.4 — Ferramentas e permissões.** Read-only; allowed/disallowed; MCP; **permissão real > prompt "não faça"**; "se pode ler dados, assuma que vai". _Desenho:_ camada de permissão vs prompt. _Exemplo:_ `code-reviewer` só com Read/Grep/Glob. _Exercício:_ travar um agente como read-only.
- **M2.5 — Projeto × Global.** `.claude/agents/` (repo, compartilha via git) vs `~/.claude/agents/` (só você, todos os projetos); mover é só mover o `.md`. _Desenho:_ duas pastas. _Exemplo:_ mesmo agente nos dois escopos. _Exercício:_ decidir escopo de 4 agentes.
- **M2.6 — Criando com `/agents` + como o Claude escolhe.** generate-with-Claude × manual; caso real **plan-roaster** (incl. o bug do frontmatter não fechado e a colisão skill×agente); seleção automático/proativo/`@nome`/`--agent`. _Desenho:_ wizard do `/agents` (monitor-mock/Remotion). _Exemplo:_ `plan-roaster.md` enxuto. _Exercício:_ criar e invocar um agente por nome.

### Trilha 3 — Modelo, custo e orquestração (Amber)

- **M3.1 — Modelo por tarefa.** Haiku/Sonnet/Opus: raciocínio × custo × latência; a armadilha do subagente que começa em branco e precisa reunir contexto. _Desenho:_ escada de modelos. _Exemplo:_ 3 agentes, 3 modelos justificados. _Exercício:_ atribuir modelo a 6 tarefas.
- **M3.2 — A economia real.** Chefe Opus/Sonnet + frota Haiku; `CLAUDE_CODE_SUBAGENT_MODEL`; `maxTurns`; multiagente ~15× um chat simples. _Desenho:_ chefe + trabalhadores. _Exemplo:_ env var + maxTurns num agente. _Exercício:_ desenhar a divisão chefe/trabalhador de um job.
- **M3.3 — Encadeando subagentes.** Maestro coordena A→B→C; **skills↔agents** se chamam, **agents não chamam agents**; relação 1-para-1 (subagentes não conversam entre si). _Desenho:_ cadeia + setas proibidas. _Exemplo:_ skill que dispara subagentes. _Exercício:_ modelar uma sequência de 3 passos.
- **M3.4 — O revisor imparcial.** Contexto fresco = veredito honesto; **quem constrói escreve, quem valida verifica**; aplicar a código, plano e pesquisa. _Desenho:_ builder × reviewer (caixa branca). _Exemplo:_ `plan-roaster`/`code-reviewer` como validador. _Exercício:_ rodar um revisor sobre o próprio M3 anterior.
- **M3.5 — Workflows dinâmicos.** Escala (dezenas/centenas de subagentes); gatilho `ultracode` (era "workflow"); custo/limite de sessão; quando vale. _Desenho:_ fan-out grande. _Exemplo:_ caso "41 subagentes". _Exercício:_ decidir se um job pede workflow (sim/não justificado).
- **M3.6 — Padrões de orquestração.** Paralelo independente, pipeline, fan-out; subagente × **agent teams** (que conversam e custam mais); regra prática "10+ arquivos ou parede de output". _Desenho:_ 3 topologias lado a lado. _Exemplo:_ tabela de padrão→caso. _Exercício:_ escolher topologia pra 3 problemas.

### Trilha 4 — Avançado & cross-ambiente (Purple)

- **M4.1 — Arquitetura multiagente em escala.** Frota, loop-until-dry, verificação adversarial (refutar), completeness critic, sem cap silencioso. _Desenho:_ pipeline find→verify→synthesize. _Exemplo:_ esqueleto de harness. _Exercício:_ desenhar um loop-until-dry.
- **M4.2 — Subagentes no Codex e outros ambientes.** Como o conceito mapeia fora do Claude Code (Codex CLI); **cross-runtime** (polyskill) — uma fonte, vários ambientes; equivalências de ferramentas. _Desenho:_ mesma `.md` → 2 runtimes. _Exemplo:_ skill portável. _Exercício:_ apontar o equivalente de 1 conceito no Codex.
- **M4.3 — Comparando modelos por papel.** Opus/Sonnet/Haiku × GPT/Codex × outros; raciocínio profundo × barato/rápido; quando trocar de modelo/provedor por papel do agente. _Desenho:_ matriz papel×modelo. _Exemplo:_ frota heterogênea. _Exercício:_ montar a matriz pro próprio caso.
- **M4.4 — Segurança.** Baixar `.md` de terceiros; **prompt injection**; subagente verificador read-only que nunca envia dados; higiene de permissões/MCP. _Desenho:_ verificador isolando o `.md`. _Exemplo:_ `repo-verifier` read-only. _Exercício:_ auditar um `.md` suspeito (checklist).
- **M4.5 — Bibliotecas e reuso.** `awesome-claude-code-subagents` e afins; **copiar > criar**; baixar `.md`, colocar em `.claude/agents/`, adaptar; cuidados. _Desenho:_ catálogo → seu projeto. _Exemplo:_ adaptar um agente da comunidade. _Exercício:_ importar e adaptar 1 agente.
- **M4.6 — Montando seu sistema.** Sua biblioteca de especialistas (security-auditor, test-runner, doc-writer, db-expert…); templates prontos; checklist de produção; recap geral. _Desenho:_ "fábrica" de especialistas. _Exemplo:_ pacote inicial de 5 agentes. _Exercício:_ projeto final — desenhar e criar a própria suíte.

---

## 4. O molde do módulo (padrão de conteúdo repetível)

Toda página de módulo segue esta ordem (respeitando os erros críticos do `formato-curso`):

1. **Nav completo** — Logo + INEMA.CLUB (`text-sky-400`) + 4 botões de trilha + theme toggle; trilha ativa destacada.
2. **Header** — título `text-2xl font-bold`, subtítulo, duração estimada, badge da trilha.
3. **Abertura + SVG conceitual** — 1 parágrafo de gancho + **1 diagrama SVG futurista** (cor da trilha + ciano `#38bdf8`, glow, `role="img"`+`aria-label`, animação sob `prefers-reduced-motion`).
4. **`<h2>Conteúdo detalhado</h2>** (simples, sem divisor ornado).
5. **≥6 tópicos** — cada um com as 3 seções "O que é / Por que aprender / Conceitos-chave" + **variedade** de componentes (≥2 grids ✓/✗, ≥1 timeline, ≥2 tip boxes, ≥1 code box). Indicador = número em círculo (nunca seta).
6. **Bloco Exemplo real** — arquivo `.md` de subagente completo, em code box, comentado.
7. **Bloco Prompts prontos** — 2–3 prompts copy-paste (criar via `/agents`, invocar, iterar descrição).
8. **Bloco Tela simulada** — SVG monitor-mock (ou PNG Remotion) do fluxo correspondente.
9. **Bloco Exercício** — tarefa prática + critério de verificação ("como saber que acertou").
10. **Navegação anterior/próximo** + reforço do link INEMA.CLUB.

Profundidade-alvo: **500–800 linhas**, 6–8 seções, sem template repetido.

**Index de cada trilha:** hero SVG + cabeçalho + **"Mapa da trilha"** (grid de cards-âncora, `justify-between` com X.Y + duração, emoji no título, subtítulo punchy) + cards de módulo com botão "Ver Completo" + modal via `<iframe>`.

---

## 5. Estratégia visual — desenhos e telas simuladas

**Princípio:** zero captura real. Tudo é recriação on-brand.

### 5.1 SVG futurista (desenhos conceituais)
Biblioteca `formato-curso/references/SVG-FUTURISTA.md`. Mapeamento dos slides da apresentação → diagrama:

| Conceito (slide) | Diagrama SVG | Onde |
|---|---|---|
| Chat → Subagente → Resumo (s2) | fan-out / fluxo | M1.1 |
| Contexto acumula × limpo (s3) | duas janelas | M1.2 |
| Built-in × Custom (s4) | bifurcação | M1.4 |
| Anatomia do `.md` (s5/s6) | monitor-mock + rótulos | M1.6 / M2.1 |
| Descrição fraca × gatilho (s7) | fluxo de decisão | M2.2 |
| Projeto × Global (s8) | duas pastas | M2.5 |
| Especialistas (s9) | grade de papéis | M4.6 |
| Como o Claude escolhe (s10) | fluxo de decisão | M2.6 |
| Ferramentas/read-only (s11) | camadas de permissão | M2.4 |
| Tiers de modelo (s12) | escada Haiku→Sonnet→Opus | M3.1 |
| Chefe + trabalhadores (s13) | fan-out hierárquico | M3.2 |
| A pergunta que decide (s14) | fluxo sim/não | M1.5 |
| Sinais usar/evitar (s15) | duas colunas | M1.5 |
| Encadeamento/maestro (s16) | cadeia A→B→C | M3.3 |
| Revisor imparcial (s17) | builder × reviewer | M3.4 |
| Resumo em 1 tela (s18) | painel-resumo | landing / M4.6 |

### 5.2 Monitor-mock (telas estáticas, padrão)
SVG inline recriando a janela: prompt, status line (tokens/%), lista do `/agents`, subagentes em paralelo, cor do agente custom. Segue tema claro/escuro. Usado na maioria dos módulos (bloco "Tela simulada").

### 5.3 Remotion (momentos-chave, render → PNG; MP4 opcional)
Projeto React em `_build/remotion/` (fora do site publicado). Renderiza frames → PNG exportados pra `curso/trilhaN/assets/img/`. Cenas marquee:
- 5 subagentes disparando em paralelo (T1).
- Wizard do `/agents` passo a passo (T2).
- plan-roaster invocado → resumo limpo de volta (T3).
- Fan-out de workflow dinâmico (T3/T4).

MP4 curto só pra 1–2 heros por trilha, **se** o usuário pedir; default = PNG.

> Geração de imagens temáticas (hero/banner) é **opcional** via `inemaimg`/`flux2-klein`, nunca substitui o SVG inline obrigatório (erro crítico #17).

---

## 6. Conteúdo canônico de referência (consistência entre módulos)

### 6.1 Tabela Agente × Subagente × Skill (espinha da T1, M1.3)
Eixos: contexto (próprio/limpo vs compartilhado), paralelismo (sim/não), modelo próprio (sim/herda), quem invoca quem (skill↔agent; agent **não** chama agent), onde mora (`.md`), quando preferir cada um.

### 6.2 Tabela Modelo × Raciocínio × Custo × Quando usar (espinha da T3, M3.1; reforço T4 M4.3)
- **Haiku** — barato/rápido — escanear, buscar, resumir, ler docs.
- **Sonnet** — médio — construir, revisar.
- **Opus** — caro — raciocínio profundo, segurança, arquitetura.
- Padrão de economia: **chefe inteligente (Opus/Sonnet) + frota barata (Haiku)**; `CLAUDE_CODE_SUBAGENT_MODEL` força barato; multiagente ~15×.

### 6.3 Sinais usar/evitar (M1.5)
**Usar:** muitos arquivos, parede de output, tarefa repetida, jobs independentes/paralelos, revisor imparcial. **Evitar:** edição rápida, passos dependentes, agentes que precisariam conversar, precisa da conversa inteira, precisa te fazer pergunta. Regra: **10+ arquivos ou output que você nunca vai reler = subagente**; escala → workflow dinâmico.

### 6.4 Cobertura Codex/cross-ambiente (T4)
Mapear o conceito de subagente/skill pro Codex e o princípio cross-runtime (uma fonte `.md` → vários ambientes via polyskill); comparar modelos/provedores por papel. Tratar como conceitual + exemplos, não como tutorial de instalação de cada runtime.

---

## 7. Formato técnico (INEMA `formato-curso`)

- **Cores:** T1 Emerald, T2 Blue, T3 Amber, T4 Purple. Especiais: primary `#FACC15`, sky (INEMA.CLUB), red (erros), ciano `#38bdf8` (secundária dos SVGs).
- **Obrigatórios por página:** nav completo, INEMA.CLUB, light/dark com CSS completo, theme toggle, bordas suavizadas no dark (2 regras), botões `justify-start`.
- **Estrutura de arquivos:**

```
subagentes/
├── index.html                       # Landing do curso (hero + 4 trilhas + resumo s18)
├── docs/superpowers/specs/          # este spec + plano
├── _build/remotion/                 # projeto Remotion (NÃO publicado)
└── curso/
    ├── trilha1/
    │   ├── index.html               # Index T1 (hero SVG + Mapa da trilha + cards)
    │   ├── modulo-1-1.html … modulo-1-6.html
    │   └── assets/img/              # PNGs (Remotion / inemaimg)
    ├── trilha2/  (modulo-2-1 … 2-6)
    ├── trilha3/  (modulo-3-1 … 3-6)
    └── trilha4/  (modulo-4-1 … 4-6)
```

- **Verificação:** rodar o `CHECKLIST_REVISAO.md` do `formato-curso` (e/ou skill `revisar-curso`) antes de entregar cada página.

---

## 8. Publicação

1. Construir e validar localmente (abrir os HTML).
2. Repo git `subagentes` → push pro GitHub `inematds/subagentes` → habilitar **GitHub Pages** → `inematds.github.io/subagentes/`.
3. Registrar no **portal** (`/home/nmaldaner/projetos/portal`): `src/data/courses.ts` (`platformsData` em ordem alfabética, `updatesData` no topo) + lista de trilhas hardcoded em `Portal.tsx` **e** `PortalV2.tsx` se entrar numa trilha do portal (ex.: "Claude Code"). Commit + push (deploy automático Vercel).

---

## 9. Plano de construção (trilha por trilha, com gate)

1. **Fase 0 — Fundações:** landing `index.html` + `curso/trilha1/index.html` + 1 módulo-piloto **M1.1** completo (define o molde visual real). Setup do `_build/remotion/`.
2. **Gate de validação:** usuário revisa M1.1 (densidade, desenhos, exemplo, exercício, telas). Ajustes viram o molde aprovado.
3. **Fase 1:** completar T1 (M1.2–M1.6) + index.
4. **Fase 2–4:** T2, T3, T4 (cada uma: index + 6 módulos), reaproveitando o molde.
5. **Fase 5 — Telas Remotion** marquee + revisão final (`revisar-curso`) + publicação + registro no portal.

Cada fase = checkpoint de review.

---

## 10. Definição de pronto / critérios de sucesso

- 24 módulos + 4 índices + landing, todos passando no `CHECKLIST_REVISAO.md`.
- Cada módulo: ≥6 tópicos, ≥1 SVG, 1 exemplo `.md`, ≥2 prompts prontos, 1 tela simulada, 1 exercício com critério.
- Comparações canônicas (§6.1, §6.2) presentes e consistentes entre módulos.
- T4 cobre Codex/cross-runtime e comparação de modelos por papel.
- Tema claro/escuro e nav completos em todas as páginas.
- Publicado em `inematds.github.io/subagentes/` e registrado no portal.

---

## 11. Fora de escopo (YAGNI)

- Captura de tela real de produtos.
- Tutorial de instalação passo a passo de cada runtime/modelo de terceiros.
- Backend, login, quiz interativo com estado, LMS.
- Tradução pra outros idiomas (só PT-BR agora).
- MP4 em todos os módulos (só heros, e só se pedido).

---

## 12. Riscos e questões abertas

- **Volume (24 módulos):** mitigado pela construção faseada com molde aprovado no gate.
- **Consistência visual entre 24 páginas:** mitigada pelo molde (§4) + checklist + `revisar-curso`.
- **Fidelidade das telas simuladas:** SVG/Remotion aproximam a UI real; risco baixo de "parecer datado" se a UI do Claude Code mudar — manter genérico o suficiente.
- **Aberto:** nome final/slug do repo (`subagentes`?), e se o curso entra numa trilha existente do portal (ex.: "Claude Code") — confirmar na fase de publicação.
