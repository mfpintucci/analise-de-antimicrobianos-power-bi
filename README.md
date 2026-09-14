# 📊 Análise de Antimicrobianos — Power BI | Data Engineering & Analytics

Projeto desenvolvido em **Microsoft Power BI** com foco em **pipeline de dados, transformação, modelagem dimensional e análise do consumo mensal de antimicrobianos**.

O projeto foi originalmente inspirado por uma necessidade gerencial real de acompanhamento de consumo ao longo do ano e posteriormente adaptado para portfólio utilizando **dados inteiramente sintéticos**, sem qualquer informação real de pacientes, profissionais, instituições ou operações assistenciais.

Mais do que a construção de um dashboard, este projeto demonstra a implementação de um fluxo de dados reutilizável, contemplando:

- ingestão automatizada de múltiplos arquivos CSV;
- consolidação de arquivos mensais;
- transformação e limpeza com Power Query;
- aplicação de regras de negócio;
- camada intermediária de preparação dos dados;
- criação de tabela fato;
- criação de dimensão temporal;
- modelagem dimensional;
- desenvolvimento de medidas DAX;
- criação de camada semântica;
- atualização automatizada pela inclusão de novos arquivos;
- construção de dashboards orientados ao usuário.

---

# ▶️ Como visualizar o projeto

> **Importante:** arquivos `.pbix` não são visualizados diretamente na interface do GitHub.

## 🖥️ Dashboard interativo

Para explorar filtros, segmentações, KPIs e gráficos:

1. faça o download do arquivo `Panorama_Antimicrobianos.pbix`;
2. abra o arquivo utilizando o **Microsoft Power BI Desktop**.

📥 **[Baixar arquivo PBIX](./Panorama_Antimicrobianos.pbix)**

> O arquivo `.pbix` contém os dados sintéticos já importados e pode ser explorado normalmente no Power BI Desktop.  
> A atualização da fonte de dados depende da disponibilidade dos arquivos utilizados no projeto.

---

## 📄 Visualização em PDF

Para quem não possui o Power BI Desktop instalado, também está disponível uma versão em PDF com as páginas do relatório.

📄 **[Visualizar relatório em PDF](./Panorama_Antimicrobianos.pdf)**

> A versão PDF permite visualizar o resultado final do projeto, mas não possui os recursos interativos disponíveis no Power BI.

---

# 🎯 Objetivo

O objetivo principal do projeto é responder à seguinte pergunta:

> **Como o consumo de cada antimicrobiano evolui ao longo dos meses?**

Além da evolução temporal, o dashboard permite:

- selecionar individualmente um antimicrobiano;
- visualizar o consumo acumulado no período;
- acompanhar a média mensal de consumo;
- identificar o maior consumo registrado em um mês;
- identificar o mês de maior consumo;
- consultar o ranking dos antimicrobianos mais consumidos;
- analisar o ranking por período;
- atualizar a base com novos arquivos mensais mantendo a mesma estrutura.

---

# 🧪 Dados Demonstrativos

Todos os dados utilizados na versão pública deste projeto são **inteiramente sintéticos**.

Eles foram gerados exclusivamente para demonstrar a arquitetura do projeto e o funcionamento do fluxo de transformação e análise.

Os dados não correspondem a:

- pacientes reais;
- profissionais reais;
- atendimentos reais;
- instituições reais;
- movimentações reais;
- quantitativos reais;
- registros provenientes de qualquer unidade de saúde.

Os nomes dos medicamentos são utilizados apenas para contextualização da análise.

Todos os identificadores, pacientes, profissionais, quantidades e movimentações utilizados na versão demonstrativa foram criados exclusivamente para fins de portfólio.

---

# 🏗️ Arquitetura da Solução

A solução foi estruturada separando ingestão, preparação, regra de negócio, modelagem e consumo analítico.

```text
Arquivos CSV mensais
        ↓
Camada de Ingestão
 Power Query / Folder
        ↓
Camada Intermediária
 Dados consolidados
        ↓
Transformação
 limpeza + tipagem + filtros
        ↓
Regra de Negócio
 saídas + estornos
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

Essa organização permite incorporar novos arquivos mensais sem necessidade de reconstruir o relatório.

---

# 🗂️ Fonte dos Dados

Os dados são organizados originalmente em arquivos `.CSV` mensais com estrutura padronizada.

Exemplo conceitual:

```text
2026_01_JANEIRO.csv
2026_02_FEVEREIRO.csv
2026_03_MARCO.csv
2026_04_ABRIL.csv
...
```

O Power BI utiliza o conector de **Pasta**, permitindo a ingestão e combinação automática dos arquivos compatíveis existentes no diretório.

A inclusão de um novo período exige apenas um novo arquivo respeitando o schema esperado.

> Os arquivos de origem não são disponibilizados neste repositório.

---

# 🔄 Pipeline de Dados

O fluxo executa as seguintes etapas:

1. leitura dos arquivos CSV existentes na pasta;
2. identificação e combinação dos arquivos;
3. padronização da estrutura;
4. tipagem das colunas;
5. tratamento de datas;
6. limpeza de campos textuais;
7. seleção das colunas necessárias;
8. filtragem das operações relevantes;
9. tratamento dos estornos;
10. aplicação das regras de negócio;
11. criação da quantidade ajustada;
12. geração da tabela fato;
13. carregamento das estruturas necessárias ao modelo analítico.

Fluxo simplificado:

```text
CSV
 ↓
Ingestão
 ↓
Preparação
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

# 🧱 Camada Intermediária

A consulta:

```text
Dados Mensais
```

é responsável pela consolidação dos arquivos mensais antes da construção da tabela analítica.

Essa camada:

- recebe os arquivos;
- consolida os diferentes períodos;
- preserva a origem dos registros;
- serve como base para as transformações posteriores.

Seu carregamento permanece **desabilitado**, evitando redundância dentro do modelo.

---

# 📦 Tabela Fato

A partir da camada intermediária foi criada:

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
| `ARQUIVO_ORIGEM` | Arquivo responsável pelo registro |
| `IDADE_PACIENTE` | Informação complementar utilizada na base demonstrativa |

A estrutura final foi preparada de acordo com a finalidade analítica do relatório.

---

# 🧹 Transformação e Qualidade dos Dados

O processo de transformação foi realizado utilizando **Power Query**.

Entre as principais etapas estão:

- combinação de múltiplos arquivos;
- padronização do schema;
- definição de tipos;
- tratamento de datas;
- limpeza de strings;
- remoção de espaços e caracteres desnecessários;
- tratamento de valores nulos;
- validação de erros;
- filtragem das operações;
- remoção de campos não utilizados;
- criação de atributos derivados;
- rastreabilidade por arquivo de origem;
- validação da qualidade das colunas.

A coluna:

```text
ARQUIVO_ORIGEM
```

foi mantida para permitir rastrear um registro até o arquivo mensal responsável por sua ingestão.

Essa estratégia facilita auditoria e investigação de inconsistências.

---

# ⚙️ Regra de Negócio

Nem toda movimentação registrada representa consumo efetivo.

Foi necessário diferenciar operações de saída, estornos e movimentações que não deveriam participar da análise.

Foi criada a coluna:

```text
QUANTIDADE_AJUSTADA
```

com a seguinte lógica:

```powerquery
if Text.StartsWith([OPERACAO], "ESTORNO")
then -[QUANTIDADE]
else [QUANTIDADE]
```

Assim:

```text
Consumo Líquido = Saídas - Estornos
```

A coluna original `QUANTIDADE` foi preservada, garantindo rastreabilidade em relação ao dado recebido.

---

# 🗃️ Modelagem Dimensional

O modelo utiliza uma estrutura dimensional simples:

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

A dimensão calendário controla as análises temporais utilizadas no dashboard.

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

A coluna `MesAno` é ordenada utilizando `AnoMes`, garantindo a sequência cronológica correta.

---

# 📐 Camada Semântica e Medidas DAX

## Consumo Líquido

```DAX
Consumo Liquido =
SUM(Fato_Antimicrobianos[QUANTIDADE_AJUSTADA])
```

---

## Consumo Mensal

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

O título é atualizado automaticamente de acordo com o medicamento selecionado.

---

# 📊 Estrutura do Dashboard

## Página 01 — Capa

Apresentação do projeto.

Na versão de portfólio, referências institucionais relacionadas ao contexto profissional original foram removidas.

---

## Página 02 — Evolução Mensal

Página principal do dashboard.

Apresenta os seguintes indicadores:

- **Consumo acumulado**
- **Média mensal**
- **Maior consumo mensal**
- **Mês de maior consumo**

Uma segmentação permite selecionar individualmente o antimicrobiano analisado.

A seleção atualiza automaticamente:

- os KPIs;
- o gráfico mensal;
- o título dinâmico.

---

## Página 03 — Ranking de Consumo

Apresenta os antimicrobianos com maior consumo no período selecionado.

O visual utiliza:

```text
Top N = 10
```

baseado na medida:

```text
[Consumo Liquido]
```

Os resultados são apresentados em ordem decrescente.

A página possui filtro mensal, permitindo recalcular o ranking conforme o período selecionado.

---

# 🔄 Processo de Atualização

A solução foi construída para permitir a incorporação de novos períodos sem reconstrução do dashboard.

Exemplo:

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
Transformações do Power Query
   ↓
Regras de negócio
   ↓
Atualização da tabela fato
   ↓
Atualização da dimensão calendário
   ↓
Recálculo das medidas
   ↓
Atualização do dashboard
```

Desde que o novo arquivo respeite o schema esperado, as transformações configuradas são reaplicadas durante a atualização.

---

# 🔁 Reutilização da Solução

A arquitetura foi desenvolvida para reduzir intervenções manuais.

A inclusão de um novo período não exige:

- criação de nova consulta;
- criação de nova tabela;
- reconstrução dos gráficos;
- recriação das medidas;
- reconstrução do modelo.

Essa abordagem também permitiu reutilizar a estrutura como base para novas análises de consumo.

---

# 📁 Estrutura do Repositório

```text
analise-de-antimicrobianos-power-bi/
│
├── README.md
├── Panorama_Antimicrobianos.pbix
└── Panorama_Antimicrobianos.pdf
```

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
- pipeline baseado em diretório;
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
- separação entre camada intermediária e modelo analítico;
- redução de carregamento redundante;
- atualização automatizada por novos arquivos.

---

## Modelagem de Dados

- criação de tabela fato;
- criação de dimensão calendário;
- relacionamento `1:N`;
- modelagem dimensional;
- construção de camada semântica;
- criação de medidas reutilizáveis.

---

## Analytics e Business Intelligence

- desenvolvimento de medidas DAX;
- KPIs dinâmicos;
- análise temporal;
- ranking Top N;
- filtros interativos;
- títulos dinâmicos;
- desenvolvimento de dashboards orientados ao usuário.

---

# 📌 Principais Aprendizados

O principal aprendizado deste projeto foi compreender que a construção de um dashboard confiável depende da qualidade do fluxo de dados e da modelagem existentes antes da camada visual.

O trabalho começou pela análise da estrutura dos arquivos, entendimento das movimentações e definição das regras de negócio.

A partir disso, foi criado um processo capaz de receber novos arquivos mensais, reaplicar as transformações configuradas e disponibilizar uma tabela fato consistente para análise.

A separação entre uma consulta intermediária e a tabela analítica permitiu reduzir redundância e melhorar a organização do modelo.

Outro ponto importante foi a necessidade de validar os dados antes da construção dos gráficos.

Durante o desenvolvimento foram analisados:

- valores nulos;
- erros de tipagem;
- operações inconsistentes;
- movimentações administrativas;
- estornos;
- diferenças entre movimentação e consumo efetivo.

A camada visual foi construída somente após a preparação e validação dos dados.

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

A versão pública foi reconstruída utilizando dados inteiramente sintéticos.

Não são disponibilizados:

- nomes reais de pacientes;
- CNS reais;
- CPF reais;
- identificadores assistenciais reais;
- profissionais reais;
- informações institucionais;
- quantitativos reais de consumo;
- arquivos originais;
- informações que permitam identificar a instituição relacionada à demanda inicial.

Quaisquer identificadores eventualmente presentes na versão demonstrativa são inteiramente sintéticos.

---

# 👩‍💻 Autora

**Mariane Pintucci**

Projeto desenvolvido com foco em demonstrar competências em **Engenharia de Dados, Analytics Engineering e Business Intelligence**, utilizando Power Query, modelagem dimensional, DAX e Microsoft Power BI.

---

# 📄 Direitos Autorais

© 2026 Mariane Pintucci. Todos os direitos reservados.

Este projeto é disponibilizado exclusivamente para fins de **portfólio e demonstração técnica**.

Não é autorizada a reprodução, redistribuição, comercialização ou utilização integral deste projeto sem autorização prévia da autora.

Os dados utilizados na versão pública são inteiramente sintéticos e foram criados exclusivamente para fins demonstrativos.

Este repositório **não possui licença open source**.

A ausência de uma licença não concede autorização automática para utilização, modificação ou redistribuição do conteúdo.

---

# ⚠️ Aviso

Este repositório possui finalidade exclusivamente educacional, demonstrativa e de portfólio.

Os resultados apresentados no dashboard são derivados de uma base sintética e **não devem ser interpretados como indicadores reais de consumo, assistência, prescrição ou utilização de antimicrobianos em qualquer instituição de saúde**.
