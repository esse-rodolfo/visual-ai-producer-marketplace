# Changelog

Formato [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/),
versionamento [SemVer](https://semver.org/lang/pt-BR/).

## [0.2.0] - 2026-10-22

### Adicionado

- Skill `gerar-3d`: modelo 3D, textura e GLB por IA, pelo CLI ou pelas
  ferramentas `bl_generate_*` do conector, com a tabela de modelos e a trava
  de custo que vieram do `blender-manager` 0.7.0. O GLB vai para
  `producao/entregues/` e o `blender-manager` importa.

## [0.1.1] - 2026-10-22

### Corrigido

- Os scripts da biblioteca da O-Side rodam com `python -X utf8 -s`, a partir
  da pasta dela. Com `-I` eles não se acham (importam uns aos outros) e, sem
  UTF-8, o console do Windows quebra no primeiro `✓`.
- O doctor confere o lint do Seedance, que é o script que o agente usa, no
  lugar do `validate.py`, que é ferramenta de manutenção da biblioteca e
  falha com snapshot de especificação velho.

## [0.1.0] - 2026-10-22

### Adicionado

- Agente `visual-ai-producer`: direção artística e produção de imagem, vídeo
  e modelo 3D por IA. Cinco travas no corpo: custo antes e Sim depois, só foto
  do cliente como imagem de entrada, quadro aprovado antes do clipe, registro
  de cada job no projeto, e `higgsfield auth token` nunca.
- Skill `producao-visual`: pasta `producao/` no planning do projeto, nomes de
  arquivo, os três degraus de aprovação, tabela de custo medida em 22/10 e
  peso alvo para web.
- Skill `direcao-de-cena`: roteiro vira tabela de cenas, prompt de imagem e
  prompt de movimento por cena, personagem por imagem de referência.
- Skill `gerar-higgsfield`: router para o envelope em
  `~/.claude/plugins/data/`, que guarda a biblioteca de prompt da O-Side Media
  (MIT) e o pacote oficial `higgsfield-ai/skills`, fora do catálogo global.
- Setup e doctor cobrem o CLI, o envelope, o deny do comando que imprime o
  token, o MCP `higgsfield-blender` e a ausência de skill Higgsfield global.
