# ClawPro — Product Requirements Document (PRD)

## Ecossistema Brasileiro de OpenClaw: SaaS + Consultoria + Skills Marketplace

**Versao:** 1.0 · **Data:** 17/03/2026
**Status:** Aprovado para execucao
**Autor:** Morgan (PM) — consolidacao de Atlas (Analyst) + Traffic Masters + Copy Chief
**Operador:** Marco (unico operador nesta fase)
**Naming:** ClawPro e placeholder interno. Nome definitivo pendente para Mes 2.

**Changelog:**

| Data | Versao | Descricao | Autor |
|------|--------|-----------|-------|
| 17/03/2026 | 1.0 | PRD inicial consolidado a partir de handoff de 3 especialistas (Atlas, Traffic Masters, Copy Chief) | Morgan (PM) |

---

## 0. Visao do Produto (Uma Frase)

**ClawPro e o ecossistema brasileiro que transforma o hype do OpenClaw em resultado para empresarios: consultoria de implementacao ao vivo, SaaS self-service e marketplace de skills -- tres frentes integradas, zero concorrencia estruturada.**

---

## 1. Contexto e Analise do Projeto Existente

### 1.1 Estado Atual

O TurboClaw ja existe como SaaS funcional (6 epics completos, 42 E2E testes, MercadoPago integrado, dashboard v2 com sidebar + tab routing). Stack: Next.js 14 + Supabase + Vercel + Express API + Docker + Mercado Pago. PRD v6.0 documentado em `docs/prd.md`.

As outras duas frentes (Consultoria e Marketplace) sao novos produtos que compartilham infraestrutura transversal com o TurboClaw (Supabase, Resend, Meta Pixel, sistema de afiliados).

### 1.2 Documentacao Disponivel

- [x] Tech Stack — `docs/prd.md` (secao 4)
- [x] Arquitetura — `docs/prd.md` (secao 4 + 12)
- [x] Schema de banco — `docs/prd.md` (secao 4.5)
- [x] Plano geral do ecossistema — `docs/claw-pro/plano-geral.md`
- [x] Estrategia de trafego — `docs/claw-pro/trafego-afiliados.md`
- [x] Plano da consultoria — `docs/claw-pro/consultoria/plano.md`
- [x] Copy kit da consultoria — `docs/claw-pro/consultoria/copy-kit.md`
- [x] Plano do TurboClaw — `docs/claw-pro/turboclaw/plano.md`
- [x] Plano do marketplace — `docs/claw-pro/skills-marketplace/plano.md`

### 1.3 Tipo de Enhancement

- [x] New Feature Addition (consultoria, marketplace)
- [x] Integration with New Systems (Meta Pixel, CAPI, Calendly, Gumroad)
- [x] UI/UX Additions (landing consultoria, pagina afiliados, site umbrella)
- [ ] Major Feature Modification (TurboClaw recebe ajustes, nao reescrita)

### 1.4 Avaliacao de Impacto

**Significativo** — adiciona 2 novos produtos ao ecossistema, requer novas tabelas no Supabase, novas paginas web, integracoes com Meta Ads e sistema de afiliados. Porem, o nucleo TurboClaw permanece intocado.

---

## 2. Goals e Background Context

### 2.1 Goals

1. Lancar consultoria de implementacao OpenClaw ao vivo em 2 semanas (Semana 1-2)
2. Instalar infraestrutura de tracking (Pixel Meta + CAPI + UTM) antes de rodar qualquer ad (Semana 2)
3. Ativar programa de afiliados com 5 primeiros parceiros em 4 semanas (Semana 3-4)
4. Publicar primeiros 2-3 skill packs no marketplace ate o Mes 2
5. Atingir R$18k/mes de receita ate o Mes 3, R$39k ate Mes 6, R$55k ate Mes 12
6. Integrar as 3 frentes em flywheel com cross-sell automatizado
7. Manter TurboClaw existente funcionando sem regressoes

### 2.2 Background Context

O mercado brasileiro de wrappers SaaS de OpenClaw esta saturando (3+ players). A oportunidade esta na camada superior: consultoria premium de implementacao (zero concorrencia estruturada no Brasil) e marketplace de skills (ClawHub com 320k users, zero skills de terceiros publicadas).

O TurboClaw funciona como porta de entrada de baixo ticket, enquanto a consultoria e o motor financeiro (47% da receita em 12 meses) e o marketplace e escalabilidade passiva (margem ~100%).

### 2.3 Tese de Negocio

O prospect esta no Awareness Level 2 (Problem-Aware) e Sophistication Stage 1 (Mercado Virgem). Sabe que IA existe, sabe que precisa, nao sabe que existe consultoria de implementacao. Claim direto funciona: "Eu instalo, configuro e treino seu agente de IA em 4 horas."

### 2.4 Frase-Guia

> "Voce viu o hype. Eu faco funcionar para o seu negocio."

---

## 3. Modelo de Receita Consolidado

### 3.1 Streams por Frente

| Frente | Tipo | Ticket | Frequencia |
|--------|------|--------|------------|
| TurboClaw | Setup fee | R$307 medio | Por novo cliente |
| TurboClaw | Mensalidade | R$50 medio | Mensal (cobre infra) |
| Consultoria | Sessao unica | R$2.225 medio | Por sessao |
| Consultoria | Upsell skill pack | R$197 (30% attach) | Por sessao |
| Consultoria | Retainer | R$550 medio | Mensal (20% conversao) |
| Marketplace | Venda avulsa | R$47-297 | Por compra |

### 3.2 Projecao 12 Meses (Cenario Base)

| Metrica | Valor |
|---------|-------|
| Receita total 12M | R$420.000 |
| Lucro bruto 12M | R$336.000 |
| Margem media | ~80% |
| Run rate Mes 12 | R$55.645/mes |
| Break-even | Mes 1 |

### 3.3 Contribuicao por Frente (12 meses)

| Frente | Receita | % |
|--------|---------|---|
| Consultoria (sessoes) | R$197.175 | 47% |
| TurboClaw (setup + mensalidade) | R$131.988 | 31% |
| Retainer | R$48.500 | 12% |
| Marketplace | R$38.100 | 9% |
| Skill packs upsell | R$4.921 | 1% |

### 3.4 Unit Economics

| Produto | Ticket Medio | CAC | LTV | LTV/CAC | Payback |
|---------|-------------|-----|-----|---------|---------|
| TurboClaw | R$307 setup | R$130 | R$307 | 2,4x | Imediato |
| Consultoria | R$2.944 total | R$250 | R$2.944 | 11,8x | Na sessao |
| Marketplace | R$197 | R$0 | R$197+ | infinito | Imediato |

---

## 4. Requirements

### 4.1 Functional Requirements

**Consultoria**

| ID | Requisito |
|----|-----------|
| FR-C1 | Landing page da consultoria com 11 blocos (hero, credenciais, problema, sessao, para quem, tiers, garantia, depoimentos, sobre, FAQ, CTA) conforme copy kit |
| FR-C2 | Formulario de discovery com 8 perguntas, enviado automaticamente apos pagamento, prazo 48h antes da sessao |
| FR-C3 | Checkout tier Essencial (R$1.500) com compra direta via Mercado Pago (Pix + cartao) |
| FR-C4 | Formulario de interesse para tiers Pro/Premium (nome, negocio, urgencia) com redirect para agendamento |
| FR-C5 | Integracao com Calendly (ou similar) para call de qualificacao de 20min (Pro/Premium) |
| FR-C6 | Sequencia automatizada de 5 e-mails pos-lead em 10 dias (Resend), com cadencia configuravel |
| FR-C7 | Pagina de confirmacao pos-compra com instrucoes de formulario de discovery e link de agendamento |
| FR-C8 | Sistema de retainer com checkout recorrente (R$400/R$700 mensal) via Mercado Pago |

**Infraestrutura de Tracking**

| ID | Requisito |
|----|-----------|
| FR-T1 | Meta Pixel instalado na landing de consultoria e TurboClaw (PageView, Lead, Purchase events) |
| FR-T2 | Conversions API (CAPI) configurada no backend para eventos de compra server-side |
| FR-T3 | Campos UTM no Supabase: `utm_source`, `utm_medium`, `utm_campaign`, `utm_content` na tabela customers |
| FR-T4 | Campo `referral_code` na tabela customers para rastreamento de afiliados |
| FR-T5 | Captura automatica de parametros UTM da URL e persistencia no formulario/checkout |

**Programa de Afiliados**

| ID | Requisito |
|----|-----------|
| FR-A1 | Tabela `affiliates` no Supabase (id, nome, email, canal, audiencia, slug, tier, status, comissoes acumuladas, created_at) |
| FR-A2 | Tabela `affiliate_commissions` no Supabase (id, affiliate_id, customer_id, produto, valor_venda, comissao_pct, comissao_valor, status, paid_at) |
| FR-A3 | Pagina `/afiliados` com formulario de cadastro (nome, canal, audiencia), aprovacao manual |
| FR-A4 | Geracao automatica de link de rastreamento unico por afiliado (slug no URL) |
| FR-A5 | Dashboard de afiliado: comissoes em tempo real, vendas, link de rastreamento, materiais |
| FR-A6 | Comissoes por produto: TurboClaw 40%, Consultoria 15%, Marketplace 30% |
| FR-A7 | Tres tiers de afiliados: Starter (cadastro), Pro (5+ vendas/mes), Partner (15+ vendas/mes) |

**Skills Marketplace**

| ID | Requisito |
|----|-----------|
| FR-M1 | 2-3 skill packs iniciais criados (Kit Imobiliaria, Kit Criador de Conteudo, Kit WhatsApp Business) |
| FR-M2 | Pagina de marketplace com catalogo de packs, preco, descricao e CTA de compra |
| FR-M3 | Checkout via Gumroad ou Lemon Squeezy (Fase 1) com entrega automatica por email |
| FR-M4 | 5 skills gratuitas publicadas no ClawHub como topo de funil organico |
| FR-M5 | Cross-sell banner no dashboard TurboClaw: "Quer mais skills para o seu agente?" |

**TurboClaw (Ajustes)**

| ID | Requisito |
|----|-----------|
| FR-TC1 | Cross-sell para consultoria no dashboard: popup apos 30 dias "Quer customizar para o seu negocio?" |
| FR-TC2 | Campos UTM e referral_code integrados no fluxo de checkout existente |
| FR-TC3 | Meta Pixel no fluxo de landing + checkout existente |

**Site Umbrella (Mes 2+)**

| ID | Requisito |
|----|-----------|
| FR-U1 | Site unificado apresentando as 3 frentes com navegacao clara entre elas |
| FR-U2 | Naming definitivo aplicado em todos os pontos de contato |

### 4.2 Non-Functional Requirements

| ID | Requisito |
|----|-----------|
| NFR1 | Landing da consultoria deve carregar em < 3s (Core Web Vitals green) |
| NFR2 | Sequencia de emails deve ser idempotente (nao reenviar se ja enviou) |
| NFR3 | Sistema de afiliados deve rastrear atribuicao por 30 dias (cookie/localStorage) |
| NFR4 | CAPI deve enviar eventos server-side em < 5s apos conversao |
| NFR5 | Toda comunicacao com usuario deve ser em pt-BR |
| NFR6 | Zero regressao no TurboClaw existente — todos os 42 E2E testes devem continuar passando |
| NFR7 | Budget de ferramentas: R$0 para MVP (usar Resend free tier, Gumroad, Calendly free) |
| NFR8 | Marco e unico operador da consultoria — max 10 sessoes/mes de capacidade |
| NFR9 | RLS ativo em tabelas de afiliados — afiliado so acessa seus proprios dados |

### 4.3 Compatibility Requirements

| ID | Requisito |
|----|-----------|
| CR1 | Novas paginas devem usar o mesmo design system do TurboClaw (dark mode, Inter, glassmorphism, Framer Motion) |
| CR2 | Novas tabelas no Supabase devem seguir o padrao existente (UUID PK, created_at, updated_at, RLS) |
| CR3 | Novos API routes devem seguir o padrao existente do Next.js BFF (Supabase Auth, error handling) |
| CR4 | Integracoes com Mercado Pago devem usar o mesmo access token e webhook patterns ja implementados |

---

## 5. User Interface Enhancement Goals

### 5.1 Integracao com UI Existente

Todas as novas paginas seguem o design system TurboClaw: dark mode exclusivo, Inter + JetBrains Mono, glassmorphism cards, Framer Motion transitions. Componentes UI existentes (`Button`, `Input`, `Card`, `Badge`, `Container`, `SectionHeader`) devem ser reutilizados.

### 5.2 Telas Novas/Modificadas

| # | Tela | Tipo | Frente |
|---|------|------|--------|
| 1 | Landing da Consultoria | Nova | Consultoria |
| 2 | Formulario de Discovery | Nova | Consultoria |
| 3 | Checkout Consultoria (Essencial) | Nova | Consultoria |
| 4 | Formulario de Interesse (Pro/Premium) | Nova | Consultoria |
| 5 | Confirmacao pos-compra consultoria | Nova | Consultoria |
| 6 | Pagina `/afiliados` (cadastro) | Nova | Afiliados |
| 7 | Dashboard do Afiliado | Nova | Afiliados |
| 8 | Pagina do Marketplace | Nova | Marketplace |
| 9 | Banner cross-sell no Dashboard TurboClaw | Modificacao | TurboClaw |
| 10 | Popup cross-sell consultoria (30 dias) | Modificacao | TurboClaw |
| 11 | Site Umbrella (Mes 2+) | Nova | Transversal |

### 5.3 Consistencia Visual

- Reutilizar componentes `ui/` existentes
- Manter palette Ouro Negro (gold/amber accents conforme commits recentes)
- Mobile-first responsivo
- Acessibilidade WCAG AA (aria-labels, contraste, keyboard nav)

---

## 6. Technical Constraints and Integration Requirements

### 6.1 Stack Existente (Nao Mudar)

| Camada | Tecnologia |
|--------|------------|
| Frontend | Next.js 14 (App Router) + Tailwind + Framer Motion |
| Auth | Supabase Auth (magic link + Google OAuth) |
| Database | Supabase PostgreSQL + Realtime |
| Pagamento | Mercado Pago (Pix + cartao + boleto) |
| Email | Resend |
| Deploy frontend | Vercel |
| Deploy backend | VPS (Express API + Docker) |

### 6.2 Integracoes Novas

| Integracao | Produto | Prioridade |
|-----------|---------|-----------|
| Meta Pixel (client-side) | Todas as landings | P0 (Semana 2) |
| Meta CAPI (server-side) | Backend checkout | P0 (Semana 2) |
| Calendly (embed ou link) | Consultoria | P0 (Semana 1) |
| Gumroad/Lemon Squeezy | Marketplace | P1 (Mes 2) |
| ClawHub CLI | Marketplace | P1 (Mes 2) |

### 6.3 Novas Tabelas no Supabase

```sql
-- Afiliados
CREATE TABLE affiliates (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES auth.users(id),
  name TEXT NOT NULL,
  email TEXT NOT NULL,
  channel TEXT,           -- instagram, youtube, blog, etc
  audience_size TEXT,
  slug TEXT UNIQUE NOT NULL,
  tier TEXT CHECK (tier IN ('starter', 'pro', 'partner')) DEFAULT 'starter',
  status TEXT CHECK (status IN ('pending', 'approved', 'rejected', 'suspended')) DEFAULT 'pending',
  total_earned NUMERIC DEFAULT 0,
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now()
);

-- Comissoes de afiliados
CREATE TABLE affiliate_commissions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  affiliate_id UUID REFERENCES affiliates(id) ON DELETE CASCADE,
  customer_id UUID REFERENCES customers(id),
  product TEXT CHECK (product IN ('turboclaw', 'consultoria', 'marketplace')),
  sale_value NUMERIC NOT NULL,
  commission_pct NUMERIC NOT NULL,
  commission_value NUMERIC NOT NULL,
  status TEXT CHECK (status IN ('pending', 'approved', 'paid')) DEFAULT 'pending',
  paid_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- Campos UTM adicionados na tabela customers existente
ALTER TABLE customers ADD COLUMN IF NOT EXISTS utm_source TEXT;
ALTER TABLE customers ADD COLUMN IF NOT EXISTS utm_medium TEXT;
ALTER TABLE customers ADD COLUMN IF NOT EXISTS utm_campaign TEXT;
ALTER TABLE customers ADD COLUMN IF NOT EXISTS utm_content TEXT;
ALTER TABLE customers ADD COLUMN IF NOT EXISTS referral_code TEXT;

-- Leads de consultoria
CREATE TABLE consulting_leads (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL,
  email TEXT NOT NULL,
  business TEXT,
  urgency TEXT CHECK (urgency IN ('low', 'medium', 'high')),
  tier_interest TEXT CHECK (tier_interest IN ('essencial', 'pro', 'premium')),
  status TEXT CHECK (status IN ('new', 'contacted', 'qualified', 'converted', 'lost')) DEFAULT 'new',
  discovery_form JSONB,         -- respostas do formulario de 8 perguntas
  session_date TIMESTAMPTZ,
  payment_id TEXT,
  utm_source TEXT,
  utm_medium TEXT,
  utm_campaign TEXT,
  referral_code TEXT,
  email_sequence_step INT DEFAULT 0,
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now()
);

-- Retainers de consultoria
CREATE TABLE consulting_retainers (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  lead_id UUID REFERENCES consulting_leads(id),
  plan TEXT CHECK (plan IN ('basico', 'pro')),
  price NUMERIC NOT NULL,
  status TEXT CHECK (status IN ('active', 'canceled', 'past_due')) DEFAULT 'active',
  mp_subscription_id TEXT,
  started_at TIMESTAMPTZ DEFAULT now(),
  canceled_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- RLS em todas as novas tabelas
-- Indices em affiliate_id, customer_id, status, slug, email
```

### 6.4 Risk Assessment

| Risco | Severidade | Mitigacao |
|-------|-----------|-----------|
| Meta Pixel/CAPI mal configurado desperdicando budget de ads | Alta | Testar eventos com Meta Events Manager antes de rodar qualquer campanha |
| Marco vira gargalo (max 10 sessoes/mes) | Media | Documentar playbook desde Mes 1, treinar operador a partir do Mes 4 |
| Formulario de discovery nao preenchido | Media | Taxa de remarcacao R$300, sessao so inicia com formulario respondido |
| OpenClaw muda estrutura e quebra instancias TurboClaw | Media | Docker image pinada, monitoramento do repo upstream |
| Gumroad/Lemon Squeezy taxa muito alta (5-10%) | Baixa | Migrar para marketplace proprio quando volume justificar (Mes 4+) |
| Demanda de consultoria baixa no inicio | Media | 3 sessoes de depoimento a R$750 geram prova social rapida |
| Churn alto no retainer de consultoria (~10%/mes) | Media | Entregar valor consistente, report mensal, renovacao proativa |

---

## 7. Epic Structure

### 7.1 Decisao de Estrutura

**6 epicos sequenciais**, priorizados por: (1) receita imediata, (2) pre-requisito tecnico para ads, (3) escala. Ordem alinhada com cronograma de trafego de 8 semanas.

A consultoria vem primeiro porque:
- 47% da receita total
- Zero dependencia de infraestrutura de tracking (vende direto com organico)
- Gera depoimentos que alimentam ads e landing
- Zero concorrencia — first-mover advantage

### 7.2 Resumo dos Epicos

| Epic | Titulo | Prazo | Valor |
|------|--------|-------|-------|
| **E1** | Consultoria MVP (Landing + Checkout + Formularios) | Semana 1-2 | Motor financeiro principal |
| **E2** | Tracking e Analytics (Pixel + CAPI + UTM) | Semana 2-3 | Pre-requisito para ads |
| **E3** | Programa de Afiliados | Semana 3-4 | Canal de aquisicao R$0 upfront |
| **E4** | Skills Marketplace (Packs + Gumroad + ClawHub) | Mes 2 | Escalabilidade passiva |
| **E5** | TurboClaw Cross-Sell + Ajustes | Mes 2 | Integrar no flywheel |
| **E6** | Site Umbrella + Brand Definitivo | Mes 2-3 | Unificacao do ecossistema |

---

## 8. Epic 1: Consultoria MVP

**Goal:** Ter a frente de consultoria vendendo em 2 semanas -- landing page, checkout, formularios, integracao com Calendly e sequencia de email.

**Integracao:** Reutiliza design system TurboClaw, Mercado Pago, Resend. Nao modifica TurboClaw.

### Story 1.1 — Landing Page da Consultoria

**Como** empresario que viu hype do OpenClaw,
**quero** uma pagina que explique a consultoria de implementacao,
**para que** eu entenda o que e, quanto custa e como contratar.

**Acceptance Criteria:**
1. Pagina com 11 blocos na ordem do copy kit (hero, credenciais, problema, sessao, para quem, tiers, garantia, depoimentos, sobre, FAQ, CTA)
2. 3 tiers apresentados com precos e diferenciais (R$1.500, R$2.500, R$4.000)
3. CTA Essencial leva para checkout direto
4. CTA Pro/Premium leva para formulario de interesse
5. Mobile-first responsivo
6. Design system TurboClaw (dark mode, Ouro Negro palette)
7. Meta tags OG para compartilhamento social

**Integration Verification:**
- IV1: Landing TurboClaw continua acessivel e funcional
- IV2: Componentes UI reutilizados sem duplicacao
- IV3: 42 E2E testes TurboClaw continuam passando

### Story 1.2 — Checkout Consultoria (Tier Essencial)

**Como** empresario decidido,
**quero** pagar R$1.500 diretamente na pagina,
**para que** minha sessao seja agendada rapidamente.

**Acceptance Criteria:**
1. Checkout via Mercado Pago (Pix + cartao) para R$1.500
2. Apos confirmacao de pagamento, redireciona para pagina de confirmacao
3. Pagina de confirmacao exibe: instrucoes do formulario de discovery + link do formulario + prazo (48h antes da sessao)
4. Registro criado na tabela `consulting_leads` com status 'converted'
5. Email de confirmacao enviado via Resend com instrucoes e link do formulario

**Integration Verification:**
- IV1: Webhook Mercado Pago reutiliza pattern existente
- IV2: Supabase RLS ativo na tabela consulting_leads

### Story 1.3 — Formulario de Interesse (Pro/Premium)

**Como** empresario interessado no tier Pro ou Premium,
**quero** preencher um formulario curto,
**para que** Marco entre em contato e agende minha call de qualificacao.

**Acceptance Criteria:**
1. Formulario com campos: nome, email, negocio, urgencia (baixa/media/alta), tier de interesse
2. Submit cria registro na tabela `consulting_leads` com status 'new'
3. Email de confirmacao imediato (E-mail 1 da sequencia) via Resend
4. Dados de UTM capturados da URL e salvos no lead

**Integration Verification:**
- IV1: Resend reutiliza configuracao existente
- IV2: Formulario captura e persiste UTM params

### Story 1.4 — Formulario de Discovery (8 Perguntas)

**Como** cliente que comprou a consultoria,
**quero** preencher um formulario detalhado sobre meu negocio,
**para que** Marco chegue preparado na sessao.

**Acceptance Criteria:**
1. 8 perguntas do plano da consultoria (negocio, dreno de tempo, tarefas, ferramentas, provider IA, API key, resultado esperado, contexto extra)
2. Acessivel via link unico enviado apos pagamento
3. Respostas salvas em `consulting_leads.discovery_form` (JSONB)
4. Status do lead atualizado para 'qualified' apos submit
5. Notificacao para Marco (email ou Telegram) quando formulario e submetido

**Integration Verification:**
- IV1: Link unico gerado com token seguro
- IV2: Formulario funciona em mobile

### Story 1.5 — Sequencia de Email Pos-Lead

**Como** lead que preencheu formulario de interesse,
**quero** receber emails uteis nos proximos 10 dias,
**para que** eu tome a decisao de agendar a call.

**Acceptance Criteria:**
1. 5 emails em cadencia automatica (0h, dia 2, dia 5, dia 7, dia 10) conforme copy kit
2. Enviados via Resend com personalizacao (nome do lead)
3. Tracking de qual step o lead esta (`email_sequence_step`)
4. Idempotencia: nao reenvia email ja enviado
5. Unsubscribe link funcional em cada email

**Integration Verification:**
- IV1: Resend API reutiliza pattern existente do welcome email
- IV2: Cron ou trigger Supabase para avancar steps

### Story 1.6 — Integracao Calendly

**Como** lead qualificado para tier Pro/Premium,
**quero** agendar uma call de 20 minutos diretamente,
**para que** o processo seja rapido e sem troca de emails.

**Acceptance Criteria:**
1. Embed ou link direto para Calendly na pagina de confirmacao e nos emails
2. Calendario mostra disponibilidade real do Marco
3. Confirmacao automatica por email (via Calendly) com link Zoom/Meet
4. Marco pode definir max 3 calls de qualificacao por semana

**Integration Verification:**
- IV1: Calendly free tier e suficiente para MVP
- IV2: Link funciona em mobile

---

## 9. Epic 2: Tracking e Analytics

**Goal:** Instalar toda a infraestrutura de rastreamento antes de rodar qualquer campanha paga. Nenhuma campanha de ads roda sem esse epic completo.

### Story 2.1 — Meta Pixel (Client-Side)

**Como** operador de marketing,
**quero** o Meta Pixel instalado em todas as landings,
**para que** eu possa criar publicos e rastrear conversoes.

**Acceptance Criteria:**
1. Pixel Meta instalado via `<script>` no `<head>` do layout principal
2. Eventos automaticos: PageView em todas as paginas
3. Eventos custom: Lead (formulario consultoria), Purchase (checkout TurboClaw), Purchase (checkout consultoria)
4. Pixel ID configuravel via env var `NEXT_PUBLIC_META_PIXEL_ID`
5. Funciona nas landings de consultoria e TurboClaw

**Integration Verification:**
- IV1: Verificar eventos no Meta Events Manager
- IV2: Nao impacta performance (script async/defer)

### Story 2.2 — Conversions API (CAPI)

**Como** operador de marketing,
**quero** eventos server-side no Meta,
**para que** o tracking funcione mesmo com ad blockers.

**Acceptance Criteria:**
1. CAPI configurada nos API routes de checkout (confirmacao de pagamento)
2. Eventos enviados: Purchase (com valor, moeda, content_name)
3. Event deduplication com event_id (evitar duplicata pixel + CAPI)
4. Access token do Meta configuravel via env var `META_CAPI_TOKEN`
5. Eventos enviados em < 5s apos conversao

**Integration Verification:**
- IV1: Eventos aparecem no Meta Events Manager com match de dedup
- IV2: API route de checkout continua respondendo em < 2s

### Story 2.3 — Campos UTM no Supabase

**Como** analista de marketing,
**quero** saber de onde cada cliente veio,
**para que** eu possa otimizar canais de aquisicao.

**Acceptance Criteria:**
1. Campos `utm_source`, `utm_medium`, `utm_campaign`, `utm_content`, `referral_code` adicionados na tabela `customers`
2. Captura automatica de params UTM da URL via hook client-side
3. Params persistidos em localStorage ate o checkout
4. Enviados junto com dados de checkout e salvos no customer
5. Mesma logica aplicada para `consulting_leads`
6. Migration Supabase criada

**Integration Verification:**
- IV1: Checkout TurboClaw existente continua funcionando
- IV2: UTM params nao afetam fluxo de checkout se ausentes

---

## 10. Epic 3: Programa de Afiliados

**Goal:** Ativar canal de aquisicao com custo zero upfront. 5 afiliados onboardados ate Semana 4.

### Story 3.1 — Schema de Afiliados no Supabase

**Como** sistema,
**preciso** das tabelas de afiliados e comissoes,
**para que** o programa funcione com rastreamento e pagamento.

**Acceptance Criteria:**
1. Tabela `affiliates` criada conforme schema da secao 6.3
2. Tabela `affiliate_commissions` criada conforme schema
3. RLS: afiliado so ve seus proprios dados
4. Indices em `slug`, `status`, `affiliate_id`
5. Migration Supabase criada e aplicada

### Story 3.2 — Pagina de Cadastro de Afiliados

**Como** potencial afiliado,
**quero** me cadastrar na pagina `/afiliados`,
**para que** eu possa comecar a promover os produtos.

**Acceptance Criteria:**
1. Rota `/afiliados` com formulario: nome, email, canal principal, tamanho da audiencia
2. Submit cria registro em `affiliates` com status 'pending'
3. Email de confirmacao enviado ao afiliado
4. Notificacao para Marco (para aprovacao manual)
5. Pagina explica o programa: comissoes, tiers, materiais, como funciona

### Story 3.3 — Dashboard do Afiliado

**Como** afiliado aprovado,
**quero** ver minhas vendas e comissoes em tempo real,
**para que** eu saiba quanto estou ganhando.

**Acceptance Criteria:**
1. Dashboard acessivel via login (Supabase Auth)
2. Exibe: link unico de rastreamento, vendas realizadas, comissoes pendentes/pagas, tier atual
3. Kit de materiais para download (banners, copy pronto)
4. Dados atualizados em tempo real via Supabase Realtime
5. Mobile responsivo

### Story 3.4 — Rastreamento de Afiliados no Checkout

**Como** sistema,
**preciso** atribuir vendas ao afiliado correto,
**para que** as comissoes sejam calculadas automaticamente.

**Acceptance Criteria:**
1. URL com `?ref={slug}` salva referral_code em cookie/localStorage (30 dias)
2. No checkout (TurboClaw e consultoria), referral_code e enviado junto
3. Apos confirmacao de pagamento, comissao calculada e criada em `affiliate_commissions`
4. Comissoes: TurboClaw 40%, Consultoria 15%, Marketplace 30%
5. Atribuicao first-click (primeiro afiliado que trouxe o lead ganha)

---

## 11. Epic 4: Skills Marketplace

**Goal:** Publicar primeiros skill packs e ter canal de venda funcionando ate Mes 2.

### Story 4.1 — Criacao dos Primeiros Skill Packs

**Como** criador de conteudo do marketplace,
**preciso** criar 2-3 skill packs iniciais,
**para que** haja catalogo minimo para venda.

**Acceptance Criteria:**
1. Kit Imobiliaria (R$247): 4 skills (triagem leads, qualificacao por regiao, relatorio visitas, follow-up)
2. Kit Criador de Conteudo (R$197): 4 skills (triagem DMs, FAQ, agendamento posts, metricas)
3. Kit WhatsApp Business (R$197): 4 skills (fluxo atendimento, triagem, respostas auto, escalacao)
4. Cada pack com SKILL.md, scripts/, README de instalacao
5. Versao simplificada de cada pack para publicacao gratuita no ClawHub

### Story 4.2 — Pagina do Marketplace

**Como** usuario de OpenClaw,
**quero** ver os skill packs disponiveis para compra,
**para que** eu possa expandir meu agente.

**Acceptance Criteria:**
1. Pagina listando todos os packs com: nome, descricao, skills incluidas, preco, CTA de compra
2. CTA redireciona para Gumroad/Lemon Squeezy (Fase 1)
3. Design system TurboClaw
4. Filtro por vertical/tipo (opcional MVP)

### Story 4.3 — Publicacao no ClawHub

**Como** operador do marketplace,
**quero** publicar skills gratuitas no ClawHub,
**para que** o ecossistema de 320k users descubra a marca.

**Acceptance Criteria:**
1. 5 skills basicas publicadas no ClawHub via `clawhub publish`
2. README de cada skill aponta para site da marca para versoes premium
3. Skills em portugues (gap 100% aberto no ClawHub)

---

## 12. Epic 5: TurboClaw Cross-Sell e Ajustes

**Goal:** Integrar TurboClaw no flywheel do ecossistema com cross-sell e tracking.

### Story 5.1 — Cross-Sell Consultoria no Dashboard

**Como** cliente TurboClaw ha mais de 30 dias,
**quero** saber que existe consultoria personalizada,
**para que** eu possa evoluir meu agente.

**Acceptance Criteria:**
1. Banner/popup exibido no dashboard apos 30 dias de conta criada
2. Texto: "Quer customizar o agente para o seu negocio? Conheca a consultoria 1-a-1"
3. Link leva para landing da consultoria
4. Dismissivel (nao aparece novamente apos fechar)
5. Nao intrusivo — nao bloqueia uso do dashboard

### Story 5.2 — Cross-Sell Marketplace no Dashboard

**Como** cliente TurboClaw,
**quero** saber que posso adicionar mais skills,
**para que** meu agente fique mais poderoso.

**Acceptance Criteria:**
1. Card ou banner sutil na secao "Meu Agente" do dashboard
2. Texto: "Quer mais skills para o seu agente?"
3. Link leva para pagina do marketplace
4. Sempre visivel (nao dismissivel — e feature, nao promo)

### Story 5.3 — Meta Pixel e UTM no Checkout TurboClaw

**Como** operador de marketing,
**quero** tracking completo no fluxo TurboClaw existente,
**para que** as campanhas sejam otimizadas.

**Acceptance Criteria:**
1. Meta Pixel eventos no fluxo TurboClaw: ViewContent (landing), InitiateCheckout (wizard step 4), Purchase (confirmacao)
2. UTM params capturados e salvos no customer (reutiliza Story 2.3)
3. referral_code do afiliado capturado e atribuido (reutiliza Story 3.4)

---

## 13. Epic 6: Site Umbrella e Brand Definitivo

**Goal:** Unificar as 3 frentes sob uma marca e site unico. Mes 2-3.

### Story 6.1 — Definicao de Naming e Brand

**Como** operador do ecossistema,
**preciso** de um nome definitivo,
**para que** toda comunicacao seja consistente.

**Acceptance Criteria:**
1. Nome definitivo escolhido e validado (dominio disponivel)
2. Logo e identidade visual basica
3. Aplicado em todos os pontos de contato existentes
4. [AUTO-DECISION] Naming: Marco decide pessoalmente. PM fornece lista de criterios. (reason: marca e decisao do fundador, nao do agente)

### Story 6.2 — Site Umbrella

**Como** visitante do ecossistema,
**quero** entender as 3 frentes e navegar entre elas,
**para que** eu encontre o produto certo para mim.

**Acceptance Criteria:**
1. Pagina principal com: visao geral do ecossistema, 3 frentes com descricao e CTA
2. Navegacao clara: TurboClaw (self-service), Consultoria (premium), Marketplace (skills)
3. Design system unificado
4. Mobile-first responsivo
5. Meta tags OG

---

## 14. KPIs e Criterios de Sucesso

### Por Fase

| Fase | Periodo | Meta Principal | Criterio de Done |
|------|---------|---------------|-----------------|
| 1 — Consultoria MVP | Semana 1-2 | Landing + checkout + 3 sessoes de depoimento | Landing no ar, 1 venda concluida, formulario discovery funcionando |
| 2 — Tracking + Afiliados | Semana 2-4 | Pixel + CAPI + 5 afiliados | Eventos aparecendo no Meta Events Manager, 5 afiliados aprovados |
| 3 — Marketplace + Cross-sell | Mes 2 | 2-3 packs publicados, banner no dashboard TC | Pelo menos 1 venda de skill pack |
| 4 — Umbrella + Escala | Mes 2-3 | Site unificado, ads rodando | Campanha 1 (TurboClaw) com ROAS > 3x |

### KPIs Globais

| KPI | Mes 3 | Mes 6 | Mes 12 |
|-----|-------|-------|--------|
| Receita mensal | R$18k | R$39k | R$55k |
| Sessoes consultoria/mes | 5 | 10 | 10+ |
| Clientes TC ativos | 30 | 90 | 181 |
| Retainers ativos | 1 | 5 | 20 |
| Skill packs no marketplace | 3 | 8 | 15 |
| Skills no ClawHub | 10 | 20 | 30 |
| Afiliados ativos | 20 | 40 | 60 |
| CAC blended TurboClaw | < R$150 | < R$120 | < R$100 |
| CAC blended Consultoria | < R$350 | < R$250 | < R$200 |
| ROAS Meta Ads total | > 3x | > 5x | > 6x |

---

## 15. Out of Scope (Fase 1)

| Item | Quando | Razao |
|------|--------|-------|
| Marketplace proprio (checkout proprio) | Mes 4+ | Gumroad suficiente para volume inicial |
| Membership de skills (R$97/mes) | Mes 8+ | Precisa de 15+ produtos no catalogo |
| Segundo operador de consultoria | Mes 4+ | Marco documenta playbook antes |
| WhatsApp como canal TurboClaw | v1.5+ | Foco em Telegram primeiro |
| App nativo (mobile) | Nenhum plano | Web responsivo e suficiente |
| Sistema de tickets para suporte | Mes 4+ | WhatsApp/email manual suficiente agora |
| Desconto/cupom automatizado | Mes 3+ | Nao necessario para lancamento |
| Multi-idioma | Nenhum plano | Mercado 100% BR |

---

## 16. Restricoes Criticas

1. **ClawPro e placeholder** — nome definitivo pendente para Mes 2
2. **Budget enxuto** — zero ferramentas pagas no MVP (Resend free, Calendly free, Gumroad free)
3. **Marco e unico operador** da consultoria — max 10 sessoes/mes, max 3/semana
4. **TurboClaw nao e reescrito** — apenas ajustes de tracking e cross-sell
5. **Consultoria nao depende do site umbrella** — pode ir ao ar antes
6. **Nenhum ad roda antes do tracking estar completo** (Epic 2 bloqueia campanhas)
7. **Zero regressao** — 42 E2E testes TurboClaw devem continuar passando em todos os epics

---

*PRD v1.0 — ClawPro Ecossistema. Consolidado a partir de 6 documentos de planejamento (Atlas, Traffic Masters, Copy Chief).*
*Qualquer decisao nao coberta aqui: escolher a opcao mais simples que funcione.*
