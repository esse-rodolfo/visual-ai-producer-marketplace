---
name: gerar-higgsfield
description: >-
  Router para gerar no Higgsfield: onde está o envelope com a biblioteca de
  prompt da O-Side Media e as skills oficiais, qual arquivo ler para cada
  pedido e a sequência lint, custo, Sim, geração. Gatilhos: "gera no
  Higgsfield", "roda o Seedance", "qual skill do Higgsfield", "foto de
  produto", "brandkit", "thumbnail", "Soul ID", "imagem para 3D".
---

# gerar-higgsfield

```bash
U=~/.claude/plugins/data/visual-ai-producer-visual-ai-producer-marketplace/upstream
O=$U/oside                      # OSideMedia/higgsfield-ai-prompt-skill (MIT)
H=$U/higgsfield/.agents/skills  # higgsfield-ai/skills (oficial)
```

Nada disso aparece no catálogo do Claude: leia pelo caminho, com `Read`.

## Qual arquivo ler

| Pedido | Leia |
|---|---|
| Qualquer prompt de vídeo ou imagem | `$O/SKILL.md` (HARD RULES e "Route to the Right Skill") e `$O/skills/higgsfield-prompt/SKILL.md` |
| Clipe de imagem para vídeo, Seedance 2.0; resultado errado (mão extra, reversão) | `$O/skills/higgsfield-seedance/SKILL.md`, `FAILURE-MODES.md` ao lado, `$O/skills/higgsfield-troubleshoot/SKILL.md` |
| Escolher modelo, duração, 2.0 contra 2.5 contra Kling; câmera | `$O/skills/higgsfield-models/SKILL.md`, `$O/model-guide.md`, `$O/skills/higgsfield-camera/SKILL.md` |
| Personagem, ficha, "visual DNA", sem rosto | `$O/skills/higgsfield-character-design/SKILL.md` |
| Moodboard e consistência do projeto | `$O/skills/higgsfield-moodboard/SKILL.md` |
| Shotlist a partir de roteiro | `$O/skills/higgsfield-shotlist-director/SKILL.md` |
| Rodar o CLI (modelos, flags, `--wait`) | `$H/higgsfield-generate/SKILL.md` |
| Foto de produto · cards · thumbnail · explicativo | `$H/higgsfield-product-photoshoot/` · `-marketplace-cards/` · `-youtube-thumbnail/` · `-video-explainer/` |
| Marca (paleta, logo, brandbook) | `$H/higgsfield-brandkit/SKILL.md` |
| Rosto de pessoa real (com autorização) | `$H/higgsfield-soul-id/SKILL.md` |
| Modelo 3D, textura, GLB | skill `gerar-3d` deste plugin |

As skills oficiais citam as irmãs pelo nome ("use `higgsfield-brandkit`
instead"): o nome é a pasta em `$H/`, não uma skill invocável.

## Sequência de um job

1. Prompt pronto e aprovado (de `direcao-de-cena`).
2. Lint, só para Seedance: `(cd "$O" && python -X utf8 -s scripts/seedance_lint.py --preflight --model seedance_2_0 --ar 16:9 --duration 4 --file <prompt.txt>)`. `FAIL` volta para o prompt; `WARN` se corrige ou se justifica.
3. Custo: `higgsfield generate cost <modelo> --prompt "..." [flags]`. Mostre o número e espere o **Sim**.
4. Geração: `higgsfield generate create <modelo> --prompt "..." [--image|--start-image|--end-image <foto-do-cliente>] [flags] --wait`. Imagem de referência só de `producao/entrada/` ou gerada por você.
5. Salve o resultado em `producao/quadros/` ou `producao/clipes/`, olhe com `Read`, registre em `registro.md`.

Padrões: vídeo `seedance_2_0 --resolution 720p --generate_audio false --duration 4`; rascunho `--mode fast`; imagem `nano_banana_2 --aspect_ratio 16:9`; imagem com texto ou layout `gpt_image_2`.

## O que as skills de terceiro mandam e aqui não vale

- Plano, assinatura, app e Canvas da Higgsfield: só o CLI e o MCP.
- `python3`: nesta máquina é `python -X utf8 -s`, a partir de `$O` (com `-I` os scripts não se acham). `--generate-audio`: no Seedance 2.0 é `--generate_audio false`.
- Gravar no `db/ledger` da O-Side: o registro é o `registro.md` do projeto. O envelope é descartável na atualização. `higgsfield auth token` nunca.

## Como verificar que ainda funciona

- `test -f "$O/SKILL.md" && ls "$H" | wc -l` → existe e pelo menos 8
- `higgsfield generate cost nano_banana_2 --prompt test --aspect_ratio 16:9` → um número
- `ls ~/.claude/skills | grep -c higgsfield` → `0`
