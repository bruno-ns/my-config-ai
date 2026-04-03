# Regra: Padrões de Qualidade para Planejamento

Ao criar planos de projeto, os seguintes padrões de qualidade devem ser seguidos:

## PROIBIDO

As seguintes expressões e práticas são proibidas em planos:
- "v1", "v2", "versão simplificada"
- "codificado por enquanto", "estático por enquanto"
- "Aprimoramento futuro", "espaço reservado"
- "Será conectado posteriormente"
- "versão básica", "implementação mínima"

## OBRIGATÓRIO

- Respeitar todas as decisões do usuário em IDEA.md
- Dividir decisões grandes em partes menores (nunca simplificar silenciosamente)
- Identificar áreas não cobertas explicitamente
- Usar linguagem de ação concluída nas tarefas (ex: "implementado", "adicionado", "corrigido")
- Cada tarefa deve ter critérios de aceitação claros
- Incluir análise de dependências entre tarefas

## Estrutura Obrigatória

Toda tarefa deve conter:
- **Descrição**: Detalhamento completo do que será feito
- **Dependências**: Referência explícita a tarefas anteriores
- **Critérios de aceitação**: Condições que definem conclusão
- **Tipo**: classification (feature, fix, refactor, tdd, docs, build)