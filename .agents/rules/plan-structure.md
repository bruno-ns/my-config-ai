# Regra: Estrutura de Arquivos de Plano

Ao criar arquivos de plano no diretório `.kilo/plans/`, siga a seguinte estrutura:

## Estrutura do Arquivo `.kilo/plans/PLAN.md`

```
# Plano: [Nome do Projeto]

## Contexto
- **Objetivo**: [Breve descrição]
- **Escopo**: [O que está incluído e excluído]
- **Restrições**: [Limitações identificadas]

## Fases

### Fase 1: [Nome da Fase]
- **Objetivo**: [O que será alcançado]
- **Estimativa**: [Tempo estimado]

#### Tarefas

##### Tarefa 1.1: [Nome]
- **Descrição**: [Detalhamento]
- **Dependências**: [Tarefas anteriores]
- **Critérios de aceitação**: [O que define conclusão]
- **Tipo**: [feature|fix|refactor|tdd|docs|build]
```

## Convenções de Nomenclatura

- Nome do arquivo: `PLAN.md` para plano principal, ou `fase-1.md`, `fase-2.md` para planos de fase específicos
- Fases numeradas: "Fase 1", "Fase 2", etc.
- Tarefas numeradas: "Tarefa 1.1", "Tarefa 1.2", etc.
- Usar linguagem de ação concluída nos títulos

## Seções Obrigatórias

Todo plano deve conter:
1. **Contexto**: Objetivo, escopo e restrições
2. **Fases**: Divisão do trabalho em etapas executáveis
3. **Tarefas**: Detalhamento de cada item de trabalho
4. **Grafo de Dependências**: Representação visual das dependências
5. **Verificação**: Checklist de cobertura de requisitos