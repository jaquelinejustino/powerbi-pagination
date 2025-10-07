# Power BI - Paginação em Tabelas

Este projeto demonstra como implementar **paginação dinâmica** em tabelas no Power BI, permitindo controlar a quantidade de linhas exibidas (ex: 10, 20, 50, 100) e navegar entre páginas (1/2/3...), semelhante ao comportamento de sistemas web.

---

## 🎯 Objetivo
- Criar uma tabela com paginação customizada no Power BI.  
- Simular botões de **"Show X rows"** e **navegação por páginas**.  
- Garantir que a quantidade de páginas se ajuste dinamicamente conforme a quantidade de registros.  
- Integrar a paginação com filtros adicionais como *Paid*, *Unpaid* e *Recent Request*.  

---

## ⚙️ Estrutura do projeto
powerbi-pagination
│
├── README.md # Explicação geral do projeto
├── docs/
│ ├── imagens/ # Prints do dashboard e filtros
│ └── explicacao.md # Documentação detalhada (DAX/M)
├── dax/
│ ├── IndiceDinamico.dax
│ ├── TotalPaginas.dax
│ ├── PageStart.dax
│ ├── PageEnd.dax
│ └── TabelaFiltrada.dax
└── powerquery/
└── Calendar.m # Código M ou scripts auxiliares

---

## 🛠️ Como funciona

### 1. Criar tabelas auxiliares
```DAX
ShowBy = DATATABLE("Qtd", INTEGER, { {10}, {20}, {50}, {100} })

Paginacao = GENERATESERIES(1, 20, 1)  -- até 20 páginas (ajustado dinamicamente depois)
```
2. Criar medidas principais

## 🛠️ Medidas DAX

- [IndiceDinamico](dax/IndiceDinamico.dax)
- [TotalPaginas](dax/TotalPaginas.dax)
- [PageStart](dax/PageStart.dax)
- [PageEnd](dax/PageEnd.dax)
- [TabelaFiltrada](dax/TabelaFiltrada.dax)

## ⚙️ Power Query (M)

- [Calendar.m](powerquery/Calendar.m)

---
📊 Exemplo

![Exemplo de Tabela Paginada](imagens/exemplo.png)

No exemplo acima:

O usuário escolheu Show 10 (10 linhas por página).

Página atual: 1/4.

A tabela mostra os registros correspondentes à página 1, respeitando os filtros ativos (Paid, Unpaid, etc.).

---
🚀 Como usar

1. Adicione as tabelas ShowBy e Paginacao no modelo.

2. Crie as medidas conforme descrito.

3. Insira dois slicers no relatório:

- ShowBy → quantidade de linhas por página.

- Paginacao → seleção da página.

4. Use a medida TabelaFiltrada para exibir os dados filtrados no visual de tabela.

---

📌 Observações

- O número de páginas é calculado dinamicamente (evita páginas vazias).

- Compatível com filtros adicionais (Paid, Unpaid, Recent Request, etc.).

- O índice pode ser gerado tanto no Power Query (coluna Index) quanto via DAX (RANKX), conforme necessidade.
