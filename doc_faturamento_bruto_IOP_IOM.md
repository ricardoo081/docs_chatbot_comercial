# 📘 Faturamento IOP/IOM — Workflow

---

## 🧭 Finalidade
Use este workflow para responder perguntas relacionadas ao **faturamento ou carregamento de IOP e IOM** nas unidades da Inpasa Brasil.  
Ele cobre **volume faturado (litros)**, **valor faturado (R$)**, **notas fiscais**, **clientes**, **cidades/UF** e **unidades/filiais**.

Abrange os produtos:
- 🟧 IOP  
- 🟨 IOM  

---

## 🧱 Colunas disponíveis
O Sub-agent SQL possui acesso aos seguintes campos:

| Campo | Descrição |
|--------|------------|
| `cod_filial` | Código da unidade / filial / usina |
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
| `volume_litros` | Volume faturado/carregado em litros |
| `valor_total` | Valor total faturado (R$) |

---

## ⚙️ Regras de negócio
- Volume sempre em **litros** e valor em **R$**.  
- Pode agrupar ou filtrar por **produto (IOP/IOM)**, **filial/unidade**, **cliente**, **cidade**, **estado**, **período**, **mercado**, **tipo de frete**.  
- Se o usuário mencionar “faturamento” ou “carregamento” sem filtros:
  - Considerar **todas as filiais/unidades**.  
  - Considerar **todos os produtos** (IOP e IOM).  
  - Agrupar por **produto** e **filial**.  
- Se o período **não for mencionado**, considerar o **mês atual**.  

---

## 🧩 Exemplos de instruções para o Sub-agent

1. Retornar o **volume faturado/carregado (litros)** e o **valor faturado (R$)** de **IOP**, agrupando por filial, considerando o mês atual.  
2. Retornar o faturamento total de **IOP e IOM**, no mês anterior, **agrupado por filial e produto**, com volume em litros e valor em reais.  
3. Retornar o faturamento diário de **IOM** em **Nova Mutum** na **última semana**, agrupando por cliente.  
4. Retornar o faturamento total de **IOP e IOM** em todas as unidades no **ano atual**, agrupado por mês e cidade.

---

## 🧮 Exemplos de interpretação de perguntas do usuário

| Pergunta do usuário | Instrução esperada para o Sub-agent |
|----------------------|-------------------------------------|
| “Qual foi o faturamento de IOP em Dourados este mês?” | Retornar o volume faturado/carregado (litros) e valor faturado (R$) de IOP na unidade de Dourados durante o mês atual, agrupando por produto e filial. |
| “Como está o faturamento total de IOP e IOM?” | Retornar o faturamento total (litros e R$) de ambos os produtos, em todas as unidades, agrupando por filial e produto, no mês atual. |
| “Faturamento de IOM semana passada” | Retornar o faturamento total (litros e R$) de IOM na semana anterior, agrupando por filial, cliente e produto. |

---

## 📌 Observações importantes
- Sempre reportar o **período considerado** e os **filtros aplicados** (produto, filial, cliente, agrupamento).  
- Sempre expressar o **volume em litros** e o **valor em R$**.  
- Caso o dado solicitado não exista ou não esteja disponível, retornar:
  > 👉 “Não tenho acesso a essa informação no momento.”

---

## 🧠 Contexto adicional para Rafinha
Quando identificar que a pergunta do usuário trata de **faturamento de IOP/IOM**, utilize este documento como base para montar o prompt de instrução para o Sub-agent.  
Sempre descreva claramente:
- o **produto** (IOP, IOM ou ambos),  
- a **unidade / filial / usina** (ou todas, se não especificado),  
- o **período** (mês atual por padrão, se não especificado),  
- a **métrica** (volume faturado/carregado em litros e valor faturado em R$),  
- e, se relevante, **cliente, cidade, estado, mercado e tipo de frete**.  

Exemplo final de prompt para o Sub-agent:  
> “Retornar o volume faturado/carregado (litros) e o valor faturado (R$) de IOP na unidade de Dourados durante o mês atual, agrupando por produto e filial.”
