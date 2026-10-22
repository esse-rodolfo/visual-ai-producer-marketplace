# visual-ai-producer

Agente de direção artística e produção de mídia por IA. Recebe um roteiro, um
briefing ou um pedido de asset e devolve imagem, vídeo ou modelo 3D gerados,
com o custo dito antes e cada peça aprovada em lote.

## O que faz hoje (0.1.1)

| Peça | Estado |
|---|---|
| Roteiro em tabela de cenas, prompt de imagem e de movimento por cena | funciona |
| Moodboard e regra de referência (só foto do cliente entra no gerador) | funciona |
| Personagem consistente por imagem de referência, inclusive "sem rosto" | funciona |
| Quadro inicial (imagem) aprovado, depois clipe (Seedance 2.0 / Kling 3.0) | funciona |
| Loop fechado com o mesmo quadro no início e no fim | funciona |
| Foto de produto, brandkit, thumbnail, Soul ID | pelas skills oficiais no envelope |
| Modelo 3D por IA (imagem para GLB) pelo MCP `higgsfield-blender` | funciona com o Blender aberto |
| Segundo gerador (OpenRouter, fal, Gemini) | próxima fatia, com ADR |

## Como chamar

```
@visual-ai-producer:visual-ai-producer gera a cena 0 e 1 do roteiro em docs/mockups/roteiro-video-ex2.md
```

Ou pela Aura. Nada é gerado sem um Sim na conversa depois do custo.

## Como ele trabalha

1. Lê o roteiro e o moodboard do projeto e separa referência de entrada:
   foto do cliente entra no gerador; foto de fabricante, Street View ou banco
   de imagem só descreve o prompt.
2. Escreve a tabela de cenas e os prompts, pela biblioteca da O-Side Media.
3. Diz o custo de cada job (`higgsfield generate cost`) e espera o Sim.
4. Gera os quadros, você aprova, só então anima.
5. Grava tudo em `producao/` do planning do projeto, com `registro.md`.

## O que não faz

- Não corta nem monta o vídeo final (`video-editor`).
- Não coloca na página (`frontend-specialist`).
- Não escreve copy (`copywriter`) nem modela no Blender (`blender-manager`).
- Não treina Soul ID de pessoa real sem autorização escrita.
- Não roda `higgsfield auth token`, que imprime o token.

## Skills de terceiro, envelopadas

A biblioteca de prompt da O-Side Media (35 sub-skills, MIT) e o pacote oficial
`higgsfield-ai/skills` não são copiados para este repositório nem instalados
no catálogo do Claude. O setup os baixa para
`~/.claude/plugins/data/visual-ai-producer-visual-ai-producer-marketplace/upstream/`
e só este agente os lê. O envelope é descartável: o registro de gasto mora no
projeto, não no `db/` da biblioteca. Atualizar é rodar o setup de novo.

## Instalar e conferir

```
/plugin marketplace add esse-rodolfo/visual-ai-producer-marketplace
/plugin install visual-ai-producer@visual-ai-producer-marketplace
/visual-ai-producer:setup
/visual-ai-producer:doctor
```

## Remover

```bash
claude plugin uninstall visual-ai-producer@visual-ai-producer-marketplace
rm -rf ~/.claude/plugins/data/visual-ai-producer-visual-ai-producer-marketplace
```

O CLI `@higgsfield/cli` e a sessão em `~/.config/higgsfield/` ficam: são do
usuário, não do plugin.
