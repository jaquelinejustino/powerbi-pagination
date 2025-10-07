# Power BI - Paginação em Tabelas
  Implementação de paginação dinâmica em tabelas no Power BI utilizando DAX, parâmetros e tabelas auxiliares.

1. Objetivo
  Este projeto mostra como criar paginação em tabelas do Power BI, simulando controles de "show X rows" e navegação de páginas (1/2/3...), semelhante a sistemas web.

2. Funcionalidades

  • Escolher quantidade de linhas por página (ShowBy)
  
  • Paginação dinâmica (Paginacao)
  
  • Cálculo automático do número de páginas (TotalPaginas)
  
  • Compatível com filtros adicionais (Paid, Unpaid, Recent Request, etc.)

3. Exemplo

(docs/imagens/exemplo.png)

4. Como usar
  1. Crie as tabelas auxiliares:
   - ShowBy = DATATABLE("Qtd",{ {10},{20},{50},{100} }) 
   - Paginacao = GENERATESERIES(1,20,1)

  2. Crie as medidas DAX:
   - IndiceDinamico
   - TotalPaginas
   - PageStart
   - PageEnd
   - TabelaFiltrada

  3. Adicione slicers para ShowBy e Paginacao.
  4. Use a TabelaFiltrada como base da visualização.

