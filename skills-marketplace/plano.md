# Skills Marketplace — Plano da Frente de Produtos Digitais
**Parte do ecossistema:** ClawPro (guarda-chuva)
**Versão:** 1.0 · **Data:** 17/03/2026

---

## O que é

Loja de skill packs para OpenClaw — automações, integrações e capacidades prontas para instalar. Produto autônomo com margem de 100% (produto digital, sem COGS) e canal de upsell natural das outras duas frentes.

**Papel no ecossistema:** escalabilidade passiva. Cada consultoria realizada alimenta o marketplace com novas skills. Cada skill publicada no ClawHub atrai novos leads para o ecossistema.

---

## Público-alvo

Pessoas que **já têm OpenClaw funcionando** (via consultoria, via TurboClaw ou via instalação própria) e querem expandir as capacidades do agente.

**Diferença em relação às outras frentes:** não é para quem está começando. É para quem já tem e quer mais.

---

## Catálogo de Produtos

### Tipos de produto

| Tipo | Descrição | Faixa de Preço |
|------|-----------|----------------|
| Skill individual | Uma automação específica pronta | R$47–97 |
| Pack vertical | 4–6 skills para um segmento de negócio | R$197–297 |
| Pack de integração | Skills para uma ferramenta específica | R$97–197 |
| Template de personalidade | Configuração completa de soul/personalidade | R$47–97 |

### Packs verticais planejados

| Pack | Skills incluídas | Preço | Prioridade |
|------|-----------------|-------|-----------|
| **Kit Imobiliária** | Triagem de leads, qualificação por região, relatório de visitas, follow-up automático | R$247 | Alta |
| **Kit Criador de Conteúdo** | Triagem de DMs, FAQ, agendamento de posts, relatório de métricas | R$197 | Alta |
| **Kit Clínica Médica** | Confirmação de consultas, pré-anamnese, lembretes, retorno pós-consulta | R$247 | Média |
| **Kit E-commerce** | Status de pedido, rastreamento, suporte pós-venda, devoluções | R$247 | Média |
| **Kit Consultoria/Serviços** | Qualificação de leads, proposta automatizada, follow-up, NPS | R$247 | Média |
| **Kit WhatsApp Business** | Fluxo de atendimento, triagem, respostas automáticas, escalação | R$197 | Alta |
| **Kit Google Workspace** | Gmail automático, Calendar, Sheets, Drive | R$247 | Média |
| **Kit Relatórios** | Relatório diário vendas, pipeline, métricas personalizadas | R$197 | Baixa |

---

## Mecanismo de Upsell

### Via Consultoria
- Cliente pode adicionar packs durante o checkout (antes da sessão)
- Marco chega preparado e instala durante a mesma call
- Attach rate esperado: **30%** dos clientes de consultoria
- Ticket adicional médio: **R$197**

### Via TurboClaw
- Banner no dashboard: "Quer mais skills para o seu agente?"
- Link direto para marketplace
- Usuários que já têm OpenClaw funcionando são os mais propensos a comprar

### Compra direta
- Clientes chegam via ClawHub, indicação ou busca orgânica
- Sem necessidade de passar pelas outras frentes

---

## ClawHub — Canal de Autoridade

O ClawHub é o marketplace oficial do OpenClaw (320k users), atualmente com **zero skills de terceiros publicadas**.

**Estratégia:**
- Publicar versões gratuitas/básicas das skills no ClawHub
- README de cada skill aponta para o site da marca: "skills avançadas e instalação completa → [site]"
- Não monetizar pelo ClawHub — usá-lo como topo de funil orgânico
- Ser o primeiro publisher ocupar os rankings de busca semântica sem disputar

**Skills para publicar primeiro no ClawHub:**
1. Skills com maior volume de busca (cotidiano: clima, agenda, e-mail básico)
2. Skills em português (nenhuma existe hoje — gap 100% aberto)
3. Skills de negócio básicas (atendimento simples, FAQ)

**Meta:** 10 skills no ClawHub até mês 2, 30 até mês 12.

---

## Estrutura Técnica do Skill

Uma skill para OpenClaw é tecnicamente simples — uma pasta com um arquivo `SKILL.md`:

```
/skills/
  nome-da-skill/
    SKILL.md         ← obrigatório
    scripts/         ← código executável (opcional)
    references/      ← docs carregados sob demanda (opcional)
    assets/          ← templates, assets (opcional)
```

**Publicação no ClawHub:**
```bash
clawhub publish ./nome-da-skill --slug nome-da-skill --version 1.0.0
```

**Distribuição no marketplace próprio:**
- Arquivo zip com a pasta da skill + guia de instalação em PDF
- Entrega automática por e-mail pós-pagamento (Gumroad ou Lemon Squeezy no início)

---

## Plataforma de Venda

### Fase 1 (Meses 2–3): Gumroad ou Lemon Squeezy
- Setup em horas, sem desenvolvimento
- Integração nativa com Stripe/cartão
- 5–10% de taxa por transação — aceitável no início

### Fase 2 (Mês 4+): Marketplace Próprio
- Migrar para site próprio quando houver 5+ packs publicados
- Controle total sobre checkout, upsells, afiliados
- Integração com Mercado Pago (Pix)

### Possível Fase 3 (Mês 8+): Membership
- Assinatura mensal R$97/mês para acesso ao catálogo completo
- Faz sentido quando o catálogo tiver 15+ produtos
- Modelo similar ao n8n/Make — valor no catálogo, não por item

---

## Unit Economics

| Métrica | Valor |
|---------|-------|
| Ticket médio | R$197 |
| COGS | R$0 (produto digital) |
| Taxa plataforma (fase 1) | ~R$10–20 (5–10%) |
| **Margem bruta** | **~90–100%** |
| CAC via upsell | R$0 |
| CAC via ClawHub | R$0 |
| **LTV/CAC** | **∞** |

---

## Projeção de Receita

| Mês | Receita | Observação |
|-----|---------|------------|
| 1 | R$0 | Marketplace não lançado ainda |
| 2 | R$0 | Em preparação |
| 3 | R$300 | 2 packs, vendas iniciais (upsell consultoria) |
| 4 | R$800 | 3 packs, crescimento |
| 5 | R$1.500 | — |
| 6 | R$2.500 | — |
| 9 | R$4.500 | — |
| 12 | R$6.000 | 8+ packs ativos |
| **Total 12M** | **R$38.100** | — |

---

## Flywheel de Conteúdo

```
Consultoria (sessão) → gera skills customizadas
         ↓
Skills validadas → entram no marketplace como packs pagos
         ↓
Versão básica → publicada no ClawHub (gratuita)
         ↓
ClawHub → usuários descobrem a marca → chegam no site
         ↓
Compram marketplace ou contratam consultoria
         ↓  (loop)
```

---

## Roadmap do Marketplace

| Semana/Mês | Ação |
|------------|------|
| Mês 2, semana 1 | Criar Kit Imobiliária e Kit Criador de Conteúdo |
| Mês 2, semana 2 | Publicar no Gumroad, integrar checkout no site |
| Mês 2, semana 3 | Publicar primeiras 5 skills gratuitas no ClawHub |
| Mês 3 | Criar Kit WhatsApp Business, Kit Clínica |
| Mês 4 | Migrar para site próprio se volume justificar |
| Mês 4–5 | Criar 3 packs adicionais com base em pedidos dos clientes de consultoria |
| Mês 8 | Avaliar modelo Membership (R$97/mês) |

---

*Ver `../plano-geral.md` para financeiro consolidado e KPIs do ecossistema.*
