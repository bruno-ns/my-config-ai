---
name: plan
description: Cria planos de fase executáveis com detalhamento de tarefas, análise de dependências e verificação de objetivos. Use quando o usuário pedir para planejar um projeto, criar roadmap, definir fases e tarefas, ou estruturar um desenvolvimento.
license: Apache-2.0
metadata:
  author: bruno-ns
  version: 1.0.0
  created: 2026-04-03
  mode: skill
  trigger: /plan
compatibility: Requer acesso a sistema de arquivos para criar arquivos de plano em .kilo/plans/
---

# Habilidade: Plan (Planejamento de Projetos)

## Objetivo

Criar planos de fase executáveis com detalhamento de tarefas, análise de dependências e verificação de objetivos.

## Contexto

- **Usuário**: Visionário/dono do produto (1 pessoa)
- **Executor**: Agent AI (Kilo) (1 executor)
- **Sem equipes**: Sem cerimônias, partes interessadas ou custos de coordenação
- **Estimativa**: Tempo de execução do Agent AI, não tempo de desenvolvimento humano

## Fluxo de Execução

### 1. Pesquisa (se necessário)
- Buscar informações relevantes sobre tecnologias, bibliotecas ou padrões
- Usar ferramentas de busca e fetching para coletar contexto

### 2. Planejamento
- Ler arquivos de contexto: `IDEA.md`, `AGENTS.md`, `CLAUDE.md` (se existirem)
- Extrair todos os requisitos, histórias de usuário, critérios de aceitação e restrições
- Identificar áreas não cobertas
- Dividir planos em fases e tarefas
- Construir grafos de dependência e atribuir ondas de execução

### 3. Verificação
- Revisar planos existentes (modo de revisão)
- Loop de revisão: máximo 3 iterações
- Verificar se objetivos foram atingidos

### 4. Concluído
- Retornar resultados estruturados ao orquestrador

## Estrutura do Plano

### Arquivo: `.kilo/plans/PLAN.md`

```markdown
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

##### Tarefa 1.2: [Nome]
- **Descrição**: [Detalhamento]
- **Dependências**: [Tarefas anteriores]
- **Critérios de aceitação**: [O que define conclusão]
- **Tipo**: [feature|fix|refactor|tdd|docs|build]

### Fase 2: [Nome da Fase]
[...]

## Grafo de Dependências

```
[tarefa1] → [tarefa2] → [tarefa3]
     ↓
[tarefa4] → [tarefa5]
```

## Verificação

- [ ] Requisito 1 coberto
- [ ] Requisito 2 coberto
- [ ] Áreas não cobertas identificadas
```

## Metodologia: Planejamento Reverso de Objetivos

1. **Iniciar do objetivo final**: O que o usuário quer alcançar?
2. **Derivar requisitos essenciais**: Quais funcionalidades são necessárias?
3. **Identificar dependências**: O que precisa ser feito primeiro?
4. **Atribuir ondas de execução**: Quais tarefas podem ser paralelizadas?

## TDD (Test Driven Development)

- **Determinar pelo IDEA.md/AGENTS.md/CLAUDE.md**
- Se TDD está ativado:
  - Criar plano TDD dedicado (tipo: `tdd`)
  - Incluir: vermelho → verde → refatorar
- Se não:
  - Tarefa padrão em plano padrão
- **Candidatos TDD**:
  - Lógica de negócios com E/S definida
  - Endpoints de API com contratos de request/response
  - Transformações de dados
  - Regras de validação
  - Algoritmos
  - Máquinas de estado
  - Segurança

## Regras de Qualidade

### PROIBIDO
- "v1", "v2", "versão simplificada"
- "codificado por enquanto", "estático por enquanto"
- "Aprimoramento futuro", "espaço reservado"
- "Será conectado posteriormente"
- Simplificar decisões do usuário silenciosamente

### OBRIGATÓRIO
- Respeitar todas as decisões do usuário em IDEA.md
- Dividir decisões grandes em partes menores
- Identificar áreas não cobertas explicitamente
- Usar linguagem de ação concluída nas tarefas

## Loop de Revisão (Máximo 3 iterações)

1. **Primeira revisão**: Verificar completude dos requisitos
2. **Segunda revisão**: Validar dependências e sequência
3. **Terceira revisão**: Confirmar critérios de aceitação

## Arquivos de Contexto

Sempre ler, nesta ordem:
1. `IDEA.md` - Visão e objetivos do projeto
2. `AGENTS.md` - Diretrizes e padrões do projeto
3. `CLAUDE.md` - Configurações específicas (se existir)

## Output

Retornar estrutura organizada:
- Resumo do plano
- Lista de fases e tarefas
- Dependências identificadas
- Áreas não cobertas
- Próximos passos sugeridos

## Regras Relacionadas

Esta habilidade utiliza as seguintes regras do projeto:
- `.agents/rules/plan-quality.md` - Padrões de qualidade para planejamento
- `.agents/rules/plan-structure.md` - Estrutura de arquivos de plano

---

## Referências

Esta habilidade referencia e expande o protocolo de commit atômico definido em:
- `.agents/skills/senior-dev/SKILL.md` - Seção 4: Protocolo de Commit Atômico
- `.agents/workflow/atomic-commit/WORKFLOW.md`