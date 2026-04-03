# my-config-ai

Repositório de configuração de Inteligência Artificial (AI) com skills, regras de código e padrões de projeto reutilizáveis em outros projetos.

Minha intenção aqui é pegar o melhor de vários "mundos" e sintetizar para a minha realidade de desenvolvimento de software. Onde aqui eu crio agentes, skills, regras, padrões e posso reutilizar não apenas em vários projetos mas em diversos tipos de IA como Kilo AI, Claude Code, Codex, etc.

## Estrutura

```
my-config-ai/
├── AGENTS.md              # Diretrizes para agentes IA neste projeto
├── LICENSE                # Apache 2.0 with Commons Clause
├── README.md              # Documentação principal
├── .kilo/                 # Configurações do Kilo para este projeto
└── .agents/               # Copiar para pasta padrão da IA
    ├── agent/
    │   ├── security-auditor.md
    │   └── doc-write.md
    ├── skills/
    │   ├── senior-dev/
    │   │   └── SKILL.md
    │   └── plan/
    │       └── SKILL.md
    └── workflow/
        └── atomic-commit/
            └── WORKFLOW.md
```

Preste bem atenção no mapa da estrutura desse projeto onde a pasta mais importante é `.agents/` onde ela deve ser levada para a pasta padrão da sua IA de preferência. Por exemplo:
Imagine que você tem Claude Code e deseja usar as configurações de IA deste projeto, basta você copiar a pasta `.agents` e renomeá-la para `.claude`. Ira ficar algo semelhante à:
```
meu-projeto/
├── CLAUDE.md              # O "Manual do Projeto" (Instruções, estilos e comandos)
├── .claude/
│   ├── settings.json      # Configurações compartilhadas (vai para o Git)
│   └── settings.local.json # Configurações privadas (Ignorado pelo Git)
│   ├── agent/
│   │   ├── security-auditor.md  # Agent de auditoria de segurança
│   │   └── doc-write.md         # Agent para documentação
│   └── skills/
│       └── senior-dev/
│           └── SKILL.md # Skill para desenvolvimento sênior
├── .gitignore             # O Claude adiciona .claude/settings.local.json aqui
└── ... (seu código)
```

## Uso em Outros Projetos
Aqui estou criando exemplo para reaproveitar estas configurações de IA em outros projetos utilizando Kilo AI.

### Opção 1: Via kilo.jsonc (Caminho Absoluto)

Adicione no `kilo.jsonc` do seu projeto:

```jsonc
{
  "skills": {
    "paths": ["/home/bruno/Projetos/my-config-ai/.agents/skills"]
  },
  "agents": {
    "paths": ["/home/bruno/Projetos/my-config-ai/.agents/agent"]
  }
}
```

### Opção 2: Via URL Remota (GitHub)

```jsonc
{
  "skills": {
    "urls": [
      "https://github.com/bruno-ns/my-config-ai/.agents/skills/senior-dev/SKILL.md"
    ]
  },
  "agents": {
    "urls": [
      "https://github.com/bruno-ns/my-config-ai/.agents/agent/security-auditor.md",
      "https://github.com/bruno-ns/my-config-ai/.agents/agent/doc-write.md"
    ]
  }
}
```

### Opção 3: Configuração Global (~/.kilo/kilo.jsonc)

Edite o arquivo `~/.config/kilo/kilo.jsonc` (crie se não existir):

```jsonc
{
  "skills": {
    "paths": ["https://github.com/bruno-ns/my-config-ai/.agents/skills"]
  },
  "agents": {
    "paths": ["https://github.com/bruno-ns/my-config-ai/.agents/agent"]
  }
}
```

### Opção 4: Via Terminal Kilo AI

Durante uma sessão, chame diretamente pelo nome:

```
/use security-auditor
```

Ou usando a ferramenta skill com o subagent:

```
skill name: security-auditor
```

## Skills Disponíveis

### senior-dev

Skill para desenvolvimento sênior com TDD, segurança, atomicidade e práticas de código limpo.

**Quando usar**: Quando pedir para escrever, implementar, codificar ou refatorar código.

Inclui:
- Princípios de execução sênior
- Fluxo TDD obrigatório
- Regras de desvio (bug fixes, segurança, bloqueios)
- Protocolo de commit atômico
- Diretrizes de autenticação JWT
- Checkpoints para decisões humanas

### plan

Skill para planejamento de projetos com análise de dependências e verificação de objetivos.

**Quando usar**: Ao pedir para planejar um projeto, criar fases e tarefas, ou definir roadmap.

Inclui:
- Análise de requisitos e histórias de usuário
- Decomposição em fases e tarefas
- Grafos de dependência e ondas de execução
- Verificação de objetivos (loop de revisão)
- Metodologia de planejamento reverso

**Ativação:**
```
/plan
```
Ou via skill tool:
```
skill name: plan
```

## Subagents Disponíveis

### security-auditor

Subagent especializado em auditoria de segurança, detecção de vulnerabilidades e aplicação de correções seguras.

**Quando usar**: Ao pedir para auditar código, verificar vulnerabilidades, ou aplicar correções de segurança.

Inclui:
- Análise estática de código
- Detecção de IDOR, Race Condition, SQL Injection, XSS, SSRF
- Testes de segurança automatizados
- Correções com explicação detalhada
- Auto-Ataque sob perspectiva ofensiva

**Método de ativação**:
```
/use security-auditor
```
Ou via skill tool:
```
skill name: security-auditor
```

### doc-write

Subagent especializado em escrever e atualizar documentação técnica baseada no código-fonte.

**Quando usar**: Ao pedir para criar, atualizar ou补完 documentação (README, architecture, getting_started, etc.).

Inclui:
- Geração de documentação a partir do código
- Suporte a múltiplos formatos (MD, API, deployment)
- Atualização automática baseada em mudanças
- Verificação de consistência

**Método de ativação**:
```
/use doc-write
```
Ou via skill tool:
```
skill name: doc-write
```

## O que está incluído

| Recurso                   | Descrição                                                        |
| ------------------------- | ---------------------------------------------------------------- |
| AGENTS.md                 | Diretrizes de estilo, nomenclatura, imports, tratamento de erros |
| senior-dev/SKILL.md       | Regras para desenvolvimento sênior com TDD e metodologia XP      |
| plan/SKILL.md             | Planejamento de projetos com análise de dependências              |
| agent/security-auditor.md | Auditoria de segurança e detecção de vulnerabilidades            |
| agent/doc-write.md        | Escrita e atualização de documentação técnica                    |

## Configurações Recomendadas

### kilo.jsonc completo

```jsonc
{
  "$schema": "https://app.kilo.ai/config.json",
  "model": "kilo/kilo-auto/free",
  "permission": {
    "bash": "allow"
  },
  "agent": {
    "paths": {
      "custom": "/home/bruno/Projetos/my-config-ai/.agents/agent"
    }
  },
  "skills": {
    "paths": ["/home/bruno/Projetos/my-config-ai/.agents/skills"]
  },
  "instructions": [
    "Sempre siga as diretrizes do AGENTS.md",
    "Use TDD para novas funcionalidades",
    "Commits atômicos - um arquivo por vez"
  ]
}
```

## Requisitos

- Kilo CLI ou VS Code Extension instalado
- Para usar via URL: repositório público no GitHub

## Contribuindo

1. Fork este repositório
2. Adicione suas skills em `.agents/skills/` ou agents em `.agents/agent/`
3. Atualize o AGENTS.md com novas regras
4. Atualize o README.md com a nova ferramenta
5. Commit: `feat: adicionar [skill|agent] [nome]`
6. Push e use em seus projetos

---

## Licença

Licenciado sob **Apache 2.0 com Cláusula Commons**.

- Uso livre para fork, PR e uso pessoal
- Proibido vender como SaaS, serviço ou produto comercial
- Proteção de patentes bidirecional
- Copyright Bruno Nascimento (bruno-ns)

Veja [LICENSE](./LICENSE) para detalhes.
