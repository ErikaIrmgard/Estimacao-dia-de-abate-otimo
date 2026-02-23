# Estimacao-dia-de-abate-otimo
Este projeto ajusta curvas de crescimento sigmoides a dados de peso por idade (dias) de frangos, para estimar a idade ótima de abate (dia em que o lote/produtor atinge 2800 g) e gerar indicadores para comparação com a curva do produtor no Power BI.
Entrada

Planilha Excel com medições de:
produtor (CODIGO_DO_PRODUTOR)
sexo do lote, linhagem, tipo de aviário
idade em dias (IDADE)
peso (PESO)
identificação do lote (NOME_DO_LOTE)

O script converte números no padrão PT-BR (vírgula decimal), remove registros inválidos e padroniza as chaves (trim de strings).

Pré-processamento e agregação
Conta quantos lotes existem por grupo (produtor, sexo, linhagem, tipo de aviário).
Calcula a curva média do produtor por idade (média do peso em cada idade dentro do grupo).
Essa curva agregada é a base usada para modelagem “por produtor”.

Modelos de crescimento testados

Para cada grupo (produtor/sexo/linhagem/aviário), o script tenta ajustar automaticamente um dos modelos:
Gompertz
Logístico
Von Bertalanffy
Richards

O ajuste é feito com scipy.optimize.curve_fit, com limites biológicos (assíntota entre 2000 e 6000g, e limites coerentes de parâmetros), para evitar soluções absurdas.
Escolha automática do melhor modelo
Quando o grupo tem 5 ou mais pontos, os quatro modelos são ajustados e comparados usando o AIC (Akaike Information Criterion).
O modelo com menor AIC é escolhido.
Também é calculado: 
R² do ajuste
LOOCV (validação cruzada Leave-One-Out), quando aplicável:
R2_LOOCV
RMSE_LOOCV

Estratégias quando há poucos dados

O script inclui regras para grupos com pouca informação:
Se o produtor tem 1–2 lotes: usa um “pool” de outros produtores com mesmo sexo/linhagem/aviário, ajusta Gompertz e aplica como referência.
Se há 2–4 pontos: ajusta Gompertz parcial, fixando a assíntota A a partir de uma curva-base.
Se há 0–1 ponto: usa apenas a curva-base (Gompertz) estimada previamente.

Curvas-base

Antes de tudo, o script ajusta uma curva-base Gompertz para cada combinação (produtor/sexo/linhagem/aviário) para ter parâmetros iniciais robustos e fallback em cenários com poucos dados.
Estimativas principais
Para o modelo escolhido, calcula:
IDADE_PARA_2800G: idade em que o peso previsto atinge 2800 g
(inversão numérica com brentq)
aplica um filtro biológico: IDADE_MIN_BIO = 28 e IDADE_MAX_BIO = 65
se cair fora, marca e zera (fica NaN) em IDADE_PARA_2800G_AJUST
peso_previsto_42: peso previsto no dia 42
peso_real_42: peso real observado no dia 42 (se existir)

Patch importante (Logístico)
Em alguns casos o parâmetro A do modelo Logístico pode ficar inválido/NaN.
O script tem um “PATCH” que reconstrói A usando uma observação de referência (preferencialmente o peso real aos 42 dias), garantindo consistência do modelo.
Saída
Gera um CSV consolidado com: chaves do grupo, modelo escolhido e parâmetros, assíntota e flags de qualidade, métricas (R², LOOCV), idade estimada para 2800g (crua e ajustada), peso previsto e real aos 42 dias,
número de pontos usados

## 📊 Dashboard – Curvas de Crescimento

![Dashboard](URL_DA_IMAGEM_AQUI)
