# ProbLab

Este projeto tem objetivo de construir um sistema baseado em probabilidade, estatística e simulação para analisar sorteios aleatórios, gerar combinações otimizadas e fornecer insights quantitativos — sem prometer previsão de resultados. Criar uma plataforma de análise probabilística aplicada a jogos aleatórios, com foco em geração de jogos, simulação e visualização de dados — podendo evoluir primeiro para API e depois em outro momento (por último) SaaS.

Este é um projeto que terá dois ambiente, um de desenvolvimento e outro de produção. Será enviado para produção real. Deve ser feito em camadas ou módulos.

## Nome do Projeto e Sobre

O nome deve ser `ProbLab` é um projeto de análise de sorteios e loterias voltado ao estudo estatístico e probabilístico de resultados de sorteios. Ele combina ciência de dados e simulação para investigar padrões, distribuições de probabilidade, experimentos baseados em sorte e estratégias de modelagem que ajudam a compreender o comportamento de jogos de azar. Seu foco é analítico e produzir simulação, não preditivo.

### Principais fatos

* **Tipo de projeto:** Análise e simulação de sorteios e loterias
* **Área principal:** Probabilidade, Distribuições Hipergeométrica, Combinação (análise combinatória) e estatística aplicada
* **Finalidade:** Explorar padrões e conceitos de incerteza

### Contexto e objetivos

O ProbLab surgiu como um ambiente de exploração para estudantes e pesquisadores interessados em compreender conceitos de probabilidade por meio de simulações interativas. Em vez de buscar previsões, o projeto enfatiza a análise de variações, tendências aparentes, padrões de sequências, as sequências mais longas e mais curta, e limites do raciocínio probabilístico em contextos de loteria.
Para cada item explorado deve ser atribuído um peso, por exemplo as tendências deve ter um peso baseado nos números já sorteados.

Com os dados e insumos gerados a intenção é expor isso em um site e aplicativo web disponibilizando as combinações e suas estatísticas geradas.

### Metodologia e ferramentas

O projeto utiliza coleções de dados históricos de sorteios, geradores de números aleatórios e representações gráficas para demonstrar leis como a dos grandes números, Distribuições Hipergeométrica, Combinação (análise combinatória), Independência Estatística (Cada sorteio não “lembra” o anterior), e distribuições binomiais. Ferramentas computacionais e visualizações permitem comparar resultados simulados e reais, incentivando o pensamento estatístico crítico.

### Onde entra a análise (o que realmente dá pra fazer)

Para gerar valor no projeto:
- Análises úteis (e honestas)
- Frequência de números sorteados
- Frequência de números pares e impares
- Distribuição de pares/ímpares
- Intervalo entre aparições
- Soma dos números sorteados
- Clusterização de padrões
- Identificar vieses (se existirem)
- Criar heurísticas


## Arquitetura e Tecnologia

As bibliotecas e frameworks só devem ser adicionados no projeto de acordo com o que for precisando no avanço do projeto, para não ficar com bibliotecas e frameworks não utilizados.
Para o Python deve ser utilizadas frameworks e bibliotecas mais utilizadas na comunidade de desenvolvimento que sejam estáveis e com mais stars no GitHub.

### Backend
- Python
  - FastAPI
  - NumPy
  - Pandas
  - SciPy
  - Matplotlib
- PostgreSQL (última versão estável)

### Frontend

- Tailwind CSS
- Framework open source de autenticação

## Módulos

Módulo Simulação
gerar milhões de sorteios com base no histórico de sorteio
validar distribuição

Módulo Análise - Números já sorteados
frequência de números
padrões estatísticos
distribuição de resultados

Módulo Geração inteligente
criar jogos com critérios:
balanceamento
dispersão
evitar padrões ruins

Módulo Produto
API ou sistema web
dashboard
possível monetização

## Objetivo a ser evitado - errado (evite isso)

“Prever números da loteria”

Isso coloca no caminho errado:
- matematicamente inválido
- perde credibilidade
- não escala como produto sério