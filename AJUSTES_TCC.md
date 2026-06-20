# Ajustes a fazer no texto do TCC I

Lista consolidada das alterações no **documento** (PDF/Word) para que o texto
fique de acordo com o sistema realmente entregue. O app **não** precisa mudar —
estes são ajustes de **escrita**.

> Decisões tomadas: manter o app como está, usar as métricas reais geradas aqui,
> deixar Notícias / Histórico de Alertas / Previsões & Macro como "em
> desenvolvimento", e simplificar o visual (sem emojis, título "Econom-IA").

---

## 1. Métricas do Random Forest — Seção 5.3 (trocar os números)

| No texto está | Trocar por (valor real atual) |
|---|---|
| CV F1 macro médio **0,3354** / desvio **0,0245** | **0,3307** / desvio **0,0299** |
| Teste — F1 macro **0,3361** | **0,3238** |
| Teste — Precision macro **0,3395** | **0,3263** |
| Teste — Recall macro **0,3365** | **0,3234** |
| Teste — Acurácia **0,3386** | **0,3254** |
| Out-of-sample (SANB11) — F1 macro **0,3584** | **0,3596** |
| Out-of-sample — Precision macro **0,3587** | **0,3601** |
| Out-of-sample — Recall macro **0,3589** | **0,3600** |
| Out-of-sample — Acurácia **0,3648** | **0,3678** |

A narrativa continua válida: o out-of-sample (0,3596) segue **ligeiramente
superior** ao teste (0,3238), mantendo o argumento de ausência de overfitting.

## 2. Importância das features — Seção 5.3 (a ordem mudou)

O 3º lugar **deixou de ser o RSI** e passou a ser a SMA de 50.

- **Antes:** macd_hist_z (16,61%), preço/SMA-9 (12,76%), **RSI-14 (12,56%)**.
- **Agora:** macd_hist_z **(15,99%)**, preço/SMA-9 **(13,35%)**, **preço/SMA-50 (12,87%)**;
  o RSI-14 aparece em seguida (12,14%).
- A soma dos três principais continua ≈ **42%**, então essa frase permanece.
- Ajustar a frase final para: combinação entre um indicador de momento (MACD) e
  indicadores de tendência (médias móveis de 9 e 50 períodos).

## 3. Sinais agora vêm do Random Forest (já consistente)

O texto (seções 1.1, 3.3 e 3.5) já diz que os sinais de compra/venda são
gerados pelos modelos. O app foi corrigido para isso — os sinais agora saem do
**Random Forest** (antes eram regras heurísticas de RSI/MACD). **Nenhuma
mudança de texto necessária**; apenas confirme que não há menção a "regras
heurísticas" como forma final de gerar sinais.

## 4. Barra lateral — Seção 5.1

- "barra lateral estruturada em **quatro seções: Início, Análise, Ferramentas e
  Sistema**" → **"três seções: Início, Análise e Ferramentas"** (a seção Sistema
  foi removida).

## 5. Seção Sistema — Seção 5.3 (página 13)

- **Remover** a frase: *"A seção Sistema reúne três telas voltadas à
  transparência arquitetural do projeto: Performance...; Dados & Modelos...; e
  Sobre o Projeto..."*.
- **Manter** a parte final do parágrafo, sobre o cache do Streamlit
  (`@st.cache_data` / `@st.cache_resource`), que continua verdadeira.
- Como a tela Performance foi removida, **rever a frase da Seção 5.2** que diz
  *"se reflita automaticamente nos painéis de performance"* (não há mais painel
  de performance na interface).

## 6. Notícias — Seção 5.1

- "uma **área dedicada à manchetes** do mercado financeiro" → **"uma área
  reservada para manchetes do mercado, prevista para o TC II"**.

## 7. Histórico de alertas — Seção 3.5

- O **banner de alerta** (no Painel Avançado) já existe e pode ser mantido na
  descrição.
- O **histórico de alertas** está como "em desenvolvimento": indicar que a tela
  de histórico está **prevista para o TC II**.

## 8. Previsões & Macro — Seção 5.5 (já coerente)

A seção já descreve o painel como "em desenvolvimento", com automação via APIs
(BCB, FRED, Polymarket) prevista para o TC II — **continua coerente**. No app a
aba está marcada como em desenvolvimento.

> Observação: os 4 cards macro da tela **Início** (IBOVESPA, SELIC, USD/BRL e
> IPCA) já são atualizados ao vivo (Yahoo Finance + API do Banco Central). Se
> quiser, pode mencionar isso na Seção 5.1 como adiantamento parcial.

## 9. Identidade visual — Seção 5.1

- Nome do sistema na interface passou a ser **"Econom-IA"** (antes "B3 ML
  Advisor"). Padronizar o texto se citar o nome do app.
- **Emojis/ícones** foram removidos da interface; o visual foi **simplificado**
  (tema escuro mantido, sem gradientes/sombras). Suavizar descrições muito
  "estilizadas" para algo como "interface limpa em tema escuro".

---

## Pontos que NÃO mudam (continuam corretos no texto)

- Coleta via yfinance, OHLCV diário, jan/2019 em diante (3.1 / 5.2).
- Contagens do dataset: 6.968 brutos, 6.732 válidos; treino 3.534, teste 1.515,
  OOS 1.683 (5.2 / 5.3).
- Indicadores: RSI-14, MACD (12,26,9), SMA e EMA de 9/21/50, Bollinger-20 (3.2).
- SANB11 como out-of-sample; TimeSeriesSplit 5 folds; divisão 70/30 (3.3).
- Persistência em Parquet + pickle + JSON; versionamento no GitHub (3.4 / 5.2).
- SVM, MLP e LSTM permanecem como escopo do **TC II** (3.3 / 5.4).