---
name: marfin-cold-email-engine
description: "Use this skill when the user wants to write a cold email to a specific prospect. Triggers include: 'cold email pra esse prospect', 'escreve um email pro [empresa]', 'me ajuda com outbound', 'cold outreach', 'preciso mandar email frio pra X', 'write a cold email to Y'. The skill takes the prospect URL or name + the user's ICP and offer, then generates 3 variations (pain-hook, social-proof-hook, curiosity-hook) with subject lines under 90 words each. Do NOT use this skill for mass mailing (parece spam), newsletter writing (use the user's email provider), or follow-up emails to existing leads (use call-postmortem instead)."
license: MIT
version: 1.0.0
author: Marfin Co.
requires_mcp: []
optional_mcps:
  - name: Web Search (Claude built-in)
    why: "fetches the prospect's site to extract a real, specific hook (recent post, hire, launch). Without it, hooks become generic"
  - name: Filesystem
    why: "reads previous winning emails from disk to match the user's voice"
---

# Marfin Cold Email Engine

## What this skill does

Takes a specific prospect (URL or company + role) plus the user's ICP and offer in one sentence each, and generates 3 cold email variations with different hooks: pain point, social proof, and curiosity. Each variation is under 90 words, ends with a specific ask (not "let me know what you think"), and includes a subject line.

The skill refuses to write mass-template emails. Every output is anchored on something verifiable about the prospect.

## When to use

Trigger on:
- "cold email pra <empresa>" / "outreach pro <fulano>"
- "escreve um email pro CEO da <X>"
- "me ajuda a falar com <empresa>"
- "cold email" / "cold outreach" / "outbound 1:1"
- User pastes a URL and asks for an email

Do NOT trigger for:
- Mass campaigns (sounds like spam, defeats the skill's purpose)
- Newsletter or sequenced flows (use the user's email provider)
- Follow-ups to existing conversations (use `marfin-call-postmortem` for next-step emails)
- Sales pitch decks (use `marfin-pitch-deck-roast`)

## Required setup

None. Pure-LLM skill.

If the user's Claude has Web Search enabled, the skill fetches the prospect's site to anchor on a real, recent hook (post, hire, launch). Without it, ask the user to paste a paragraph from the prospect's site or a recent post.

## Workflow

### Step 1: Anchor the prospect

Confirm three things:

- The prospect: URL or company + role (e.g., "CEO da Acme")
- Why now: a specific, verifiable hook (recent post, funding, hire, launch)
- The user's ICP in 1 sentence (who they help, in what context)
- The user's offer in 1 sentence (what they do, in concrete outcome)

If the user passes a URL and Web Search is available, fetch the site and find ONE recent signal. If not available, ask the user for a hook.

### Step 2: Identify the strongest hook angle

Look at the prospect signal and pick which of the 3 hook types fits:

- **Pain hook** — they posted about a frustration, opened a role, mentioned a metric problem
- **Social proof hook** — they raised, won, hired someone notable, shipped something
- **Curiosity hook** — they made a contrarian claim, took a strange technical decision

Generate ALL THREE. The user picks. Do not assume.

### Step 3: Draft the 3 variations

For each variation, follow this structure:

```
Subject: <≤ 7 words, no clickbait, no question marks unless natural>

Linha 1: por que ELE, agora (the hook, in 1 sentence)
Linha 2-3: o que você sabe sobre o problema dele (specific, not generic)
Linha 4: o que você faz, em 1 frase (concrete outcome, not feature)
Linha 5: pedido específico (15min? deck? intro?)

— <signature>
<email>
<URL>
```

Word limit: 90 words per email body, hard. Each line ≤ 14 words.

### Step 4: Output the 3 variations + recommend one

Output all three, clearly labeled (Variation 1/2/3 + hook type). Recommend ONE based on:

- If the prospect is a founder → curiosity or pain
- If the prospect is a sales/biz leader → social proof
- If unsure → pain (highest reply rate in cold)

Explain in 1 sentence why you recommended that one.

### Step 5: Offer next steps

End with at most 2 next steps:

- "Quer que eu escreva 1-2 follow-ups caso ele não responda?"
- "Quer que eu valide o deck/landing antes de mandar?" (link to `marfin-landing-copy-engine` ou `marfin-pitch-deck-roast`)

## Output rules (Marfin voice)

- **Português brasileiro por padrão.** Inglês se o usuário escrever em inglês.
- No emojis. No hashtags. No em-dash (— ou –). Use dois pontos ou parênteses.
- **Banned phrases** (instant rewrite if found): "I hope this finds you well", "espero que esteja bem", "just following up", "circling back", "synergy", "leverage", "unlock", "10x", "best regards" (use só "—" + nome).
- **Don't sell.** First line is about THEM, not you.
- **Don't praise.** "Adorei seu post" é morto. Cite uma observação específica do post.
- **Don't pitch features.** Outcome ("transforma 10h/sem em 1h/sem") not feature ("workflow automatizado").
- **Specific over vague.** Bad: "vi que vocês cresceram". Good: "vi que vocês contrataram 3 SDRs em janeiro".
- **Subject lines:** ≤ 7 words. No "Re:" or "Fwd:" tricks. No questions unless natural.

## Error handling

| Situation | Action |
|---|---|
| User does not have Web Search and refuses to paste prospect info | Tell them: "Sem hook real, o email vira template. Cole 1 parágrafo do site/post deles ou desativa essa skill por hora." Stop. |
| User asks for "10 emails for these 10 prospects" | Refuse mass mode: "Cada cold tem que ser 1:1. Faça 1 por vez aqui. Pra volume, exporta no formato e ajusta nome/empresa fora." |
| User's offer is vague ("we do consulting") | Push back: "'Consulting' é genérico. O que vocês mudam pra cliente em 90 dias?" Wait for concrete answer. |
| Prospect site is unreachable / 404 | Skip web fetch, ask user for any 1 fact about prospect. Continue. |
| User wants email > 200 words | Refuse: "Cold > 100 palavras tem reply rate < 3%. Vamos cortar pra 90 ou pular." |
| User asks to flatter the prospect ("compliment them on the brand") | Refuse: "Elogio inicial é morto. Mostra que você entende a dor — isso é o respeito de verdade." |

## Examples

See `examples/` folder:
- `example-input-pt.md` — input fictício (founder solo, ICP agências, oferta workflows IA)
- `example-output-pt.md` — 3 variações geradas + recomendação

## Files in this skill

```
marfin-cold-email-engine/
├── SKILL.md
├── README.md
├── templates/
│   └── email-variations.md
└── examples/
    ├── example-input-pt.md
    └── example-output-pt.md
```
