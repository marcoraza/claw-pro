# ClawPro — Roadmap de Execucao

**Versao:** 1.0 · **Data:** 17/03/2026
**Referencia:** `docs/claw-pro/prd.md`

---

## Visao Geral do Timeline

```
Semana 1-2     Semana 2-3     Semana 3-4     Mes 2          Mes 2-3        Mes 3+
   E1              E2             E3           E4+E5           E6           ESCALA
Consultoria    Tracking       Afiliados     Marketplace    Umbrella      Ads +
MVP            Pixel+CAPI     Programa      + Cross-sell   + Brand       Otimizacao
               UTM            5 afiliados   Skill packs    Naming        Retainers
```

---

## Fase 1 — Consultoria MVP (Semana 1-2)

**Epic 1 · Motor financeiro · 47% da receita**

| Story | Entrega | Executor | Dependencia | Criterio de Done |
|-------|---------|----------|-------------|-----------------|
| 1.1 | Landing page consultoria (11 blocos) | @dev | Copy kit pronto (docs/claw-pro/consultoria/copy-kit.md) | Pagina no ar, mobile OK, design system aplicado |
| 1.2 | Checkout tier Essencial (R$1.500) | @dev | Mercado Pago config existente | Pagamento Pix/cartao processado, lead criado no Supabase |
| 1.3 | Formulario de interesse (Pro/Premium) | @dev | — | Submit cria lead, email 1 disparado |
| 1.4 | Formulario de discovery (8 perguntas) | @dev | 1.2 (link pos-pagamento) | Respostas salvas em JSONB, notificacao para Marco |
| 1.5 | Sequencia de 5 emails (Resend) | @dev | 1.3 (leads criados) | 5 emails configurados com cadencia automatica |
| 1.6 | Integracao Calendly | @dev | — | Embed/link funcional, disponibilidade real |

**Marco (operacional, paralelo ao dev):**
- [ ] Configurar conta Calendly free
- [ ] Agendar 3 sessoes de depoimento (R$750 cada) com pessoas da rede
- [ ] Preparar script da call de qualificacao (copy-kit.md secao 3)

**Milestone:** Landing no ar + 1 sessao de depoimento realizada

---

## Fase 2 — Tracking e Analytics (Semana 2-3)

**Epic 2 · Pre-requisito BLOQUEANTE para ads**

| Story | Entrega | Executor | Dependencia | Criterio de Done |
|-------|---------|----------|-------------|-----------------|
| 2.1 | Meta Pixel (client-side) | @dev | NEXT_PUBLIC_META_PIXEL_ID definido | PageView + Lead + Purchase visivel no Events Manager |
| 2.2 | Conversions API (CAPI) | @dev | META_CAPI_TOKEN definido | Eventos server-side com dedup aparecendo no Events Manager |
| 2.3 | Campos UTM no Supabase | @dev | Migration criada | UTM params capturados e persistidos em customers e consulting_leads |

**GATE:** Nenhuma campanha paga roda ate que o Events Manager mostre eventos reais com dedup funcionando.

**Marco (paralelo):**
- [ ] Criar conta Meta Business (se nao tem)
- [ ] Criar pixel no Events Manager
- [ ] Gerar CAPI access token
- [ ] Realizar 2-3 sessoes de depoimento e coletar video/texto

**Milestone:** Eventos de Purchase aparecendo no Events Manager (client + server com dedup)

---

## Fase 3 — Programa de Afiliados (Semana 3-4)

**Epic 3 · Canal de aquisicao com custo zero upfront**

| Story | Entrega | Executor | Dependencia | Criterio de Done |
|-------|---------|----------|-------------|-----------------|
| 3.1 | Schema afiliados no Supabase | @dev | — | Tabelas criadas, RLS ativo, migration aplicada |
| 3.2 | Pagina `/afiliados` (cadastro) | @dev | 3.1 | Formulario funcional, email de confirmacao, notificacao Marco |
| 3.3 | Dashboard do afiliado | @dev | 3.1, 3.2 | Login, metricas, link unico, kit materiais |
| 3.4 | Rastreamento no checkout | @dev | 3.1, 2.3 (UTM pronto) | referral_code trackeado, comissao calculada automaticamente |

**Marco (paralelo):**
- [ ] Identificar e convidar 5 primeiros afiliados
- [ ] Criar kit de materiais (banners, copy, video demo)
- [ ] Aprovar afiliados manualmente
- [ ] Depoimentos prontos → publicar na landing

**Milestone:** 5 afiliados aprovados com links unicos ativos

---

## Fase 4 — Marketplace + Cross-Sell (Mes 2)

**Epic 4 + Epic 5 · Rodando em paralelo**

### Epic 4 — Skills Marketplace

| Story | Entrega | Executor | Dependencia | Criterio de Done |
|-------|---------|----------|-------------|-----------------|
| 4.1 | 2-3 skill packs criados | Marco | — | Packs com SKILL.md, scripts, README |
| 4.2 | Pagina do marketplace | @dev | 4.1 (packs prontos) | Catalogo com packs, preco, CTA Gumroad |
| 4.3 | Publicacao no ClawHub | Marco | 4.1 | 5 skills gratuitas publicadas, README apontando para site |

### Epic 5 — TurboClaw Cross-Sell

| Story | Entrega | Executor | Dependencia | Criterio de Done |
|-------|---------|----------|-------------|-----------------|
| 5.1 | Cross-sell consultoria (popup 30 dias) | @dev | E1 completo (landing consultoria existe) | Popup dismissivel, nao intrusivo |
| 5.2 | Cross-sell marketplace (banner) | @dev | E4 completo (marketplace existe) | Banner na secao "Meu Agente" |
| 5.3 | Pixel + UTM no checkout TC | @dev | E2 completo (tracking pronto) | Eventos Meta no fluxo TurboClaw |

**Marco (paralelo):**
- [ ] Configurar conta Gumroad/Lemon Squeezy
- [ ] Fazer upload dos packs com entrega automatica
- [ ] Comecar a publicar no ClawHub (clawhub publish)

**Milestone:** 1 venda de skill pack realizada + cross-sell ativo no dashboard TC

---

## Fase 5 — Site Umbrella + Brand (Mes 2-3)

**Epic 6 · Unificacao do ecossistema**

| Story | Entrega | Executor | Dependencia | Criterio de Done |
|-------|---------|----------|-------------|-----------------|
| 6.1 | Naming e brand definitivo | Marco | — | Nome escolhido, dominio registrado |
| 6.2 | Site umbrella | @dev | 6.1 (nome definido), E1+E4 (landings existem) | Pagina unificada com 3 frentes, design system, mobile OK |

**Marco (paralelo):**
- [ ] Decidir nome definitivo (criterios: disponibilidade .com.br, memoravel, BR-friendly)
- [ ] Encomendar logo e identidade basica
- [ ] Registrar dominio

**Milestone:** Site umbrella no ar com navegacao para 3 frentes

---

## Fase 6 — Escala (Mes 3+)

**Nao e epic de desenvolvimento — e operacao de marketing + ads**

| Semana | Acao | Budget | Meta |
|--------|------|--------|------|
| 5 | Campanha 1: TurboClaw Conversao (Meta Ads) | R$1.375/mes | Learning phase 7 dias |
| 6 | Campanha 2: Consultoria Lead Gen | R$875/mes | Learning phase 7 dias |
| 7 | Analise resultados semana 5-6, otimizar publicos/criativos | — | ROAS > 3x |
| 8 | Campanha 3: Remarketing cruzado entre frentes | R$250/mes | Cross-sell com dados reais |

**A partir do Mes 3:**
- Escalar budget de ads conforme ROAS
- Primeiro retainer de consultoria ofertado
- Skills Marketplace com 3+ packs ativos
- Avaliar segundo operador para consultoria (Mes 4+)
- Avaliar marketplace proprio vs Gumroad (Mes 4+)

---

## Dependencias Criticas (Blocking Chain)

```
E2 (Tracking) ──BLOQUEIA──→ Campanhas de Ads (Fase 6)
E1 (Consultoria) ──BLOQUEIA──→ E5.1 (Cross-sell consultoria)
E4 (Marketplace) ──BLOQUEIA──→ E5.2 (Cross-sell marketplace)
E2 (UTM) ──BLOQUEIA──→ E3.4 (Rastreamento afiliados)
E6.1 (Naming) ──BLOQUEIA──→ E6.2 (Site Umbrella)
```

**Nao ha dependencia entre:**
- E1 (Consultoria) e E2 (Tracking) — consultoria pode vender via organico sem ads
- E1 (Consultoria) e E3 (Afiliados) — afiliados nao sao pre-requisito
- E4 (Marketplace) e E3 (Afiliados) — marketplace pode vender sem afiliados inicialmente

---

## Marcos de Receita Esperados

| Marco | Quando | Receita Mensal | Receita Acumulada |
|-------|--------|---------------|-------------------|
| Primeira venda consultoria (depoimento R$750) | Semana 2 | R$2.250 | R$2.250 |
| Consultoria preco cheio (R$1.500+) | Semana 3+ | R$6.675 | R$8.925 |
| Primeira venda TurboClaw via afiliado | Semana 5 | +R$1.535 | ~R$10.460 |
| Primeira venda marketplace | Mes 2 | +R$300 | ~R$17.000 |
| Ads rodando + retainers | Mes 3 | R$18.424 | ~R$33.000 |

---

## Ramp de Equipe

| Periodo | Quem faz o que |
|---------|---------------|
| Mes 1-3 | Marco: consultoria + conteudo organico. @dev: todo o desenvolvimento. |
| Mes 4+ | Marco: consultoria + estrategia. Operador treinado: executa playbook da consultoria. |
| Mes 6+ | Considerar VA para gestao de afiliados e suporte. |
| Mes 8+ | Considerar segundo dev para marketplace proprio. |

---

## Checklist Pre-Lancamento por Fase

### Fase 1 (Semana 1-2)
- [ ] Landing consultoria no ar (11 blocos)
- [ ] Checkout Essencial (R$1.500) funcionando
- [ ] Formulario de interesse (Pro/Premium) funcional
- [ ] Formulario de discovery (8 perguntas) funcional
- [ ] 5 emails configurados no Resend
- [ ] Calendly configurado e funcional
- [ ] 3 sessoes de depoimento agendadas

### Fase 2 (Semana 2-3)
- [ ] Meta Pixel instalado e disparando
- [ ] CAPI configurada e enviando server-side
- [ ] Campos UTM no Supabase e capturando
- [ ] Events Manager mostrando eventos com dedup
- [ ] Conta Meta Business criada

### Fase 3 (Semana 3-4)
- [ ] Tabelas afiliados criadas no Supabase
- [ ] Pagina /afiliados no ar
- [ ] Dashboard do afiliado funcional
- [ ] Rastreamento no checkout funcionando
- [ ] 5 afiliados aprovados
- [ ] Kit de materiais criado
- [ ] Depoimentos publicados na landing

### Fase 4 (Mes 2)
- [ ] 2-3 skill packs criados
- [ ] Pagina marketplace no ar
- [ ] Gumroad/Lemon Squeezy configurado
- [ ] 5 skills no ClawHub
- [ ] Cross-sell no dashboard TurboClaw
- [ ] Pixel + UTM no fluxo TurboClaw

### Fase 5 (Mes 2-3)
- [ ] Nome definitivo escolhido
- [ ] Dominio registrado
- [ ] Site umbrella no ar
- [ ] Brand aplicado em todos os pontos

### Fase 6 (Mes 3+)
- [ ] Campanha 1 TurboClaw rodando
- [ ] Campanha 2 Consultoria rodando
- [ ] ROAS > 3x confirmado
- [ ] Campanha 3 Remarketing ativa
- [ ] Primeiro retainer ativo

---

*Roadmap v1.0 — ClawPro Ecossistema. 6 fases em 8 semanas + escala contínua.*
*Prioridade: receita primeiro, tracking segundo, escala terceiro.*
