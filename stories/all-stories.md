# TurboClaw — Refined Stories (SM Output)

**Versão:** 1.0 · **Data:** 24/02/2026
**Autor:** River (Scrum Master Agent)
**Input:** PRD v3.0 + Architecture v1.0

**Referências:**
- PRD: `docs/prd.md`
- Arquitetura: `docs/architecture.md`

---

## Convenções

- **Paths** são relativos à raiz do monorepo (`/Users/marko/Desktop/CLAUDE/PROJETOS/CLAWPRO`)
- **AC** = Acceptance Criteria (testável)
- **Deps** = Stories prerequisito
- **Agent** = Agente AIOS recomendado para execução
- **Files** = Arquivos que devem ser criados/modificados

---

# Epic 1: Foundation & Auth & Landing

---

## Story 1.1 — Project Bootstrap & Monorepo Setup

**Deps:** Nenhuma (primeira story)
**Agent:** `@dev`
**Estimativa:** ~2h

> Como desenvolvedor,
> quero o projeto inicializado com monorepo, linting e deploy automático,
> para que toda a equipe trabalhe com a mesma base desde o dia zero.

### Files to Create

```
apps/web/                          # Next.js 14 App Router
├── src/
│   ├── app/
│   │   ├── layout.tsx             # Root layout: html lang="pt-BR", dark mode class, Inter + JetBrains Mono
│   │   ├── globals.css            # Tailwind @layer base/components/utilities
│   │   └── (public)/
│   │       └── page.tsx           # Canary page: "TurboClaw — Em breve"
│   ├── lib/
│   │   └── utils.ts               # cn() helper (clsx + tailwind-merge)
│   └── types/
│       └── api.ts                 # Placeholder
├── public/
│   └── favicon.ico
├── tailwind.config.ts             # Dark mode: 'class', Inter + JetBrains Mono fonts
├── next.config.ts
├── tsconfig.json                  # strict: true
└── package.json

packages/provisioning/             # Placeholder com package.json
├── package.json
└── src/
    └── index.ts

infra/
├── docker/                        # Placeholder
├── nginx/                         # Placeholder
└── scripts/                       # Placeholder

supabase/
└── migrations/                    # Placeholder

docs/
├── prd.md                         # Já existe
└── architecture.md                # Já existe

.env.example                       # Todas as variáveis documentadas
.eslintrc.json                     # ESLint config
.prettierrc                        # Prettier config
package.json                       # Root: workspaces config
```

### Technical Context

- Usar `create-next-app@14` com App Router + TypeScript
- Fontes via `next/font/google` (Inter) e `next/font/local` ou google (JetBrains Mono)
- Tailwind com dark mode via `class` strategy
- Root `package.json` com `"workspaces": ["apps/*", "packages/*"]`
- `.env.example` deve conter: `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`, `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, `VPS_API_URL`, `VPS_API_TOKEN`

### Acceptance Criteria

1. `npm install` na raiz instala dependências de todos os workspaces
2. `npm run dev` em `apps/web` inicia Next.js na porta 3000
3. Página canary renderiza em `localhost:3000` com texto "TurboClaw — Em breve" em dark mode
4. TypeScript strict sem erros (`npx tsc --noEmit`)
5. ESLint sem erros (`npx eslint .`)
6. `.env.example` contém todas as 7 variáveis documentadas com comentários
7. Deploy automático na Vercel funcional a partir de push em `main`

### Definition of Done

- [ ] Todos os AC passam
- [ ] Código commitado e pushado
- [ ] Vercel deploy green

---

## Story 1.2 — Supabase Auth & Database Schema

**Deps:** Story 1.1
**Agent:** `@dev` + `@data-engineer` (para schema/RLS)
**Estimativa:** ~3h

> Como usuário,
> quero criar conta e fazer login com email/senha ou magic link,
> para acessar minha área após o pagamento.

### Files to Create/Modify

```
apps/web/src/
├── lib/
│   └── supabase/
│       ├── client.ts              # createBrowserClient()
│       ├── server.ts              # createServerClient() com cookies
│       └── middleware.ts          # Auth middleware para rotas (auth)
├── app/
│   ├── (public)/
│   │   └── login/
│   │       └── page.tsx           # Form login/cadastro com Supabase Auth
│   ├── (auth)/
│   │   └── layout.tsx             # Layout protegido (redireciona se não auth)
│   └── middleware.ts              # Next.js middleware: check session

supabase/
└── migrations/
    └── 001_initial_schema.sql     # CREATE TABLE customers, instances, provision_logs + RLS
```

### Technical Context

- Usar `@supabase/supabase-js` + `@supabase/ssr` para SSR auth
- Middleware Next.js (`apps/web/src/middleware.ts`) intercepta rotas `/(auth)/*` e redireciona para `/login` se não autenticado
- Supabase client-side: `createBrowserClient(url, anonKey)`
- Supabase server-side: `createServerClient(url, anonKey, { cookies })` — ler cookies do request
- RLS policies conforme `docs/architecture.md` seção 4.2

### Schema SQL

```sql
-- 001_initial_schema.sql
-- Tables: customers, instances, provision_logs
-- Conforme docs/prd.md seção 4.3
-- RLS: conforme docs/architecture.md seção 4.2

-- Ativar RLS
ALTER TABLE customers ENABLE ROW LEVEL SECURITY;
ALTER TABLE instances ENABLE ROW LEVEL SECURITY;
ALTER TABLE provision_logs ENABLE ROW LEVEL SECURITY;

-- Policies (copiar da architecture.md seção 4.2)
```

### Acceptance Criteria

1. Projeto Supabase criado com tabelas `customers`, `instances`, `provision_logs` conforme schema do PRD
2. RLS ativo em todas as tabelas — teste: user A não vê dados de user B
3. Login com email/senha funcional em `/login`
4. Magic link funcional (email recebido, click loga)
5. Rotas `/(auth)/*` redirecionam para `/login` se não autenticado
6. Migration SQL versionada em `supabase/migrations/001_initial_schema.sql`
7. Tipos TypeScript gerados via `supabase gen types` em `types/database.ts`

### Definition of Done

- [ ] Todos os AC passam
- [ ] RLS testado com 2 usuários diferentes
- [ ] Migration aplicável via `supabase db push`

---

## Story 1.3 — Stripe Checkout Integration (One-Time)

**Deps:** Story 1.2
**Agent:** `@dev`
**Estimativa:** ~3h

> Como usuário,
> quero escolher um plano e pagar via Pix ou cartão,
> para adquirir meu assistente IA.

### Files to Create/Modify

```
apps/web/src/
├── lib/
│   └── stripe/
│       └── client.ts              # Stripe SDK config (server-side)
├── app/
│   ├── api/
│   │   └── webhooks/
│   │       └── stripe/
│   │           └── route.ts       # POST: webhook handler
│   ├── (public)/
│   │   └── page.tsx               # Modificar: adicionar botões de plano que chamam Stripe
│   └── (auth)/
│       └── setup/
│           └── page.tsx           # Placeholder: wizard (story 2.1)
```

### Technical Context

- Stripe SDK server-side: `stripe` package
- Checkout Session com `mode: 'payment'` (NÃO `subscription`)
- Webhook `checkout.session.completed`: verificar signature com `stripe.webhooks.constructEvent()`
- Após pagamento confirmado:
  1. `INSERT INTO customers` (user_id, stripe_payment_id, plan, status: 'provisioning')
  2. `INSERT INTO instances` (customer_id, status: 'creating')
  3. Enviar email via Supabase Edge Function com link `/setup?token={onboarding_token}`
- Pix: ativar no dashboard Stripe + configurar `payment_method_types: ['card', 'pix']`
- Usar `STRIPE_WEBHOOK_SECRET` para validar webhook

### Acceptance Criteria

1. Dois produtos Stripe configurados: `turboclaw_personal` (R$197) e `turboclaw_professional` (R$397)
2. Botão "Começar agora" cria Stripe Checkout Session e redireciona para Stripe
3. Pix ativado como método de pagamento aceito
4. Webhook `checkout.session.completed` cria registros em `customers` + `instances`
5. Email de boas-vindas enviado com link `/setup?token=XYZ`
6. Página de sucesso Stripe redireciona para `/setup`
7. Webhook valida signature (`STRIPE_WEBHOOK_SECRET`) — rejeita requests inválidos

### Definition of Done

- [ ] Todos os AC passam
- [ ] Testado com Stripe test mode (cartão 4242... e Pix test)
- [ ] Webhook testável via `stripe listen --forward-to`

---

## Story 1.4 — Landing Page

**Deps:** Story 1.3 (para os botões de checkout funcionarem)
**Agent:** `@dev` (+ `@ux-design-expert` se necessário)
**Estimativa:** ~4h

> Como visitante,
> quero entender o que é TurboClaw, ver os planos e decidir comprar,
> para ter meu assistente IA pessoal.

### Files to Create/Modify

```
apps/web/src/
├── components/
│   ├── ui/
│   │   ├── button.tsx             # Variants: primary, secondary, ghost
│   │   ├── card.tsx               # Container card reutilizável
│   │   ├── badge.tsx              # "Pagamento único", "Mais popular"
│   │   └── input.tsx              # Input padrão (reuso no wizard)
│   ├── landing/
│   │   ├── hero.tsx               # Headline + CTA + animated background
│   │   ├── how-it-works.tsx       # 3 passos visuais
│   │   ├── features.tsx           # Grid de feature cards
│   │   ├── pricing.tsx            # 2 cards (Pessoal vs Profissional)
│   │   ├── faq.tsx                # Accordion com 8+ perguntas
│   │   └── footer.tsx             # Links legais + copyright
│   └── shared/
│       ├── logo.tsx               # TurboClaw logo (text + ícone)
│       └── navbar.tsx             # Sticky header com anchor links
├── app/
│   └── (public)/
│       └── page.tsx               # Compor seções da landing

public/
└── og-image.png                   # Open Graph preview image
```

### Technical Context

- Reaproveitar design existente (clawdia-gamma): dark mode, gradients animados, noise texture, grid dots
- Framer Motion para animações de entrada (fade-in on scroll)
- Pricing cards com `onClick` → cria Stripe Checkout Session (integração da Story 1.3)
- Copy 100% pt-BR, tom "turbo" (energia, velocidade)
- Mobile-first: testar em 375px e 390px
- Lighthouse > 90: lazy load images, font-display swap, minimal JS

### Acceptance Criteria

1. Hero section com headline, sub-headline e CTA "Começar agora" funcional
2. Seção "Como funciona" com 3 passos visuais
3. Seção features com grid de cards (mínimo 6 features)
4. Pricing com 2 cards: Pessoal (R$197) e Profissional (R$397), badge "Pagamento único"
5. FAQ accordion com 8+ perguntas sobre pricing, suporte, API key, BotFather, etc.
6. Footer com links placeholder para `/termos` e `/privacidade`
7. Dark mode com efeitos visuais (gradients, noise, grid)
8. Responsivo em 375px (iPhone SE) — sem overflow horizontal
9. Copy 100% pt-BR — zero inglês acidental
10. Lighthouse > 90 em performance, accessibility e SEO

### Definition of Done

- [ ] Todos os AC passam
- [ ] Testado em Chrome e Safari mobile
- [ ] Lighthouse report screenshot salvo

---

# Epic 2: Wizard & Provisioning Engine

---

## Story 2.1 — Wizard UI (4 Telas)

**Deps:** Story 1.2 (auth), Story 1.3 (customer exists)
**Agent:** `@dev`
**Estimativa:** ~4h

> Como cliente que acabou de pagar,
> quero configurar meu assistente em 4 passos simples,
> para personalizar meu bot sem conhecimento técnico.

### Files to Create/Modify

```
apps/web/src/
├── components/
│   └── wizard/
│       ├── wizard-shell.tsx       # Container: progress bar + navigation + step renderer
│       ├── step-name.tsx          # Step 1: input nome + preview
│       ├── step-personality.tsx   # Step 2: 4 cards visuais
│       ├── step-api-key.tsx       # Step 3: provider select + key input (masked)
│       └── step-botfather.tsx     # Step 4: tutorial inline + token input
├── hooks/
│   └── use-wizard-state.ts       # State management + Supabase persistence
├── app/
│   └── (auth)/
│       └── setup/
│           └── page.tsx           # Wizard page: valida token, renderiza wizard-shell
└── types/
    └── api.ts                     # ProvisionSchema (zod) — conforme architecture.md seção 2.4
```

### Technical Context

- Wizard state: `useState` local para UX rápida + persist no Supabase (campo JSON em `instances` ou tabela auxiliar)
- Validação com Zod em cada step antes de avançar
- Step 3 (API key): input type password com toggle de visibilidade, NUNCA loga a key
- Step 4 (BotFather): tutorial visual step-by-step (imagens ou animações mostrando @BotFather → /newbot → copiar token)
- Token BotFather regex: `/^\d+:[A-Za-z0-9_-]{35}$/`
- Ao submeter: `POST /api/provision` com todos os dados → redirect para loading screen
- URL de entrada: `/setup?token=XYZ` — validar token contra `customers` table

### Acceptance Criteria

1. Tela 1: Input nome (2-50 chars) + preview visual com o nome digitado
2. Tela 2: 4 cards clicáveis (Profissional, Amigável, Técnico, Custom com textarea)
3. Tela 3: Select de provider (OpenAI/Claude/Gemini) + input de API key com máscara
4. Tela 4: Tutorial BotFather visual (mínimo 4 steps com imagens/texto) + input do token
5. Progress indicator visível (step 1/4, 2/4, etc.)
6. Validação bloqueia avanço se campos inválidos
7. Estado persiste no Supabase — fechar browser e reabrir retoma do último step
8. Submit chama `POST /api/provision` e redireciona para tela de loading

### Definition of Done

- [ ] Todos os AC passam
- [ ] Testado em mobile (375px)
- [ ] Validação de token BotFather funcional

---

## Story 2.2 — VPS Setup & Docker Infrastructure

**Deps:** Nenhuma (pode ser paralelo com 2.1)
**Agent:** `@dev` + `@devops`
**Estimativa:** ~3h

> Como operador da plataforma,
> quero o VPS configurado com Docker, Nginx e estrutura de diretórios,
> para receber containers de clientes automaticamente.

### Files to Create

```
infra/
├── scripts/
│   ├── vps-setup.sh               # Script de setup inicial da VPS (one-time)
│   ├── provision.sh               # Cria container para cliente (chamado pelo Express)
│   ├── destroy.sh                 # Remove container + volumes
│   └── health-check.sh            # Cron: verifica containers + atualiza Supabase
├── docker/
│   └── docker-compose.template.yml # Template referência
├── nginx/
│   ├── nginx.conf                 # Config principal (conforme architecture.md seção 3.4)
│   └── conf.d/
│       └── .gitkeep
└── systemd/
    └── turboclaw-health.timer     # Systemd timer para health check a cada 60s
```

### Technical Context

- VPS-setup.sh: `apt update && apt install -y docker.io docker-compose-plugin nginx certbot`
- Estrutura conforme `docs/architecture.md` seção 3.1
- Nginx config conforme architecture.md seção 3.4 (SSL, proxy para Express :4000)
- Health check como systemd timer (não dentro do Express — ADR-004)
- Docker image `node:22-slim` pré-pulled
- OpenClaw pinado: `npm pack openclaw@latest` salvo em `/opt/turboclaw/fallback/`
- Firewall: `ufw allow 80,443,22/tcp && ufw enable`

### Acceptance Criteria

1. `vps-setup.sh` executável e idempotente (rodar 2x não quebra)
2. Estrutura `/opt/turboclaw/{api, clients, nginx, scripts, fallback}` criada
3. `provision.sh` cria container com nome `turboclaw-{clientId}` e recursos do plano
4. `destroy.sh` remove container + volumes do cliente
5. `health-check.sh` lista containers `turboclaw-*`, faz curl health, atualiza Supabase
6. Nginx serve HTTPS com cert Let's Encrypt
7. Docker image OpenClaw salva localmente em `/opt/turboclaw/fallback/`
8. Scripts documentados com comentários em pt-BR

### Definition of Done

- [ ] Todos os AC passam
- [ ] Testado em VPS real (ou similar local com Docker)
- [ ] Health check timer ativo e logando

---

## Story 2.3 — Provisioning API & Container Orchestration

**Deps:** Story 2.1 (wizard envia dados), Story 2.2 (VPS pronta)
**Agent:** `@dev`
**Estimativa:** ~4h

> Como sistema,
> quero receber os dados do wizard e provisionar automaticamente um container,
> para que o bot do cliente fique online em menos de 3 minutos.

### Files to Create

```
# VPS-side: Express API server
infra/api/
├── package.json                   # express, dockerode, helmet, express-rate-limit, winston, dotenv, @supabase/supabase-js
├── server.js                      # Express entry: middleware stack + routes
├── .env.example                   # VPS_API_TOKEN, SUPABASE_URL, SUPABASE_SERVICE_KEY
├── middleware/
│   ├── auth.js                    # Bearer token validation
│   ├── rate-limit.js              # 5 req/min por IP
│   └── logger.js                  # Winston request logging
├── routes/
│   ├── provision.js               # POST /provision
│   ├── destroy.js                 # POST /destroy/:clientId
│   ├── restart.js                 # POST /restart/:clientId
│   ├── update-key.js              # PUT /update-key/:clientId
│   ├── metrics.js                 # GET /metrics/:clientId
│   └── health.js                  # GET /health
└── services/
    ├── docker.js                  # dockerode wrapper (conforme architecture.md seção 3.2)
    ├── soul-generator.js          # Gera SOUL.md: nome + personalidade → markdown
    ├── env-generator.js           # Gera .env: bot token + LLM config
    └── metrics-reader.js          # Lê logs do container para token usage

# Vercel-side: API Route proxy
apps/web/src/
├── lib/
│   └── vps/
│       └── client.ts              # callVps() conforme architecture.md seção 5.2
├── app/
│   └── api/
│       └── provision/
│           └── route.ts           # POST: valida auth + customer → proxy para VPS
```

### Technical Context

- Express middleware stack: `helmet()` → `logger` → `rateLimit` → `auth` → routes
- Bearer token: comparar `req.headers.authorization` com `process.env.VPS_API_TOKEN`
- dockerode: conforme code sample em architecture.md seção 3.2
- Status updates no Supabase via `@supabase/supabase-js` com service role key
- Sequência de status: `creating` → `installing` → `configuring` → `online` (ou `error`)
- Cada transição: `INSERT INTO provision_logs`
- Vercel API Route: padrão BFF conforme architecture.md seção 2.3

### Acceptance Criteria

1. Express server inicia em porta 4000 com middleware stack completo
2. `POST /provision` cria container via dockerode com recursos do plano
3. SOUL.md gerado contém nome e personalidade do assistente em pt-BR
4. API key injetada no `.env` do container — verificar que NÃO está no Supabase
5. Status atualizado em `instances` a cada step (creating → installing → configuring → online)
6. Logs gravados em `provision_logs` a cada transição
7. Timeout de 3 minutos — status → `error` se exceder
8. Rate limit: 6a request no mesmo minuto retorna 429
9. Request sem bearer token retorna 401

### Definition of Done

- [ ] Todos os AC passam
- [ ] Testado end-to-end: API Route → Express → Docker → container online
- [ ] Logs verificáveis em `provision_logs`

---

## Story 2.4 — Loading Screen & Success Flow

**Deps:** Story 2.3 (provisioning funcional)
**Agent:** `@dev`
**Estimativa:** ~2h

> Como cliente,
> quero ver o progresso da criação do meu assistente em tempo real,
> para saber que está funcionando e quando estará pronto.

### Files to Create/Modify

```
apps/web/src/
├── components/
│   └── shared/
│       └── loading-screen.tsx     # Progress steps + Supabase Realtime subscription
├── hooks/
│   └── use-instance-status.ts     # Supabase Realtime: subscribe to instances.status
├── app/
│   └── (auth)/
│       ├── setup/
│       │   └── page.tsx           # Modificar: após submit wizard → show loading-screen
│       └── success/
│           └── page.tsx           # Tela de sucesso: nome, link Telegram, CTAs
```

### Technical Context

- Supabase Realtime: `supabase.channel('instance-status').on('postgres_changes', { event: 'UPDATE', schema: 'public', table: 'instances', filter: `id=eq.${instanceId}` }, callback)`
- Loading screen: 5 steps visuais que mapeiam para os status do DB:
  - `creating` → "Criando seu servidor..."
  - `installing` → "Instalando OpenClaw..."
  - `configuring` → "Configurando personalidade..."
  - `configuring` (2a fase) → "Conectando Telegram..."
  - `online` → "Testando conexão... ✓"
- Animação: checkmark verde ao completar cada step, spinner no step atual
- Ao `online`: auto-redirect para `/success` após 2s
- Error state: mensagem amigável + "Tentar novamente" (chama `POST /api/provision` de novo)

### Acceptance Criteria

1. Loading screen exibe 5 progress steps com animação
2. Status atualiza em real-time (sem polling — via Supabase Realtime)
3. Cada step muda visualmente quando status muda no banco
4. Ao atingir `online` → redirect para `/success` após 2s
5. Tela de sucesso exibe: nome do assistente, link `t.me/{bot_username}`, CTAs
6. Email de notificação enviado (via Supabase Edge Function)
7. Se error/timeout → mensagem amigável + botão "Tentar novamente"

### Definition of Done

- [ ] Todos os AC passam
- [ ] Testado com status mudando no Supabase (simular via SQL)
- [ ] Error state testado

---

# Epic 3: Dashboard & Monitoring

---

## Story 3.1 — Dashboard Layout & Status

**Deps:** Story 1.2 (auth), Story 2.3 (instances com status)
**Agent:** `@dev`
**Estimativa:** ~2h

> Como cliente,
> quero ver o status do meu assistente e informações básicas,
> para saber se está funcionando.

### Files to Create

```
apps/web/src/
├── components/
│   └── dashboard/
│       └── status-card.tsx        # Card principal: nome, status, uptime, link Telegram
├── app/
│   └── (auth)/
│       └── dashboard/
│           └── page.tsx           # Dashboard page: fetch instance + realtime
```

### Technical Context

- Server Component: fetch initial data via Supabase server client
- Client Component wrapper: Supabase Realtime para status updates
- Indicadores: 🟢 online, 🟡 provisioning/installing, 🔴 error/offline
- Uptime: calcular desde `last_health_check` ou `created_at`
- Link Telegram: `https://t.me/{telegram_bot_username}` — sempre visível

### Acceptance Criteria

1. `/dashboard` protegido por auth — redireciona se não logado
2. Card principal exibe: nome do assistente, status com cor, uptime
3. Status atualiza em real-time via Supabase Realtime
4. Indicador visual: verde (online), amarelo (provisioning), vermelho (error/offline)
5. Link direto para o bot no Telegram sempre visível e clicável
6. Layout responsivo em dark mode

### Definition of Done

- [ ] Todos os AC passam
- [ ] Testado com os 3 status visuais

---

## Story 3.2 — Actions: Restart & API Key

**Deps:** Story 3.1, Story 2.3 (VPS API endpoints)
**Agent:** `@dev`
**Estimativa:** ~2h

> Como cliente,
> quero reiniciar meu bot e trocar a API key quando necessário,
> para resolver problemas sem depender de suporte.

### Files to Create/Modify

```
apps/web/src/
├── components/
│   └── dashboard/
│       └── actions-panel.tsx      # Restart button + API key form
├── app/
│   └── api/
│       ├── restart/
│       │   └── route.ts           # POST: proxy → VPS /restart/:clientId
│       └── update-key/
│           └── route.ts           # PUT: proxy → VPS /update-key/:clientId
```

### Technical Context

- Restart: `POST /api/restart` → Vercel valida auth → `callVps(hostVps, '/restart/${clientId}')` → Express → `docker.getContainer().restart()`
- Update key: `PUT /api/update-key` → valida com `updateKeySchema` (zod) → VPS reescreve `.env` → restart container
- Loading state no botão (disable + spinner durante request)
- Toast/alert de sucesso ou erro após response

### Acceptance Criteria

1. Botão "Reiniciar" chama API e executa restart do container
2. Restart exibe loading state e confirma sucesso/falha via toast
3. Form "Trocar API Key": select provider + input key (masked) + submit
4. Nova key injetada no container — verificar que NÃO vai para Supabase
5. Após trocar key, container reinicia automaticamente
6. Confirmação visual: "API key atualizada com sucesso"

### Definition of Done

- [ ] Todos os AC passam
- [ ] Testado: restart + status volta online
- [ ] Testado: trocar key + container reinicia

---

## Story 3.3 — Token Usage Monitoring

**Deps:** Story 3.1, Story 2.3 (VPS metrics endpoint)
**Agent:** `@dev`
**Estimativa:** ~3h

> Como cliente,
> quero ver quanto estou gastando de tokens da minha API key,
> para controlar meus custos com o provider de LLM.

### Files to Create/Modify

```
apps/web/src/
├── components/
│   └── dashboard/
│       └── token-usage.tsx        # Gráfico de barras + resumo diário/mensal
├── app/
│   └── api/
│       └── metrics/
│           └── [id]/
│               └── route.ts      # GET: proxy → VPS /metrics/:clientId

# VPS-side (já criado em 2.3, refinar):
infra/api/services/
└── metrics-reader.js              # Lê logs OpenClaw, extrai token counts
```

### Technical Context

- VPS `metrics-reader.js`: parsear logs do OpenClaw dentro do container para extrair token usage
- Se OpenClaw não expor métricas nativamente: estimar via tamanho de mensagens ou usar fallback "dados indisponíveis"
- Gráfico: barras simples (últimos 7 dias) — pode usar `recharts` ou SVG manual
- Dados: `{ date, tokens_in, tokens_out, total }` por dia
- Alerta: se `today.total > avg_7d * 3` → mostrar badge de alerta
- Tooltip: "O custo dos tokens é cobrado pelo seu provider (OpenAI/Claude/Gemini), não pelo TurboClaw"

### Acceptance Criteria

1. Seção "Consumo" exibe tokens usados (diário e mensal)
2. Dados lidos do container via VPS API endpoint `/metrics/:clientId`
3. Gráfico de barras dos últimos 7 dias
4. Alerta visual quando consumo diário > 3x a média
5. Tooltip explicativo sobre custos do provider

### Definition of Done

- [ ] Todos os AC passam
- [ ] Funciona mesmo se métricas indisponíveis (fallback graceful)

---

## Story 3.4 — Health Check System

**Deps:** Story 2.2 (scripts), Story 2.3 (containers existem)
**Agent:** `@dev` + `@devops`
**Estimativa:** ~2h

> Como operador da plataforma,
> quero monitorar automaticamente todos os containers,
> para detectar e resolver problemas antes do cliente perceber.

### Files to Create/Modify

```
infra/
├── scripts/
│   └── health-check.sh           # Refinar: conforme architecture.md seção 3.3
└── systemd/
    ├── turboclaw-health.service   # Systemd service unit
    └── turboclaw-health.timer     # Timer: a cada 60s
```

### Technical Context

- Health check conforme architecture.md seção 3.3
- Usa `supabase` CLI ou `curl` direto para API REST do Supabase com service key
- Failure tracking: contar falhas consecutivas por container (arquivo temp ou in-memory)
- Auto-restart: `docker restart turboclaw-{id}` após 3 falhas
- Alerta: se restart falhar → `curl` para webhook (Slack, email, ou Supabase Edge Function)
- Log rotation: `/var/log/turboclaw/health.log` com `logrotate` (7 dias)

### Acceptance Criteria

1. Systemd timer executa health-check.sh a cada 60 segundos
2. Health check faz curl no `/health` de cada container `turboclaw-*`
3. Após 3 falhas consecutivas → `instances.status` atualizado para `error`
4. Auto-restart tentado 1 vez após 3 falhas
5. Se restart falhar → alerta enviado ao operador
6. Logs mantidos por 7 dias com logrotate

### Definition of Done

- [ ] Todos os AC passam
- [ ] Testado: parar container manualmente → health check detecta → restart → volta online
- [ ] Timer ativo via `systemctl status turboclaw-health.timer`

---

# Epic 4: Polish, E2E Tests & Launch

---

## Story 4.1 — E2E Tests (Full Flow)

**Deps:** Todos os épicos anteriores (testa o fluxo completo)
**Agent:** `@qa`
**Estimativa:** ~3h

> Como desenvolvedor,
> quero testes automatizados do fluxo completo,
> para garantir que compra → setup → bot online funciona sem regressões.

### Files to Create

```
apps/web/
├── playwright.config.ts           # Config: baseURL, projects (chromium, mobile)
├── e2e/
│   ├── landing.spec.ts            # Landing page renders, navigation, Lighthouse
│   ├── checkout.spec.ts           # Stripe test mode → redirect → callback
│   ├── wizard.spec.ts             # 4 steps, validation, submit
│   ├── dashboard.spec.ts          # Status, restart, update key
│   └── full-flow.spec.ts          # End-to-end: landing → checkout → wizard → loading → success → dashboard
└── package.json                   # Adicionar: @playwright/test

.github/
└── workflows/
    └── e2e.yml                    # GitHub Action: run Playwright on push to main
```

### Technical Context

- Stripe test mode: usar cartão `4242 4242 4242 4242`
- Para Pix test: verificar se Stripe test mode suporta, senão skip
- Supabase: seed data para testes ou usar Supabase local via Docker
- VPS mock: para CI, mockar as chamadas ao VPS (nock ou MSW)
- Mobile viewport: 375x812 (iPhone SE)

### Acceptance Criteria

1. Playwright configurado com projetos: chromium desktop + chromium mobile
2. Teste: landing page renderiza todas as seções
3. Teste: checkout Stripe test mode → webhook simulado → customer criado
4. Teste: wizard completo (4 steps) com dados válidos
5. Teste: dashboard → restart → status atualiza
6. GitHub Action roda testes no push para `main`

### Definition of Done

- [ ] Todos os AC passam
- [ ] CI green no GitHub Actions
- [ ] Testes passam em < 5 minutos

---

## Story 4.2 — Mobile Responsiveness & Copy Final

**Deps:** Story 1.4 (landing), Story 2.1 (wizard), Story 3.1 (dashboard)
**Agent:** `@dev` + `@qa`
**Estimativa:** ~2h

> Como visitante mobile,
> quero navegar toda a experiência TurboClaw no celular,
> para comprar e configurar meu bot de qualquer dispositivo.

### Files to Modify

```
# Todos os componentes que precisem de ajuste mobile:
apps/web/src/components/landing/*.tsx
apps/web/src/components/wizard/*.tsx
apps/web/src/components/dashboard/*.tsx
apps/web/src/app/layout.tsx         # Meta tags OG

public/
└── og-image.png                    # Open Graph image (1200x630)
```

### Technical Context

- Breakpoints Tailwind: sm (640px), md (768px), lg (1024px)
- Testar em: 375px (iPhone SE), 390px (iPhone 14), 768px (iPad)
- Copy audit: grep por strings em inglês, substituir por pt-BR
- OG tags: `<meta property="og:title">`, `og:description`, `og:image`
- Tom da copy: "turbo", energia, simplicidade, sem jargão técnico

### Acceptance Criteria

1. Landing page sem overflow horizontal em 375px
2. Wizard funcional em mobile: inputs acessíveis, cards tocáveis, tutorial scroll
3. Dashboard usável em mobile: cards empilhados, botões com tap target ≥ 44px
4. Copy 100% pt-BR — zero inglês acidental (verificado via grep)
5. Meta tags OG configuradas e preview funcional no WhatsApp

### Definition of Done

- [ ] Todos os AC passam
- [ ] Screenshots em 375px e 390px salvos
- [ ] OG preview testado via metatags.io

---

## Story 4.3 — Legal & Security Hardening

**Deps:** Story 1.4 (footer com links legais)
**Agent:** `@dev`
**Estimativa:** ~2h

> Como operador,
> quero documentação legal e segurança revisada,
> para operar em conformidade e proteger dados dos clientes.

### Files to Create/Modify

```
apps/web/src/app/
├── (public)/
│   ├── termos/
│   │   └── page.tsx               # Termos de Uso
│   └── privacidade/
│       └── page.tsx               # Política de Privacidade

# VPS-side:
infra/api/middleware/
└── rate-limit.js                  # Verificar: 5 req/min configurado

infra/scripts/
└── rotate-token.sh                # Documentação/script para rotação do bearer token
```

### Technical Context

- Termos de Uso: template brasileiro padrão adaptado para SaaS one-time, mencionando: pagamento único sem reembolso (ou política de reembolso), responsabilidade sobre API key, uso do OpenClaw como engine
- Política de Privacidade: LGPD-compliant, mencionar: dados coletados (email, nome), dados NÃO armazenados (API key), uso de cookies (Supabase session), compartilhamento com Stripe
- Rate limiting: confirmar 5 req/min no Express (já implementado em 2.3)
- Token rotation: documentar processo de trocar `VPS_API_TOKEN` em Vercel + VPS sem downtime

### Acceptance Criteria

1. `/termos` renderiza Termos de Uso completos em pt-BR
2. `/privacidade` renderiza Política de Privacidade LGPD-compliant em pt-BR
3. Footer da landing linka para ambas as páginas
4. API keys em repouso: verificar permissão 600 no `.env` do container
5. Bearer token rotation documentado em `docs/operations.md`
6. Rate limit funcional: 6a request em 1 minuto retorna 429

### Definition of Done

- [ ] Todos os AC passam
- [ ] Textos legais revisados (não precisam ser perfeitos, mas completos)

---

## Story 4.4 — Docker Fallback & Launch Checklist

**Deps:** Todas as stories anteriores
**Agent:** `@dev` + `@devops`
**Estimativa:** ~2h

> Como operador,
> quero contingência para indisponibilidade do OpenClaw e checklist de lançamento validado,
> para lançar com confiança.

### Files to Create/Modify

```
infra/scripts/
├── pin-openclaw.sh                # Salva Docker image + npm pack como fallback
└── provision.sh                   # Modificar: fallback para image local se npm install falha

docs/
├── operations.md                  # Runbook: deploy, scaling, token rotation, fallback
└── launch-checklist.md            # Checklist completo (conforme PRD seção 10)
```

### Technical Context

- `pin-openclaw.sh`: `docker pull node:22-slim && npm pack openclaw@latest` → salvar em `/opt/turboclaw/fallback/`
- Modificar `provision.sh` / `docker.js`: try `npm install -g openclaw@latest`, se falhar → copiar do fallback local
- Launch checklist: todas as 21 items do PRD seção 10
- Smoke test: compra real no Stripe live mode (valor mínimo ou coupon 100% para teste)
- DNS: verificar `turboclaw.com.br` → Vercel
- VPS monitoring: `htop` ou script simples que alerta em 80% RAM/CPU

### Acceptance Criteria

1. Docker image OpenClaw (versão estável) salva em `/opt/turboclaw/fallback/`
2. Provisioning usa fallback local se `npm install openclaw` falha
3. Launch checklist documentado em `docs/launch-checklist.md`
4. Smoke test: compra real (Stripe live) → setup → bot online → dashboard ✓
5. DNS `turboclaw.com.br` apontando para Vercel e resolvendo
6. VPS monitoring ativo com alerta em 80%

### Definition of Done

- [ ] Todos os AC passam
- [ ] Smoke test manual aprovado
- [ ] `docs/launch-checklist.md` 100% checked

---

## Summary

| Epic | Stories | Total Estimativa |
|------|---------|-----------------|
| 1 — Foundation & Auth & Landing | 1.1, 1.2, 1.3, 1.4 | ~12h |
| 2 — Wizard & Provisioning | 2.1, 2.2, 2.3, 2.4 | ~13h |
| 3 — Dashboard & Monitoring | 3.1, 3.2, 3.3, 3.4 | ~9h |
| 4 — Polish & Launch | 4.1, 4.2, 4.3, 4.4 | ~9h |
| **Total** | **14 stories** | **~43h** |

### Dependency Graph

```
1.1 ──→ 1.2 ──→ 1.3 ──→ 1.4
              │
              ├──→ 2.1 ──┐
              │           ├──→ 2.3 ──→ 2.4
              └──→ 2.2 ──┘
                            │
                            ├──→ 3.1 ──→ 3.2
                            │         └──→ 3.3
                            └──→ 3.4
                                        │
                                        └──→ 4.1, 4.2, 4.3, 4.4
```

**Note:** Stories 2.1 e 2.2 podem ser desenvolvidas em paralelo (Marco + Frank).

---

*Stories v1.0 — River (SM). 14 stories refinadas com contexto técnico completo.*
*Próximo: @dev para executar Story 1.1 (bootstrap).*
