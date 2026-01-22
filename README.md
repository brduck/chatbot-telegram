# 🌤️ Chatbot de Clima no Telegram com n8n

Este repositório contém um workflow automatizado para o **n8n** que atua como um chatbot no Telegram. O bot recebe o nome de uma cidade, consulta a API da OpenWeather e utiliza a **IA do Google Gemini** para gerar uma resposta natural. Caso a IA falhe, o sistema possui um fallback automático.

## 📋 Funcionalidades

- **Recebimento de Mensagens:** Telegram Trigger.
- **Tratamento de Dados:** Normalização de texto (remoção de acentos, espaços extras e formatação compatível com a API).
- **Integração de API:** Consulta ao endpoint da OpenWeather.
- **Resposta Inteligente (IA):**
  - **Sucesso (Gemini):** Gera uma mensagem amigável, variada e com emojis usando os dados do clima.
  - **Fallback:** Se a IA falhar, envia o padrão: `🌤️ A temperatura em [Cidade] é de [X]°C.`
  - **Erro na OpenWeather:** `❌ Cidade não encontrada. Use o formato Cidade,UF (ex.: São Paulo,SP).`

## 🛠️ Pré-requisitos e Variáveis de Ambiente

Para executar este projeto, é necessário configurar as seguintes variáveis de ambiente no seu sistema (Docker, ou `.env`), pois o workflow não contém credenciais embutidas:

| Variável | Descrição |
| :--- | :--- |
| `TELEGRAM_BOT_TOKEN` | O token do seu bot gerado pelo @BotFather. |
| `OPENWEATHER_API_KEY` | Sua chave de API da OpenWeather. |
| `GEMINI_API_KEY` | Sua chave de API da Google Gemini. |
| `N8N_BLOCK_ENV_ACCESS_IN_NODE` | Definir como `false`. Necessário para permitir a leitura segura das variáveis pelos nós. |

---

## 🚀 Guia de Importação e Configuração

### Passo 1: Importar o Workflow
1. Baixe o arquivo `workflow-chatbot-telegram.json` deste repositório.
2. No seu n8n, clique em **Add Workflow** (ou menu no topo direito) > **Import from File**.
3. Selecione o arquivo baixado.

### Passo 2: Configurar as Credenciais do Telegram e Gemini
Embora o token esteja na variável de ambiente, é necessário criar 2 objetos de credencial no n8n para conectar com os serviços.

#### Telegram:
1. No menu lateral do n8n, vá em **Credentials**.
2. Clique em **Create New** e busque por **Telegram API**.
3. No campo **Access Token**:
   - Mude o modo de entrada para **Expression** (clique na engrenagem ou ícone `f(x)`).
   - Insira a expressão: `{{ $env.TELEGRAM_BOT_TOKEN }}`
4. Clique em **Save**.
5. **Importante:** Verifique se os nós de envio de mensagem no workflow estão selecionando essa nova credencial.

#### Gemini:
1. No menu lateral do n8n, vá em **Credentials**.
2. Clique em **Create New** e busque por **Google Gemini(PaLM) Api**.
3. No campo **API Key**:
   - Mude o modo de entrada para **Expression** (clique na engrenagem ou ícone `f(x)`).
   - Insira a expressão: `{{ $env.GEMINI_API_KEY }}`
4. Clique em **Save**.
5. **Importante:** Verifique se o nó **"Gerar resposta com IA"** (ou equivalente) no workflow está selecionando essa nova credencial.

### Passo 3: Configuração da OpenWeather
Não é necessário configurar credencial manual. O nó **HTTP Request** já está configurado para ler automaticamente a variável `{{ $env.OPENWEATHER_API_KEY }}`.