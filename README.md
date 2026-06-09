# 🌞 Previsão de Flares Solares com Machine Learning
### Global Solution — IA e Machine Learning | FIAP 2025

> **Pergunta central:** Qual é o perfil das regiões solares de alto risco — e como calibrar um sistema de alerta que minimize o custo real de proteger satélites e infraestrutura?

---

## 👥 Integrantes

| Nome | RM |
|---|---|
| Kaio Vinicius Meireles Alves | RM553282 |
| Lucas Alves de Souza | RM553956 |
| Guilherme Fernandes de Freitas | RM554323 |
| João Pedro Chizzolini de Freitas | RM553172 |

---

## 📁 Estrutura do Repositório

```
solar-flare-gs/
├── solar_flare_FINAL.ipynb       # Notebook principal com toda a análise
├── solar_flare.csv               # Dataset (UCI Solar Flare, 1.066 observações)
├── relatorio_solar_flare.pdf     # Relatório completo em PDF
├── apresentacao_solar.pptx       # Slides da apresentação
└── README.md
```

---

## 📊 Sobre o Dataset

**UCI Solar Flare Dataset** — coletado pelo NOAA e disponibilizado no UCI Machine Learning Repository.

- **Fonte original:** https://archive.ics.uci.edu/ml/datasets/solar+flare (ID 89)
- **1.066 observações** de regiões ativas do Sol
- **10 features** físicas observadas (morfologia, área, evolução, etc.)
- **3 targets** originais: número de flares C, M e X nas próximas 24h

O arquivo `solar_flare.csv` já está pronto para uso. Para baixar o dataset original diretamente da UCI:

```python
from ucimlrepo import fetch_ucirepo
dataset = fetch_ucirepo(id=89)
```

---

## 🚀 Como Rodar

### 1. Clone o repositório

```bash
git clone https://github.com/seu-usuario/solar-flare-gs.git
cd solar-flare-gs
```

### 2. Instale as dependências

```bash
pip install pandas numpy matplotlib seaborn scikit-learn imbalanced-learn scipy
```

### 3. Abra o notebook

```bash
jupyter notebook solar_flare_FINAL.ipynb
```

Ou abra diretamente no **Google Colab** — basta fazer upload do `.ipynb` e do `solar_flare.csv`.

---

## 🔍 O que foi feito

### Análise Exploratória
- Mapas de calor de risco: Zurich × Área e Zurich × Evolução
- Verificação estatística das hipóteses (Qui-Quadrado e Mann-Whitney)
- Distribuição de classes e score de risco por intensidade

### Pré-processamento
- Label Encoding nas variáveis categóricas
- Split estratificado 80/20
- **SMOTE** aplicado apenas no treino (sem data leakage)
- StandardScaler com fit apenas no treino

### Modelos Comparados
| Modelo | F1-Score | ROC-AUC |
|---|---|---|
| Regressão Logística | 0.6550 | 0.6711 |
| Random Forest | 0.6496 | 0.6500 |
| SVM | 0.6425 | 0.6575 |
| MLP | 0.6174 | 0.6462 |
| **Random Forest (Otimizado)** | **0.6468** | **0.6579** |

### Resultado Principal
A **classe Zurich** é o preditor mais importante (importância ~36%), seguida pela área da região solar — alinhado com a física solar conhecida.

O threshold ótimo para aplicações críticas é **0.28** (não 0.5), calibrado para minimizar o custo assimétrico onde Falsos Negativos custam muito mais que Falsos Positivos.

---

## 📋 Perfil de Risco

| Zona | Condições | Probabilidade |
|---|---|---|
| 🔴 Crítica | Zurich E/F + Área ≥ 3 | 82% |
| 🟠 Alerta | Zurich C/D + Área ≥ 3 | 64% |
| 🟡 Moderada | Zurich B/C + Área ≤ 2 | 41% |
| 🟢 Segura | Zurich A/B + Área ≤ 2 | 14% |

---

## 📚 Referências

- Bradshaw, G. (1989). UCI Solar Flare Dataset. UCI ML Repository.
- Florios, K. et al. (2018). Forecasting Solar Flares Using a Random Forest Classifier. *Solar Physics*, 293(2).
- Chawla, N. V. et al. (2002). SMOTE: Synthetic Minority Over-sampling Technique. *JAIR*, 16.
- NOAA Space Weather Prediction Center: https://www.swpc.noaa.gov
