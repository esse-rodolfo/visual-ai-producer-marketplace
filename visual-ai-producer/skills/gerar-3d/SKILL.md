---
name: gerar-3d
description: >-
  Gera modelo 3D (GLB), textura e referência por IA no Higgsfield, pelo CLI ou
  pelas ferramentas `bl_generate_*` do conector com o Blender aberto, e
  entrega o GLB para o `blender-manager` importar. Gatilhos: "gera o modelo
  a partir dessa imagem", "image to 3D", "faz um GLB", "retextura esse
  modelo", "gera uma textura", "modelo 3D por IA".
---

# gerar-3d

A trava de custo de `producao-visual` vale igual: modelo, quantos jobs e
custo ditos em uma linha, depois o Sim. Peça isolada primeiro, em um job; só
depois o lote. Confira o saldo com `higgsfield account status` antes de propor.

## Qual modelo para quê

| Pedido | Modelo | Nota |
|---|---|---|
| Peça a partir de 1 imagem | `meshy_v7_image_to_3d` | `--model_type lowpoly` e `--target_polycount N` entregam malha perto do orçamento; `--enable_pbr true` traz mapas PBR |
| Peça a partir de 1 a 4 vistas | `multi_image_to_3d` | frente, lado, trás; `--should_texture true` |
| Só texto | `meshy_v6_text_to_3d`, `hunyuan3d_v3_1_text_to_3d` | imagem de referência aprovada antes costuma sair melhor que texto puro |
| Trocar a textura de um modelo | `meshy_v5_retexture` | |
| Limpar malha | `meshy_v5_remesh` | ou `decimate` no Blender, grátis: prefira |
| Rig | `3d_rigging` | só se pedirem; personagem orgânico sai sem topologia de animação |
| Referência ou textura 2D | modelo de imagem de `producao-visual` | `recraft_v4_*` para vetor e padrão |

`--help` e a busca não bastam: `higgsfield model list` inteiro e `higgsfield
model get <nome>` antes de assumir que um parâmetro existe. O catálogo
completo está em `$H/higgsfield-generate/references/model-catalog.md`, seção
`## 3D models` (`$H` em `gerar-higgsfield`).

A imagem de entrada segue a regra de sempre: foto do cliente ou imagem que
você gerou e o Rodolfo aprovou. Foto de fabricante não vira GLB.

## Pelo Blender aberto

Com o Rodolfo olhando a cena, as ferramentas do conector `higgsfield-blender`
geram direto nela: `get_host_status` (precisa de `blr: true`), `bl_list_models`,
`bl_generate_image`, `bl_generate_3d`, `bl_image_to_3d`, `bl_generation_status`
até `completed`. O módulo `blender-generation` do conector diz que pedido
explícito autoriza enviar sem segunda confirmação; aqui **vale a regra mais
estrita**: custo dito e Sim antes. Mexer na cena (importar, posicionar,
renderizar) é do `blender-manager`.

## Entrega

1. Baixe o GLB para `producao/entregues/` do projeto, nome `modelo-<peça>-vNN.glb`.
2. Registre em `registro.md`: modelo, parâmetros, créditos, contagem de polígonos se o CLI informar.
3. Diga ao Rodolfo que veio de modelo gerativo e que o próximo passo é o `blender-manager` importar, conferir triângulos e renderizar (skill `blender-ao-vivo` dele).

## Como verificar que ainda funciona

- Modelos 3D vivos: `higgsfield model list | grep -ci 3d` → maior que `0`
- Custo: `higgsfield generate cost meshy_v7_image_to_3d --help` mostra os parâmetros
