# marfin-cold-email-engine

Cold email personalizado pra um prospect específico, em 90 segundos, dentro do Claude.

## O que essa Skill faz

- Recebe URL ou nome do prospect + seu ICP + sua oferta em 1 frase cada
- Gera 3 variações com hooks diferentes: pain, social proof, curiosity
- Cada email ≤ 90 palavras, com subject ≤ 7 palavras, pedido específico no fim
- Recomenda qual mandar com 1 frase de justificativa

Recusa modo "mass mail". Cada output é ancorado em algo verificável sobre o prospect.

## Demo

Pergunta no Claude: *"cold email pra <empresa>"* + cole o hook (post recente, vaga aberta, etc).

Resposta em ~90s: 3 variações + recomendação. [Veja exemplo](examples/example-output-pt.md)

## Como instalar

### One-liner (recomendado)

```bash
curl -sSL https://raw.githubusercontent.com/marfin-lab/skills/main/install.sh | sh -s cold-email-engine
```

### Manual

```bash
git clone https://github.com/marfin-lab/skills-marfin-cold-email-engine.git ~/.claude/skills/marfin-cold-email-engine
```

## Pré-requisitos

- Nenhum obrigatório.
- **Recomendado:** Web Search built-in do Claude — fetcha o site do prospect e ancora o hook em algo real (post, vaga, lançamento). Sem isso, você precisa colar o hook manualmente.

## Quando usar

- Outbound 1:1 pra prospect qualificado
- Re-engagement de lead que sumiu
- Pedido de intro pra alguém específico

## Quando NÃO usar

- Mass email (defeats o propósito da skill — vai parecer spam)
- Newsletter (use seu provedor de email)
- Follow-up de conversa existente (use [marfin-call-postmortem](#))

## Princípios da copy

- Linha 1: por que ELE, agora (não você)
- Linha 2-3: o que você sabe sobre o problema dele (específico)
- Linha 4: o que você faz, em 1 frase de outcome (não feature)
- Linha 5: pedido específico (15min? deck? intro?)
- Sem "I hope this finds you well", "just following up", "leverage", "10x"

## Roadmap

- [x] V1: 3 variações + recomendação + subject
- [ ] V2: 1-2 follow-ups automáticos (caso o primeiro não responda)
- [ ] V3: integração com `marfin-call-postmortem` pra fechar o loop pós-call

## Licença

MIT.

## Outras Skills da Marfin

| Skill | O que faz |
|---|---|
| [marfin-pitch-deck-roast](https://github.com/marfin-lab/skills-marfin-pitch-deck-roast) | Crítica brutal do seu pitch deck |
| [marfin-competitor-teardown](https://github.com/marfin-lab/skills-marfin-competitor-teardown) | Teardown público de concorrente |
| [marfin-weekly-review](https://github.com/marfin-lab/skills-marfin-weekly-review) | Weekly review pra founder solo |
| [marfin-prd-writer](https://github.com/marfin-lab/skills-marfin-prd-writer) | PRD a partir de ideia bruta |

Catálogo completo: https://marfin.co/skills

---

Feito por [Marfin Co.](https://marfin.co) — venture builder de produtos com IA.
