#📘 Pacote de Medidas (DAX)

Para facilitar a manutenção, todas as medidas foram organizadas em um pacote:

IndiceDinamico → cria um índice para ordenação dos registros considerando a tabela filtrada

PaginaSelecionada → retorna a página atual escolhida no slicer

TamanhoPagina → retorna o valor selecionado no slicer de quantidade de registros (ShowBy)

TotalPaginas → calcula o número total de páginas com base nos filtros e no tamanho da página

PaginaValida → garante que a página selecionada nunca ultrapasse o total de páginas existentes

PageStart → retorna o índice inicial para exibição da página

PageEnd → retorna o índice final para exibição da página

MostrarNaPagina → medida booleana usada como filtro visual, define se o registro deve ser exibido na página

Cada medida está documentada dentro da pasta `dax/`.
