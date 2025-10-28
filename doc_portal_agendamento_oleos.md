📘 Portal Agendamento de Oleos — Workflow

🧭 Contexto
Os dados tratados neste workflow são provenientes do **Portal Logística Comercial da Inpasa Brasil**, que gerencia o ciclo de carregamento e faturamento de óleos.  
O sistema organiza e acompanha todas as etapas desde a **reserva de volumes**, passando pelo **agendamento de transporte**, até a **realização/faturamento efetivo**, permitindo ao usuário ter uma visão completa do planejamento e execução comercial.

Abrange os produtos:
- 🔴 Óleo Bruto (cod_produto: 4,150)
- 🟠 Óleo Semi-Refinado (cod_produto: 47,56,87)
- 🟡 Ácido Graxo (cod_produto: 48,57,88)
- 🔵 Óleo Fúsel (cod_produto: 60)

🧱 Colunas disponíveis
O Sub-agent SQL possui acesso aos seguintes campos:

Campo | Descrição
--- | ---
data | Data de referência
cod_filial | Código da filial/usina/unidade
filial | Descrição da filial/usina/unidade
nr_pedido | Número do pedido de venda
cod_produto | Código do produto
descricao_produto | Descrição do produto
cod_cliente | Código do cliente
desc_cliente | Nome do cliente/fornecedor
status_financeiro | 'GERA FINANCEIRO' ou 'NÃO GERA FINANCEIRO'
tipo_frete | CIF ou FOB
mercado | 'Mercado Interno' ou 'Mercado Externo'
cidade | Cidade do cliente/fornecedor
estado | UF do cliente/fornecedor
quantidade_pedido | Quantidade total do pedido em toneladas
saldo_pedido | Quantidade pendente em toneladas
capacidade | Capacidade de reserva em toneladas
reservado | Quantidade reservada em toneladas
valor_reservado | Valor reservado em reais (previsto ou histórico)
agendado | Quantidade agendada pelo transportador em toneladas
valor_agendado | Valor agendado em reais

⚙️ Regras e interpretação dos dados
- **Valor reservado (valor_reservado)**  
  - Se a data for igual ou posterior ao dia atual, representa o valor **previsto** de faturamento em reais.  
  - Se a data for anterior ao dia atual, representa o que se **esperava faturar**, mas que pode ou não ter sido reservado.
- **Volume reservado (reservado)**  
  - Indica a quantidade solicitada para carregar ou faturar, em toneladas.  
  - Apenas quando o volume é efetivamente faturado ou carregado, ele passa a compor os registros de reservado.
- **Dias passados**  
  - Representam volumes ou valores que já deveriam ter sido executados, oferecendo um histórico da operação.
- **Previsão de faturamento**  
  - Calculada somando os valores de `valor_reservado` dentro do período definido pelo usuário ou, por padrão, do dia atual até o final do mês.
- **Agrupamento por mercado**  
  - Sempre que houver múltiplos mercados (Interno e Externo), os dados devem ser apresentados separados, para refletir corretamente a operação comercial.
- **Filtragem padrão**  
  - Quando não especificado, considera todas as filiais/unidades e todos os produtos.  
  - Status financeiro deve ser considerado conforme o tipo de consulta:  
    - Para previsão: apenas registros/rotinas que **geram financeiro**.  
    - Para histórico: pode trazer ambos os tipos, se solicitado.

🧩 Exemplos de instruções para o Sub-agent
- Retornar a previsão de faturamento (R$) e volume (ton) deste mês, agrupando por mercado.
- Retornar o volume agendado por filial para Óleo Semi-Refinado, filtrando apenas unidades Sinop e Nova Mutum.
- Retornar o reservado em Ton de Ácido Graxo em Dourados na última semana, agrupando por cliente.

🧮 Exemplos de interpretação de perguntas do usuário

Pergunta do usuário | Instrução esperada para o Sub-agent
--- | ---
“Qual a previsão de faturamento de óleo bruto para este mês?” | Retornar valor_reservado e reservado de Óleo Bruto, do dia atual até o último dia do mês, filtrando status_financeiro = 'GERA FINANCEIRO', agrupando por mercado.
“Qual o volume agendado de todos os óleos em Sinop?” | Retornar agendado e valor_agendado de todos os produtos na filial Sinop, agrupando por produto e mercado.
“Faturamento previsto de ácido graxo em Dourados” | Retornar reservado e valor_reservado de Ácido Graxo na unidade de Dourados, agrupando por cliente e mercado.

📌 Observações importantes
- Sempre reportar filtros aplicados: período, filial/unidade, produto, status_financeiro, mercado.
- Sempre expressar volume em toneladas e valor em reais.

🧠 Contexto adicional para Rafinha
Quando identificar que a pergunta do usuário trata do Portal Agendamento de Óleos, utilize este documento como base para montar o prompt de instrução para o Sub-agent.  
Sempre descreva claramente:
- produto (ou todos, se não especificado)
- unidade / filial / usina (ou todas, se não especificado)
- período (dia, mês ou intervalo)
- métrica (reservado/agendado/reservado e valores correspondentes)
- agrupamento (colunas teoricas descritas acima)

Exemplo final de prompt para o Sub-agent:
> “Retornar o valor reservado e quantidade prevista de Óleo Bruto na unidade de Dourados durante o mês atual, agrupando por mercado.”
