# 📘 Produção de Óleos — Workflow

---

## 🧭 Finalidade
Use este workflow para responder perguntas relacionadas à **produção de óleos** nas unidades da Inpasa Brasil.  
Ele cobre **volumes produzidos**, **rendimentos**, **quantidades fabricadas** e **análises de produção** dos seguintes produtos:
- 🔴 Óleo Bruto  
- 🟡 Ácido Graxo  
- 🟠 Óleo Semi-Refinado  

---

## 🧱 Colunas disponíveis
O Sub-agent SQL possui acesso aos seguintes campos:

| Campo | Descrição |
|--------|------------|
| `DATA` | Data da produção (exemplo: 01/01/2025) |
| `produto` | Descrição do produto (Óleo Bruto, Ácido Graxo, Óleo Semi-Refinado) |
| `cod_filial` | Código da usina / filial / unidade |
| `filial` | Nome da usina / filial / unidade |
| `producao` | Quantidade produzida em toneladas |

---

## ⚙️ Regras de negócio
- O volume produzido é **sempre em toneladas**.  
- Pode agrupar ou filtrar por **produto**, **filial/unidade** e **período** (dia, semana, mês, ano).  
- Se o usuário mencionar “produção” sem especificar produto ou unidade:
  - Considerar **todas as filiais/unidades**.  
  - Considerar **todos os produtos (Óleo Bruto, Ácido Graxo e Óleo Semi-Refinado)**.  
  - Agrupar por **filial** e **produto**.  
- Se o período **não for mencionado**, considerar o **mês atual**.  
- Sempre informar ao Sub-agent **quais filtros devem ser aplicados** e **como agrupar os dados**, de acordo com o pedido do usuário.  
- **Nunca** considerar o dia atual no filtro de data da producção, pois a produção do dia só é consolidada no dia seguinte.  
- Se o usuário perguntar sobre o **dia atual**, informe que os dados do dia só são disponibilizados no dia seguinte e, portanto, não estão disponíveis ainda.
  
---


## 📊 Agrupamentos permitidos
- Por produto  
- Por filial/unidade  
- Por período (dia, semana, mês, ano)  
- Combinações (ex: por produto e filial)

---

## 🧩 Exemplos de instruções para o Sub-agent

1. Retornar o volume total produzido de **óleo bruto** na **unidade de Dourados** durante o **mês atual**.  
2. Retornar o volume total de produção de **todos os óleos** no **mês anterior**, **agrupado por filial e produto**, com resultado em **toneladas**.  
3. Retornar a **produção diária de óleo semi-refinado** em **Nova Mutum** na **última semana**.  
4. Retornar o **total de produção de ácido graxo** em **todas as unidades** no **ano atual**, agrupado por **mês**.  

---

## 🧮 Exemplos de interpretação de perguntas do usuário

| Pergunta do usuário | Instrução esperada para o Sub-agent |
|----------------------|-------------------------------------|
| “Qual foi a produção de óleo bruto em Dourados este mês?” | Retornar o volume total produzido de óleo bruto na unidade de Dourados durante o mês atual, com resultado em toneladas. |
| “Como está a produção total dos óleos?” | Retornar o volume total produzido de todos os tipos de óleo (bruto, semi refinado e ácido graxo), considerando todas as unidades, agrupado por filial e produto, no mês atual. |
| “Produção de ácido graxo semana passada” | Retornar o volume total produzido de ácido graxo na semana anterior, agrupando por filial, com resultado em toneladas. |

---

## 📌 Observações importantes
- Sempre reportar o **período considerado** e os **filtros aplicados** (produto, unidade, agrupamento).  
- Sempre expressar o volume em **toneladas**.  
- Se a informação não estiver disponível:
  - Caso os filtros existam, mas não há dados para o período ou unidade solicitada:
    👉 Explique que valor está zerado ou não há registro para os filtros aplicados.
  - Caso ocorra um erro técnico ao tentar recuperar os dados:
    👉 Explique que ocorreu um erro técnico ao consultar a informação, tente novamente mais tarde.
  - Caso a informação não esteja na base de conhecimento do agente ou não seja permitida:
    👉 “Não tenho acesso a essa informação no momento.” 
- Para formatação da resposta ao usuário (WhatsApp), utilize os emojis correspondentes:
  - 🔴 Óleo Bruto  
  - 🟠 Óleo Semi-Refinado  
  - 🟡 Ácido Graxo  

---

## 🧠 Contexto adicional para Rafinha
Quando identificar que a pergunta do usuário trata de **produção dos óleos**, utilize este documento como base para montar o prompt de instrução para o Sub-agent.  
Sempre descreva claramente:
- o produto (ou todos, se não especificado),
- o período (mês atual por padrão),
- a unidade (ou todas, se não especificada),
- e a métrica (volume produzido em toneladas).

Exemplo final de prompt para o Sub-agent:
> “Retornar o volume total produzido de óleo bruto na unidade de Dourados durante o mês atual, agrupando por produto e unidade, com resultado em toneladas.”

---

