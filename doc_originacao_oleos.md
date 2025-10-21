# 📘 CapturaNF - Call Workflow

## Quando usar
Este workflow deve ser utilizado para solicitações relacionadas a **notas fiscais de originação*, ou seja, para monitorar **compras de óleos** e a quantidade adquirida de cada fornecedor.

> **Originação:** refere-se às nossas **compras registradas**, ou seja, quando negociamos e contratamos o fornecimento do produto.  
> **Captura de NF:** refere-se ao momento em que **recebemos a nota fiscal do fornecedor**, confirmando que o produto foi efetivamente enviado ou entregue.

---

## Campos principais
| Campo | Descrição |
|-------|-----------|
| `DATA` | Data da emissão da nota de origem (quando compramos ou capturamos a NF) |
| `FORNECEDOR` | Nome do fornecedor de quem compramos o produto |
| `QTD` | Quantidade do produto, em toneladas |
| `PRODUTO` | Descrição do produto comprado (originação) |
| `ENDERECO_FORNECEDOR` | Cidade e estado do fornecedor |
| `CHAVE_ACESSO` | Chave de acesso da nota fiscal |

---

## Regras de negócio
- A quantidade retornada **sempre em toneladas**.
- Permite **agrupar por fornecedor, data ou produto**.
- Caso o período não seja informado, deve-se assumir o mês atual.

---

## Exemplo de instrução para o Sub-agent
> Retornar a data e a quantidade das notas fiscais do fornecedor **São Martinho**, referentes ao **mês anterior**.

---

> 🔹 Observação: O foco desse workflow é acompanhar **tanto a previsão de compra (originação)** quanto **a confirmação de envio pelo fornecedor (captura de NF)**, permitindo controle completo do processo de aquisição.
