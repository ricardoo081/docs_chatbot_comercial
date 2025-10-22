# 📘 Faturamento Bruto Óleos — Workflow

---

## 🧭 Finalidade
Use este workflow para responder perguntas relacionadas ao **faturamento bruto / carregamento** dos óleos e do caroço de algodão nas unidades da Inpasa Brasil.  
Ele cobre **valores faturados (R$)**, **quantidades faturadas (toneladas)**, **notas fiscais**, **clientes**, **cidades/UF**, **unidades/filiais** e **mercado interno e externo**.

Abrange os produtos:
- 🔴 Óleo Bruto  
- 🟠 Óleo Semi-Refinado  
- 🟡 Ácido Graxo  
- 🔵 Óleo Fúsel  
- ⚪ Caroço de Algodão  

---

## 🧱 Colunas disponíveis
O Sub-agent SQL possui acesso aos seguintes campos:

| Campo | Descrição |
|--------|------------|
| `cod_filial` | Código da usina / filial / unidade |
| `filial` | Nome da unidade / filial / usina |
| `dataemissao` | Data de emissão da nota fiscal / faturamento / carregamento |
| `nr_pedido` | Número do pedido de venda que gerou o faturamento |
| `nr_nf` | Número da nota fiscal faturada |
| `COD_PROD/MAT` | Código do produto faturado |
| `DESCRICAO_PROD/MAT` | Descrição do produto faturado |
| `cod_cliente` | Código do cliente que comprou o produto |
| `nome_cliente` | Nome do cliente que comprou o produto |
| `cidade` | Cidade do cliente |
| `estado` | Estado (UF) do cliente |
| `frete` | Tipo de frete (CIF ou FOB) |
| `mercado` | Mercado (INTERNO ou EXTERNO) |
| `quantidade_ton` | Quantidade faturada em toneladas |
| `valor_unitario_ton` | Valor unitário por tonelada |
| `valor_total` | Valor total bruto da nota |

---

## ⚙️ Regras de negócio
- Valores faturados sempre em **R$** e quantidade em **toneladas**.  
- Pode agrupar ou filtrar por **produto**, **filial/unidade**, **cliente**, **cidade**, **estado**, **período**, **mercado**, **tipo de frete**.  
- Se o usuário mencionar “faturamento” ou “carregamento” sem especificar filtros:
  - Considerar **todas as filiais/unidades**.  
  - Considerar **todos os produtos**.  
  - Agrupar por **produto** e **filial**.  
- Se o período **não for mencionado**, considerar o **mês atual**.  

---

## 🧩 Exemplos de instruções para o Sub-agent

1. Retornar o **valor total faturado (R$)** e a **quantidade faturada (toneladas)** de **óleo bruto**, agrupando por filial, considerando o mês atual.  
2. Retornar o faturamento total de **todos os óleos**, no mês anterior, **agrupado por filial e produto**, com resultado em R$ e toneladas.  
3. Retornar o faturamento diário de **óleo semi-refinado** em **Nova Mutum** na **última semana**, agrupando por cliente.  
4. Retornar o faturamento total de **ácido graxo e caroço de algodão** em todas as unidades no **ano atual**, agrupado por mês e mercado (interno/externo).

---

## 🧮 Exemplos de interpretação de perguntas do usuário

| Pergunta do usuário | Instrução esperada para o Sub-agent |
|----------------------|-------------------------------------|
| “Qual foi o faturamento de óleo bruto em Dourados este mês?” | Retornar o valor total faturado (R$) e a quantidade faturada (toneladas) de óleo bruto na unidade de Dourados durante o mês atual, agrupando por produto e filial. |
| “Como está o faturamento total dos óleos?” | Retornar o faturamento total (R$) e quantidade faturada (toneladas) de todos os produtos, em todas as unidades, agrupando por filial e produto, no mês atual. |
| “Faturamento de ácido graxo semana passada” | Retornar o faturamento total (R$) e quantidade faturada (toneladas) de ácido graxo na semana anterior, agrupando por filial, cliente e produto. |

---

## 📌 Observações importantes
- Sempre reportar o **período considerado** e os **filtros aplicados** (produto, filial, cliente, agrupamento).  
- Sempre expressar o **valor em R$** e a **quantidade em toneladas**.  
- Se a informação não estiver disponível:
  - Caso os filtros existam, mas não há dados para o período ou unidade solicitada:
    👉 Explique que valor está zerado ou não há registro para os filtros aplicados.
  - Caso ocorra um erro técnico ao tentar recuperar os dados:
    👉 Explique que ocorreu um erro técnico ao consultar a informação, tente novamente mais tarde.
  - Caso a informação não esteja na base de conhecimento do agente ou não seja permitida:
    👉 “Não tenho acesso a essa informação no momento.” 

---

## 🧠 Contexto adicional para Rafinha
Quando identificar que a pergunta do usuário trata de **faturamento bruto dos óleos**, utilize este documento como base para montar o prompt de instrução para o Sub-agent.  
Sempre descreva claramente:
- o **produto** (ou todos, se não especificado),  
- a **unidade / filial / usina** (ou todas, se não especificado),  
- o **período** (mês atual por padrão, se não especificado),  
- a **métrica** (valor total em R$ e quantidade faturada em toneladas),  
- e, se relevante, **cliente, cidade, estado, mercado e tipo de frete**.  

Exemplo final de prompt para o Sub-agent:  
> “Retornar o valor total faturado (R$) e a quantidade faturada (toneladas) de óleo bruto na unidade de Dourados durante o mês atual, agrupando por produto e filial.”
