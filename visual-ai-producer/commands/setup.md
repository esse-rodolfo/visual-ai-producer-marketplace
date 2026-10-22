---
name: setup
description: Instala o que o visual-ai-producer usa nesta máquina — confere o CLI do Higgsfield, baixa a biblioteca de prompt da O-Side Media e o pacote oficial de skills para um envelope fora do catálogo, e confere o deny do comando que imprime o token.
---

# /visual-ai-producer:setup

Um passo por vez. Não instale runtime por conta própria: se faltar Node,
Python ou git, diga qual e pare. Nada aqui gasta crédito.

## 1. Pré-requisitos

```bash
higgsfield --version          # @higgsfield/cli, npm global
higgsfield account status     # conta, plano e créditos
python --version              # 3.10 ou mais
git --version && npx --version
```

Se `account status` disser `Not authenticated` ou `Session expired`, peça ao
usuário para rodar `higgsfield auth login` numa janela dele e espere. Se o
CLI faltar: `npm install --global @higgsfield/cli`.

## 2. Envelope

```bash
U=~/.claude/plugins/data/visual-ai-producer-visual-ai-producer-marketplace/upstream
mkdir -p $U/higgsfield
```

## 3. Biblioteca de prompt da O-Side Media

```bash
if [ -d $U/oside/.git ]; then
  git -C $U/oside checkout -- . && git -C $U/oside clean -fdq && git -C $U/oside pull --ff-only
else
  git clone --depth 1 https://github.com/OSideMedia/higgsfield-ai-prompt-skill $U/oside
fi
test -f $U/oside/LICENSE && head -3 $U/oside/SKILL.md
```

A biblioteca grava `db/` e `workspace/` dentro da própria pasta e não os
ignora no git; por isso a atualização descarta o que estiver lá. O registro
de gasto mora no projeto, nunca ali. O `LICENSE` (MIT) fica.

## 4. Pacote oficial de skills

```bash
(cd $U/higgsfield && npx -y skills add higgsfield-ai/skills --skill '*' -a universal -y)
ls $U/higgsfield/.agents/skills
ls ~/.claude/skills | grep -c higgsfield     # tem que dar 0
```

Sempre de dentro da pasta do envelope e sem `-g`: com `-g`, ou rodando de
outro lugar, as skills vão para o catálogo global. Se a última linha der mais
que 0, há skill Higgsfield global: avise, é o usuário quem apaga.

## 5. Deny do comando que imprime o token

```bash
grep -c 'higgsfield auth token' ~/.claude/settings.json
```

Esperado: `1`. Se der `0`, peça ao usuário para adicionar
`"Bash(higgsfield auth token*)"` em `permissions.deny` do
`~/.claude/settings.json`. Não edite esse arquivo por conta própria.

## 6. VERSOES.md

Regrave `$U/VERSOES.md` com uma tabela Upstream | Instalado | Data | Testado:
o commit curto e a `version` do frontmatter de `$U/oside/SKILL.md`, e a
contagem de pastas em `$U/higgsfield/.agents/skills`.

## 7. Conferir

Rode `/visual-ai-producer:doctor`.

## Atualizar

Repetir os passos 3 e 4 entre uma peça e outra, nunca no meio de um lote, e
rodar o doctor depois.

## O que fica de fora

A conta, o plano e o login da Higgsfield são do usuário. O MCP
`higgsfield-blender` já está configurado em escopo user, com OAuth feito;
este setup não mexe nele. Chaves de outros geradores (OpenRouter, fal,
Gemini) não são pré-requisito e entram em release própria.
