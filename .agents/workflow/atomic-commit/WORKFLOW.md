---
name: atomic-commit
description: Protocolo de commit atômico para Git com validação de tickets, análise automática de mudanças e interação com o usuário.
mode: workflow
trigger: /commit
author: bruno-ns
version: 1.0.0
created: 2026-04-03
updated: 2026-04-03
permissions:
  read: allow
  bash: allow
  grep: allow
  glob: allow
---

# Workflow: Commit Atômico

Este workflow garante commits atômicos, rastreáveis e bem documentados seguindo as melhores práticas de desenvolvimento.

## Regras Fundamentais

- **PROIBIDO:** `git add .` ou `git add -A`
- **OBRIGATÓRIO:** Adicionar arquivos individualmente com `git add path/to/file`
- **MENSAGENS:** Devem ser concisas, explicativas e seguir Conventional Commits

## Fluxo de Execução

### Passo 1: Verificar Tickets Relacionados

**Pergunta ao usuário:**
> Este commit está relacionado a alguma história ou tarefa? Se sim, quais são os códigos?
> (exemplo: `PX-212`, `TPCP-288`, `#1582`, `PROJ-123`)

**Resposta esperada:** Código(s) do(s) ticket(s) no formato:
- Jira: `XX-999`
- GitHub/GitLab: `#1582`
- Azure DevOps: `PROJ-123`

---

### Passo 2: Analisar Alterações

Execute automaticamente `git status` e `git diff --cached` para analisar as mudanças.

**Determinar o Tipo de Mudança:**

| Tipo       | Quando usar                                                     |
| :--------- | :-------------------------------------------------------------- |
| `feat`     | Novos arquivos de feature, novos módulos, novas funcionalidades |
| `fix`      | Correção de bugs, correção de erros de lógica                   |
| `docs`     | Apenas alterações em documentação (.md, comentários JSDoc)      |
| `style`    | Formatação de código sem mudança de lógica (espaços, imports)   |
| `refactor` | Mudança de código que não corrige bug nem adiciona feature      |
| `perf`     | Mudanças que melhoram performance                               |
| `test`     | Apenas arquivos de teste                                        |
| `build`    | Mudanças em build, dependências, configurações de compilação    |
| `ci`       | Arquivos de CI/CD                                               |
| `chore`    | Outras tarefas de manutenção (deps update, config)              |
| `revert`   | Reverter commit anterior                                        |
| `merge`    | Merge de branch                                                 |

**Regras de análise automática:**
1. Se mudou `.spec.ts`, `.test.ts` → tipo `test`
2. Se mudou `.md` (exceto em pasta de config) → tipo `docs`
3. Se mudou `package.json`, `Cargo.toml`, `go.mod` → tipo `build`
4. Se mudou `.yml`, `.yaml` em `.github/`, `.gitlab-ci` → tipo `ci`
5. Se mudou apenas formatação/imports sem lógica → tipo `style`
6. Se adicionou nova funcionalidade → tipo `feat`
7. Se corrigiu bug/erro → tipo `fix`

---

### Passo 3: Gerar Descrição Curta

**Formato:** `{tipo}({escopo}): {descrição em ação concluída}`

**Regras:**
- Usar verbos no passado: adicionado, corrigido, atualizado, removido, refatorado, implementado
- Primeira letra minúscula
- Máximo 72 caracteres (pode estender até ~108 se necessário para clareza)
- Deve ser compreensível por qualquer pessoa
- Escopo é opcional mas recomendado para projetos maiores

**Exemplos de descrições geradas:**
- `feat(auth): adicionado módulo de pix por qrcode`
- `fix: corrigido validação de CPF no formulário`
- `docs: atualizada documentação da API`
- `refactor: refatorado componente TcsGet para usar validações`
- `test(user): adicionados testes de integração para autenticação`
- `build(deps): atualizado jose para versão 2.1.0`

---

### Passo 4: Montar Mensagem de Commit

**Componentes:**
1. **Linha curta** (obrigatório): `{tipo}({escopo}): {descrição}`
2. **Ticket** (se aplicável): `{TICKET}` na linha final
3. **Corpo** (opcional): Descrição mais detalhada

**Exemplo completo:**
```
feat(auth): implementado sistema de rotação JWT

- Adicionada biblioteca jose para manipulação de tokens
- Atualizado middleware de autenticação
- Implementado refresh token rotativo

PX-212
```

---

### Passo 5: Mostrar Mensagem e Confirmar

**Ação:** Montar a mensagem de commit e mostrar ao usuário:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 Mensagem de Commit
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
tipo:     {tipo}
escopo:   {escopo}
descrição:{descrição}
ticket:   {ticket}
arquivos: {lista de arquivos}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Esta mensagem está correta?
[C]onfirmar → Faz o commit
[E]ditar   → Permite alterar
[A]bortar  → Cancela o processo
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### Passo 6: Editar (Se Solicitado)

Se o usuário escolher **[E]ditar**, oferecer opções:

> O que você gostaria de alterar?
> - **[T]ipo** → Alterar o tipo (feat, fix, etc)
> - **[E]scopo** → Alterar o escopo
> - **[D]escrição** → Alterar a descrição curta
> - **[C]orpo** → Alterar/adicionar itens do corpo
> - **[T]icket** → Alterar o ticket
> - **[T]udo** → Editar toda a mensagem

Após receber a resposta:
- Aplicar a alteração solicitada
- Retornar ao Passo 5 com mensagem atualizada
- Repetir até confirmar ou cancelar

---

### Passo 7: Executar Commit

**Se o usuário confirmar com [C]:**

1. Executar `git commit -m "mensagem"`
2. Mostrar resultado do commit
3. Opcionalmente perguntar se deseja fazer push

**Se o usuário escolher [A]bortar:**
- Limpar staging area se necessário
- Informar que o processo foi cancelado

---

## Exemplos de Uso

### Exemplo 1: Commit simples (sem ticket)
```
> /commit
Este commit está relacionado a alguma história ou tarefa? # Responde: não

Analisando mudanças...
tipo detectado: feat
descrição gerada: adicionado componente de QR Code

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 Mensagem de Commit
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
tipo:     feat
descrição:adicionado componente de QR Code
arquivos: src/components/QRCode.tsx
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[C]onfirmar [E]ditar [A]bortar
```

### Exemplo 2: Commit com ticket
```
> /commit
Este commit está relacionado a alguma história ou tarefa? # Responde: PX-212

Analisando mudanças...
tipo detectado: fix
descrição gerada: corrigido bug de expiração de token

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 Mensagem de Commit
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
tipo:     fix
descrição:corrigido bug de expiração de token
ticket:   PX-212
arquivos: src/auth/token.ts
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[C]onfirmar [E]ditar [A]bortar
```

---

## Referência

Este workflow referencia o protocolo definido em:
`.agents/skills/senior-dev/SKILL.md` - Seção 4: Protocolo de Commit Atômico
