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

## Integração com IAs Conhecidas

Cada ferramenta de IA espera uma estrutura de pastas específica. Abaixo o guia para integrar este repositório nas principais IAs.

### Tabela Rápida

| IA | Pasta/Arquivo | Ação |
|---|---|---|
| **Claude Code** | `.claude/` + `CLAUDE.md` | Renomear `.agents/` → `.claude/`, `AGENTS.md` → `CLAUDE.md` |
| **Cursor** | `.cursor/rules/` | Copiar arquivos `.md` para `.cursor/rules/` |
| **Windsurf** | `.windsurf/rules/` | Copiar arquivos `.md` para `.windsurf/rules/` |
| **GitHub Copilot** | `.github/copilot-instructions.md` | Copiar conteúdo do `AGENTS.md` |
| **Roo Code / Cline** | `CLAUDE.md` + `.claude/` | Copiar `AGENTS.md` → `CLAUDE.md`, `.agents/` → `.claude/` |
| **Kilo AI** | `.kilo/` + `kilo.jsonc` | Referenciar por path absoluto ou URL (detalhado abaixo) |
| **Codex CLI** | `CODEX.md` | Copiar `AGENTS.md` → `CODEX.md` |

### Claude Code

1. Copie `AGENTS.md` para a raiz do seu projeto como `CLAUDE.md`
2. Copie a pasta `.agents/` como `.claude/`

```
meu-projeto/
├── CLAUDE.md
├── .claude/
│   ├── agent/
│   │   ├── security-auditor.md
│   │   └── doc-write.md
│   └── skills/
│       ├── senior-dev/SKILL.md
│       └── plan/SKILL.md
└── ... (seu código)
```

**Como usar:** O Claude Code lê automaticamente o `CLAUDE.md` como instrução principal do projeto. Para usar skills/agents, referencie os arquivos no `CLAUDE.md`:

```markdown
## Instruções Modulares

- Leia `.claude/agent/security-auditor.md` para auditoria de segurança
- Leia `.claude/skills/senior-dev/SKILL.md` para desenvolvimento sênior
```

Os arquivos em `.claude/` funcionam como instruções modulares carregadas sob demanda durante a conversa.

### Cursor

Copie os arquivos de regras para `.cursor/rules/`:

```bash
cp -r .agents/agent/* meu-projeto/.cursor/rules/
cp -r .agents/skills/* meu-projeto/.cursor/rules/
cp -r .agents/workflow/* meu-projeto/.cursor/rules/
```

O Cursor carrega automaticamente arquivos `.mdc` em `.cursor/rules/`.

**Como usar:** As regras em `.cursor/rules/` são aplicadas automaticamente com base no contexto do arquivo. Para usar um agent/skill específico, mencione no chat em modo Agent que deseja seguir as diretrizes do arquivo desejado (ex.: "siga as regras do security-auditor").

### Windsurf

Copie os arquivos para `.windsurf/rules/`:

```bash
cp -r .agents/agent/* meu-projeto/.windsurf/rules/
cp -r .agents/skills/* meu-projeto/.windsurf/rules/
cp -r .agents/workflow/* meu-projeto/.windsurf/rules/
```

**Como usar:** O Windsurf carrega automaticamente as regras em `.windsurf/rules/` conforme o contexto. Use o Cascade mencionando qual diretriz deseja ativar (ex.: "use o security-auditor para revisar este código").

### GitHub Copilot

Copie o conteúdo do `AGENTS.md` para `.github/copilot-instructions.md` no seu projeto. O Copilot lê este arquivo para entender o contexto e seguir as instruções.

**Como usar:** O Copilot aplica as instruções automaticamente nas sugestões de código — não há ativação manual de agents/skills. As diretrizes do `AGENTS.md` influenciam o comportamento das sugestões em todo o projeto.

### Roo Code / Cline

Mesma estrutura do Claude Code:

1. Copie `AGENTS.md` → `CLAUDE.md` na raiz do projeto
2. Copie `.agents/` → `.claude/`

**Como usar:** O Roo Code lê `CLAUDE.md` como instrução de sistema. Para usar agents como custom modes, adicione no `CLAUDE.md` referências aos arquivos em `.claude/agent/`. Também é possível configurar modos personalizados nas settings da extensão apontando para cada arquivo `.md`.

### Kilo AI

O Kilo AI permite referenciar as skills e agents diretamente por caminho absoluto, URL remota ou config global.

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

> As formas de ativação abaixo são específicas para **Kilo AI**. Para outras ferramentas, veja [Integração com IAs Conhecidas](#integração-com-ias-conhecidas).

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

> As formas de ativação abaixo são específicas para **Kilo AI**. Para outras ferramentas, veja [Integração com IAs Conhecidas](#integração-com-ias-conhecidas).

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

- A ferramenta de IA de sua escolha (Claude Code, Cursor, Windsurf, Copilot, Kilo AI, Roo Code, etc.)
- Para usar via URL remota: repositório público no GitHub

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
