# Casos de Uso

# UC-001 — Processar novo alerta de vaga

## Ator principal

Candidato

## Ator secundário

Gmail

## Pré-condições

- Gmail conectado ao n8n;
- marcador `Vagas Emprego` configurado;
- workflow publicado.

## Gatilho

Novo e-mail de vaga identificado.

## Fluxo principal

1. Gmail Trigger detecta a mensagem.
2. O sistema obtém o e-mail completo.
3. O sistema identifica a fonte.
4. O sistema extrai os dados da vaga.
5. O sistema padroniza os campos.
6. O sistema filtra o cargo.
7. O sistema filtra o modelo de trabalho.
8. O sistema verifica duplicidade.
9. O sistema registra a vaga no Google Sheets.
10. O sistema chama o WF02.

## Fluxos alternativos

### A1 — Cargo fora do interesse

1. O sistema identifica que o cargo não corresponde ao filtro.
2. A vaga é descartada.
3. O fluxo termina.

### A2 — Modelo presencial

1. O sistema identifica modelo presencial.
2. A vaga é descartada.
3. O fluxo termina.

### A3 — Duplicidade

1. `id_email` já foi processado.
2. O item é descartado.
3. O fluxo termina.

## Pós-condição

A vaga válida é registrada e enviada para análise.

---

# UC-002 — Analisar vaga com currículo

## Ator principal

Candidato

## Ator secundário

Google Gemini

## Pré-condições

- vaga válida recebida do WF01;
- currículo disponível;
- Gemini configurado.

## Fluxo principal

1. WF02 recebe os dados.
2. O currículo é adicionado.
3. A descrição da vaga é enviada ao Gemini.
4. O Gemini identifica os requisitos.
5. Cada requisito é comparado ao currículo.
6. O Gemini devolve status e evidências.
7. O n8n calcula o match.
8. O Google Sheets é atualizado.
9. O WF03 é acionado.

## Exceções

### E1 — Descrição indisponível

1. A descrição está vazia.
2. A IA retorna lista de requisitos vazia.
3. O match é 0.
4. A planilha é atualizada.

### E2 — Resposta inválida da IA

1. A resposta não pode ser interpretada.
2. O node deve falhar.
3. A execução fica disponível para diagnóstico.

## Pós-condição

A vaga possui análise e match registrados.

---

# UC-003 — Notificar vaga no Telegram

## Ator principal

Candidato

## Ator secundário

Telegram

## Pré-condições

- WF03 publicado;
- Telegram Bot configurado;
- Chat ID válido;
- análise da vaga concluída.

## Fluxo principal

1. WF03 recebe os dados da vaga.
2. O sistema lê o match.
3. O sistema compara com o limite.
4. O match atinge o limite.
5. O sistema monta a mensagem.
6. O Telegram envia a mensagem.

## Fluxo alternativo

### A1 — Match abaixo do limite

1. O match é inferior ao limite.
2. O fluxo segue pelo ramo falso.
3. Nenhuma mensagem é enviada.

## Pós-condição

O candidato recebe uma notificação somente para vagas prioritárias.

