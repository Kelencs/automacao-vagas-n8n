# Requisitos Funcionais

## RF-001 — Monitorar alertas de vagas

O sistema deve monitorar e-mails recebidos no Gmail com o marcador `Vagas Emprego`.

## RF-002 — Recuperar mensagem completa

O sistema deve recuperar o conteúdo completo do e-mail utilizando o ID da mensagem.

## RF-003 — Identificar fonte da vaga

O sistema deve identificar a origem do alerta, contemplando inicialmente:

- Indeed;
- LinkedIn;
- Gupy.

## RF-004 — Extrair dados da vaga

O sistema deve tentar extrair:

- ID do e-mail;
- fonte;
- cargo;
- empresa;
- localização;
- modelo de trabalho;
- tipo de contrato;
- salário;
- descrição;
- link.

## RF-005 — Tratar dados ausentes

Quando uma informação não estiver disponível, o sistema deve registrar:

```text
Não informado
```

## RF-006 — Filtrar cargos de interesse

O sistema deve permitir filtrar vagas por cargos e palavras-chave configuradas.

## RF-007 — Filtrar modelo de trabalho

O sistema deve permitir a passagem de vagas com modelo:

- remoto;
- não informado.

## RF-008 — Remover duplicidades

O sistema deve impedir o reprocessamento do mesmo alerta utilizando `id_email`.

## RF-009 — Registrar vaga

O sistema deve registrar novas vagas no Google Sheets.

## RF-010 — Disponibilizar currículo

O sistema deve disponibilizar o currículo ao workflow de análise.

## RF-011 — Analisar requisitos da vaga

A IA deve identificar requisitos presentes na descrição da vaga.

## RF-012 — Classificar requisitos

A IA deve classificar cada requisito com um dos status:

- `ATENDIDO`;
- `PARCIALMENTE_ATENDIDO`;
- `NAO_ENCONTRADO`.

## RF-013 — Classificar tipo do requisito

A IA deve classificar o requisito como:

- `OBRIGATORIO`;
- `DESEJAVEL`;
- `NAO_INFORMADO`.

## RF-014 — Apresentar evidência

Para cada requisito, a IA deve informar a evidência encontrada no currículo.

## RF-015 — Identificar pontos fortes

A IA deve listar os principais pontos fortes relacionados à vaga.

## RF-016 — Identificar lacunas

A IA deve listar lacunas entre vaga e currículo.

## RF-017 — Calcular match

O sistema deve calcular o match sem delegar o percentual à IA.

## RF-018 — Atualizar análise

O sistema deve atualizar a linha da vaga com:

- match;
- total de requisitos;
- pontos obtidos;
- pontos fortes;
- lacunas;
- justificativa;
- status.

## RF-019 — Alterar status

Após análise, o status da vaga deve ser alterado para:

```text
ANALISADA
```

## RF-020 — Verificar limite para notificação

O sistema deve comparar o match com um limite configurável.

## RF-021 — Montar mensagem

O sistema deve montar uma mensagem com os principais dados da vaga e da análise.

## RF-022 — Enviar Telegram

Quando o match atingir o limite, o sistema deve enviar uma mensagem pelo Telegram.

## RF-023 — Não candidatar automaticamente

O sistema não deve realizar candidatura automática.

