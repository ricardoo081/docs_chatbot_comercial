# 📘 Estoque Óleos — Workflow

---

## 🧭 Finalidade
Use este workflow para responder perguntas relacionadas ao **estoque de óleos** nas unidades da Inpasa Brasil.  
Ele cobre **quantidades disponíveis (toneladas)**, **agrupamentos por unidade, produto, tanque**, e permite cálculos de totais ou médias quando necessário.

Abrange os produtos:
- 🔴 Óleo Bruto  
- 🟡 Ácido Graxo  
- 🟠 Óleo Semi-Refinado  

---

## 🧱 Colunas disponíveis
O Sub-agent SQL possui acesso aos seguintes campos:

| Campo | Descrição |
|-------|-----------|
| `filial` | Nome da unidade / filial / usina |
| `produto` | Tipo de óleo (ex: óleo bruto, ácido graxo, óleo semi-refinado) |
| `deposito` | Tanque de armazenamento |
| `quantidade_ton` | Quantidade disponível em toneladas |

---

## ⚙️ Regras de negócio
- Os volumes são sempre em **toneladas** (nunca litros ou reais).  
- Pode agrupar ou filtrar por **filial/unidade**, **produto**, **tanque** ou combinações destes.  
- Se o usuário não especificar unidade nem produto:
  - Considerar **todas as filiais**.  
  - Considerar **todos os produtos** (BRUTO, SEMI REFINADO, GRAXO).  
  - Agrupar por **produto** e **filial**.  

---

## 🧩 Exemplos de instruções para o Sub-agent

1. Retornar a **quantidade total de óleo bruto** por **filial**, em toneladas, considerando todas as unidades.  
2. Retornar a **quantidade disponível de ácido graxo** em **Dourados**, agrupada por **tanque**.  
3. Retornar o **estoque de todos os óleos** em **Nova Mutum**, agrupando por produto e depósito, com resultado em toneladas.  

---

## 🧮 Exemplos de interpretação de perguntas do usuário

| Pergunta do usuário | Instrução esperada para o Sub-agent |
|-------------------|------------------------------------|
| “Qual o estoque de óleo bruto em Dourados?” | Retornar a quantidade total de óleo bruto em Dourados, em toneladas, agrupando por filial. |
| “Como está o estoque de todos os óleos?” | Retornar o estoque total em toneladas de todos os produtos, em todas as unidades, agrupando por filial e produto. |
| “Quantidade de ácido graxo por tanque em Nova Mutum” | Retornar a quantidade de ácido graxo em cada tanque na unidade de Nova Mutum, em toneladas. |

---

## 📌 Observações importantes
- Sempre reportar o **filtro aplicado** (produto, filial, depósito) e o **agrupamento** utilizado.  
- Sempre expressar a **quantidade em toneladas**.  
- Caso o dado solicitado não exista ou não esteja disponível, retornar:
  > 👉 “Não tenho acesso a essa informação no momento.”  

---

## 🧠 Contexto adicional para Rafinha
Quando identificar que a pergunta do usuário trata de **estoque de óleos**, utilize este documento como base para montar o prompt de instrução para o Sub-agent.  
Sempre descreva claramente:
- o **produto** (ou todos, se não especificado),  
- a **unidade / filial / usina** (ou todas, se não especificado),  
- o **tanque/deposito** (se solicitado),  
- a **métrica** (quantidade disponível em toneladas).  

Exemplo final de prompt para o Sub-agent:  
> “Retornar a quantidade total de óleo bruto na unidade de Dourados, agrupando por filial, em toneladas.”
