# 📊 Estimacao-dia-de-abate-otimo

Projeto para ajuste de curvas de crescimento sigmoides a dados de peso por idade (dias) de frangos, com objetivo de estimar a idade ótima de abate (quando o lote atinge 2800g) e gerar indicadores comparativos para uso em dashboard no Power BI.

---

## 🎯 Objetivo

- Estimar o **dia ótimo de abate (2800g)**
- Comparar desempenho do lote vs curva do produtor
- Gerar métricas estatísticas de ajuste
- Alimentar visual analítico no Power BI

---

## 📥 Dados de Entrada

Planilha Excel contendo:

- CODIGO_DO_PRODUTOR  
- NOME_DO_LOTE  
- SEXO  
- LINHAGEM  
- TIPO_DE_AVIARIO  
- IDADE (dias)  
- PESO (g)  

---

## ⚙️ Pré-processamento

- Conversão para padrão PT-BR (vírgula decimal)
- Remoção de registros inválidos
- Padronização de chaves (trim de strings)
- Agregação por grupo (produtor/sexo/linhagem/aviário)
- Cálculo da curva média por idade

---

## 📈 Modelos de Crescimento Testados

Para cada grupo o script testa automaticamente:

- Gompertz  
- Logístico  
- Von Bertalanffy  
- Richards  

O ajuste é feito com `scipy.optimize.curve_fit`, com limites biológicos:

- Assíntota entre 2000g e 6000g
- Controle para evitar soluções absurdas

### 🔎 Seleção do Melhor Modelo

- Comparação via **AIC (Akaike Information Criterion)**
- Escolha do menor AIC
- Cálculo adicional:
  - R²
  - LOOCV
  - RMSE_LOOCV

---

## 🧠 Estratégia para Poucos Dados

Regras aplicadas:

- 1–2 lotes → uso de "pool" de referência
- 3–4 pontos → ajuste parcial Gompertz
- 0–1 ponto → uso da curva-base estimada

---

## 📊 Métricas Calculadas

- IDADE_PARA_2800G  
- IDADE_MIN_BIO  
- IDADE_MAX_BIO  
- IDADE_PARA_2800G_AJUST (quando necessário)  
- PESO_PREVISTO_42  
- PESO_REAL_42  
- R²  
- LOOCV  

---

## 🛠 Patch Importante (Modelo Logístico)

Em casos onde o parâmetro A fica inválido, o script reconstrói a assíntota usando observação de referência (preferencialmente peso real aos 42 dias).

---

## 📤 Saída

Geração de CSV consolidado contendo:

- Grupo
- Modelo escolhido
- Parâmetros estimados
- Métricas estatísticas
- Idade estimada para 2800g
- Peso previsto vs real aos 42 dias

---

# 📊 Dashboard – Curvas de Crescimento

Visual desenvolvido no Power BI para análise comparativa de desempenho produtivo.

## 🖼️ Visual do Dashboard

![Dashboard Curvas de Crescimento](Dashboard%20Curvas%20de%20Crescimento.png)

---

## 🚀 Tecnologias Utilizadas

- Python
- Pandas
- NumPy
- SciPy
- Power BI

---

## 👤 Autor

Erika L. M. Gard  
Projeto desenvolvido para portfólio de análise de dados aplicada ao setor agroindustrial.
