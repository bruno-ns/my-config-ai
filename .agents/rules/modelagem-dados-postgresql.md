# Regra: Padrões de Modelagem de Dados PostgreSQL

Esta regra estabelece os padrões obrigatórios para criação de estruturas de banco de dados PostgreSQL, seguindo as diretrizes estabelecidas no arquivo de referência "criar_estrutura_banco_dados_ncmflow.sql".

## Diretrizes Obrigatórias de Implementação

### 1. Uso de Domínios (Domains)
- **PROIBIDO**: Utilizar tipos primitivos diretamente para campos padronizados
- **OBRIGATÓRIO**: Utilizar sempre os domínios previamente definidos para garantir a consistência dos tipos de dados em todo o esquema
- Exemplo correto: `criado_em public.dm_criado_em NOT NULL`
- Exemplo proibido: `criado_em TIMESTAMPTZ NOT NULL`

### 2. Colunas de Auditoria Obrigatórias
Toda e qualquer tabela criada deve, obrigatoriamente, conter as seguintes colunas de controle:
- `criado_em`: Tipo `public.dm_criado_em`, com restrição `NOT NULL`
- `criado_bd_usuario`: Tipo `public.dm_usuario_banco`, com restrição `NOT NULL`
- `alterado_em`: Tipo `public.dm_data_hora`, com valor padrão `DEFAULT NULL`
- `alterado_bd_usuario`: Tipo `public.dm_usuario_banco`, com valor padrão `DEFAULT NULL`

### 3. Documentação via Comentários (Metadata)
Todo objeto criado (tabela, coluna, domínio, trigger, função) deve ser documentado utilizando o comando `COMMENT ON`. Os comentários devem ser detalhados, em Português do Brasil, especificando:
- O objetivo do objeto/coluna
- O que deve ser armazenado
- Opções permitidas ou regras de negócio associadas
- Detalhes técnicos relevantes

Formato obrigatório:
```sql
COMMENT ON TABLE nome_tabela IS E'Descrição detalhada da tabela em Português do Brasil.';
COMMENT ON COLUMN nome_tabela.nome_coluna IS E'Descrição detalhada da coluna em Português do Brasil.';
```

### 4. Sistema de Log de Alterações (Audit Log)
Implementar mecanismo de rastreabilidade para cada tabela seguindo exatamente o padrão presente no arquivo de referência:

#### Funções Obrigatórias
1. **fun_trg_antes_atualizar_todas_tabelas()**
   - Atualiza automaticamente os campos `alterado_em` e `alterado_bd_usuario` antes de UPDATE
   - Inclui proteção contra execução durante replicação
   - Comentário obrigatório detalhando seu propósito

2. **fun_trg_gera_logger_todas_tabelas()**
   - Salva alterações (UPDATE/DELETE) em tabela de log no schema `logger_tabela`
   - Compara valores OLD e NEW para registrar apenas alterações reais
   - Inclui proteção contra execução durante replicação
   - Armazena dados em formato JSONB contendo as alterações
   - Comentário obrigatório detalhando seu propósito

#### Triggers Obrigatórios por Tabela
- **BEFORE INSERT OR UPDATE**: Executa `public.fun_trg_antes_atualizar_todas_tabelas()`
- **AFTER DELETE OR UPDATE**: Executa `public.fun_trg_gera_logger_todas_tabelas()`

### 5. Idioma e Estilo
- Todo o código SQL, nomes de objetos (quando aplicável ao padrão), comentários e documentação deve ser redigido em Português do Brasil
- Nomes de objetos devem seguir convenção `snake_case`
- Domínios devem ter prefixo `dm_` (ex: `dm_id`, `dm_nome`, `dm_criado_em`)
- Funções de trigger devem ter prefixo `fun_trg_` 
- Triggers devem ter padrões de nomeação:
  - `bef_i_u_nometabela` para BEFORE INSERT OR UPDATE
  - `aft_u_d_log_nometabela` para AFTER DELETE OR UPDATE

## Estrutura Padrão de Tabela

Toda nova tabela deve seguir esta estrutura mínima:

```sql
CREATE TABLE public.nome_tabela (
    id bigserial NOT NULL,
    uuid public.dm_uuid NOT NULL,
    criado_em public.dm_criado_em NOT NULL,
    criado_por public.dm_id DEFAULT null,
    criado_bd_usuario public.dm_usuario_banco NOT NULL,
    alterado_em public.dm_data_hora DEFAULT null,
    alterado_por public.dm_id DEFAULT null,
    alterado_bd_usuario public.dm_usuario_banco DEFAULT null,
    -- [campos específicos da entidade]
    CONSTRAINT pk_nometabela_id PRIMARY KEY (id)
);
```

## Padrões de Comentários Obrigatórios

### Para Tabelas
```sql
COMMENT ON TABLE public.nome_tabela IS E'Descrição clara do propósito da tabela em Português do Brasil.';
```

### Para Colunas
```sql
COMMENT ON COLUMN public.nome_tabela.coluna IS E'Descrição detalhada do que a coluna armazena, incluindo formato, restrições e exemplos quando relevante.';
```

### Para Domínios
```sql
COMMENT ON DOMAIN public.dm_nomedominio IS E'Descrição do domínio e seu uso previsto.';
```

### Para Funções de Trigger
```sql
COMMENT ON FUNCTION public.fun_trg_nomefuncao() IS E'Descrição detalhada do que a função faz, quando é executada e seu impacto.';
```

## Validação Pré-Implementação

Antes de gerar qualquer código SQL, deve-se:
1. Verificar se todos os domínios necessários já existem (caso contrário, defini-los primeiro)
2. Confirmar que as funções de trigger obrigatórias estão presentes no schema public
3. Validar que o schema `logger_tabela` existe para armazenamento de logs
4. Assegurar que todos os comentários serão em Português do Brasil

## Exemplo de Implementação Correta

Para uma entidade "Produto", a implementação seguiria exatamente este padrão:

```sql
-- Domínios específicos (se necessários)
CREATE DOMAIN public.dm_sku AS varchar(20);
COMMENT ON DOMAIN public.dm_sku IS E'Código de Estoque Unitário (SKU) para identificação única de produtos.';

-- Tabela principal
CREATE TABLE public.produto (
    id bigserial NOT NULL,
    uuid public.dm_uuid NOT NULL,
    criado_em public.dm_criado_em NOT NULL,
    criado_por public.dm_id DEFAULT null,
    criado_bd_usuario public.dm_usuario_banco NOT NULL,
    alterado_em public.dm_data_hora DEFAULT null,
    alterado_por public.dm_id DEFAULT null,
    alterado_bd_usuario public.dm_usuario_banco DEFAULT null,
    sku public.dm_sku NOT NULL,
    descricao public.dm_descricao_longa NOT NULL,
    ativo public.dm_ativo NOT NULL DEFAULT true,
    CONSTRAINT pk_produto_id PRIMARY KEY (id),
    CONSTRAINT unique_produto_sku UNIQUE (sku)
);

-- Comentários da tabela e colunas
COMMENT ON TABLE public.produto IS E'Cadastro de produtos comercializados pela empresa, contendo informações essenciais para controle de estoque e vendas.';
COMMENT ON COLUMN public.produto.id IS E'Identificador único do produto gerado automaticamente.';
COMMENT ON COLUMN public.produto.uuid IS E'Código único universal para o produto, usado em integrações externas.';
COMMENT ON COLUMN public.produto.sku IS E'Código de Estoque Unitário (SKU) para identificação única de produtos no controle de estoque.';
COMMENT ON COLUMN public.produto.descricao IS E'Descrição detalhada do produto, incluindo características relevantes para identificação e vendas.';
COMMENT ON COLUMN public.produto.ativo IS E'Indica se o produto está ativo disponível para venda ou inativo descontinuado.';

-- Triggers de auditoria
CREATE OR REPLACE TRIGGER bef_i_u_produto
    BEFORE INSERT OR UPDATE
    ON public.produto
    FOR EACH ROW
    EXECUTE PROCEDURE public.fun_trg_antes_atualizar_todas_tabelas();

CREATE OR REPLACE TRIGGER aft_u_d_log_produto
    AFTER DELETE OR UPDATE
    ON public.produto
    FOR EACH ROW
    EXECUTE PROCEDURE public.fun_trg_gera_logger_todas_tabelas();
```