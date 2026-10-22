---
name: producao-visual
description: >-
  Regras da casa para todo trabalho de mídia por IA: pastas, nomes, o que
  pode entrar no gerador, os degraus de aprovação e o custo por modelo.
  Gatilhos: "onde salvo a imagem", "quanto custa gerar", "posso usar essa
  foto de referência", "qual modelo usar".
---

# producao-visual

Convenção, sem procedimento próprio; toda skill de peça parte daqui.

## As peças

- **Projeto:** `producao/` dentro do repo de planning do cliente, com `moodboard/` (referências, nunca entram no gerador), `entrada/` (fotos do cliente, as únicas que entram), `quadros/`, `clipes/`, `entregues/` e `registro.md`
- **Nomes:** `cena-NN-vNN.<ext>`; personagem `ref-<nome>-vNN.png`; quadro final de uma cena `cena-NN-fim-vNN.png`
- **Registro:** uma linha por job em `registro.md`: data · modelo · parâmetros · créditos · arquivo · aprovado? · observação. Fecha com o saldo
- **Peso alvo para web:** vídeo até 1 MB em 720p (540p no celular), mudo, loop; poster AVIF até 80 KB. Quem comprime é o `video-editor`
- **Loop:** o mesmo quadro em `--start-image` e `--end-image` fecha o loop sem corte (padrão da Tradebras)

## Custo (medido em 22/10/2026, confirme com `higgsfield generate cost`)

| Modelo | Créditos |
|---|---|
| Seedance 2.0, 720p, sem áudio | 4,5 por segundo |
| Seedance 2.0, 720p, `--mode fast` (rascunho) | 2,5 por segundo |
| Seedance 2.0, 1080p | 12 por segundo; o site não precisa |
| Kling 3.0, `--sound off` | 1,5 por segundo; movimento mais fraco |
| Nano Banana 2 (imagem, quadro inicial) | 2 |
| GPT Image 2 (imagem com texto ou layout) | 6,5 |

Projeção de um vídeo de 6 cenas de 4 s, com rascunho e duas refações: cerca
de 290 créditos. Sem rascunho e tudo certo de primeira: cerca de 120.

## Degraus de aprovação

1. **Direção:** tabela de cenas (câmera, ação, o que comunica). Custa zero.
2. **Quadro:** imagem de referência do personagem e quadro inicial de cada cena (final também, quando o fim importa). Poucos créditos. Aprovado um a um.
3. **Clipe:** só as cenas com quadro aprovado. Corte seco entre cenas; a passagem animada entre duas cenas deforma e custa um clipe a mais.

Nenhum degrau começa sem o Sim do degrau anterior na conversa.

## Nunca faz

- Subir foto, vídeo ou frame de terceiro (fabricante, Street View, banco de imagem, comercial) como entrada do gerador.
- Gerar sem dizer o custo antes, ou gerar fora do lote aprovado.
- Rodar `higgsfield auth token`.
- Treinar Soul ID de pessoa real sem autorização escrita dela.
- Entregar sem olhar o resultado com `Read`.

## Como verificar que ainda funciona

- CLI logado: `higgsfield account status` mostra conta e créditos
- Custo: `higgsfield generate cost seedance_2_0 --prompt test --duration 5 --resolution 720p --generate_audio false` imprime um número
- Projeto: `ls producao/` no planning do cliente mostra as pastas acima
