[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22096666.svg)](https://doi.org/10.5281/zenodo.22096666)

ClimaAra — Extração e validação de acurácia em séries de dados climatológicas
===========================================

O ClimaAra é um aplicativo desktop desenvolvido para extrair variáveis
climáticas do Google Earth Engine (GEE) e compará-las com dados observados de
estações meteorológicas. O programa calcula métricas de acurácia e produz
gráficos e planilhas para apoiar a validação dos dados.

Instituto de Geociências e Ciências Exatas (IGCE)
Universidade Estadual Paulista "Júlio de Mesquita Filho" (UNESP) — Rio Claro


PRINCIPAIS RECURSOS
-------------------

- Extração de variáveis climáticas para Shapefile ou GeoJSON.
- Integração com o Google Earth Engine.
- Extração mensal de dados, com agregação automática de coleções diárias.
- Suporte a uma ou várias feições no mesmo arquivo espacial.
- Identificação das feições por um campo ID do arquivo ou por IDs sequenciais
  criados automaticamente (0, 1, 2, ...).
- Importação de dados observados em planilhas Excel (.xlsx).
- Associação entre as colunas observadas e as variáveis extraídas.
- Cálculo de R², RMSE, NSE, KGE e PBIAS.
- Gráficos de regressão, ciclo sazonal, resíduos mensais e PBIAS anual.
- Exportação de dados e métricas em XLSX e gráficos em PNG.


COLEÇÕES E VARIÁVEIS
--------------------

As coleções disponíveis são configuradas no arquivo collections_config.json.
Atualmente, o programa inclui coleções como:

- BR-DWGD v2025 (diário e mensal): precipitação, evapotranspiração, umidade
  relativa, radiação solar, temperatura máxima e mínima e velocidade do vento.
- CHIRPS diário: precipitação.

Novas coleções podem ser incluídas editando collections_config.json, sem
precisar alterar o código da interface.


REQUISITOS
----------

- Windows.
- Python 3.12 ou superior, com a opção "Add python.exe to PATH" marcada na
  instalação.
- Uma conta Google com acesso ao Google Earth Engine.
- Um projeto do Google Cloud/Earth Engine para informar no aplicativo.
- Acesso à internet durante a autenticação e a extração.

As bibliotecas Python necessárias estão listadas em requirements.txt.


COMO INICIAR
------------

1. Baixe ou clone este repositório.
2. Abra a pasta do projeto.
3. Execute o arquivo iniciar_app.bat.

Na primeira execução, o arquivo instala automaticamente as dependências de
requirements.txt. Na primeira extração, o navegador poderá abrir para a
autenticação da sua conta Google no Earth Engine.

Como alternativa, pelo terminal, execute:

    python -m pip install -r requirements.txt
    python app.py


COMO USAR
---------

1. Área

   Selecione um arquivo Shapefile (.shp) ou GeoJSON (.geojson/.json).

   Para Shapefile, mantenha os arquivos complementares (.dbf, .shx e .prj) na
   mesma pasta do arquivo .shp. Arquivos em outros sistemas de coordenadas são
   reprojetados para WGS84 quando necessário.

2. Uma ou várias feições

   Informe se o arquivo possui uma única feição ou mais de uma feição.

   Para múltiplas feições, selecione um campo de identificação com valores
   únicos. Esse campo será exportado como a coluna "id_feicao". Caso o arquivo
   não tenha um campo adequado, marque a opção para criar IDs sequenciais de
   0 até N−1.

   A extração para múltiplas feições produz uma linha para cada combinação de
   feição e mês. A validação de acurácia da interface deve ser feita para uma
   feição por vez, pois ela compara uma única série observada a uma única série
   extraída.

3. Variáveis (GEE)

   Informe o ID do projeto do Google Earth Engine, o ano inicial e o ano final.
   Marque uma ou mais variáveis e clique em "Extrair variáveis selecionadas do
   Earth Engine".

   Os dados extraídos podem ser baixados mesmo sem uma planilha observada.

4. Dados observados

   Selecione uma planilha Excel (.xlsx) com uma coluna de data e uma coluna por
   variável observada. As datas podem estar em resolução diária ou mensal.

   Exemplo:

       Data       | Precipitacao | TMedia | TMax | TMin | Umidade | VelocidadeVento
       -----------+--------------+--------+------+------+---------+-----------------
       2020-01    | 210.4        | 24.3   | 30.1 | 18.2 | 78      | 1.4
       2020-02    | 180.2        | 24.8   | 30.5 | 19.0 | 75      | 1.6

   O programa reconhece nomes comuns de variáveis e sugere o pareamento com os
   dados extraídos. Confirme ou altere os pareamentos antes de validar.

5. Resultados

   Clique em "Executar validação". Para cada variável associada, o programa
   calcula métricas de acurácia e disponibiliza gráficos e opções de exportação.


AGREGAÇÃO TEMPORAL
------------------

As extrações são entregues em base mensal. Quando a coleção de origem contém
dados diários, o programa faz a agregação automaticamente:

- Soma para variáveis acumulativas, como precipitação e evapotranspiração.
- Média para variáveis de estado, como temperatura, umidade, vento e radiação.

Dados observados diários também são agregados mensalmente antes da comparação.
Utilize sempre dados observados que correspondam à mesma escala temporal e à
mesma variável da coleção escolhida.


ARQUIVOS PRINCIPAIS
-------------------

- app.py: interface gráfica do aplicativo.
- gee_service.py: autenticação, leitura da área e extração no Earth Engine.
- collections_config.json: configuração das coleções e variáveis disponíveis.
- observed_data.py: leitura e preparação dos dados observados.
- metrics.py: cálculo das métricas de acurácia.
- plotting.py: criação dos gráficos.
- export_utils.py: exportação de planilhas e imagens.
- iniciar_app.bat: inicialização do aplicativo no Windows.


CITAÇÃO
--------

TAVARES, G.; SILVA, C. R. Software para extração e validação de dados
climatológicos: ClimaAra. Versão 2.0. Rio Claro: Departamento de Geografia e
Planejamento Ambiental, IGCE, UNESP, 2026. DOI: 10.5281/zenodo.22096666.
Disponível em: https://doi.org/10.5281/zenodo.22096666.


CONTATO
-------

guilherme.tavares-silva@unesp.br
