
# Histórias de Usuário

## Épico EP-01 — Coleta de Vagas

### US-001 — Receber alertas de vagas

**Como** candidata  
**Quero** que os alertas de vagas recebidos por e-mail sejam identificados automaticamente  
**Para** não precisar abrir manualmente cada mensagem.

**Critérios de aceite**

- Dado um novo e-mail com marcador `Vagas Emprego`
- Quando o Gmail Trigger detectar a mensagem
- Então o workflow deve iniciar o processamento.

---

### US-002 — Extrair informações da vaga

**Como** candidata  
**Quero** que os dados principais da vaga sejam extraídos do e-mail  
**Para** visualizar a oportunidade de forma estruturada.

**Critérios de aceite**

- cargo deve ser extraído quando disponível;
- empresa deve ser extraída quando disponível;
- descrição deve ser extraída quando disponível;
- link deve ser extraído quando disponível;
- campos ausentes devem receber `Não informado`.

---

### US-003 — Filtrar cargos relevantes

**Como** candidata  
**Quero** receber apenas vagas relacionadas aos cargos de interesse  
**Para** reduzir oportunidades sem aderência ao meu objetivo profissional.

**Critérios de aceite**

- cargos configurados devem passar;
- cargos não configurados devem ser descartados;
- a comparação deve ser case-insensitive.

---

### US-004 — Evitar vagas duplicadas

**Como** candidata  
**Quero** evitar o processamento repetido do mesmo alerta  
**Para** não duplicar registros e notificações.

**Critérios de aceite**

- o sistema deve comparar `id_email`;
- e-mails já processados devem ser descartados.

---

## Épico EP-02 — Análise com IA

### US-005 — Comparar vaga e currículo

**Como** candidata  
**Quero** que a descrição da vaga seja comparada ao meu currículo  
**Para** entender minha aderência à oportunidade.

**Critérios de aceite**

- a IA deve usar apenas vaga e currículo;
- a IA não deve inventar competências;
- cada requisito deve possuir status e evidência.

---

### US-006 — Visualizar pontos fortes

**Como** candidata  
**Quero** visualizar os pontos fortes do meu perfil em relação à vaga  
**Para** identificar rapidamente onde tenho maior aderência.

---

### US-007 — Visualizar lacunas

**Como** candidata  
**Quero** visualizar os requisitos não encontrados ou parcialmente atendidos  
**Para** avaliar se a vaga faz sentido antes de me candidatar.

---

### US-008 — Receber percentual de compatibilidade

**Como** candidata  
**Quero** receber um percentual de match calculado de forma objetiva  
**Para** priorizar a análise das vagas.

**Critérios de aceite**

- a IA não deve definir o percentual;
- o n8n deve calcular;
- atendido vale 1;
- parcialmente atendido vale 0,5;
- não encontrado vale 0.

---

## Épico EP-03 — Notificação

### US-009 — Receber vagas com alto match

**Como** candidata  
**Quero** receber no Telegram vagas que atingirem o limite configurado  
**Para** ser avisada rapidamente sobre oportunidades prioritárias.

**Critérios de aceite**

- match abaixo do limite não envia mensagem;
- match igual ou superior envia mensagem;
- a mensagem deve conter cargo, empresa, match e link.

---

### US-010 — Acessar a vaga pela notificação

**Como** candidata  
**Quero** receber o link da oportunidade na mensagem  
**Para** acessar a vaga diretamente.

---

## Épico EP-04 — Controle e Rastreabilidade

### US-011 — Acompanhar vagas analisadas

**Como** candidata  
**Quero** manter as vagas e análises em uma planilha  
**Para** consultar histórico e resultados.

---

### US-012 — Diferenciar vagas novas e analisadas

**Como** candidata  
**Quero** visualizar o status da vaga  
**Para** saber quais oportunidades já passaram pela análise.

Estados:

```text
NOVA
ANALISADA
```
