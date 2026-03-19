# TurboClaw — Plano da Frente SaaS
**Parte do ecossistema:** ClawPro (guarda-chuva)
**Versão:** 1.0 · **Data:** 17/03/2026

---

## O que é

Plataforma que provisiona, configura e gerencia instâncias OpenClaw para clientes não-técnicos. O cliente nunca vê um terminal — paga, escolhe nome para o assistente, conecta a IA preferida e recebe um agente funcionando no Telegram em menos de 5 minutos.

**Papel no ecossistema:** porta de entrada de baixo ticket, gerador de volume e dados, upsell natural para consultoria e marketplace.

---

## Público-alvo

Pessoas físicas tech-curious que viram OpenClaw no Twitter, Reddit ou YouTube, tentaram instalar, desistiram. Querem experimentar sem aprender Docker, Node.js ou terminal.

**Não é o público:** empresários que querem customização para o negócio → esses vão para a consultoria.

---

## Modelo de Receita

| Tipo | Descrição | Relevância |
|------|-----------|------------|
| **Setup fee (principal)** | Pagamento único pela instalação | Receita e margem real |
| **Mensalidade (operacional)** | Sustentação do servidor ativo | Cobre custo variável de infra |

> A mensalidade (R$50 médio) cobre exatamente o custo variável por cliente (R$50). O lucro do TurboClaw está inteiramente no setup fee.

---

## Tiers e Preços

| Tier | Setup Fee | Mensalidade | Mix esperado | Inclui |
|------|-----------|-------------|--------------|--------|
| Essencial | R$197 | R$39/mês | 60% | Instalação + personalidade base + 1 integração |
| Pro | R$397 | R$59/mês | 30% | Essencial + skills avançadas + integrações múltiplas |
| Premium | R$697 | R$89/mês | 10% | Pro + suporte prioritário + configuração avançada |
| **Média ponderada** | **R$307** | **R$50** | — | — |

---

## Arquitetura Técnica

- **Frontend/Auth:** Next.js + Tailwind + Supabase + Vercel
- **Backend:** Express API + Dockerode (multi-tenant no VPS)
- **Pagamento:** Mercado Pago (Pix + cartão + boleto)
- **Auth:** Magic link + Google OAuth
- **Dashboard:** Sidebar + tab routing (Overview, Meu Agente, Métricas, Plano, Ajustes)
- **VPS:** 1 VPS 8GB ($17/mês), multi-tenant Docker

---

## Unit Economics

| Métrica | Valor |
|---------|-------|
| Setup fee médio | R$307 |
| Custo variável/cliente/mês | R$50 |
| Margem sobre mensalidade | R$0 (break-even) |
| **Margem sobre setup** | **R$307 (100%)** |
| CAC Meta Ads | ~R$150 |
| CAC via afiliados | ~R$123 (40% de R$307) |
| **CAC blended** | **~R$130** |
| **LTV** | **R$307** |
| **LTV/CAC** | **2,4x** |
| Payback | Imediato (setup > CAC no ato) |

---

## Aquisição

### Programa de Afiliados
- Comissão: **40% do setup fee** por venda
- Por venda: R$78 (Essencial) / R$159 (Pro) / R$279 (Premium)
- Acesso: todos os afiliados nível Starter podem promover TurboClaw
- Material: landing page, banners, copy pronto, link rastreável

### Meta Ads
- Budget alocado: **R$1.375/mês** (55% do total de R$2.500)
- Campanha de conversão (compra direta do setup)
- Ad sets: interesse em IA/OpenClaw, lookalike de compradores, retargeting visitantes
- CAC target: R$150

### Orgânico
- Conteúdo de demo: "veja como é fácil" — comparação DIY vs TurboClaw
- Referral: cada cliente ativo tem incentivo para indicar

---

## Cross-Sell

| De | Para | Gatilho |
|----|------|---------|
| TurboClaw | Marketplace | "Quer mais skills?" — banner no dashboard |
| TurboClaw | Consultoria | "Quer customizar para o seu negócio?" — popup após 30 dias |

---

## Projeção de Clientes

| Mês | Novos | Ativos (5% churn) | Setup Revenue | Mensalidade |
|-----|-------|-------------------|---------------|------------|
| 1 | 5 | 5 | R$1.535 | R$250 |
| 2 | 10 | 15 | R$3.070 | R$750 |
| 3 | 15 | 29 | R$4.605 | R$1.450 |
| 6 | 22 | 88 | R$6.754 | R$4.400 |
| 12 | 22 | 181 | R$6.754 | R$9.050 |

---

## Riscos Específicos

| Risco | Mitigação |
|-------|-----------|
| OpenClaw muda estrutura e quebra instâncias | Docker image pinada + monitoramento do repo |
| Concorrente reduz preço | Diferenciação via qualidade + brand umbrella |
| Churn alto (>5%) | Melhorar onboarding + email cadence pós-setup |
| VPS satura com muitos clientes | Escalar VPS ou adicionar segundo servidor |

---

*Ver `../plano-geral.md` para contexto do ecossistema completo.*
