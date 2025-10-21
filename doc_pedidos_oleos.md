# 📘 Pedidos Óleos – Call Workflow

---

## 🧭 Finalidade
Use este workflow para responder perguntas relacionadas aos **pedidos de venda de óleos** nas unidades da Inpasa Brasil.  
Aplica-se a consultas sobre **volumes contratados**, **quantidades pendentes**, **prazos de entrega**, **status de pedidos**, **clientes**, **unidades relacionadas a contratos** e **vigências de pedidos**.

Abrange os produtos:
- 🔴 Óleo Bruto  
- 🟠 Óleo Semi-Refinado  
- 🟡 Ácido Graxo  

---

## 🧱 Colunas disponíveis
O Sub-agent SQL possui acesso aos seguintes campos:

| Campo | Descrição |
|-------|-----------|
| `cod_filial` | Código da usina / filial / unidade |
| `filial` | Nome da unidade / filial / usina |
| `nr_pedido` | Número do pedido de venda |
| `datapedido` | Data de criação do pedido |
| `cod_cliente` | Código do cliente |
| `nome_cliente` | Nome do cliente |
| `cidade` | Cidade do cliente |
| `estado` | Estado (UF) do cliente |
| `status_pedido` | Situação do pedido |
| `COD_PROD/MAT` | Código do produto |
| `DESCRICAO_PROD/MAT` | Descrição do produto |
| `frete` | Tipo de frete (CIF ou FOB) |
| `mercado` | Tipo de mercado (Interno ou Externo) |
| `data_inicio` | Data inicial do contrato (`DD/MM/YYYY`) |
| `data_termino` | Data final do contrato (`DD/MM/YYYY`) — o mês desta data define o mês que deve ser entregue o volume |
| `quantidade_ton` | Quantidade total em toneladas |
| `quantidade_pendente_ton` | Quantidade pendente de entrega (saldo) |
| `valor_unitario_ton` | Valor unitário em R$/ton |
| `valor_total` | Valor total do pedido em R$ |

---

## ⚙️ Regras de negócio
- Valores sempre em **R$** e quantidade em **toneladas**.  
- Pode agrupar ou filtrar por **produto**, **filial/unidade**, **cliente**, **cidade**, **estado**, **período**, **status do pedido**, **vigência de contrato** ou **tipo de frete**, etc.  
- Se não houver filtro de unidade/filial ou produto:
  - Considerar **todas as filiais/unidades**  
  - Considerar **todos os produtos** (BRUTO, SEMI REFINADO, GRAXO)  
  - Agrupar por **produto** e **filial**  
- Ajuste os períodos de acordo com `datapedido` do contrato, se aplicável.  

---

## 🧩 Exemplos de instruções para o Sub-agent
1. Retornar o volume total contratado e o saldo pendente de entrega de óleo bruto na unidade de Dourados, agrupando por filial e produto.  
2. Retornar todos os pedidos de óleo semi-refinado para o mês de setembro de 2025, incluindo quantidade contratada, pendente, cliente e status do pedido.  
3. Retornar o valor total faturado e quantidade pendente de ácido graxo em todas as unidades, agrupando por filial e produto, considerando contratos vigentes no mês atual.  

---

## 📌 Observações importantes
- Sempre reportar o **período considerado** e os **filtros aplicados** (produto, filial, cliente, status do pedido, agrupamento).  
- Sempre expressar o **valor em R$** e a **quantidade em toneladas**.  
- Caso o dado solicitado não exista ou não esteja disponível, retornar:  
  > 👉 “Não tenho acesso a essa informação no momento.”  

---

## 🧠 Contexto adicional para Rafinha
Quando identificar que a pergunta do usuário trata de **pedidos de óleo**, utilize este documento como base para montar o prompt de instrução para o Sub-agent.  
Sempre descreva claramente:
- o **produto** (ou todos, se não especificado),  
- a **unidade / filial / usina** (ou todas, se não especificado),  
- o **período** (definido pelo contrato ou mês atual se não especificado),  
- a **métrica** (quantidade total contratada, quantidade pendente, valor total),  
- e, se relevante, **cliente, cidade, estado, mercado, status do pedido e tipo de frete**.  

Exemplo final de prompt para o Sub-agent:  
> “Retornar a quantidade total contratada e a quantidade pendente de entrega de óleo bruto na unidade de Dourados, agrupando por produto e filial, considerando todos os contratos ativos.”
