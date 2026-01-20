# 📘 Pedidos DDGS – Call Workflow

---

## 🧭 Finalidade
Use este workflow para responder perguntas relacionadas aos **pedidos de venda de DDGS** nas unidades da Inpasa Brasil.  
Aplica-se a consultas sobre **volumes contratados**, **quantidades pendentes**, **prazos de entrega**, **volumes cadenciados**, **clientes**, **unidades relacionadas a contratos** e **vigências de pedidos**.

---

## 🧱 Colunas disponíveis
O Sub-agent SQL possui acesso aos seguintes campos:

| Campo | Descrição |
|-------|-----------|
| `cod_filial` | Código da usina / filial / unidade |
| `filial` | Nome da unidade / filial / usina |
| `nr_pedido` | Número do pedido de venda / contrato |
| `datapedido` | Data de criação do pedido |
| `cod_cliente` | Código do cliente |
| `nome_cliente` | Nome do cliente|
| `cidade` | Cidade do cliente em maiúsculo|
| `estado` | Estado (UF) do cliente|
| `nome_supervisor` | retorna o nome do SUPERVISOR/ VENDEDOR/ RESPONSAVEL|
| `status` | Situação do pedido|
| `cod_produto` | Código do produto |
| `descricao_produto` | Descrição do produto |
| `deposito` | retorna de onde ta saindo o produto, pode ser  (COOPRATA, PARAGUAI, YUKAER, REMPEL ou ARMAZEM DDGS INPASA)|
| `frete` | Tipo de frete (CIF ou FOB) |
| `data_inicio` | Data inicial do contrato (`DD/MM/YYYY`) |
| `data_termino` | Data final do contrato (`DD/MM/YYYY`) — o mês final do range de entrega do volume |
| `data_termino_original` | Primeira data final do contrato (`DD/MM/YYYY`) — o mês final do range de entrega do volume que foi incluso primeiro, pode ser diferente da data termino se o contrato tiver sido postergado |
| `dias_postergados` | retorna a quantidade de dias que o contrato foi postergado data_termino - data_termino_original |
| `quantidade_ton` | Quantidade total vendida em toneladas |
| `quantidade_pendente_ton` | Quantidade pendente de entrega (saldo) |
| `quantidade_ton_fat` | Quantidade faturada/carregada |
| `valor_unitario_ton` | Valor unitário bruto em R$/ton |
| `valor_net` | Valor liquido em R$/ton |
| `valor_total` | Valor total bruto do pedido em R$ |
| colunas `cadencia_janeiro_2026, cadencia_fevereiro_2026, cadencia_marco_2026...` até cadencia_dezembro_2026| retornam a qtd/volume cadenciado/vendido/para carregar por mês |
| colunas reservado_janeiro_2026, reservado_fevereiro_2026, reservado_marco_2026... até reservado_dezembro_2026 | retornam a qtd/volume reservado (cota distribuída no portal) por mês |
| colunas carregado_janeiro_2026, carregado_fevereiro_2026, carregado_marco_2026... até carregado_dezembro_2026 | retornam a qtd/volume carregado/faturado/expedido por mês |

---

## ⚙️ Regras de negócio
- Valores sempre em **R$** e quantidade em **toneladas**.  
- Pode agrupar ou filtrar por **produto**, **filial/unidade**, **cliente**, **nome_supervisor**, **cidade**, **estado**, **período**, **status do pedido**, **vigência de contrato**, **tipo de frete**, etc.  
- Se não houver filtro de unidade/filial ou produto:
  - Considerar **todas as filiais/unidades**  
  - Agrupar por **filial**  
- Ajuste os períodos de acordo com `datapedido` do contrato, se aplicável.
- Sempre que o usuario pedir NET ou NET MEDIO, peça ao subagent net medio ponderado, so trabalhamos com net ponderado

---

## 🧩 Exemplos de instruções para o Sub-agent
1. Retornar o volume total contratado e o saldo pendente de entrega de ddgs na unidade de Dourados, agrupando por filial.  
2. Retornar todos o volume cadenciado de ddgs no mes de janeiro, fevereiro e março do supervisor DOUGLAS
3. Retornar o volume pendente do contrato 123 

---

## 🧠 Contexto adicional para Farol
Quando identificar que a pergunta do usuário trata de **pedidos de ddgs**, utilize este documento como base para montar o prompt de instrução para o Sub-agent.  
Sempre descreva claramente:
- o **supervisor** (ou todos, se não especificado),  
- a **unidade / filial / usina** (ou todas, se não especificado),  
- o **período** (definido pelo contrato ou mês atual se não especificado),  
- a **métrica** (quantidade total contratada, quantidade pendente, valor total),  
- e, se relevante, **cliente, cidade, estado, mercado, status do pedido e tipo de frete**.  

Exemplo final de prompt para o Sub-agent:  
> “Qual o volume cadenciado e o volume reservado de DDGS com origem no depósito YUKAER?”
