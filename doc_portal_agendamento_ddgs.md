📘 Portal Agendamento de DDGS — Workflow

🧭 Contexto  
Os dados tratados neste workflow são provenientes do **Portal Logística Comercial da Inpasa Brasil**, que gerencia o ciclo de carregamento e faturamento de **DDGS**.  
O sistema organiza e acompanha todas as etapas desde a **reserva de volumes**, passando pelo **agendamento de transporte**, até a **realização/faturamento efetivo**, permitindo ao usuário ter uma visão completa do planejamento e execução comercial.

---

🧱 **Colunas disponíveis**  
O Sub-agent SQL possui acesso aos seguintes campos:

| Campo | Descrição |
|--------|------------|
| data | Data de referência |
| cod_filial | Código da filial/usina/unidade |
| filial | Descrição da filial/usina/unidade |
| nr_pedido | Número do pedido de venda |
| cod_produto | Código do produto |
| descricao_produto | Descrição do produto |
| cod_cliente | Código do cliente |
| desc_cliente | Nome do cliente/fornecedor |
| status_financeiro | 'GERA FINANCEIRO' ou 'NÃO GERA FINANCEIRO' |
| tipo_frete | CIF ou FOB |
| mercado | 'Mercado Interno' ou 'Mercado Externo' |
| cidade | Cidade do cliente/fornecedor |
| estado | UF do cliente/fornecedor |
| quantidade_pedido | Quantidade total do pedido em toneladas |
| saldo_pedido | Quantidade pendente em toneladas |
| capacidade | Capacidade de reserva em toneladas |
| reservado | Quantidade reservada em toneladas |
| valor_reservado | Valor reservado em reais (previsto ou histórico) |
| agendado | Quantidade agendada pelo transportador em toneladas |
| valor_agendado | Valor agendado em reais |
| realizado | Quantidade faturada/carregada em toneladas |
| valor_realizado | Valor faturado/carregado em reais |

---

⚙️ **Regras e interpretação dos dados**

**Valor reservado (valor_reservado)**  
- Se a data for **igual ou posterior ao dia atual**, representa o **valor previsto de faturamento** em reais.  
- Se a data for **anterior ao dia atual**, representa o que se esperava faturar, mas que pode ou não ter sido realizado.  

**Volume reservado (reservado)**  
- Indica a quantidade **solicitada para carregar ou faturar**, em toneladas.  
- Apenas quando o volume é efetivamente faturado ou carregado, ele passa a compor os registros de **realizado**.  

**Dias passados**  
- Representam volumes ou valores que **já deveriam ter sido executados**, oferecendo um **histórico da operação**.  

**Previsão de faturamento**  
- Calculada somando os valores de **valor_reservado** dentro do período definido pelo usuário ou, por padrão, **do dia atual até o final do mês**.  

**Agrupamento por mercado**  
- Sempre que houver múltiplos mercados (Interno e Externo), os dados devem ser apresentados **separados**, para refletir corretamente a operação comercial.  

**Filtragem padrão**  
- Quando não especificado, considera **todas as filiais/unidades** e **todos os produtos**.  
- **Status financeiro** deve ser considerado conforme o tipo de consulta:  
  - Para **previsão**: apenas registros/rotinas que **geram financeiro**.  
  - Para **histórico**: pode trazer ambos os tipos, se solicitado.  
- **Cálculo de capacidade**: deve ser usado **média agrupado por filial** (cada linha mostra a capacidade total da filial, por isso use média).  
- **Para cálculo de saldo a reservar**:  
  → **MÉDIA(CAPACIDADE)** (agrupado por filial) **– SOMA(RESERVADO)** (agrupado por filial).  

---

🧩 **Exemplos de instruções para o Sub-agent**

- Retornar a **previsão de faturamento (R$)** e **volume (ton)** deste mês, agrupando por mercado.  
- Retornar o **saldo a reservar por filial**, filtrando apenas unidades **Sinop** e **Nova Mutum**.  
- Retornar o **reservado de hoje**, agrupando por cliente.  

---

🧮 **Exemplos de interpretação de perguntas do usuário**

| Pergunta do usuário | Instrução esperada para o Sub-agent |
|----------------------|-------------------------------------|
| “Qual a previsão de faturamento de ddgs para este mês?” | Retornar valor_reservado e reservado de DDGS, do dia atual até o último dia do mês, filtrando status_financeiro = 'GERA FINANCEIRO', agrupando por mercado. |
| “Qual o volume agendado DDGS em Sinop hoje?” | Retornar agendado e valor_agendado de ddgs na filial Sinop, agrupando por mercado. |
| “Capacidade de carregamento?” | Retornar capacidade de carregamento de ddgs agrupando por filial. |

---

📌 **Observações importantes**
- Sempre reportar os **filtros aplicados**: período, filial/unidade, status_financeiro, mercado, etc.  
- Sempre expressar **volume em toneladas** e **valor em reais**.  

---

🧠 **Contexto adicional para Rafinha**  
Quando identificar que a pergunta do usuário trata do **Portal Agendamento de DDGS**, utilize este documento como base para montar o prompt de instrução para o **Sub-agent**.  
Sempre descreva claramente:

- Unidade / filial / usina (ou todas, se não especificado)  
- Período (dia, mês ou intervalo)  
- Métrica (reservado/agendado/realizado e valores correspondentes)  
- Agrupamento (colunas teóricas descritas acima)  

💬 **Exemplo final de prompt para o Sub-agent:**  
> “Retornar o valor reservado e quantidade reservada de hoje até final do mês de DDGS por filial”
