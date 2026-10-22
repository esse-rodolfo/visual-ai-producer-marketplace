---
name: direcao-de-cena
description: >-
  Transforma roteiro ou briefing em tabela de cenas e, para cada cena, um
  prompt de imagem (o quadro) e um prompt de movimento (o clipe), com
  personagem consistente. Gatilhos: "roteiro em cenas", "shotlist", "prompt
  de cada cena", "personagem consistente", "sem rosto".
---

# direcao-de-cena

## Passos

1. Leia o roteiro, o moodboard e as fotos de `producao/entrada/`. Separe o que descreve o prompt (referência) do que entra no gerador (foto do cliente).
2. Escreva a **tabela de cenas**: número, cena (uma frase), câmera, o que comunica. Sem texto na tela, salvo pedido. Mostre ao Rodolfo e espere o Sim.
3. Para cada cena aprovada, escreva dois prompts em inglês, até 200 palavras cada:
   - **Imagem:** descreve a cena inteira (sujeito, cenário, luz, lente, enquadramento) e cita as referências por papel: "the silver car from the reference image", "the man from the character reference". Fecha com `16:9` ou o formato pedido.
   - **Movimento:** só o que muda a partir do quadro: verbo da câmera (dolly, orbit, push-in, tracking), ação do sujeito, ritmo. Não redescreve o quadro.
4. Quando o fim da cena importa (mão que pluga, porta que abre, chave que cai), escreva também o **quadro final**: "Same framing: ..." com só o que mudou. O clipe recebe início e fim.
5. **Personagem:** antes das cenas, um prompt de imagem de referência, corpo inteiro, roupa fixa e reconhecível (uma cor de camiseta), fundo neutro. Pedido "sem rosto": referência de costas, e cada cena diz `seen from behind`, `over the shoulder` ou `only the hands`. A mesma referência entra em toda cena com a pessoa.
6. Passe cada prompt pela biblioteca da O-Side Media (`gerar-higgsfield` diz o caminho): HARD RULES do `SKILL.md` raiz, estrutura MCSLA de `skills/higgsfield-prompt/`, negativas de `skills/shared/negative-constraints.md`, e a sub-skill do caso (`higgsfield-seedance`, `higgsfield-character-design`, `higgsfield-shotlist-director`, `higgsfield-moodboard`).
7. Entregue ao Rodolfo o par de prompts por cena com um resumo de uma linha em português embaixo de cada um. Só depois do Sim entra `gerar-higgsfield`.

## Regras de prompt

- Nome de marca, de loja ou de app nunca aparece; placa fica `blank license plate`; letreiro, `no readable signs`.
- Frase positiva: `seen from behind`, não `no face`; `tack sharp`, não `no blur`.
- Luz e lugar vêm das fotos reais do cliente, descritos em palavras (cobertura solar, muro branco, fios no poste), não de material de terceiro enviado.
- Um bloco `STYLE` comum entra antes de cada cena: estilo, formato, luz, descrição do produto, regra de rosto.
- Corte seco entre cenas. Transição animada só com pedido.

## Nunca faz

- Gerar antes de a tabela de cenas e os prompts serem aprovados.
- Rosto de pessoa real sem Soul ID autorizado por escrito.
- Texto na tela como forma de comunicar a vantagem: a cena mostra, o texto da página explica.

## Como verificar que ainda funciona

- Exemplo canônico: `joyenergy-belem-planning/docs/mockups/roteiro-video-ex2.md` (v3, 6 cenas sem rosto, prompts de imagem e movimento)
- Biblioteca: `test -f "$U/oside/skills/higgsfield-prompt/SKILL.md"` com `U` de `gerar-higgsfield`
