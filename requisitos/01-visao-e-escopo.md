# Visão e Escopo — Automação Inteligente de Vagas

## 1. Objetivo

Automatizar o recebimento, filtragem, análise e priorização de vagas de emprego recebidas por e-mail, comparando os requisitos das oportunidades com o currículo do candidato e notificando vagas com maior compatibilidade.

## 2. Problema

A busca manual por vagas exige tempo para:

- abrir alertas recebidos por e-mail;
- identificar cargos relevantes;
- verificar modelo de trabalho;
- ler a descrição da vaga;
- comparar requisitos com o currículo;
- registrar oportunidades;
- priorizar as vagas mais aderentes.

## 3. Solução proposta

A solução utiliza n8n para orquestrar três workflows:

- **WF01 - Coleta de Vagas**
- **WF02 - Análise e Match com Currículo**
- **WF03 - Notificação de Vagas**

Integrações principais:

- Gmail;
- Google Sheets;
- Google Gemini;
- Telegram.

## 4. Escopo incluído

- monitorar alertas de vagas no Gmail;
- processar e-mails marcados como `Vagas Emprego`;
- identificar a fonte da vaga;
- extrair dados da oportunidade;
- filtrar cargos de interesse;
- filtrar modelo de trabalho;
- impedir reprocessamento do mesmo e-mail;
- registrar vagas no Google Sheets;
- comparar vaga e currículo com IA;
- classificar requisitos;
- calcular percentual de compatibilidade;
- registrar pontos fortes e lacunas;
- notificar no Telegram quando o match atingir o limite configurado.

## 5. Fora de escopo

- candidatura automática;
- alteração automática do currículo;
- envio automático de mensagens para recrutadores;
- decisão automática sobre aceitar ou recusar uma vaga;
- previsão de contratação;
- garantia de entrevista.

## 6. Stakeholders

| Stakeholder | Interesse |
|---|---|
| Candidato | Encontrar vagas compatíveis com menor esforço manual |
| Analista de Requisitos | Definir regras, fluxos, exceções e critérios de aceite |
| Administrador da automação | Manter integrações e workflows |
| Plataformas de vagas | Origem dos alertas recebidos por e-mail |

## 7. Premissas

- o usuário recebe alertas de vagas por e-mail;
- os e-mails relevantes possuem o marcador `Vagas Emprego`;
- o currículo utilizado no WF02 está atualizado;
- as credenciais das integrações estão válidas;
- o conteúdo dos alertas pode variar entre plataformas.

## 8. Restrições

- algumas vagas não informam salário, localização ou tipo de contrato;
- templates de e-mail podem mudar;
- a qualidade da análise depende das informações disponíveis no alerta;
- o match é uma heurística interna do projeto.

## 9. Critério de sucesso

A solução será considerada funcional quando:

1. um novo alerta de vaga for identificado;
2. os dados essenciais forem extraídos ou marcados como `Não informado`;
3. vagas fora do perfil forem filtradas;
4. a oportunidade válida for registrada;
5. a IA classificar os requisitos;
6. o match for calculado;
7. a planilha for atualizada;
8. vagas acima do limite configurado forem notificadas no Telegram.

