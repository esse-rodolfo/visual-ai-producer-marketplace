# visual-ai-producer-marketplace

Marketplace de plugin do Claude Code com um plugin: **`visual-ai-producer`**,
o agente de direção artística e produção de imagem, vídeo e 3D por IA.

## Instalar

```
/plugin marketplace add esse-rodolfo/visual-ai-producer-marketplace
/plugin install visual-ai-producer@visual-ai-producer-marketplace
```

Depois rode `/visual-ai-producer:setup` e `/visual-ai-producer:doctor`.

## O que tem dentro

| Plugin | O quê |
|---|---|
| [visual-ai-producer](visual-ai-producer/) | roteiro vira cenas e prompts; moodboard; personagem consistente; custo antes; geração de imagem, vídeo e GLB no Higgsfield em lote aprovado |

## Verificar antes de publicar

```bash
wc -l visual-ai-producer/skills/*/SKILL.md      # cada um até 60 linhas
grep -c "Gatilhos:" visual-ai-producer/agents/visual-ai-producer.md   # 1
```

## Licença

MIT. Ver [LICENSE](LICENSE). A biblioteca de prompt da O-Side Media que o
setup baixa é MIT e fica com o `LICENSE` dela no envelope.
