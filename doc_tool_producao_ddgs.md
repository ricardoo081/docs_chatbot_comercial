# 📘 Produção de DDGS — Workflow

---

## 🧭 Finalidade
Use este workflow para responder perguntas relacionadas à **produção de DDGS / farelo de milho** nas unidades da Inpasa Brasil.  
Ele cobre **volumes produzidos**, **rendimentos**, **quantidades fabricadas** e **comparações com projeção**.

---

## 🧱 Colunas disponíveis
O Sub-agent SQL possui acesso aos seguintes campos:

| Campo | Descrição |
|--------|------------|
| `DATA` | Data da produção (exemplo: 01/01/2025) |
| `cod_filial` | Código da filial / unidade / GEF |
| `GEF` | Nome da filial / unidade / usina |
| `PROD_DDGS` | Quantidade produzida em toneladas |
| `PROJECAO_PROD` | Projeção de produção em toneladas (o que devia ter sido produzido) |

---

## ⚙️ Regras de negócio
- O volume produzido é **sempre em toneladas**.  
- Pode agrupar ou filtrar por **unidade/GEF** e **período** (dia, semana, mês, ano).  
- Se o usuário mencionar “produção DDGS” ou “produção total DDGS” sem especificar unidade/filiais:
  - Considerar **todas as filiais/unidades**.  
  - Agrupar por **filial / GEF**.  
- Se o período **não for mencionado**, considerar o **mês atual**.  
- Sempre informar ao Sub-agent **quais filtros foram aplicados** e **como agrupar os dados**, de acordo com o pedido do usuário.  
- Para análises de variação, considere a **diferença entre produção real e projeção**:  
  > Diferença = `PROD_DDGS - PROJECAO_PROD`
- **Nunca** considerar o dia atual no filtro de data da producção, pois a produção do dia só é consolidada no dia seguinte.  
- Se o usuário perguntar sobre o **dia atual**, informe que os dados do dia só são disponibilizados no dia seguinte e, portanto, não estão disponíveis ainda.

---

## 📊 Agrupamentos permitidos
- Por filial / unidade / GEF  
- Por período (dia, semana, mês, ano)  
- Combinações (ex: por unidade e período)

---

## 🧩 Exemplos de instruções para o Sub-agent

1. Retornar o volume total produzido de **DDGS** na **unidade de Dourados** durante o **mês atual**.  
2. Retornar o volume total produzido de **todos os DDGS** no **mês anterior**, **agrupado por filial**, com resultado em **toneladas**.  
3. Retornar a **produção diária de DDGS** em **Nova Mutum** na **última semana**, com comparação à projeção.  
4. Retornar o **total de produção de DDGS** em **todas as unidades** no **ano atual**, agrupado por **mês**, incluindo diferença entre produção e projeção.

---

## 🧮 Exemplos de interpretação de perguntas do usuário

| Pergunta do usuário | Instrução esperada para o Sub-agent |
|----------------------|-------------------------------------|
| “Qual foi a produção de DDGS em Dourados este mês?” | Retornar o volume total produzido de DDGS na unidade de Dourados durante o mês atual, com resultado em toneladas. |
| “Como está a produção total de DDGS?” | Retornar o volume total produzido de DDGS em todas as unidades, agrupado por filial, no mês atual, com diferença em relação à projeção. |
| “Produção DDGS semana passada” | Retornar o volume total produzido de DDGS na semana anterior, agrupando por filial, com resultado em toneladas e diferença em relação à projeção. |

---

## 📌 Observações importantes
- Sempre reportar o **período considerado** e os **filtros aplicados** (unidade, agrupamento).  
- Sempre expressar o volume em **toneladas**.  
- Caso o dado solicitado não exista ou não esteja disponível, retornar:
  > 👉 “Não tenho acesso a essa informação no momento.”  

---

## 🧠 Contexto adicional para Rafinha
Quando identificar que a pergunta do usuário trata de **produção de DDGS**, utilize este documento como base para montar o prompt de instrução para o Sub-agent.  
Sempre descreva claramente:
- a **unidade / filial / GEF** (ou todas, se não especificado),  
- o **período** (mês atual por padrão),  
- a **métrica** (volume produzido em toneladas),  
- e, se relevante, a **diferença entre produção e projeção**.  

Exemplo final de prompt para o Sub-agent:  
> “Retornar o volume total produzido de DDGS na unidade de Dourados durante o mês atual, agrupando por filial, com resultado em toneladas, incluindo diferença em relação à projeção.”
