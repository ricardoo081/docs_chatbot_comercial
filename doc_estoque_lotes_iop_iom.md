# 📘 Estoque Lote IOP/IOM — Workflow

---

## 🧭 Finalidade
Use este workflow para responder perguntas relacionadas ao **estoque de lotes de IOP e IOM** nas unidades da Inpasa Brasil.  
Ele cobre **saldo disponível**, **validades**, **quantidades por filial** e **tipo de produto**.  
O volume é sempre em **litros** (não toneladas).

---

## 🧱 Colunas disponíveis
O Sub-agent SQL possui acesso aos seguintes campos:

| Campo | Descrição |
|--------|------------|
| `filial` | Nome da unidade / filial |
| `cod_filial` | Código da filial (identificador único) |
| `material` | Descrição do produto (IOP ou IOM) |
| `cod_lote` | Identificador único do lote |
| `lote` | Descrição do lote |
| `data_fabricacao` | Data de fabricação do lote |
| `data_validade` | Data de validade do lote |
| `saldo_lote_litros` | Quantidade disponível do lote em litros |

---

## ⚙️ Regras de negócio
- Volume em **litros**.  
- Pode agrupar ou filtrar por **filial/unidade**, **tipo de produto (IOP/IOM)**, **lote**, **validade** ou **outros filtros solicitados pelo usuário**.  
- Sempre informe ao Sub-agent **quais filtros foram aplicados** e **como agrupar os dados** de acordo com o pedido do usuário.  
- Se o período ou filtros **não forem mencionados**, considere todas as filiais e produtos, agrupando por tipo de produto e filial.  
- Não invente significados ou informações

---

## 🧩 Exemplos de instruções para o Sub-agent

1. Retornar o **saldo total em litros** de IOP e IOM, agrupando por filial e tipo de produto.  
2. Retornar o **estoque de lotes vencendo no próximo mês**, agrupado por filial e produto.  
3. Retornar a **quantidade disponível por lote** em uma unidade específica, filtrando apenas IOM.  

---

## 🧮 Exemplos de interpretação de perguntas do usuário

| Pergunta do usuário | Instrução esperada para o Sub-agent |
|----------------------|-------------------------------------|
| “Qual o saldo de IOP em Dourados?” | Retornar o saldo total em litros de IOP na filial de Dourados, agrupando por tipo de produto. |
| “Saldo total de IOM e IOP por filial” | Retornar o saldo total em litros de IOP e IOM, agrupando por filial e tipo de produto. |
| “Quais lotes de IOM estão vencendo este mês?” | Retornar todos os lotes de IOM com data de validade no mês atual, agrupando por filial e lote. |

---

## 📌 Observações importantes
- Sempre reportar o **filtro aplicado** (produto, filial, lote, validade, etc.) e o **agrupamento usado**.  
- Sempre expressar o **volume em litros**.  
- Se a informação não estiver disponível:
  - Caso os filtros existam, mas não há dados para o período ou unidade solicitada:
    👉 Explique que valor está zerado ou não há registro para os filtros aplicados.
  - Caso ocorra um erro técnico ao tentar recuperar os dados:
    👉 Explique que ocorreu um erro técnico ao consultar a informação, tente novamente mais tarde.
  - Caso a informação não esteja na base de conhecimento do agente ou não seja permitida:
    👉 “Não tenho acesso a essa informação no momento.” 

---

## 🧠 Contexto adicional para Rafinha
Quando identificar que a pergunta do usuário trata de **estoque de lotes de IOP/IOM**, utilize este documento como base para montar o prompt de instrução para o Sub-agent.  
Sempre descreva claramente:
- a **filial/unidade** (ou todas, se não especificado),  
- o **produto** (IOP, IOM ou ambos),  
- o **lote** ou **validade** (se solicitado),  
- a **métrica** (volume em litros),  
- e, se relevante, **agrupamento** por filial, produto ou lote.  

Exemplo final de prompt para o Sub-agent:  
> “Retornar o saldo total em litros de IOP e IOM, agrupando por filial e tipo de produto separadamente.”
