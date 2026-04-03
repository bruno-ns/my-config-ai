# AGENTS.md - Repositório de Configuração Kilo AI

Repositório para definições de ferramentas e configurações do Kilo AI. Use como base para configurar o Kilo em novos projetos via `.jsonc`.

> Veja exemplos de uso em [README.md](./README.md)

> Licensed under Apache 2.0 with Commons Clause - See [LICENSE](./LICENSE)

---

## Estrutura

```
my-config-ai/
├── AGENTS.md
├── LICENSE
├── README.md
├── .kilo/                 # Configurações do Kilo para este projeto
├── .agents/
│   ├── rules/              # Regras personalizadas para o agente plan
│   ├── agent/
│   │   ├── security-auditor.md
│   │   └── doc-write.md
│   ├── skills/
│   │   ├── senior-dev/SKILL.md
│   │   └── plan/SKILL.md
│   └── workflow/
│       └── atomic-commit/WORKFLOW.md
```

---

## 1. Como Usar em Novos Projetos

### Via kilo.jsonc (caminho absoluto)

```jsonc
{
  "skills": { "paths": ["/caminho/absolute/para/my-config-ai/.agents/skills"] },
  "agents": { "paths": ["/caminho/absolute/para/my-config-ai/.agents/agent"] },
  "workflow": { "paths": ["/caminho/absolute/para/my-config-ai/.agents/workflow"] }
}
```

### Via URL Remota

```jsonc
{
  "skills": { "urls": ["https://raw.githubusercontent.com/usuario/my-config-ai/main/.agents/skills/senior-dev/SKILL.md"] },
  "agents": { "urls": ["https://raw.githubusercontent.com/usuario/my-config-ai/main/.agents/agent/security-auditor.md"] },
  "workflow": { "urls": ["https://raw.githubusercontent.com/usuario/my-config-ai/main/.agents/workflow/atomic-commit/WORKFLOW.md"] }
}
```

### Via Terminal Kilo AI

```
/use security-auditor
/use doc-write
/plan
/workflow atomic-commit
```

---

## 2. Protocolo de Commit Atômico

**Para commits automatizados, use:**
```
/commit
```

O workflow `atomic-commit` executa:
1. Verificação de tickets (Jira, GitHub, Azure DevOps)
2. Análise automática do tipo de mudança
3. Geração de descrição seguindo Conventional Commits
4. Interface interativa para confirmar/editar
5. Suporte a escopo, corpo e tickets

### Regras de Commit

- **PROIBIDO:** `git add .` ou `git add -A`
- **OBRIGATÓRIO:** Adicionar arquivos individualmente
- **MENSAGENS:** Formato `{tipo}({escopo}): {descrição}`

### Tipos de Commit

| Tipo       | Quando usar                                       |
| :--------- | :------------------------------------------------ |
| `feat`     | Novos arquivos de feature, novos módulos         |
| `fix`      | Correção de bugs                                  |
| `docs`     | Apenas alterações em documentação (.md)            |
| `style`    | Formatação de código sem mudança de lógica        |
| `refactor` | Mudança de código sem correção de bug nem feature |
| `perf`     | Mudanças que melhoram performance                 |
| `test`     | Apenas arquivos de teste                          |
| `build`    | Mudanças em build e dependências                  |
| `ci`       | Arquivos de CI/CD                                |
| `chore`    | Outras tarefas de manutenção                      |

---

## 3. Regras de Estilo de Código

### Princípios

- **Execute Não Analise**: Após 5 leituras sem código, execute ou reporte.
- **Validação**: Cada trecho validado com testes antes de avançar.
- **Commits Atômicos**: `git add path/to/file`, nunca `git add .`

### Padrões de Qualidade

- **Erros**: Trate todos explicitamente. Nunca engula erros.
- **Null Safety**: Sempre verifique null/undefined.
- **Tipos**: Use tipos explícitos. Evite `any`.
- **Auth**: Erros 401/403 são portões - pare e solicite credenciais.

### Convenções de Nomes

| Elemento   | Convenção      | Exemplo            |
| ---------- | -------------- | ------------------ |
| Arquivos   | kebab-case     | `user-profile.tsx` |
| Classes    | PascalCase     | `UserService`      |
| Funções    | camelCase      | `getUserById()`    |
| Constantes | UPPER_SNAKE    | `MAX_RETRY`        |
| Interfaces | PascalCase + I | `IUserProps`       |

### Erros

```typescript
try { return await riskyOperation() }
catch (error) {
  if (error instanceof SpecificError) handleSpecificError(error)
  throw new AppError('Failed', { cause: error })
}
```

---

## 4. Senior Dev Skill

Usado para escrever, implementar, codificar ou refatorar código.

### Regras de Execução

- **Contexto**: Leia AGENTS.md antes de editar.
- **Atomicidade**: Cada tarefa = commit isolado.
- **Paralisia**: Pare após 5 leituras sem ação.

### Regras de Desvio

- **Bugs**: Corrija lógica, nulos, segurança imediatamente.
- **Críticos**: Adicione validação, tratamento de erros, CSRF/CORS sem permissão.
- **Bloqueios**: Corrija imports quebrados, tipos incorretos.
- **Arquitetura**: NOVA tabela/tabela, Pare e solicite decisão humana.

### Autenticação JWT

- Token + Refresh token, refresh rotativo (uma vez), segredos diferentes, JIT no logout.

---

## 5. Locais de Arquivos

| Propósito      | Local               |
| -------------- | ------------------- |
| Skills         | `.agents/skills/`   |
| Agents         | `.agents/agent/`    |
| Workflows      | `.agents/workflow/` |
| Config global  | `~/.kilo/`          |
| Config projeto | `.kilo/`           |

---

## 6. Proibido

```typescript
// 1. any              const data: any = fetchData()
// 2. Engolir erros    try {} catch (e) { /* vazio */ }
// 3. Magic numbers   if (status === 1)
// 4. Função sem tipos function process(data) { return data }
// 5. Secrets          const API_KEY = 'sk-123'
```

---

## 7. Checkpoints

Pare e solicite decisão humana para: novas tabelas, mudar autenticação, migrações de bibliotecas, refatoração arquitetural, actions com secrets.

---

## 8. Skills Disponíveis

- **senior-dev**: Desenvolvimento sênior com TDD, segurança, atomicidade.
- **plan**: Planejamento de projetos com análise de dependências e verificação de objetivos.

**Ativação:** `/plan`

---

## 9. Agents Disponíveis

### security-auditor

Auditor de Segurança especializado em análise estática, detecção de vulnerabilidades e aplicação de correções seguras.

**Quando usar**: Ao pedir para auditar código, verificar vulnerabilidades, ou aplicar correções de segurança.

**Ativação:** `/use security-auditor`

### doc-write

Agente especializado em escrever e atualizar documentação técnica baseada no código-fonte.

**Quando usar**: Ao pedir para criar, atualizar ou补完 documentação (README, architecture, getting_started, etc.).

**Ativação:** `/use doc-write`

---

## 10. Workflows Disponíveis

### atomic-commit

Workflow para commits atômicos com validação de tickets, análise automática e interação com usuário.

**Quando usar**: Ao pedir para fazer commit de mudanças.

**Ativação:** `/commit` ou `workflow name: atomic-commit`