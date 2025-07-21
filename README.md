# LoRa Coverage Map <!-- omit in toc -->

Projeto de **Iniciação Científica** focado em **análise de dados** e **predição do RSSI (Received Signal Strength Indicator)** em redes **LoRaWAN**. A meta é criar **mapas de cobertura** que ajudem pesquisadores, provedores de IoT e a comunidade acadêmica a compreender os fatores que afetam o alcance e a qualidade do sinal LoRa.

---

## Índice

1. [Motivação](#motivação)
2. [Objetivos](#objetivos)
3. [Metodologia](#metodologia)
4. [Principais Tecnologias](#principais-tecnologias)
5. [Requisitos](#requisitos)
6. [Instalação](#instalação)
7. [Estrutura do Repositório](#estrutura-do-repositório)
8. [Como Executar](#como-executar)
9. [Avaliação e Métricas](#avaliação-e-métricas)
10. [Contribuindo](#contribuindo)
11. [Roadmap](#roadmap)
12. [Licença](#licença)
13. [Autores e Agradecimentos](#autores-e-agradecimentos)

---

## Motivação

As aplicações de Internet das Coisas (IoT) dependem de **rede confiável** e **longo alcance**. Entretanto, o RSSI de LoRaWAN varia conforme **obstáculos físicos, condições atmosféricas** e **configurações de rede**. Mapear essa variação:

* Auxilia na **implantação estratégica de gateways**.
* Ajuda a **otimizar parâmetros** (potência, Spreading Factor, etc.).
* Oferece **insights acadêmicos** sobre propagação de sinal em cenários urbanos e rurais.

## Objetivos

* **Coletar** leituras de RSSI em diferentes ambientes.
* **Explorar & limpar** os dados para remover ruídos e outliers.
* **Criar modelos preditivos** (ML tradicionais e/ou redes neurais) para estimar RSSI em pontos não amostrados.
* **Gerar mapas interativos** de cobertura, com camadas que mostrem:

  * Valor predito do RSSI.
  * Incerteza/erro do modelo.
  * Gateways e trajetos de coleta.
* **Disponibilizar um pipeline reprodutível** para que outros pesquisadores repliquem ou estendam o estudo.

## Metodologia

| Etapa                | Descrição                                                                                                                                    |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Coleta            | Uso de nodes LoRa com GPS, varrendo a área de estudo; logs salvos em `.csv` ou banco SQLite.                                                 |
| 2. Pré-Processamento | Conversão de unidades, filtragem por qualidade do GPS, detecção de outliers via Z-score.                                                     |
| 3. Exploração        | Estatísticas descritivas, correlações e visualização geoespacial (heatmaps, histogramas).                                                    |
| 4. Modelagem         | Baseline (`Inverse Distance Weighting`) → modelos supervisionados (RandomForest, GradientBoosting) → Deep Learning (MLP, GNN) se necessário. |
| 5. Validação         | `k-fold` espacial, métricas **MAE**, **RMSE**, **R²**.                                                                                       |
| 6. Mapeamento        | Geração de camadas raster via `geopandas`/`rasterio`, renderização em `folium/leafmap`.                                                      |
| 7. Publicação        | Exportação do mapa como **GeoJSON** ou integração com **Kepler.gl** e publicação no GitHub Pages.                                            |

## Principais Tecnologias

* **Python 3.11+**
* **Pandas / NumPy / SciPy** – manipulação numérica
* **Scikit-learn / PyTorch / TensorFlow** – modelagem
* **GeoPandas / Shapely / Rasterio / leafmap** – geoprocessamento
* **Folium / Kepler.gl** – mapas interativos
* **JupyterLab** – notebooks de exploração
* **Poetry** – gerenciamento de dependências
* **Docker** – ambiente reproduzível (opcional)

## Requisitos

```bash
# Linux / macOS / Windows (WSL recomendado)
Python >= 3.11
make  # opcional para comandos simplificados
git
```

## Instalação

```bash
# 1. Clone o repositório
git clone https://github.com/<usuario>/lora-coverage-map.git
cd lora-coverage-map

# 2. Crie e ative um ambiente virtual
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate

# 3. Instale as dependências
pip install -r requirements.txt
# ou, se usar Poetry
poetry install
```

## Estrutura do Repositório

```
lora-coverage-map/
├── data/
│   ├── raw/          # CSVs brutos de coleta
│   └── processed/    # Dados limpos e features
├── notebooks/        # Exploratório & relatórios parciais
├── src/
│   ├── collect/      # Scripts de importação
│   ├── features/     # Engenharia de atributos
│   ├── models/       # Treino, validação, inferência
│   └── viz/          # Geração de mapas e plots
├── tests/            # PyTest
├── Makefile          # Atalhos (make train, make map, etc.)
└── README.md         # (este arquivo)
```

## Como Executar

1. **Preparar dados**

   ```bash
   make preprocess           # ou python src/collect/preprocess.py
   ```
2. **Treinar modelo**

   ```bash
   make train                # salva modelo em models/
   ```
3. **Gerar mapa**

   ```bash
   make map                  # cria output/map.html
   ```
4. **Abrir resultado**

   ```
   open output/map.html
   ```

## Avaliação e Métricas

O script `src/models/evaluate.py` gera:

* **Tabela de métricas** (MAE, RMSE, R²) por fold.
* **Curvas de erro espacial**.
* **Mapa de residuals** para identificar zonas de baixa performance.

## Contribuindo

1. Abra uma *issue* descrevendo a melhoria ou bug.
2. Crie um *branch* (`git checkout -b feature/minha-contrib`).
3. Faça *commit* seguindo **Conventional Commits**.
4. Envie um *Pull Request*.

## Roadmap

* [ ] Integração com **The Things Network** para coleta em tempo real
* [ ] Suporte a **semivariogramas** e **krigagem** geoespacial
* [ ] Dashboards em **Streamlit**
* [ ] Pipeline CI/CD com **GitHub Actions**

## Licença

Distribuído sob a licença **MIT**. Veja [`LICENSE`](LICENSE) para detalhes.

## Autores e Agradecimentos

* **Seu Nome** – *Pesquisador IC, Engenharia Elétrica – UFXX*
* **Orientador(a) Prof. Dr(a). Nome** – *Laboratório de IoT e Comunicações Sem Fio*

Agradecemos ao **Programa de Bolsas de Iniciação Científica (PIBIC/CNPq)** pelo apoio financeiro e ao **The Things Network Community** pelos insights sobre LoRaWAN.

---

> *“Medir é saber; predizer é otimizar.”* — Adaptado de Lord Kelvin.
