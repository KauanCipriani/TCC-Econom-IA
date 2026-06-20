# 📈 B3 ML Advisor — Sistema de Apoio à Decisão

**TCC — Ciência da Computação | UNOESC São Miguel do Oeste — 2026**

Sistema computacional baseado em técnicas de Machine Learning para predição de tendências de ações do setor financeiro da B3.

**Alunos:** Kauan Amélio Cipriani • Vitor Hugo Konzen
**Orientador:** Vinicius Almeida Santos

---
s
## 🚀 Como rodar (Passo a Passo)

### 1. Pré-requisitos

- **Python 3.10 ou superior** instalado ([baixar aqui](https://www.python.org/downloads/))
- **VS Code** instalado
- **Git** (opcional, mas recomendado)

### 2. Abra o VSCode na pasta do projeto

```
File → Open Folder → selecione esta pasta
```

### 3. Abra o terminal integrado do VSCode

```
Atalho: Ctrl + ` (acento grave)
ou: Terminal → New Terminal
```

### 4. Crie um ambiente virtual Python (recomendado)

**Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

**Mac/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

Você verá `(venv)` no início do prompt do terminal indicando que o ambiente está ativo.

### 5. Instale as dependências

```bash
pip install -r requirements.txt
```

A instalação demora 2-5 minutos (vai baixar Streamlit, Plotly, scikit-learn, etc.).

### 6. Rode o sistema

```bash
streamlit run app.py
```

Vai abrir automaticamente o navegador padrão em `http://localhost:8501` com o sistema funcionando.

Pra parar: `Ctrl + C` no terminal.

---

## 📁 Estrutura dos arquivos

```
projeto_tcc/
├── app.py                          ← Interface Streamlit (2.446 linhas, 8 telas)
├── treinar_modelos.py              ← Pipeline de treinamento do Random Forest
├── requirements.txt                ← Dependências Python
├── README.md                       ← Este arquivo
│
├── .streamlit/
│   └── config.toml                 ← Configuração do tema dark mode
│
├── dados/
│   ├── b3_financeiro_ohlcv.parquet      ← Dados brutos (saída Fase 1)
│   └── b3_financeiro_features.parquet   ← Dataset com indicadores (saída Fase 2)
│
└── modelos/
    ├── random_forest.pkl           ← Modelo treinado (2,2 MB)
    └── metricas.json               ← Métricas reais lidas pela interface
```

---

## 🎯 O que o sistema faz

### Telas disponíveis

**INÍCIO**
- 🏠 **Visão Geral** — Boas-vindas, índices macro, notícias, sinais consolidados

**ANÁLISE**
- 👁️ **Painel Iniciante** — Linguagem simples, pergunta direta, 3 cards básicos
- 📊 **Painel Avançado** — RSI/MACD/Bollinger/SMA, gráfico técnico completo

**FERRAMENTAS**
- 🤖 **EconomIA** — Assistente conversacional (responde 8+ tipos de perguntas)
- 📈 **Previsões & Macro** — Polymarket Copom + Selic/IPCA/Fed/USD + Treasury Yields
- 🔔 **Alertas** — Histórico de sinais

**SISTEMA**
- 📈 **Performance** — Métricas reais lidas de `modelos/metricas.json`
- 🗄️ **Dados & Modelos** — Transparência arquitetural do projeto
- ℹ️ **Sobre o Projeto** — Informações institucionais

---

## 🔄 Retreinar o modelo (opcional)

Se quiser executar novamente o treinamento (gera nova `metricas.json`):

```bash
python treinar_modelos.py
```

O script vai:
1. Carregar `dados/b3_financeiro_ohlcv.parquet`
2. Calcular indicadores técnicos
3. Criar target de classificação (limiar ±1,5%, horizonte 5 pregões)
4. Separar SANB11 como validação out-of-sample
5. Dividir os outros 70/30 (treino/teste) por ordem temporal
6. Treinar Random Forest com TimeSeriesSplit (5 folds)
7. Avaliar no teste e no SANB11
8. Salvar:
   - `modelos/random_forest.pkl` (modelo serializado)
   - `modelos/metricas.json` (métricas reais — a interface lê dinamicamente)

A interface **atualiza automaticamente** todos os painéis de performance quando o `metricas.json` muda.

---

## ⚠️ Problemas comuns

### "streamlit não é reconhecido como comando"
→ O ambiente virtual não foi ativado. Verifique que aparece `(venv)` no terminal.

### "ModuleNotFoundError: No module named 'streamlit'"
→ Instale as dependências: `pip install -r requirements.txt`

### A página não abre em `http://localhost:8501`
→ Verifique se algum firewall não está bloqueando. Tente `http://127.0.0.1:8501`

### "Permission denied" no Windows ao ativar venv
→ Execute o PowerShell como Administrador e rode:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Tela com tema claro (não escuro)
→ Verifique que existe o arquivo `.streamlit/config.toml` na pasta do projeto.

---

## 📊 Resultados obtidos no treinamento

**Métricas reais (Random Forest)**

| Métrica | Conjunto de teste | Out-of-sample (SANB11) |
|---|---|---|
| F1-Score (macro) | 0,3361 | 0,3584 |
| Precision (macro) | 0,3395 | 0,3587 |
| Recall (macro) | 0,3365 | 0,3589 |
| Acurácia | 0,3386 | 0,3648 |

**Top 3 features mais importantes:**
1. `macd_hist_z` (z-score do histograma MACD): **16,61%**
2. `sma_9_ratio` (razão preço/SMA9): **12,76%**
3. `rsi_14` (RSI 14 períodos): **12,56%**

---

## 📚 Tecnologias utilizadas

- **Python 3.10+** — linguagem principal
- **Streamlit** — framework web para a interface
- **Plotly** — visualizações interativas (gráficos)
- **scikit-learn** — Random Forest e métricas
- **pandas + NumPy** — manipulação de dados
- **joblib** — serialização do modelo
- **PyArrow** — leitura/escrita de Parquet
- **yfinance** — coleta de dados do Yahoo Finance

---

## ⚖️ Aviso legal

Este sistema é uma **ferramenta de apoio à decisão de natureza acadêmica**, baseada em análise técnica histórica e Machine Learning. **Não constitui recomendação de investimento.** Toda decisão de investir envolve riscos. Consulte um profissional credenciado pela CVM antes de tomar decisões financeiras.
