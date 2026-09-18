# Critérios de Aceite

## CA-001 — Coleta pelo Gmail

**Dado** um e-mail com marcador `Vagas Emprego`  
**Quando** o Gmail Trigger executar  
**Então** a mensagem deve seguir para processamento.

## CA-002 — Dados ausentes

**Dado** que o alerta não informa salário  
**Quando** a vaga for padronizada  
**Então** o campo `salario` deve receber `Não informado`.

## CA-003 — Cargo aceito

**Dado** um cargo presente na regex de interesse  
**Quando** o filtro for executado  
**Então** o item deve aparecer em `Kept`.

## CA-004 — Cargo não aceito

**Dado** um cargo fora da regex  
**Quando** o filtro for executado  
**Então** o item deve aparecer em `Discarded`.

## CA-005 — Modelo não informado

**Dado** `modelo_trabalho = Não informado`  
**Quando** o filtro de localização for executado  
**Então** a vaga pode seguir para análise.

## CA-006 — Duplicidade

**Dado** um `id_email` já processado  
**Quando** o Remove Duplicates executar  
**Então** o item deve ser descartado.

## CA-007 — Requisito atendido

**Dado** um requisito da vaga com evidência clara no currículo  
**Quando** a IA comparar os dados  
**Então** o status deve ser `ATENDIDO`.

## CA-008 — Requisito parcial

**Dado** um requisito com evidência relacionada, porém incompleta  
**Quando** a IA comparar os dados  
**Então** o status deve ser `PARCIALMENTE_ATENDIDO`.

## CA-009 — Requisito sem evidência

**Dado** um requisito sem evidência no currículo  
**Quando** a IA comparar os dados  
**Então** o status deve ser `NAO_ENCONTRADO`.

## CA-010 — Cálculo

**Dado** 4 requisitos, sendo:
- 3 atendidos;
- 1 parcialmente atendido;

**Quando** o match for calculado  
**Então** o resultado deve ser:

```text
(3 + 0,5) / 4 * 100 = 87,5%
```

A implementação pode arredondar conforme regra definida.

## CA-011 — Atualização do status

**Dado** que a análise foi concluída  
**Quando** o Google Sheets for atualizado  
**Então** `status` deve ser `ANALISADA`.

## CA-012 — Telegram verdadeiro

**Dado** `match = 83` e limite `75`  
**Quando** o IF executar  
**Então** deve seguir pelo ramo verdadeiro.

## CA-013 — Telegram falso

**Dado** `match = 71` e limite `75`  
**Quando** o IF executar  
**Então** deve seguir pelo ramo falso.

## CA-014 — Conteúdo da notificação

A mensagem deve conter, quando disponível:

- cargo;
- empresa;
- modelo de trabalho;
- match;
- pontos fortes;
- lacunas;
- justificativa;
- link.

