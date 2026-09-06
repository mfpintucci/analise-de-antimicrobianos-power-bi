# 📊 Análise de Antimicrobianos — Power BI | Data Engineering & Analytics

Projeto desenvolvido em **Microsoft Power BI** com foco em **pipeline de dados, transformação, modelagem dimensional e análise do consumo mensal de antimicrobianos**.

O projeto foi originalmente inspirado por uma necessidade gerencial real de acompanhamento de consumo ao longo do ano e posteriormente adaptado para portfólio utilizando **dados inteiramente sintéticos**, sem qualquer informação real de pacientes, profissionais, instituições ou operações assistenciais.

Mais do que a construção de um dashboard, este projeto demonstra a implementação de um fluxo de dados reutilizável, contemplando:

- ingestão automatizada de arquivos CSV;
- consolidação de múltiplos arquivos mensais;
- padronização de estrutura;
- transformação e limpeza com Power Query;
- aplicação de regras de negócio;
- construção de camada de staging;
- criação de tabela fato;
- criação de dimensão temporal;
- modelagem dimensional;
- desenvolvimento de medidas DAX;
- criação de camada semântica;
- atualização automatizada por inclusão de novos arquivos;
- construção de dashboards orientados ao usuário.

---

# 🎯 Objetivo

O objetivo principal do projeto é responder à seguinte pergunta:

> Como o consumo de cada antimicrobiano evolui ao longo dos meses?

Além da evolução temporal, o dashboard permite:

- selecionar individualmente um antimicrobiano;
- visualizar o consumo acumulado no período;
- acompanhar a média mensal de consumo;
- identificar o maior consumo registrado em um mês;
- identificar o mês de maior consumo;
- consultar o ranking dos antimicrobianos mais consumidos;
- analisar o ranking por mês;
- atualizar automaticamente a base com novos arquivos mensais.

---

# 🧪 Dados demonstrativos

Todos os dados disponibilizados neste repositório são **inteiramente sintéticos**.

Eles foram gerados exclusivamente para demonstrar a arquitetura do projeto e o funcionamento do pipeline e do dashboard.

Os dados não correspondem a:

- pacientes reais;
- profissionais reais;
- atendimentos reais;
- instituições reais;
- movimentações reais;
- quantitativos reais;
- registros provenientes de qualquer unidade de saúde.

Os nomes de medicamentos são utilizados apenas para contextualização da análise.

Todos os identificadores, datas, pacientes, profissionais, quantidades e movimentações presentes nos arquivos foram criados exclusivamente para fins demonstrativos.

---

# 🏗️ Arquitetura da Solução

A solução foi estruturada em camadas, separando ingestão, transformação, regra de negócio, modelagem e consumo analítico.

```text
Arquivos CSV mensais
        ↓
Camada de Ingestão
   Power Query / Folder
        ↓
Camada de Staging
   Dados consolidados
        ↓
Camada de Transformação
 limpeza + tipagem + filtros
        ↓
Camada de Regra de Negócio
 tratamento de saídas e estornos
        ↓
Fato_Antimicrobianos
        ↓
Modelo Dimensional
Fato + Dim_Calendario
        ↓
Camada Semântica
     Medidas DAX
        ↓
Camada Analítica
      Dashboard
```

Essa organização permite que novos arquivos mensais sejam adicionados sem necessidade de reconstrução do modelo.

---

# 🗂️ Fonte dos Dados

Os dados são organizados mensalmente em arquivos `.CSV`.

Estrutura utilizada:

```text
dados_exemplo/
├── 2026_01_JANEIRO.csv
├── 2026_02_FEVEREIRO.csv
├── 2026_03_MARCO.csv
├── 2026_04_ABRIL.csv
├── 2026_05_MAIO.csv
├── 2026_06_JUNHO.csv
├── 2026_07_JULHO.csv
└── 2026_08_AGOSTO.csv
```

O Power BI utiliza o conector de **Pasta**, permitindo a ingestão automática de todos os arquivos compatíveis existentes no diretório.

Dessa forma, a inclusão de um novo período exige apenas a adição de um novo arquivo com a mesma estrutura.

---

# 🔄 Pipeline de Dados

O pipeline executa as seguintes etapas:

1. leitura automática dos arquivos CSV existentes na pasta;
2. identificação e combinação dos arquivos;
3. padronização do schema;
4. tipagem das colunas;
5. tratamento de campos de data;
6. limpeza de valores textuais;
7. remoção de atributos não utilizados;
8. filtragem das operações relevantes;
9. aplicação das regras de negócio;
10. tratamento dos estornos;
11. criação da quantidade ajustada;
12. geração da tabela fato;
13. carregamento apenas das estruturas necessárias ao modelo analítico.

Fluxo simplificado:

```text
CSV
 ↓
Ingestão
 ↓
Staging
 ↓
Transformação
 ↓
Regra de negócio
 ↓
Tabela Fato
 ↓
Modelo dimensional
 ↓
Camada semântica
 ↓
Dashboard
```

---

# 🧱 Camada de Staging

A consulta principal utilizada para consolidação dos arquivos funciona como uma camada intermediária de preparação.

```text
Dados Mensais
```

Essa camada é responsável por:

- receber os arquivos brutos;
- consolidar os diferentes períodos;
- preservar a estrutura original;
- manter rastreabilidade dos registros;
- servir como origem para as transformações posteriores.

A consulta de staging permanece com o **carregamento desabilitado**, evitando que dados intermediários sejam carregados desnecessariamente no modelo.

Essa abordagem melhora a organização e reduz redundância.

---

# 📦 Tabela Fato

A partir da camada de staging foi criada:

```text
Fato_Antimicrobianos
```

Principais campos utilizados:

| Campo | Descrição |
|---|---|
| `DATA` | Data da movimentação |
| `ANTIMICROBIANO` | Identificação do medicamento |
| `QUANTIDADE` | Quantidade registrada originalmente |
| `QUANTIDADE_AJUSTADA` | Quantidade tratada para cálculo do consumo |
| `OPERACAO` | Tipo de movimentação |
| `ARQUIVO_ORIGEM` | Arquivo CSV responsável pelo registro |
| `IDADE_PACIENTE` | Informação complementar da base demonstrativa |

Informações que pudessem representar identificação direta de pessoas não fazem parte da versão final do projeto publicada no GitHub.

---

# 🧹 Transformação e Qualidade dos Dados

O processo de transformação foi realizado utilizando **Power Query**.

Entre as principais etapas aplicadas estão:

- combinação automática dos arquivos;
- padronização de schema;
- alteração de tipos de dados;
- tratamento de datas;
- limpeza de strings;
- remoção de espaços extras;
- remoção de caracteres invisíveis;
- tratamento de valores nulos;
- validação de erros;
- filtragem de operações;
- remoção de campos não utilizados;
- criação de campos derivados;
- rastreabilidade por arquivo de origem;
- validação da qualidade das colunas.

A coluna:

```text
ARQUIVO_ORIGEM
```

foi mantida para permitir rastrear um registro até o arquivo mensal responsável pela sua ingestão.

Essa estratégia facilita processos de auditoria, conferência e investigação de possíveis inconsistências.

---

# ⚙️ Regra de Negócio

Uma etapa fundamental do projeto foi transformar os registros transacionais em uma métrica capaz de representar corretamente o consumo.

Nem toda movimentação registrada representa consumo efetivo.

De forma geral:

- operações de saída são tratadas como valores positivos;
- estornos aplicáveis são tratados como valores negativos;
- operações administrativas não relevantes para a análise são excluídas.

Foi criada a coluna:

```text
QUANTIDADE_AJUSTADA
```

utilizando a seguinte lógica no Power Query:

```powerquery
if Text.StartsWith([OPERACAO], "ESTORNO")
then -[QUANTIDADE]
else [QUANTIDADE]
```

Dessa forma:

```text
Consumo Líquido = Saídas - Estornos
```

A coluna original `QUANTIDADE` foi preservada para garantir rastreabilidade do dado recebido da fonte.

---

# 🗃️ Modelagem Dimensional

Após o processo de transformação, os dados são organizados em um modelo dimensional simples.

```text
              Dim_Calendario
                     │
                     │ 1
                     │
                     │ *
            Fato_Antimicrobianos
```

O relacionamento é do tipo:

```text
1 : N
```

Onde:

- `Dim_Calendario` representa o lado `1`;
- `Fato_Antimicrobianos` representa o lado `N`.

A dimensão calendário controla todas as análises temporais realizadas no dashboard.

Essa separação entre fatos e atributos temporais facilita:

- agregações;
- análise por período;
- comparação mensal;
- reutilização de medidas;
- manutenção do modelo;
- expansão futura da solução.

---

# 📅 Dimensão Calendário

Foi criada uma dimensão calendário utilizando DAX:

```DAX
Dim_Calendario =
ADDCOLUMNS(
    CALENDAR(
        MIN(Fato_Antimicrobianos[DATA]),
        MAX(Fato_Antimicrobianos[DATA])
    ),
    "Ano", YEAR([Date]),
    "NumeroMes", MONTH([Date]),
    "Mes", FORMAT([Date], "MMMM"),
    "MesAbrev", FORMAT([Date], "MMM"),
    "MesAno", FORMAT([Date], "MMM/yyyy"),
    "AnoMes", YEAR([Date]) * 100 + MONTH([Date])
)
```

A coluna `MesAno` é classificada utilizando `AnoMes`, garantindo a ordenação cronológica correta.

Exemplo:

```text
jan/2026
fev/2026
mar/2026
abr/2026
mai/2026
jun/2026
jul/2026
ago/2026
```

---

# 📐 Camada Semântica e Medidas DAX

A camada semântica do modelo é composta por medidas reutilizáveis utilizadas nos diferentes visuais.

---

## Consumo Líquido

```DAX
Consumo Liquido =
SUM(Fato_Antimicrobianos[QUANTIDADE_AJUSTADA])
```

---

## Consumo Mensal

A medida garante que períodos sem movimentação sejam apresentados como `0`.

```DAX
Consumo Mensal =
COALESCE(
    [Consumo Liquido],
    0
)
```

---

## Média Mensal

```DAX
Media Mensal =
AVERAGEX(
    VALUES(Dim_Calendario[MesAno]),
    [Consumo Mensal]
)
```

---

## Maior Consumo Mensal

```DAX
Maior Consumo Mensal =
MAXX(
    VALUES(Dim_Calendario[MesAno]),
    [Consumo Mensal]
)
```

---

## Mês de Maior Consumo

```DAX
Mes Maior Consumo =
VAR TabelaMeses =
    ADDCOLUMNS(
        VALUES(Dim_Calendario[MesAno]),
        "ConsumoMes", [Consumo Mensal]
    )

VAR MaiorValor =
    MAXX(
        TabelaMeses,
        [ConsumoMes]
    )

RETURN
    MAXX(
        FILTER(
            TabelaMeses,
            [ConsumoMes] = MaiorValor
        ),
        Dim_Calendario[MesAno]
    )
```

---

## Antimicrobiano Selecionado

```DAX
Antimicrobiano Selecionado =
SELECTEDVALUE(
    Fato_Antimicrobianos[ANTIMICROBIANO],
    "Todos os antimicrobianos"
)
```

---

## Antimicrobiano para Título

Foi criada uma medida auxiliar para remover códigos internos da descrição apresentada no gráfico.

```DAX
Antimicrobiano Titulo =
VAR Nome =
    [Antimicrobiano Selecionado]

VAR PosicaoCodigo =
    SEARCH(
        " - (",
        Nome,
        1,
        0
    )

RETURN
    IF(
        PosicaoCodigo > 0,
        LEFT(Nome, PosicaoCodigo - 1),
        Nome
    )
```

---

## Título Dinâmico

```DAX
Titulo Grafico =
"Evolução mensal do consumo – " &
[Antimicrobiano Titulo]
```

Exemplo:

```text
Evolução mensal do consumo – Ceftriaxona Sódica 1 g
```

O título é alterado automaticamente de acordo com o filtro utilizado pelo usuário.

---

# 📊 Estrutura do Dashboard

## Página 01 — Capa

Página de apresentação do projeto.

Na versão publicada no GitHub, referências visuais e institucionais relacionadas ao contexto profissional original foram removidas.

---

## Página 02 — Evolução Mensal

Página principal do dashboard.

Permite selecionar um antimicrobiano e acompanhar seu comportamento ao longo dos meses.

### KPIs

São apresentados quatro indicadores:

- **Consumo acumulado**
- **Média mensal**
- **Maior consumo mensal**
- **Mês de maior consumo**

Todos os indicadores respondem dinamicamente aos filtros aplicados.

---

## Filtro de Antimicrobiano

Uma segmentação lateral permite selecionar individualmente o medicamento analisado.

Ao selecionar um antimicrobiano:

- os KPIs são recalculados;
- o gráfico mensal é atualizado;
- o título do gráfico é alterado automaticamente.

---

## Evolução Mensal

O gráfico utiliza:

```text
Dim_Calendario[MesAno]
```

como eixo temporal e:

```text
[Consumo Mensal]
```

como medida.

Meses sem consumo são apresentados como `0`, garantindo continuidade da série temporal.

---

# 📊 Página 03 — Ranking de Consumo

A terceira página apresenta os antimicrobianos de maior consumo.

Foi utilizado um gráfico de barras horizontais para facilitar a leitura das descrições.

---

## Top 10

O visual utiliza filtro:

```text
Top N = 10
```

baseado em:

```text
[Consumo Liquido]
```

Os resultados são apresentados em ordem decrescente.

---

# 📆 Filtro Mensal

A página de ranking contém uma segmentação utilizando botões para os meses do ano.

```text
Jan | Fev | Mar | Abr | Mai | Jun | Jul | Ago | Set | Out | Nov | Dez
```

O layout foi planejado para suportar os 12 meses.

Ao selecionar um período, o ranking é recalculado automaticamente.

---

# 🔄 Processo de Atualização

A solução foi construída para permitir a incorporação de novos dados sem necessidade de reconstrução do dashboard.

Exemplo de novo arquivo:

```text
2026_09_SETEMBRO.csv
```

Fluxo:

```text
Novo CSV
   ↓
Descoberta automática do arquivo
   ↓
Ingestão
   ↓
Staging
   ↓
Transformações
   ↓
Regras de negócio
   ↓
Atualização da tabela fato
   ↓
Atualização da dimensão calendário
   ↓
Recálculo das medidas DAX
   ↓
Atualização dos dashboards
```

Para atualizar o projeto:

1. inserir o novo CSV na pasta;
2. manter o mesmo schema;
3. manter o padrão de nomenclatura;
4. abrir o Power BI;
5. selecionar **Atualizar**;
6. validar os resultados.

O Power Query reaplica automaticamente todas as transformações configuradas.

---

# 🔁 Características de Reutilização

A arquitetura foi desenvolvida para reduzir intervenções manuais.

A inclusão de um novo período não exige:

- criação de uma nova consulta;
- alteração dos gráficos;
- alteração das medidas;
- criação de uma nova tabela;
- reconstrução do modelo.

Desde que o novo arquivo respeite o schema esperado, ele é processado automaticamente pelo pipeline.

---

# 📁 Estrutura do Repositório

```text
analise-de-antimicrobianos-power-bi/
│
├── README.md
│
├── dashboard/
│   └── Analise_Antimicrobianos_Demo.pbix
│
├── dados_exemplo/
│   ├── 2026_01_JANEIRO.csv
│   ├── 2026_02_FEVEREIRO.csv
│   ├── 2026_03_MARCO.csv
│   ├── 2026_04_ABRIL.csv
│   ├── 2026_05_MAIO.csv
│   ├── 2026_06_JUNHO.csv
│   ├── 2026_07_JULHO.csv
│   └── 2026_08_AGOSTO.csv
│
└── images/
    ├── capa.png
    ├── evolucao-mensal.png
    └── ranking.png
```

---

# 🖼️ Dashboard

## Página 01 — Capa

![Capa do Dashboard](images/capa.png)

---

## Página 02 — Evolução Mensal

![Evolução Mensal](images/evolucao-mensal.png)

---

## Página 03 — Ranking de Consumo

![Ranking de Consumo](images/ranking.png)

---

# 🛠️ Tecnologias Utilizadas

- Microsoft Power BI Desktop
- Power Query
- DAX
- CSV
- ETL
- Data Transformation
- Data Modeling
- Modelagem dimensional
- Data Quality
- Data Visualization
- Business Intelligence
- Analytics Engineering

---

# 🧠 Competências Aplicadas

## Engenharia e Preparação de Dados

- ingestão automatizada de múltiplos arquivos;
- construção de pipeline baseado em diretório;
- consolidação de datasets mensais;
- padronização de schema;
- transformação e limpeza de dados;
- tratamento de tipos;
- validação de qualidade;
- tratamento de valores nulos;
- aplicação de regras de negócio;
- tratamento de estornos;
- criação de atributos derivados;
- rastreabilidade da origem;
- separação entre staging e camada analítica;
- redução de carregamento redundante;
- atualização automática por novos arquivos.

---

## Modelagem de Dados

- criação de tabela fato;
- criação de dimensão calendário;
- relacionamento `1:N`;
- modelagem dimensional;
- separação de camadas;
- criação de camada semântica;
- criação de medidas reutilizáveis.

---

## Analytics e Business Intelligence

- desenvolvimento de medidas DAX;
- KPIs dinâmicos;
- análise temporal;
- ranking Top N;
- filtros interativos;
- títulos dinâmicos;
- construção de dashboards;
- desenvolvimento orientado ao usuário.

---

# 📌 Principais Aprendizados

O principal aprendizado deste projeto foi compreender que a construção de um dashboard confiável depende da qualidade do pipeline e do modelo que existem antes da camada visual.

O desenvolvimento começou pela análise da estrutura dos arquivos, entendimento das movimentações e definição das regras de negócio.

A partir disso, foi criado um pipeline capaz de receber novos arquivos mensais, reaplicar automaticamente as mesmas transformações e disponibilizar uma tabela fato consistente para análise.

A separação entre uma camada de staging e a tabela analítica permitiu reduzir redundância e melhorar a organização do projeto.

Outro aprendizado importante foi a necessidade de validar os dados antes da construção dos gráficos.

Durante o processo foram analisados:

- valores nulos;
- erros de tipagem;
- operações inconsistentes;
- registros administrativos;
- estornos;
- diferenças entre movimentação e consumo efetivo.

A camada visual foi construída somente após a validação da transformação e da modelagem.

Dessa forma, o projeto combina conceitos de:

```text
Data Engineering
        +
Analytics Engineering
        +
Business Intelligence
```

---

# 🔐 Privacidade e Minimização de Dados

O projeto foi originalmente inspirado por uma necessidade existente em ambiente profissional.

Entretanto, **nenhum dado real utilizado no contexto original é disponibilizado neste repositório**.

A versão publicada foi reconstruída utilizando dados inteiramente sintéticos.

Não são disponibilizados:

- nomes reais de pacientes;
- CNS reais;
- identificadores assistenciais;
- profissionais reais;
- informações institucionais;
- dados reais de consumo;
- arquivos originais;
- informações que permitam identificar a instituição relacionada à demanda inicial.

Os dados disponíveis neste repositório foram criados exclusivamente para fins educacionais e de demonstração técnica.

---

# 👩‍💻 Autora

**Mariane Pintucci**

Projeto desenvolvido com foco em demonstrar competências de **Engenharia de Dados, Analytics Engineering e Business Intelligence**, utilizando Power Query, modelagem dimensional, DAX e Power BI.

---

# 📄 Direitos Autorais

© 2026 Mariane Pintucci. Todos os direitos reservados.

Este projeto é disponibilizado exclusivamente para fins de **portfólio e demonstração técnica**.

Não é autorizada a reprodução, redistribuição, comercialização ou utilização integral deste projeto sem autorização prévia da autora.

Os dados disponibilizados neste repositório são inteiramente sintéticos e foram criados exclusivamente para fins demonstrativos.

Este repositório **não possui licença open source**.

A ausência de uma licença não concede autorização automática para utilização, modificação ou redistribuição do conteúdo.

---

# ⚠️ Aviso

Este repositório possui finalidade exclusivamente educacional, demonstrativa e de portfólio.

Os resultados apresentados no dashboard são derivados de uma base sintética e **não devem ser interpretados como indicadores reais de consumo, assistência, prescrição ou utilização de antimicrobianos em qualquer instituição de saúde**.
