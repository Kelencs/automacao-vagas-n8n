
# Regras de Negócio

## RN-001 — Fonte autorizada

Somente alertas recebidos por canais configurados devem ser processados.

## RN-002 — Marcador de entrada

O Gmail deve utilizar o marcador:

```text
Vagas Emprego
```

como critério principal de entrada.

## RN-003 — Informação ausente

Nenhum dado ausente deve ser inventado.

Valor padrão:

```text
Não informado
```

## RN-004 — Filtro por cargo

A vaga somente segue para análise quando o cargo corresponder aos termos configurados no filtro.

## RN-005 — Modelo de trabalho

O fluxo pode aceitar:

```text
remoto
OU
não informado
```

## RN-006 — Duplicidade

Uma vaga não deve ser processada novamente quando o mesmo `id_email` já tiver sido processado.

## RN-007 — Evidência obrigatória

Um requisito só pode ser classificado como `ATENDIDO` quando houver evidência suficiente no currículo.

## RN-008 — Evidência parcial

Quando existir conhecimento relacionado, mas não confirmação completa, deve ser utilizado:

```text
PARCIALMENTE_ATENDIDO
```

## RN-009 — Ausência de evidência

Quando não houver evidência:

```text
NAO_ENCONTRADO
```

## RN-010 — Pontuação

A pontuação será:

```text
ATENDIDO = 1
PARCIALMENTE_ATENDIDO = 0,5
NAO_ENCONTRADO = 0
```

## RN-011 — Fórmula do match

```text
match = (pontos_obtidos / total_requisitos) * 100
```

## RN-012 — Match sem requisitos

Quando nenhum requisito for identificado:

```text
match = 0
```

## RN-013 — Notificação

Somente vagas com match maior ou igual ao limite configurado devem seguir para o Telegram.

## RN-014 — Limite configurável

O limite de notificação deve poder ser alterado sem modificar o cálculo do match.

## RN-015 — Candidatura

O sistema não deve enviar candidatura automaticamente.

## RN-016 — Currículo

A IA deve considerar somente informações disponíveis no currículo fornecido.

## RN-017 — Conhecimento em estudo

Conhecimento marcado como “em estudo” não deve ser tratado automaticamente como experiência profissional consolidada.

## RN-018 — Similaridade não implica equivalência

Conhecimentos relacionados não devem ser tratados como tecnologias exatas sem evidência.

Exemplo:

```text
consultas a banco de dados ≠ PostgreSQL comprovado
```
