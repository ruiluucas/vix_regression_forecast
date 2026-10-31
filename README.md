# Previsão do VIX com Regressão Linear Múltipla

Prevê o **VIX (índice de volatilidade da CBOE) 5 dias à frente** a partir de indicadores do S&P 500,
usando regressão linear múltipla com diagnóstico completo em `statsmodels`.

## Resultado

**R² = 0,851 · R² ajustado = 0,851** · n = 3.763 observações · F = 7.173 · p < 0,001

| variável | coeficiente | p-valor | leitura |
|---|---|---|---|
| `const` | −1,5035 | 0,000 | — |
| **VIX (t)** | **0,9150** | 0,000 | volatilidade é altamente persistente |
| **RSI do SPX** | **0,0388** | 0,000 | sobrecompra no índice → VIX levemente maior |
| **Volume do SPX** | 2,754e-10 | 0,000 | efeito pequeno mas estatisticamente significativo |

Durbin-Watson = **1,992** → sem autocorrelação relevante nos resíduos.

## Metodologia

- Dados: `yfinance` — `^GSPC` (S&P 500) e `^VIX`, diário, desde 2005.
- Features: log-retorno do SPX e **RSI** (via `pandas_ta`).
- Alvo: **VIX em t+5**.
- Modelo: OLS (`statsmodels.api.OLS`) com saída estatística completa.

## O que o número significa (e o que não significa)

R² alto aqui **não é descoberta**: ~91% da variação explicada vem do próprio VIX defasado — que é
praticamente um passeio aleatório de volatilidade. O R² 0,851 diz que o modelo reproduz bem a
**persistência** do VIX, não que ele **prevê** movimentos novos.

O próximo passo honesto seria: remover a variável VIX(t) e medir quanto RSI e Volume explicam sozinhos —
que é o teste real de utilidade preditiva.

## Stack

`Python` · `pandas` · `numpy` · `scikit-learn` · `statsmodels` · `pandas_ta` · `yfinance` · `matplotlib`

## Rodar

```bash
pip install pandas numpy scikit-learn statsmodels pandas_ta yfinance matplotlib
jupyter notebook vix_regression_pred.ipynb
```

## Contexto

Estudo de 2023 sobre relação entre volatilidade implícita e indicadores técnicos do S&P 500.
