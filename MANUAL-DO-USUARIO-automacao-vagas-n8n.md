# Manual de Uso — Automação Inteligente de Vagas com n8n

## 1. Objetivo do manual

Este manual explica como utilizar, testar e manter o projeto **Automação Inteligente de Vagas com n8n**, que recebe alertas de vagas por e-mail, filtra oportunidades, compara os requisitos da vaga com o currículo, calcula um percentual de compatibilidade e envia notificações pelo Telegram.

A solução é composta por três workflows:

- **WF01 - Coleta de Vagas**
- **WF02 - Análise e Match com Currículo**
- **WF03 - Notificação de Vagas**

---

# 2. Como funciona

O fluxo completo é:

```text
Gmail
→ Coleta da vaga
→ Extração dos dados
→ Filtros
→ Remoção de duplicados
→ Google Sheets
→ Análise com IA
→ Cálculo de Match
→ Atualização da planilha
→ Verificação do limite
→ Telegram
```

A candidatura não é realizada automaticamente.

A decisão final continua sendo do usuário.

---

# 3. Pré-requisitos

Antes de utilizar o projeto, verifique se você possui:

- conta no n8n;
- conta Gmail;
- alertas de vagas configurados;
- Google Sheets;
- credencial do Google Gemini;
- conta no Telegram;
- bot do Telegram criado;
- currículo atualizado.

---

# 4. Organização dos workflows

## WF01 - Coleta de Vagas

Responsável por:

- receber e-mails de vagas;
- identificar a fonte;
- extrair dados;
- filtrar cargos;
- filtrar modelo de trabalho;
- remover duplicidades;
- registrar a vaga na planilha;
- chamar o WF02.

## WF02 - Análise e Match com Currículo

Responsável por:

- receber a vaga;
- adicionar o currículo;
- analisar vaga e currículo com IA;
- identificar requisitos;
- calcular o match;
- atualizar o Google Sheets;
- preparar os dados para o WF03.

## WF03 - Notificação de Vagas

Responsável por:

- receber o match;
- verificar o limite mínimo;
- montar a mensagem;
- enviar a vaga pelo Telegram.

---

# 5. Configuração inicial

## 5.1 Gmail

Crie no Gmail o marcador:

```text
Vagas Emprego
```

Configure os filtros do Gmail para aplicar esse marcador aos alertas recebidos de plataformas como:

- Indeed;
- LinkedIn;
- Gupy.

Exemplo de funcionamento:

```text
Novo e-mail de vaga
→ Gmail aplica "Vagas Emprego"
→ n8n identifica a mensagem
```

---

# 6. Configuração do WF01

## 6.1 Gmail Trigger

Configure:

```text
Event:
Message Received
```

Durante testes:

```text
Poll Time:
Every Minute
```

Em:

```text
Label Names or IDs
```

selecione:

```text
Vagas Emprego
```

Se o marcador já estiver filtrando corretamente os e-mails, deixe o campo `Search` vazio para evitar filtros excessivos.

---

## 6.2 Get a Message

Use:

```text
Message ID:
{{ $json.id }}
```

Esse node recupera o conteúdo completo da mensagem.

---

## 6.3 Extrair Dados da Vaga

Esse node transforma o e-mail em uma estrutura padronizada.

Campos esperados:

```text
id_email
fonte
cargo
empresa
localizacao
modelo_trabalho
tipo_contrato
salario
descricao
link
assunto_email
remetente
status
```

Exemplo:

```json
{
  "id_email": "1a0b33eec367492a",
  "fonte": "Indeed",
  "cargo": "Product Owner",
  "empresa": "Empresa Exemplo",
  "localizacao": "Não informado",
  "modelo_trabalho": "Remoto",
  "tipo_contrato": "Não informado",
  "salario": "Não informado",
  "descricao": "Descrição da vaga...",
  "link": "https://...",
  "status": "NOVA"
}
```

Quando uma informação não estiver disponível:

```text
Não informado
```

---

## 6.4 Identificar Fonte da Vaga

O node `Switch` deve utilizar:

```text
{{ $json.fonte }}
```

Saídas:

```text
Indeed
LinkedIn
Gupy
```

---

## 6.5 Filtrar Cargos

Use:

```text
{{ ($json.cargo || '').toLowerCase() }}
```

Operador:

```text
matches regex
```

Exemplo:

```regex
(analista de requisitos|analista de negócios|analista funcional|analista de implantação|analista de sistemas|analista de processos|analista de automação|business analyst|requirements analyst|functional analyst|implementation analyst|automation analyst|product owner|product manager|n8n)
```

Importante:

Como o campo foi transformado em minúsculas, a regex também deve estar em minúsculas.

---

## 6.6 Filtrar Localização

Exemplo de regra:

```text
{{ ($json.modelo_trabalho || '').toLowerCase() }}
```

Aceitar:

```text
remoto
```

OU:

```text
não informado
```

---

## 6.7 Remover Duplicidades

Configure o node:

```text
Remove Items Processed in Previous Executions
```

Usando:

```text
{{ $json.id_email }}
```

Durante testes com o mesmo e-mail, o node pode ser temporariamente desativado.

Antes de colocar o fluxo em produção, reative-o.

---

# 7. Google Sheets

Crie uma planilha chamada, por exemplo:

```text
Vagas - Automação n8n
```

Colunas recomendadas:

```text
id_email
fonte
cargo
empresa
localizacao
modelo_trabalho
tipo_contrato
salario
descricao
link
assunto_email
remetente
status
match
total_requisitos
pontos_obtidos
pontos_fortes
lacunas
justificativa
data_coleta
```

No WF01 utilize:

```text
Append Row
```

No WF02 utilize:

```text
Update Row
```

---

# 8. Configuração do WF02

## 8.1 Trigger

O primeiro node deve ser:

```text
When Executed by Another Workflow
```

Configure:

```text
Input data mode:
Accept all data
```

---

## 8.2 Adicionar Currículo

Crie um campo:

```text
curriculo
```

Tipo:

```text
String
```

Mantenha:

```text
Include Other Input Fields = ON
```

Cole uma versão atualizada e preferencialmente anonimizada do currículo.

---

# 9. Análise com IA

O node de IA recebe:

```text
vaga + currículo
```

A IA deve:

- identificar requisitos;
- comparar requisitos com o currículo;
- apresentar evidências;
- classificar os requisitos;
- apontar pontos fortes;
- apontar lacunas;
- gerar justificativa.

A IA não deve calcular o percentual de match.

---

# 10. Classificação dos requisitos

Status permitidos:

```text
ATENDIDO
PARCIALMENTE_ATENDIDO
NAO_ENCONTRADO
```

Tipo do requisito:

```text
OBRIGATORIO
DESEJAVEL
NAO_INFORMADO
```

---

# 11. Cálculo do Match

O n8n calcula a pontuação.

Regra:

```text
ATENDIDO = 1 ponto
PARCIALMENTE_ATENDIDO = 0,5 ponto
NAO_ENCONTRADO = 0 ponto
```

Fórmula:

```text
match = (pontos_obtidos / total_requisitos) × 100
```

Exemplo:

```text
4 requisitos
3 atendidos
1 parcialmente atendido

Pontos:
3 + 0,5 = 3,5

Match:
3,5 / 4 × 100 = 87,5%
```

O percentual pode ser arredondado pelo fluxo.

---

# 12. Atualização da vaga

Depois da análise, o WF02 atualiza:

```text
match
total_requisitos
pontos_obtidos
pontos_fortes
lacunas
justificativa
status
```

Status final:

```text
ANALISADA
```

---

# 13. Preparar Dados para WF03

Antes de chamar o WF03, utilize um `Edit Fields`.

Nome recomendado:

```text
Preparar Dados para WF03
```

Campos:

```text
match
{{ $('Calcular Match').item.json.match }}

cargo
{{ $('Adicionar Currículo').item.json.cargo }}

empresa
{{ $('Adicionar Currículo').item.json.empresa }}

link
{{ $('Adicionar Currículo').item.json.link }}

modelo_trabalho
{{ $('Adicionar Currículo').item.json.modelo_trabalho }}

salario
{{ $('Adicionar Currículo').item.json.salario }}

pontos_fortes
{{ $('Calcular Match').item.json.pontos_fortes.join(' | ') }}

lacunas
{{ $('Calcular Match').item.json.lacunas.join(' | ') }}

justificativa
{{ $('Calcular Match').item.json.justificativa_resumida }}

id_email
{{ $('Adicionar Currículo').item.json.id_email }}
```

---

# 14. Configuração do WF03

## 14.1 Trigger

Use:

```text
When Executed by Another Workflow
```

Configure:

```text
Accept all data
```

---

## 14.2 Match mínimo

No node:

```text
Match Mínimo para Notificação
```

use:

```text
{{ Number($json.match) }}
```

Operador:

```text
is greater than or equal to
```

Exemplo de limite:

```text
75
```

Assim:

```text
83 >= 75
→ True
→ Telegram
```

e:

```text
71 >= 75
→ False
→ não envia Telegram
```

---

# 15. Configuração do Telegram

Crie um bot utilizando:

```text
@BotFather
```

Envie:

```text
/newbot
```

Siga as instruções.

Não compartilhe o token publicamente.

---

## 15.1 Descobrir o Chat ID

Abra o bot no Telegram e envie:

```text
/start
```

Depois, no n8n:

1. crie temporariamente um `Telegram Trigger`;
2. use a mesma credencial;
3. selecione:

```text
On message
```

4. execute o trigger;
5. envie:

```text
teste
```

para o bot.

No output procure:

```text
message
→ chat
→ id
```

Esse valor é o Chat ID.

---

# 16. Mensagem do Telegram

Exemplo:

```text
🚀 NOVA VAGA COM ALTO MATCH

💼 Cargo: Product Owner
🏢 Empresa: Empresa Exemplo
📍 Modelo: Remoto

📊 Match: 83%

✅ Pontos fortes:
Experiência com requisitos | Scrum | Produto

⚠️ Lacunas:
Experiência específica não encontrada

📝 Análise:
Resumo da compatibilidade entre vaga e currículo.

🔗 Ver vaga:
https://...
```

---

# 17. Publicação dos workflows

Os workflows chamados por outros workflows precisam estar publicados.

Ordem recomendada:

```text
1. WF03 - Notificação de Vagas
2. WF02 - Análise e Match com Currículo
3. WF01 - Coleta de Vagas
```

Se aparecer:

```text
Workflow is not active and cannot be executed
```

confirme se o subworkflow foi publicado.

---

# 18. Como utilizar no dia a dia

Depois de configurado, o uso é automático.

O processo será:

```text
1. Plataforma envia alerta
2. Gmail recebe o e-mail
3. Gmail adiciona marcador
4. n8n coleta a vaga
5. filtros são executados
6. vaga é registrada
7. IA compara vaga e currículo
8. n8n calcula match
9. Google Sheets é atualizado
10. se match atingir o limite, Telegram envia aviso
```

O usuário precisa apenas analisar as vagas notificadas e decidir se deseja se candidatar.

---

# 19. Como testar

Para um teste ponta a ponta:

```text
1. Escolha um e-mail real de vaga
2. Aplique o marcador "Vagas Emprego"
3. Execute o WF01
4. Confira o parser
5. Confira o filtro de cargo
6. Confira o filtro de localização
7. Confira o Google Sheets
8. Abra a execução do WF02
9. Confira a resposta da IA
10. Confira o match
11. Confira a atualização da planilha
12. Confira a entrada do WF03
13. Confira o IF
14. Confira a mensagem no Telegram
```

---

# 20. Problemas comuns

## Vaga foi descartada no filtro de cargos

Verifique:

```text
cargo
```

e confirme se o termo existe na regex.

Exemplo:

Se o cargo for:

```text
Product Owner
```

a regex deve conter:

```text
product owner
```

---

## Filter mostra "No data"

Isso normalmente significa que o node anterior descartou o item.

Volte um node e verifique a aba:

```text
Discarded
```

---

## Remove Duplicates descarta tudo

O mesmo `id_email` já foi processado.

Durante teste:

```text
Deactivate
```

temporariamente o node.

Depois reative.

---

## WF03 vai para False mesmo com match alto

Confirme que a condição usa:

```text
{{ Number($json.match) }}
```

e não outro campo.

---

## Telegram mostra Forbidden

Erro comum:

```text
Forbidden: the bot can't send messages to the bot
```

Isso significa que provavelmente foi usado o ID do próprio bot.

Use:

```text
message.chat.id
```

da conversa entre você e o bot.

---

## IA retorna JSON inválido

Verifique:

- prompt;
- limite de tokens;
- tamanho da resposta;
- se o modelo está retornando Markdown.

Aumente, se necessário:

```text
Maximum Number of Tokens
```

---

# 21. Segurança

Nunca publique:

```text
token do Telegram
chaves de API
credenciais Google
senhas
dados pessoais
currículo completo com informações sensíveis
```

Antes de exportar workflows para o GitHub, revise os arquivos JSON.

---

# 22. Limitações

A automação depende das informações disponíveis nos alertas.

Algumas vagas podem não informar:

- salário;
- localização;
- tipo de contrato;
- modelo de trabalho;
- descrição completa.

Além disso, mudanças no template dos e-mails podem exigir atualização do parser.

---

# 23. Interpretação do Match

O match é apenas uma estimativa interna.

Ele não significa:

- chance real de contratação;
- garantia de entrevista;
- avaliação oficial do recrutador;
- recomendação definitiva para candidatura.

Ele serve para:

```text
priorizar a análise das vagas
```

---

# 24. Manutenção

Recomenda-se revisar periodicamente:

- lista de cargos;
- regex;
- currículo;
- limite de match;
- parser de e-mails;
- credenciais;
- integrações;
- mensagens do Telegram.

---

# 25. Evoluções futuras

Possíveis melhorias:

- parser dedicado para LinkedIn;
- parser dedicado para Gupy;
- pesos diferentes para requisitos obrigatórios;
- análise de senioridade;
- múltiplos currículos;
- dashboard Power BI;
- controle de candidaturas;
- status de entrevista;
- faixa de prioridade por match;
- relatórios semanais;
- notificações personalizadas.

---

# 26. Resumo

A solução permite transformar alertas de vagas em um processo automatizado:

```text
Receber
→ Extrair
→ Filtrar
→ Analisar
→ Comparar
→ Calcular
→ Registrar
→ Notificar
```

A Inteligência Artificial apoia a análise dos requisitos, enquanto o n8n mantém o controle das regras, integrações e cálculo.

A decisão final permanece humana.
