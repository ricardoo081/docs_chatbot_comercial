# 📘 Faturamento Bruto DDGS — Workflow

---

## 🧭 Finalidade
Use este workflow para responder perguntas relacionadas ao **faturamento bruto / carregamento** de DDGS / Farelo de Milho nas unidades da Inpasa Brasil.  
Ele cobre **valores faturados (R$)**, **quantidades faturadas (toneladas)**, **notas fiscais**, **clientes**, **cidades/UF**, **unidades/filiais**, **supervisores/vendedores** e **mercado interno e externo**.

Abrange o produto:
- 🌾 DDGS / Farelo de Milho  

---

## 🧱 Colunas disponíveis
O Sub-agent SQL possui acesso aos seguintes campos:

| Campo | Descrição |
|--------|------------|
| `cod_filial` | Código da filial / unidade / GEF |
| `filial` | Nome da unidade / filial / GEF |
| `dataemissao` | Data de emissão da nota fiscal / faturamento / carregamento |
| `nr_pedido` | Número do pedido de venda que gerou o faturamento |
| `nr_nf` | Número da nota fiscal faturada |
| `COD_PROD/MAT` | Código do produto faturado |
| `DESCRICAO_PROD/MAT` | Descrição do produto faturado |
| `nome_supervisor` | Nome do supervisor/vendedor responsável pela venda |
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
- Pode agrupar ou filtrar por **produto**, **filial/unidade/GEF**, **cliente**, **cidade**, **estado**, **período**, **mercado**, **tipo de frete**, **supervisor/vendedor**.  
- Se o usuário mencionar “faturamento” ou “carregamento” sem especificar filtros:
  - Considerar **todas as filiais/unidades**.  
  - Considerar **todos os produtos**.  
  - Agrupar por **filial**.  
- Se o período **não for mencionado**, considerar o **mês atual**.  

---

## 🧩 Exemplos de instruções para o Sub-agent

1. Retornar o **valor total faturado (R$)** e a **quantidade faturada (toneladas)** de **DDGS**, agrupando por filial, considerando o mês atual.  
2. Retornar o faturamento total de **DDGS**, no mês anterior, **agrupado por filial e produto**, com resultado em R$ e toneladas.  
3. Retornar o faturamento diário de **DDGS** em **Nova Mutum** na **última semana**, agrupando por cliente e supervisor.  
4. Retornar o faturamento total de **DDGS** em todas as unidades no **ano atual**, agrupado por mês, mercado e tipo de frete.

---

## 🧮 Exemplos de interpretação de perguntas do usuário

| Pergunta do usuário | Instrução esperada para o Sub-agent |
|----------------------|-------------------------------------|
| “Qual foi o faturamento de DDGS em Dourados este mês?” | Retornar o valor total faturado (R$) e a quantidade faturada (toneladas) de DDGS na unidade de Dourados durante o mês atual, agrupando por filial e produto. |
| “Como está o faturamento total de DDGS?” | Retornar o faturamento total (R$) e quantidade faturada (toneladas) de todos os produtos DDGS em todas as unidades, agrupando por filial, no mês atual. |
| “Faturamento DDGS semana passada” | Retornar o faturamento total (R$) e quantidade faturada (toneladas) de DDGS na semana anterior, agrupando por filial, cliente, produto e supervisor. |

---

## 📌 Observações importantes
- Sempre reportar o **período considerado** e os **filtros aplicados** (produto, filial, cliente, supervisor, agrupamento).  
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
Quando identificar que a pergunta do usuário trata de **faturamento bruto de DDGS / Farelo de Milho**, utilize este documento como base para montar o prompt de instrução para o Sub-agent.  
Sempre descreva claramente:
- a **unidade / filial / GEF** (ou todas, se não especificado),  
- o **período** (mês atual por padrão, se não especificado),  
- a **métrica** (valor total em R$ e quantidade faturada em toneladas),  
- e, se relevante, **cliente, cidade, estado, mercado, tipo de frete e supervisor/vendedor**.  

Exemplo final de prompt para o Sub-agent:  
> “Retornar o valor total faturado (R$) e a quantidade faturada (toneladas) de DDGS na unidade de Dourados durante o mês atual, agrupando por filial e produto, incluindo cliente e supervisor.”
