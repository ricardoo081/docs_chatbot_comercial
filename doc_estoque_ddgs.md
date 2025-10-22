📘 **Estoque DDGS — Workflow**  

---

### 🧭 **Finalidade**
Use este workflow para responder perguntas relacionadas ao **estoque de DDGS (Farelo de Milho)** nas unidades da **Inpasa Brasil**.  
Ele cobre consultas sobre **quantidades disponíveis**, **unidades (filiais / GEF)**, **armazéns** e permite cálculos de **totais ou médias em toneladas**.  

---

### 🧱 **Colunas disponíveis**
O Sub-agent SQL possui acesso aos seguintes campos (na view `CSCLIENTE.VW_ESTOQUE_DDGS_DW`):

| Campo              | Descrição |
|--------------------|-----------|
| `GEF`              | Unidade / filial / usina (ex: 1.1.1 – Unidade Sinop, 1.1.6 – Unidade Sidrolândia, etc.) |
| `DESCRICAO_ARMAZEM` | Nome do armazém onde o produto está armazenado |
| `EST_CONTR`         | Quantidade de estoque em toneladas (coluna base de cálculo) |

---

### ⚙️ **Regras de negócio**
- As **quantidades são sempre em toneladas**.  
- É possível **filtrar ou agrupar** por unidade (`GEF`) e armazém (`DESCRICAO_ARMAZEM`).  
- **Filtro padrão obrigatório:**  
  - Sempre considerar **`DESCRICAO_ARMAZEM = 'ARMAZEM INPASA'`**, **a menos que o usuário especifique outro armazém**.  
  - Esse é o **armazém principal das unidades da Inpasa**.  
- Se o usuário solicitar informações sobre produtos **fora de especificação**, **estoque bloqueado** ou **refaturamento**, deve-se ajustar o filtro conforme o caso:  
  - Fora de especificação → `DESCRICAO_ARMAZEM = 'BLOQUEADO'`  
  - Refaturamento → `DESCRICAO_ARMAZEM = 'REFATURAMENTO DDGS'`  
- Outros armazéns possíveis (caso o usuário especifique explicitamente):  
  - `ARMAZEM BIOMASSA`, `CARGILL`, `SANRY`, `COOPRATA`, `FERTILIZANTES SANTA CATARINA`, `YUKAER`
- Se o usuário **não especificar unidade nem armazém**:  
  - Considerar **todas as filiais**.  
  - Aplicar **`DESCRICAO_ARMAZEM = 'ARMAZEM INPASA'`** por padrão, sempre mencione isso, quando o usuario nao especificar, voce especificar para o subagent.  
  - Agrupar por **filial (GEF)**, sempre mencione isso tambem, quando o usuario nao especificar, a menos que ele peça o TOTAL mesmo.  

---

### 🧩 **Exemplos de instruções para o Sub-agent**
- Retornar a **quantidade total de DDGS por filial**, em toneladas, considerando o **ARMAZEM INPASA**.  
- Retornar o **estoque bloqueado** (fora de especificação) por unidade.  
- Retornar a **quantidade total no armazém Refaturamento DDGS**, agrupando por filial.  
- Retornar o **estoque total de DDGS** em **Sinop e Dourados**, agrupando por unidade, considerando o **ARMAZEM INPASA**.  

---

### 🧮 **Exemplos de interpretação de perguntas do usuário**

| Pergunta do usuário | Instrução esperada para o Sub-agent |
|----------------------|--------------------------------------|
| “Qual o estoque de DDGS em Sidrolândia e Nova Mutum?” | Retornar a quantidade total de DDGS em Sidrolândia e Nova Mutum, no armazém Inpasa, agrupando por filial. |
| “Quero ver o estoque bloqueado de DDGS” | Retornar o estoque de DDGS com armazém BLOQUEADO, agrupando por filial. |
| “Mostrar DDGS refaturamento por unidade” | Retornar a quantidade de DDGS no armazém REFATURAMENTO DDGS, agrupando por filial. |
| “Estoque geral de DDGS” | Retornar o estoque total de DDGS em toneladas, considerando todas as filiais, agrupando por GEF, com filtro no ARMAZEM INPASA. |
| “Qual estoque na Cargill” | Retornar o estoque de DDGS com armazém CARGILL, agrupando por filial. |

---

### 📌 **Observações importantes**
- Sempre **expressar as quantidades em toneladas**.  
- **Reportar claramente** no comentário final da query:  
  - Armazém utilizado  
  - Unidades filtradas  
  - Tipo de agrupamento  
- Se a informação não estiver disponível:
  - Caso os filtros existam, mas não há dados para o período ou unidade solicitada:
    👉 Explique que valor está zerado ou não há registro para os filtros aplicados.
  - Caso ocorra um erro técnico ao tentar recuperar os dados:
    👉 Explique que ocorreu um erro técnico ao consultar a informação, tente novamente mais tarde.
  - Caso a informação não esteja na base de conhecimento do agente ou não seja permitida:
    👉 “Não tenho acesso a essa informação no momento.” 

---

### 🧠 **Contexto adicional para Rafinha**
Quando o **Agent Principal** identificar que a solicitação trata de **estoque de DDGS / Farelo de Milho**, ele deve seguir este documento antes de acionar o Sub-agent SQL.

O prompt para o Sub-agent deve **sempre** descrever claramente:
- **As unidades (GEFs)** envolvidas ou todas, se não especificadas.  
- **O armazém** (padrão: ARMAZEM INPASA).  
- **O tipo de dado** (quantidade total, bloqueado, refaturamento, etc.).  
- **A métrica** (quantidade em toneladas).  
- **O agrupamento** (por GEF, armazém, ou ambos).

---

### 💬 **Exemplo final de prompt para o Sub-agent**
> “Retornar a quantidade total de DDGS nas unidades de Sidrolândia e Dourados, considerando apenas o armazém Inpasa, agrupando por filial, em toneladas.”
