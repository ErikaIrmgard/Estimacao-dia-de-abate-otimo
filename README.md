# 📊 Estimacao-dia-de-abate-otimo

Projeto de modelagem de curvas de crescimento para estimar a idade ótima de abate (2800g) e gerar indicadores comparativos utilizados em dashboard no Power BI.

---

## 📌 Contexto do Problema

Na produção avícola, decisões sobre o momento ideal de abate impactam diretamente:

- Rentabilidade
- Eficiência produtiva
- Planejamento operacional
- Comparação entre desempenho real vs produtor

Este projeto ajusta modelos matemáticos sigmoides para estimar com precisão o ponto ótimo de abate.

---

## 🎯 Objetivos do Projeto

- Ajustar curvas de crescimento por grupo
- Estimar idade para atingir 2800g
- Comparar peso real vs previsto
- Selecionar automaticamente o melhor modelo estatístico
- Gerar base estruturada para visualização no Power BI

---

## 📈 Modelos Testados

- Gompertz
- Logístico
- Von Bertalanffy
- Richards

Seleção automática via AIC (Akaike Information Criterion).

---

## 🧠 Estratégia Estatística

- Ajuste com `scipy.optimize.curve_fit`
- Controle de limites biológicos
- Cálculo de R²
- Validação cruzada LOOCV
- Tratamento especial para poucos dados

---

## 📤 Saída Gerada

Arquivo consolidado contendo:

- Modelo escolhido
- Parâmetros estimados
- Idade para 2800g
- Peso previsto vs real aos 42 dias
- Métricas estatísticas

---

# 📊 Dashboard – Curvas de Crescimento

Visual analítico desenvolvido no Power BI para acompanhamento do desempenho produtivo.

---

## 🖼️ Visual do Dashboard

![Dashboard Curvas de Crescimento](./Dashboard%20Curvas%20de%20Crescimento.png)

---

## 🚀 Tecnologias Utilizadas

- Python
- Pandas
- NumPy
- SciPy
- Power BI

---

Projeto para portfólio de análise de dados aplicada ao setor agroindustrial.
