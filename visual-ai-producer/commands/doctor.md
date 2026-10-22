---
name: doctor
description: Confere se o visual-ai-producer tem o que precisa — CLI do Higgsfield logado, o envelope com a biblioteca da O-Side Media e o pacote oficial, o deny do token, o MCP higgsfield-blender e nenhuma skill Higgsfield no catálogo global.
---

# /visual-ai-producer:doctor

Rode em ordem e reporte uma linha por item: `ok`, `falta` ou `degradado`,
com o próximo passo da tabela. Nada aqui gasta crédito.

## 1. CLI e sessão

```bash
higgsfield --version
higgsfield account status
```

Esperado: versão e uma linha com conta, plano e créditos.

## 2. Envelope

```bash
U=~/.claude/plugins/data/visual-ai-producer-visual-ai-producer-marketplace/upstream
grep -m1 'version:' $U/oside/SKILL.md
ls $U/higgsfield/.agents/skills | wc -l
cat $U/VERSOES.md
```

Esperado: a versão da biblioteca, pelo menos `8`, e a tabela.

## 3. Scripts da biblioteca rodam

```bash
(cd $U/oside && python -I scripts/validate.py 2>&1 | tail -2)
```

Esperado: termina sem traceback. Aviso de versão ou de snapshot é só aviso.

## 4. Estimativa de custo funciona

```bash
higgsfield generate cost nano_banana_2 --prompt test --aspect_ratio 16:9
```

Esperado: um número de créditos. Isto estima, não gera.

## 5. Catálogo global limpo e deny do token

```bash
ls ~/.claude/skills | grep -c higgsfield
grep -c 'higgsfield auth token' ~/.claude/settings.json
```

Esperado: `0` e `1`.

## 6. MCP higgsfield-blender

Chame `get_host_status`. Esperado: resposta do bridge; Blender desconectado
é `degradado` (só o 3D ao vivo para; imagem, vídeo e GLB pelo CLI seguem).

## 7. Atualização disponível (aviso)

```bash
npm view @higgsfield/cli version
git -C $U/oside fetch --dry-run 2>&1 | tail -1
```

Versão maior que a instalada, ou saída no fetch, é **aviso**: quem decide
atualizar é o usuário.

## Como reportar

| Sintoma | Diagnóstico | Próximo passo |
|---|---|---|
| `higgsfield` não encontrado | CLI não instalado ou fora do PATH | `npm install --global @higgsfield/cli` |
| `Not authenticated` / `Session expired` | sessão em `~/.config/higgsfield/` vencida | usuário roda `higgsfield auth login` |
| `$U/oside/SKILL.md` ausente | envelope não baixado | `/visual-ai-producer:setup` passo 3 |
| Menos de 8 skills em `$U/higgsfield` | pacote oficial não instalado | `/visual-ai-producer:setup` passo 4 |
| `validate.py` com traceback | clone incompleto ou Python abaixo de 3.10 | repetir o passo 3 do setup; conferir `python --version` |
| `generate cost` falha | sessão vencida ou modelo renomeado | passo 1; `higgsfield model list` |
| Catálogo global com `higgsfield-*` | alguém rodou `npx skills add` fora do envelope ou com `-g` | o usuário apaga essas pastas e `~/skills-lock.json`; o envelope continua valendo |
| Deny do token dá `0` | `~/.claude/settings.json` sem a regra | usuário adiciona `"Bash(higgsfield auth token*)"` em `permissions.deny` |
| `get_host_status` sem resposta | MCP fora ou OAuth vencido | `/mcp` numa sessão interativa e refazer o login do `higgsfield-blender` |
