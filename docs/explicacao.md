## 📘 Pacote de Medidas (DAX)

Para facilitar a manutenção, todas as medidas foram organizadas em um pacote:

* IndiceDinamico → cria um índice para ordenação dos registros considerando a tabela filtrada

* PaginaSelecionada → retorna a página atual escolhida no slicer

* TamanhoPagina → retorna o valor selecionado no slicer de quantidade de registros (ShowBy)

* TotalPaginas → calcula o número total de páginas com base nos filtros e no tamanho da página

* PaginaValida → garante que a página selecionada nunca ultrapasse o total de páginas existentes

* PageStart → retorna o índice inicial para exibição da página

* PageEnd → retorna o índice final para exibição da página

* MostrarNaPagina → medida booleana usada como filtro visual, define se o registro deve ser exibido na página

Cada medida está documentada dentro da pasta `dax/`.


# Explicação detalhada da paginação no Power BI

## 1. Objetivo

A paginação no Power BI não é nativa, mas pode ser simulada utilizando tabelas auxiliares e medidas DAX.
O objetivo é controlar a quantidade de registros exibidos em uma tabela visual e navegar entre páginas sem quebrar os filtros aplicados.

## 2. Tabelas auxiliares necessárias

**ShowBy**
Tabela estática para definir quantos registros mostrar por página:
```
ShowBy = DATATABLE("Qtd", INTEGER, { {10}, {20}, {50}, {100} })
```

**Paginacao**
Tabela para exibir as páginas disponíveis (quantidade de páginas calculada dinamicamente por medida):
```
Paginacao = GENERATESERIES(1, 20, 1) 
```
O número 20 aqui é apenas um limite inicial. O valor real de páginas válidas será controlado pela medida **TotalPaginas**.

## 3. Medidas criadas (em ordem lógica)

#### 3.1 IndiceDinamico

Cria um identificador para ordenar os registros.
Se você já tem uma coluna de índice no Power Query, pode utilizá-la diretamente. Caso contrário, pode usar RANKX sobre a coluna de data ou outra de ordenação.
```
IndiceDinamico =
RANKX(
    ALLSELECTED(Tabela),
    MAX(Tabela[Indice]),
    ,
    ASC,
    DENSE
)
```

#### 3.2 PaginaSelecionada

Retorna o número da página escolhida no slicer de paginação.
```
PaginaSelecionada = SELECTEDVALUE(Paginacao[Value], 1)
```

#### 3.3 TamanhoPagina

Retorna quantos registros devem ser exibidos, de acordo com o slicer ShowBy.
```
TamanhoPagina = SELECTEDVALUE(ShowBy[Qtd], 10)
```

#### 3.4 StartIndex

Primeiro índice exibido na página.
```
PageStart = 
(( [PaginaSelecionada] - 1 ) * [TamanhoPagina] ) + 1
```

#### 3.5 EndIndex

Último índice exibido na página.
```
PageEnd = 
[PaginaSelecionada] * [TamanhoPagina]
```

#### 3.6 Mostrar na Página

Filtra os registros que devem aparecer na tabela.
```
MostrarNaPagina =
VAR _indice = [IndiceDinamico]
RETURN
IF(
    _indice >= [PageStart] &&
    _indice <= [PageEnd],
    1,
    0
)
```

Use esta medida como filtro no visual, mantendo apenas quando o resultado for igual a 1.

#### 3.7 Total de Páginas

Número máximo de páginas válidas, baseado na quantidade de registros.
```
TotalPaginas =
VAR _QtdRegistros = COUNTROWS(ALLSELECTED(Tabela))
VAR _TamPagina = [TamanhoPagina]
RETURN
CEILING( DIVIDE(_QtdRegistros, _TamPagina), 1 )
```

#### 3.8 Página Válida

Garante que o usuário não selecione uma página maior que o total disponível.
```
PaginaValida =
IF(
    [PaginaSelecionada] <= [TotalPaginas],
    [PaginaSelecionada],
    [TotalPaginas]
)
```

## 4. Ordem de implementação

1. Criar as tabelas auxiliares (ShowBy e Paginação).

2. Criar as medidas básicas: IndiceDinamico, PaginaSelecionada e TamanhoPagina.

3. Criar PageStart e PageEnd para definir os limites de cada página.

4. Criar a medida MostrarNaPagina e aplicá-la como filtro no visual da tabela.

5. Criar TotalPaginas e PaginaValida para controlar a quantidade real de páginas exibidas e evitar páginas vazias.

6. Configurar os slicers:

* Slicer ShowBy para selecionar registros por página.

* Slicer Paginação para navegar entre páginas.

7. Testar com diferentes filtros (ex: status Paid/Unpaid) para validar que a lógica mantém os dados consistentes.

## 5. Considerações finais

O **IndiceDinamico** deve ser baseado em uma coluna ordenável (ex: data, número de protocolo, índice no Power Query).

Evite utilizar medidas instáveis como base para o índice.

Sempre teste a lógica com filtros de segmentação, para garantir que a paginação se ajuste dinamicamente.
