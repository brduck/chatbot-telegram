# 🌤️ Chatbot de Clima no Telegram com n8n

Este repositório contém um workflow automatizado para o **n8n** que atua como um chatbot no Telegram. O bot recebe o nome de uma cidade, consulta a API da OpenWeather e retorna a temperatura atual formatada.

## 📋 Funcionalidades

- **Recebimento de Mensagens:** Telegram Trigger.
- **Tratamento de Dados:** Normalização de texto (remoção de acentos, espaços extras e formatação compatível com a API).
- **Integração de API:** Consulta ao endpoint da OpenWeather.
- **Resposta Formatada:**
  - Sucesso: `🌤️ A temperatura em [Cidade] é de [X]°C.`
  - Erro: `❌ Cidade não encontrada. Use o formato Cidade,UF...`

## 🛠️ Pré-requisitos e Variáveis de Ambiente

Para executar este projeto, é necessário configurar as seguintes variáveis de ambiente no seu sistema (Docker, ou `.env`), pois o workflow não contém credenciais embutidas:

| Variável | Descrição |
| :--- | :--- |
| `TELEGRAM_BOT_TOKEN` | O token do seu bot gerado pelo @BotFather. |
| `OPENWEATHER_API_KEY` | Sua chave de API da OpenWeather. |

---

## 🚀 Guia de Importação e Configuração

### Passo 1: Importar o Workflow
1. Baixe o arquivo `workflow-chatbot-telegram.json` deste repositório.
2. No seu n8n, clique em **Add Workflow** (ou menu no topo direito) > **Import from File**.
3. Selecione o arquivo baixado.

### Passo 2: Configurar as Credenciais do Telegram
Embora o token esteja na variável de ambiente, é necessário criar o objeto de credencial no n8n para conectar o Trigger.

1. No menu lateral do n8n, vá em **Credentials**.
2. Clique em **Create New** e busque por **Telegram API**.
3. No campo **Access Token**:
   - Mude o modo de entrada para **Expression** (clique na engrenagem ou ícone `f(x)`).
   - Insira a expressão: `{{ $env.TELEGRAM_BOT_TOKEN }}`
4. Clique em **Save**.
5. **Importante:** Verifique se os nós "Telegram Trigger", "Temperatura Atual" e "Mensagem de erro" no workflow estão selecionando essa nova credencial.

### Passo 3: Configuração da OpenWeather
Não é necessário configurar credencial manual. O nó **HTTP Request** já está configurado para ler automaticamente a variável `{{ $env.OPENWEATHER_API_KEY }}`.