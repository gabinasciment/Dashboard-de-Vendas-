
# Dashboard Comercial

Este projeto consiste em uma solução de Business Intelligence (BI) desenvolvida para analisar e monitorar as operações de vendas de uma organização ao longo de múltiplos anos. Através da consolidação de dados históricos e cadastrais, o sistema oferece uma visão holística do desempenho comercial, permitindo a tomada de decisões estratégicas baseada em dados reais.

O projeto resolve o problema da fragmentação de dados gerenciais, unificando bases de transações anuais e informações de contexto (clientes, lojas e produtos) em um único ambiente analítico. Isso elimina a necessidade de cruzamentos manuais complexos em planilhas e reduz drasticamente o tempo de resposta para perguntas críticas do negócio.

A solução é construída utilizando o ecossistema do Microsoft Power BI, consumindo dados diretamente de planilhas locais para compor um modelo de dados relacional (provavelmente estruturado em *Star Schema* ou *Snowflake*), renderizando análises através de relatórios interativos.

## Visão Geral

O propósito do sistema é fornecer dashboards interativos que detalham o comportamento e o volume de vendas da empresa. A estrutura de diretórios e o código-fonte extraído (arquivos JSON e XML como `[Content_Types].xml`, `report.json`, e `page.json`) revelam os componentes internos de um arquivo de projeto do Power BI. O modelo semântico (`DataModel`) se conecta a múltiplas bases de Excel que servem como tabelas fato e dimensão, processando a segurança (`SecurityBindings`) e o design de layout (`DiagramLayout`) para exibição final utilizando os temas nativos da ferramenta (como o *Fluent2*).

## Funcionalidades

* **Análise Histórica de Vendas:** Consolidação e modelagem de transações comerciais englobando múltiplos períodos através da Base Vendas - 2022.xlsx, Base Vendas - 2023.xlsx e Base Vendas - 2024.xlsx.
* **Gestão de Perfil de Consumidores:** Integração de dados sociodemográficos e comportamentais consumindo a planilha Cadastro Clientes.xlsx.
* **Controle Geográfico/Filiais:** Mapeamento de métricas de performance individualizadas por unidade de negócio através do Cadastro Lojas.xlsx.
* **Análise de Portfólio:** Acompanhamento do desempenho por item comercializado a partir do Cadastro Produto.xlsx.
* **Navegação Interativa e Visualizações Dinâmicas:** Múltiplas páginas de relatório com gráficos, matrizes e KPIs renderizados dinamicamente pelo Power BI.

## Arquitetura

A arquitetura encontrada segue o padrão tradicional de fluxo de dados de Self-Service BI:

1. **Camada de Extração (Origem):** Composta por arquivos planos estruturados que fornecem os dados brutos de fatos (vendas) e dimensões (cadastros).
2. **Camada Semântica (Modelo de Dados):** Onde os relacionamentos, limpeza de dados (Power Query) e cálculos de negócio (DAX) são mantidos em memória (representado pelo arquivo interno `DataModel`).
3. **Camada de Apresentação:** Estruturada via arquivos JSON (`pages.json`, `visual.json`) que definem o estado, a formatação e a interatividade da interface do usuário final.

### Estrutura do Projeto

```text
/
├── Dados/
│   ├── Base Vendas - 2022.xlsx
│   ├── Base Vendas - 2023.xlsx
│   ├── Base Vendas - 2024.xlsx
│   ├── Cadastro Clientes.xlsx
│   ├── Cadastro Lojas.xlsx
│   └── Cadastro Produto.xlsx
├── [Content_Types].xml
├── DiagramLayout
├── Metadata
├── Settings
├── SecurityBindings
├── DataModel
└── Report/
    ├── definition/
    │   ├── report.json
    │   ├── version.json
    │   └── pages/
    │       ├── pages.json
    │       └── 400b8ad7badd93b4044c/
    │           ├── page.json
    │           └── visuals/
    │               ├── 366d70a9a43030c2c50b/visual.json
    │               ├── 3c8c142e62dc741bdac3/visual.json
    │               └── (...)
    └── StaticResources/
        └── SharedResources/
            ├── BaseThemes/Fluent2-CY26SU08.json
            └── BuiltInThemes/Frontier.json

```

## Como Instalar

1. Certifique-se de ter o **Microsoft Power BI Desktop** instalado em seu sistema operacional (disponível na Microsoft Store corporativa ou site oficial).
2. Clone o repositório ou baixe os arquivos da solução para o seu ambiente local.
3. Certifique-se de manter os arquivos Base Vendas - 2022.xlsx, Base Vendas - 2023.xlsx, Base Vendas - 2024.xlsx, Cadastro Clientes.xlsx, Cadastro Lojas.xlsx e Cadastro Produto.xlsx no diretório original esperado pelo projeto para evitar falhas de ingestão.

## Como Executar

1. Dê um duplo clique no arquivo principal do projeto (normalmente com extensão `.pbix` ou `.pbip`) para abri-lo no Power BI Desktop.
2. Aguarde a carga do mecanismo local do `DataModel` e a renderização do `DiagramLayout`.
3. Navegue através das abas de página na barra inferior para explorar as diferentes perspectivas de análise desenvolvidas.

## Como Configurar

Caso as visualizações apresentem erros de quebra de caminho (path) nos arquivos Excel após o download:

1. Abra o arquivo do Power BI e acesse a guia **Página Inicial**.
2. Clique em **Transformar Dados** (Power Query Editor) ou **Configurações da Fonte de Dados**.
3. Modifique os caminhos absolutos das fontes de dados (Base Vendas - 2022.xlsx, etc.) para refletir a nova localização local no seu computador.
4. Clique em **Aplicar e Fechar** e, em seguida, no botão **Atualizar** para carregar os dados.

## Como Contribuir

1. Faça um *Fork* do repositório.
2. Crie uma *Branch* para a sua alteração (`git checkout -b feature/novos-indicadores`).
3. Ao utilizar o formato Power BI Project (`.pbip`), as alterações refletirão diretamente nos metadados JSON internos (como `report.json` e `visual.json`).
4. Realize o *Commit* das suas alterações (`git commit -m 'feat: adicionado novo visual de YTD'`).
5. Faça o *Push* para a *Branch* (`git push origin feature/novos-indicadores`).
6. Abra um *Pull Request* para revisão das lógicas de negócio ou design aplicados.

## Como Realizar Deploy

1. Com a solução finalizada e salva no Power BI Desktop, acesse a guia **Página Inicial**.
2. Clique no botão **Publicar**.
3. Autentique-se com sua conta corporativa da Microsoft.
4. Selecione o *Workspace* apropriado no **Power BI Service** (nuvem).
5. (Opcional, porém recomendado): Instale e configure o *On-premises data gateway* no servidor ou máquina onde as planilhas estão hospedadas, configurando rotinas de atualização agendada (*Scheduled Refresh*) no portal do Power BI para que os dados do relatório se mantenham atualizados de forma automatizada.

## Tecnologias Utilizadas

* **Microsoft Power BI:** Motor de Business Intelligence, modelagem e visualização de dados.
* **Microsoft Excel (.xlsx):** Sistema de armazenamento estruturado servindo como provedor de dados de Fatos e Dimensões.
* **Linguagem M (Power Query):** *(Inferida)* Utilizada para limpeza, tratamento e ETL dos arquivos Excel.
* **DAX (Data Analysis Expressions):** *(Inferida)* Linguagem utilizada para criação de cálculos dinâmicos, inteligência de tempo e métricas de negócio.
