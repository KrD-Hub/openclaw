# Mapeamento OpenClaw (backup)

## Resumo rapido

Este backup mostra uma evolucao de configuracoes por tentativa:

1. Base minima de gateway/auth.
2. Ativacao do canal Telegram.
3. Correcao de plugin/ativacao Telegram.
4. Ajustes de modelos (Gemini/Claude, OpenRouter/Anthropic).
5. Consolidacao final, mas com simplificacoes e segredos hardcoded.

Estado atual recomendado: usar **um unico ficheiro final** com variaveis de ambiente, sem tokens/senhas em texto.

## Dossies analisados

| Ficheiro                         | Objetivo principal                       | Estado        | Nota                                                             |
| -------------------------------- | ---------------------------------------- | ------------- | ---------------------------------------------------------------- |
| `openclaw-minimal.json`          | Base minima de auth                      | Parcial       | So gateway token, sem modelo/canais                              |
| `openclaw-telegram-enabled.json` | Ligar Telegram                           | Bom           | Telegram ativo com `dmPolicy: pairing`                           |
| `openclaw-telegram-fixed.json`   | Telegram + plugins                       | Desnecessario | `plugins.entries.telegram` nao e obrigatorio para canal built-in |
| `openclaw-fixed.json`            | Definir modelo em `agents.list`          | Parcial       | Modelo num formato menos consistente com `agents.defaults`       |
| `openclaw-working-model.json`    | Modelo + fallback antigos                | Funcional     | Usa IDs de modelo antigos                                        |
| `openclaw-exact.json`            | Atualizar para `gemini-2.5-flash-lite`   | Bom           | Estrutura quase final                                            |
| `openclaw-correct.json`          | Modelo free + fallback Sonnet            | Bom           | Custo baixo, mas modelo "exp:free" pode oscilar                  |
| `openclaw-anthropic-direct.json` | Sonnet direto como primary               | Bom           | Qualidade maior, custo maior                                     |
| `openclaw-server.json`           | Perfil servidor (identidade/log/modelos) | Bom           | Faltam canais Telegram neste ficheiro                            |
| `openclaw-final.json`            | "final" minimalista                      | Incompleto    | Nao define agentes/modelos/politicas                             |
| `openclaw.config.json5`          | Config mais completa (budget + operacao) | Melhor base   | Bom ponto de partida para producao                               |
| `CREDENCIAIS-TEMPORARIAS.txt`    | Checklist/credenciais bootstrap          | Risco         | Contem password e dados sensiveis                                |
| `.env`                           | Variaveis locais                         | Necessario    | Deve ficar fora de partilha/publico                              |

## Conclusao tecnica

- A melhor base tecnica e `openclaw.config.json5`.
- O `openclaw-final.json` existente esta demasiado reduzido.
- Existem segredos expostos no backup (token gateway, bot token Telegram, password temporaria).

## Versao final sugerida

Usar o ficheiro:

- `openclaw-final-recommended.json5`

com:

- modelos em `agents.defaults` (flash como default + sonnet fallback),
- Telegram com `dmPolicy: pairing` e `requireMention: true`,
- segredos apenas por variaveis de ambiente.

## Acao imediata de seguranca

1. Rodar/revogar token do Telegram bot.
2. Rodar token de auth do gateway.
3. Trocar a password listada em `CREDENCIAIS-TEMPORARIAS.txt`.
4. Remover ficheiros sensiveis do backup quando nao forem mais necessarios.
