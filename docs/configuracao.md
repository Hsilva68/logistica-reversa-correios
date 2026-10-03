# Configuração

## 1. Importar os workflows

No n8n, use **Import from File** para cada arquivo da pasta `n8n/`. Os dois chegam **desativados** e sem credenciais.

## 2. Credenciais no n8n

| Credencial | Onde usar |
| --- | --- |
| Slack API (token do bot) | Nós HTTP Request do Slack (`views.open`, `views.update`, `chat.postMessage`) |
| Google Sheets OAuth2 | Nós Google Sheets |
| Firecrawl API | Nós Firecrawl (criar sessão, executar, encerrar) |

Escopos do bot do Slack: `commands`, `chat:write`, `im:write`.

## 3. Variáveis de ambiente

O nó **CONFIG - PREENCHER** lê os dados do portal dos Correios de variáveis de ambiente do n8n. Não escreva esses valores no nó.

| Variável | Conteúdo |
| --- | --- |
| `CORREIOS_USUARIO` | Usuário do portal |
| `CORREIOS_SENHA` | Senha do portal |
| `CORREIOS_COD_ADMINISTRATIVO` | Código administrativo do contrato |
| `CORREIOS_CARTAO_POSTAGEM` | Cartão de postagem |

Ainda no nó CONFIG, ajuste telefones e os endereços em `destinos` (unidades que recebem os equipamentos).

## 4. App do Slack

1. Troque os caminhos `troque-este-path-slash-command` e `troque-este-path-interatividade` nos dois nós Webhook por valores difíceis de adivinhar.
2. **Slash Commands:** crie `/reversa-correios` apontando para a Production URL do webhook do slash command.
3. **Interactivity & Shortcuts:** aponte a Request URL para a Production URL do webhook de interatividade.
4. **Valide a assinatura do Slack** (HMAC SHA-256 de `v0:timestamp:corpo` com o *Signing Secret*) antes de colocar em produção. Este fluxo emite autorizações reais.

## 5. Planilha

Aponte os nós Google Sheets para a sua planilha (`SEU_SPREADSHEET_ID`). Colunas usadas pelo monitor na aba **Reverso**:

| Coluna | Uso |
| --- | --- |
| Codigo Autorização de Postagem | Código emitido pelos Correios |
| ID Slack do Solicitante | Quem recebe a cobrança |
| Vencimento da Autorização | Data limite para postagem |
| Status Recebimento | Cobrança para quando o valor for "Devolvido" |
| Última cobrança | Preenchida pelo monitor |

## 6. Ativar

Ative o monitor e o fluxo principal. Teste com `/reversa-correios` em um canal de teste antes de liberar para o time.
