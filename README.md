

## PROMPT PARA TRAE — REBUILD DO SITE **IT BOOSTER GLOBAL**

Você é um **Tech Lead/Implementador**. Execute este projeto ponta-a-ponta seguindo as etapas abaixo. Avance com defaults quando faltarem credenciais, documente suposições e entregue artefatos prontos para deploy na **Vercel**.
Perfeito. Aqui vai um bloco para você colar no **Trae** (pode anexar ao prompt que já te passei). Ele força **autocommit** após cada alteração, com **Conventional Commits**, **auto-confirmação** (sem prompts) e, se existir remoto, **autopush** e **PR automático**.

---

## 0-BIS) Política de Git, Commits e PR — **AUTOCOMMIT OBRIGATÓRIO**

### Regras gerais (sempre)

* **Após QUALQUER alteração em arquivo** (criar/editar/mover/remover), execute:

  1. `git add -A`
  2. `git commit -m "<mensagem convencional>"`
  3. Se houver remoto configurado: `git push -u origin <branch>`
* **Sem prompts manuais**: assuma “sim” para confirmações e use flags não interativas quando aplicável (`-y`, `--yes`, `--force` quando seguro).
* **Commits atômicos por passo lógico**: ao finalizar cada sub-tarefa da fase atual, faça um commit.
* **Branch padrão**: `main`. Para features: `feat/<slug>`; para correções: `fix/<slug>`; para docs: `docs/<slug>`.
* **PR automático (se possível)**: se `gh` estiver disponível e houver remoto GitHub:

  * `gh pr create --fill --base main --head <branch> --title "<título>" --body "<resumo>" --label "auto"`
  * Em seguida, poste o link do PR no retorno da fase.
* Se não houver remoto/gh: apenas **documente** no output que o push/PR foi pulado.

### Conventional Commits (com sinônimos em MAIÚSCULO)

* Aceite tipos em **maiúsculo** e **minúsculo**; normalize para minúsculo no commit.
* Mapeamento:

  * `FEAT` → `feat:` (nova funcionalidade)
  * `FIX`, `BUG`, `BU`, `BUGFIX` → `fix:` (correção)
  * `DOC`, `DOCS` → `docs:` (documentação)
  * `STYLE` → `style:` (formatação; sem mudança de lógica)
  * `REFACTOR`, `REF` → `refactor:` (refatoração)
  * `PERF` → `perf:` (performance)
  * `TEST` → `test:` (testes)
  * `BUILD` → `build:` (build, deps, bundlers)
  * `CI` → `ci:` (pipelines, actions)
  * `CHORE` → `chore:` (rotinas, scripts, manutenção)
  * `REVERT` → `revert:` (reversão de commit)
* **Formato**: `type(scope)!: resumo conciso em até 72 caracteres`

  * `scope` exemplo: `home`, `sectors`, `blog`, `cases`, `about`, `contact`, `demo`, `infra`, `seo`, `a11y`, `styles`, `email`, `og`, `cms`, `routing`.
  * Use `!` quando houver breaking change (e descreva no corpo).
* **Corpo e rodapé (quando houver)**:

  * Corpo: o “porquê” da mudança, decisões e trade-offs.
  * Rodapé: `Refs: #<issue>` / `BREAKING CHANGE: ...`

### Mensagem de commit — exemplos prontos

* `feat(home): adiciona hero com CTA e seletor de setor`
* `fix(contact): corrige validação zod e mensagem de sucesso`
* `docs(readme): instruções de deploy na vercel`
* `refactor(components): extrai Section e CTA para pasta common`
* `perf(images): ativa next/image e lazy loading`
* `chore(ci): adiciona verificação de typecheck no pipeline`

### Fluxo por alteração (algoritmo)

1. Detecte o **tipo** da mudança (conforme lista acima). Se não identificar, use `chore:` e explique no corpo.
2. Defina **scope** pelo diretório/rota principal afetado.
3. Gere **resumo** objetivo (≤72 chars).
4. Execute `git add -A && git commit -m "<type(scope): resumo>"`.
5. Se branch atual for `main` e a mudança for grande, crie branch de feature:

   * `git checkout -b feat/<slug>` antes do commit; após commit: `git push -u origin feat/<slug>` e abra PR.
6. Se houver conflitos ou rebase necessário, faça `git pull --rebase` e repita o commit/push.

### Inicialização automática de git (se ainda não houver)

```bash
git init -b main
git config user.name "inael"
git config user.email "inael.rodrigues@gmail.com"
git config commit.gpgsign false
git config pull.rebase false
```

### Enforcements locais (opcional, crie se possível)

1. **Husky + Commitlint + Lint-staged**

   * `npm i -D husky @commitlint/{config-conventional,cli} lint-staged`
   * `npx husky install`
   * `.husky/commit-msg` → `npx --no commitlint --edit "$1"`
   * `.commitlintrc.cjs`:

     ```js
     export default { extends: ['@commitlint/config-conventional'] };
     ```
   * `package.json` (trechos):

     ```json
     {
       "lint-staged": { "*.{ts,tsx,md,json,css}": "prettier --write" },
       "scripts": {
         "prepare": "husky install",
         "lint": "next lint",
         "typecheck": "tsc --noEmit"
       }
     }
     ```
2. **Semantic-release** (opcional para versionar e changelog):

   * `npm i -D semantic-release @semantic-release/{changelog,git,github,npm}`
   * Config padrão com branches `main` e `feat/*`.

### Saída esperada do Trae após cada passo

* **Resumo** do que mudou (1–3 linhas).
* **Lista de arquivos** tocados (árvore curtinha).
* **Mensagem de commit usada** (linha completa).
* **Resultado de push/PR** (URL, se houver).
* **Checklist** de verificação manual (1–5 itens).

---

> **IMPORTANTE**: A partir de agora, **SEMPRE** faça commit após qualquer modificação e **nunca** espere confirmação manual. Se não puder concluir push/PR por falta de remoto/credenciais, **continue** e apenas registre essa limitação no output.

### 0) Regras de Operação do Trae (obrigatórias)

* **Commits:** Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`).
* **Ao concluir cada etapa:**

  1. Liste arquivos criados/alterados,
  2. Mostre um diff/resumo,
  3. Informe comandos para rodar e validar,
  4. Dê checklist de testes manuais.
* **Qualidade mínima:** Lighthouse ≥ 95 em Performance/SEO/A11y/Best Practices; LCP < 2.5s; CLS < 0.1; INP < 200ms.
* **A11y:** ARIA, foco visível, contraste AA.
* **Sem segredos em git:** usar `.env.local` e variáveis na Vercel.

---

## 1) Contexto e Identidade

**Empresa:** IT Booster Global
**Slogan:** “Transformamos pequenos negócios em grandes sucessos digitais”
**Missão:** Desenvolver soluções personalizadas de micro-SaaS para clínicas, barbearias, autônomos e pequenas empresas que precisam de transformação digital.
**Endereço:** CLN 201 Bloco B – Loja 59 Subsolo, CEP 70832-520, Asa Norte, Brasília/DF – Brasil.

**Tom e proposta:** foco em transformação digital para PMEs, com ROI claro, rapidez de implantação e suporte humanizado.

---

## 2) Stack e Padrões

* **Framework:** Next.js 14+ (App Router, Server Components) + TypeScript
* **UI:** Tailwind CSS + shadcn/ui + Framer Motion + Lucide React
* **Forms:** React Hook Form + Zod
* **i18n:** next-intl (pt-BR default; en opcional pronto)
* **Conteúdo:** **MDX via Contentlayer** (padrão sem credenciais). Preparar opção de CMS headless (Sanity/Contentful) mas deixar off por padrão.
* **SEO:** next-seo, sitemap, robots, OG images (rota `/api/og`)
* **E-mail:** Resend (fallback console em dev)
* **Analytics:** Vercel Analytics + GA4
* **Agendamentos:** embed Cal.com/Calendly
* **Chat:** widget (Crisp/Intercom) por script
* **Deploy:** Vercel (previews por PR)
* **DB (opcional):** Vercel KV/Postgres apenas se necessário; por padrão **não usar** DB.

**Scripts npm:** `dev`, `build`, `start`, `lint`, `typecheck`, `analyze` (bundle analyzer opcional).

**Variáveis (.env.example):**

```
RESEND_API_KEY=
NEXT_PUBLIC_VERCEL_ANALYTICS_ID=
# GA4 opcional
NEXT_PUBLIC_GA_ID=
# CMS (se ativar futuramente)
SANITY_PROJECT_ID=
SANITY_DATASET=
CONTENTFUL_SPACE_ID=
CONTENTFUL_ACCESS_TOKEN=
```

---

## 3) Arquitetura e Estrutura de Pastas

```
/app
  /(marketing)/
    page.tsx                 # Home
    solutions/page.tsx
    sectors/page.tsx
    sectors/clinicas-consultorios/page.tsx
    sectors/saloes-barbearias/page.tsx
    sectors/oficinas-assistencias/page.tsx
    sectors/profissionais-autonomos/page.tsx
    sectors/varejo-local/page.tsx
    cases/page.tsx
    about/page.tsx
    contact/page.tsx
    demo/page.tsx
    blog/page.tsx
    blog/[slug]/page.tsx
    blog/categoria/[category]/page.tsx
    blog/tag/[tag]/page.tsx
    blog/autor/[author]/page.tsx
  /api/contact/route.ts      # envio Resend + rate limit/honeypot
  /api/og/route.ts           # OG dinâmico
/components
  ui/* (shadcn)
  common/* (Header, Footer, Nav, Section, CTA, Badge, Card, Testimonial, KPI, Pricing, ROIForm)
/content
  /blog/*.mdx
  /cases/*.mdx
/lib (seo, i18n, analytics, validators, email, rate-limit)
/styles (globals.css, tailwind.css)
/public (logos, og-templates, favicon)
/config (contentlayer, next-seo)
/scripts (sitemap, seeds)
/types
```

---

## 4) Conteúdo e Páginas (com base no seu artefato)

**Menu:** Home, Soluções, Setores, Cases de Sucesso, Sobre Nós, Contato | CTAs: “Solicitar Orçamento”, “Agendar Demonstração”.

### 4.1 Home (Hero + Provas + CTAs)

* Título: “Seu negócio merece uma solução digital sob medida”.
* Subtítulo: micro-SaaS personalizados para clínicas, barbearias, consultórios, oficinas e pequenos negócios.
* Blocos: “Problemas que resolvemos”, “Nossas soluções”, “Nossa metodologia em 5 etapas”, “Diferenciais”, “Comparativo de modelos”, “Depoimentos”, CTA final.

### 4.2 Soluções

Listar soluções por setor com cards e links para cada setor.

### 4.3 Setores (páginas dedicadas)

* **Clínicas e Consultórios:** Agendamento, Prontuário, Financeiro, Teleconsulta, Convênios.
* **Salões e Barbearias:** Agenda, Catálogo, Fidelidade, Estoque, App cliente.
* **Oficinas e Assistências:** OS digital, Peças/Estoque, Orçamentos, Histórico, Portal do Cliente.
* **Profissionais Autônomos:** CRM, Projetos, Propostas, Tarefas, Financeiro.
* **Varejo Local:** PDV, E-commerce, Estoque, Fidelidade, Analytics.

### 4.4 Metodologia (5 etapas, “30 dias”)

Diagnóstico → Proposta → Desenvolvimento Ágil → Implementação/Treinamento → Go Live/Suporte.

### 4.5 Cases de Sucesso

Criar 3 cases em MDX (Clínica Dr. Silva, Barbearia Modern Cut, Oficina TurboMax) com KPIs do artefato.

### 4.6 Diferenciais

Especialização em PMEs, Tecnologia nacional, Implementação rápida, Suporte humanizado, Preço justo, Integração total.

### 4.7 Modelos de Contratação

* **Pagamento Único:** Essencial (R\$ 8.500), Completo (R\$ 15.900), Premium (R\$ 25.900).
* **Assinatura Mensal (SaaS):** Starter (R\$ 197), Growth (R\$ 397), Pro (R\$ 697).
  Incluir tabela comparativa do artefato e observações. (Valores como **exemplo**; isolar em JSON para fácil edição.)

### 4.8 Sobre Nós

Missão, visão, endereço, cultura e CTA para contato.

### 4.9 Contato

Formulário com RHF + Zod, honeypot e rate-limit simples; envio via Resend; mensagem de sucesso/erro.

### 4.10 Agendar Demo

Página com embed Cal.com/Calendly e CTA WhatsApp.

### 4.11 Blog completo (MDX + Contentlayer)

* `/blog`, `/blog/[slug]`, `/blog/categoria/[categoria]`, `/blog/tag/[tag]`, `/blog/autor/[autor]`, `/blog/sitemap.xml`.
* 3 posts demo (transformação digital PME, agendamentos inteligentes, fidelidade e recorrência).

---

## 5) Componentes Especiais

1. **Hero com demonstração interativa** (simular wizard de “descobrir minha solução”).
2. **Calculadora de ROI** (formulário que estima ROI com base em aumento de agendamentos, ticket médio, margem e custo do plano).
3. **Seletor de Setor** (cards filtráveis).
4. **Carousel de cases** (KPIs destacados).
5. **Formulário de orçamento inteligente** (passo-a-passo).
6. **Comparativo de planos** (tabela + destaques).
7. **Agenda demo** (embed).
8. **Chat WhatsApp** (link/QR).
9. **Testimonials** (vídeo/texto).
10. **FAQ por setor** (accordions).

---

## 6) SEO e OG

* `next-seo` configurado global + por página.
* `schema.org`: Organization, Service, WebSite, WebPage, BlogPosting.
* OG dinâmico em `/api/og` (usa @vercel/og).
* `sitemap.xml` e `robots.txt`.
* **Redirects** para futuros slugs herdados de WordPress (definir em `next.config.mjs` mapeando `/categoria/…`, `/tag/…`, etc.).

---

## 7) Performance

* Server Components onde possível; imports dinâmicos para blocos pesados.
* `next/image` otimizado, `next/font`, lazy loading, compressão, cache headers padrão Vercel.
* ISR nas páginas de conteúdo.
* Bundle analyzer opcional.

---

## 8) Entregáveis e Critérios de Aceite

**Entregáveis:**

* Repositório Next.js pronto para deploy (Vercel) com README.
* `.env.example` preenchido.
* 3 posts MDX + 3 cases MDX.
* Páginas e componentes listados acima.
* OG dinâmica, sitemap e robots.
* Formulário de contato funcional (Resend) com validação e rate-limit.
* Página de demo com embed.
* Scripts `dev|build|start|lint|typecheck`.

**Aceite por página (amostra):**

* **Home:** Hero acima da dobra com CTA, Lighthouse ≥ 95, sem erros de console.
* **Contato:** validação cliente/servidor, mensagem de sucesso/erro, e-mail enviado (ou logado em dev).
* **Blog:** index, post, categorias/tags/autor, sitemap específico do blog.
* **Setores:** todas as 5 páginas com conteúdo do artefato e CTA.
* **SEO:** metadados corretos, OG gerando imagem válida, sitemap acessível.

---

## 9) Passo-a-Passo (o Trae deve executar em ordem)

**Fase A — Scaffold**

1. Criar app Next.js 14+ (TS, App Router).
2. Adicionar Tailwind, shadcn/ui, Framer Motion, Lucide, RHF/Zod, next-intl, next-seo, Contentlayer.
3. ESLint/Prettier/strict mode, Vercel Analytics.
4. `README.md` com instruções e comandos.

**Fase B — Design System & Layout**

1. Header/Footer/Nav responsivos; paleta (azul tech, verde crescimento, laranja energia).
2. Componentes base (Button, Card, Section, CTA, Badge, KPI, Pricing, Testimonial, ROIForm).
3. Layout padrão e `metadata` global.

**Fase C — Páginas e Conteúdo**

1. Home com todas as seções do artefato.
2. Soluções, Setores (5 páginas), Cases (grid + 3 MDX), Sobre, Contato, Demo.
3. Blog completo (MDX + indexes por categoria/tag/autor + sitemap blog).

**Fase D — Funcional**

1. `/api/contact` via Resend (fallback console), RHF+Zod, honeypot, rate-limit simples.
2. `/api/og` para OG dinâmico.
3. Embed Cal.com/Calendly e widget de chat (script).

**Fase E — SEO/Perf/Deploy**

1. next-seo por página; schema.org; sitemap/robots.
2. Ajustes Core Web Vitals; imports dinâmicos; imagens otimizadas.
3. `next.config.mjs` com i18n (pt-BR/en) e redirects base.
4. Preparar deploy Vercel (scripts, build, previews).

**Fase F — QA**

1. Rodar Lighthouse local; corrigir até ≥ 95.
2. Checklist manual (links, formulários, OG, sitemap).

---

## 10) Comandos Esperados

* **Instalação:** `npm i`
* **Dev:** `npm run dev`
* **Build:** `npm run build`
* **Start:** `npm run start`
* **Lint/Types:** `npm run lint` / `npm run typecheck`
* **Deploy Vercel (exemplo):** `npx vercel` (preview) e `npx vercel --prod`

---

## 11) Saída Esperada do Trae (por fase)

* **Lista de arquivos** criados/alterados (árvore resumida).
* **Diffs/resumos** das mudanças críticas.
* **Instruções de execução** local/deploy.
* **Checklist de validação manual** (o que verificar).
* **Pendências/assunções** (se houver).

---

## 12) Extras (opcionais)

* **Calculadora de ROI:** exponha função `calcROI(params)` em `/lib/roi.ts`.
* **OG Template:** template base em `/public/og-templates/base.png`.
* **Feature flags:** via Edge Config (documentar, mas manter desligado).
* **CMS headless:** gerar `cms/` com clients prontos, porém **desativado** por padrão.

---

> **Inicie agora pela Fase A** e retorne:
>
> 1. árvore de arquivos, 2) diffs/resumos, 3) comandos para rodar, 4) checklist de QA.
