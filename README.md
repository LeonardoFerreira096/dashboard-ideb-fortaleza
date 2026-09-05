# 📊 Desempenho Educacional — Rede Municipal de Fortaleza

Dashboard em Power BI que investiga por que a maior rede municipal de ensino do Ceará apresenta desempenho no Ideb abaixo de municípios muito menores do mesmo estado e o que isso revela sobre gestão educacional em escala.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=flat)
![Status](https://img.shields.io/badge/status-conclu%C3%ADdo-brightgreen)

---

## 🎯 Sobre o projeto

O Ceará é hoje uma das principais referências nacionais em educação mas os melhores resultados do estado no Ideb 2025 vêm de municípios pequenos do interior, não da capital. Este projeto investiga essa diferença, comparando o desempenho da rede municipal de Fortaleza com os demais 183 municípios cearenses, e mapeia a distribuição territorial da rede dentro da própria cidade.

**Pergunta de pesquisa:** se municípios pequenos do Ceará ocupam o topo do ranking estadual, o que eles têm em comum e o que uma rede de grande escala como a de Fortaleza pode aprender com isso?

## 📈 Principais achados

- Fortaleza (rede municipal): **Ideb 6,1** nos Anos Iniciais (2025) — posição **143 de 183** municípios do Ceará
- Média da rede municipal cearense: **7,19**
- Cinco municípios empataram na nota máxima (10,0): Catunda, Coreaú, Cruz, Pedra Branca e Pires Ferreira
- A rede municipal de Fortaleza tem **622 escolas**, distribuídas em **6 distritos de educação**

## 🗂️ Fontes de dados

| Fonte | Descrição |
|---|---|
| [Inep — Resultados do Ideb](https://www.gov.br/inep/pt-br/areas-de-atuacao/pesquisas-estatisticas-e-indicadores/ideb/resultados) | Ideb por município, Anos Iniciais e Anos Finais, edição 2025 |
| [Dados Abertos Fortaleza — Parque Escolar](https://dados.fortaleza.ce.gov.br/) | Localização e distrito de cada escola municipal |

## 🛠️ Tecnologias e técnicas utilizadas

- **Power Query (linguagem M)**: limpeza de cabeçalhos multilinha, correção de encoding, unpivot de colunas por ano, tratamento de valores ausentes
- **Modelagem de dados**: esquema estrela (tabela fato + dimensões)
- **DAX**: `CALCULATE`, `RANKX`, `SWITCH`, variáveis, inteligência de tempo (variação entre edições)
- **Figma**: design do plano de fundo do dashboard, exportado em SVG
- **Formatação condicional**: destaque visual de Fortaleza no ranking estadual

## 🧮 Medidas DAX (resumo)

```DAX
IDEB Fortaleza =
CALCULATE(
    SUM(Fato_Ideb_Municipios[Nota_Ideb]),
    Fato_Ideb_Municipios[Nome_Municipio] = "Fortaleza",
    Fato_Ideb_Municipios[Ano_Ideb] = MAX(Fato_Ideb_Municipios[Ano_Ideb])
)

Ranking Ideb =
RANKX(
    ALL(Fato_Ideb_Municipios[Nome_Municipio]),
    CALCULATE(SUM(Fato_Ideb_Municipios[Nota_Ideb])),
    ,
    DESC,
    Dense
)

Classificação Fortaleza =
SWITCH(
    TRUE(),
    [IDEB Fortaleza] >= 7, "Alto desempenho",
    [IDEB Fortaleza] >= 6, "Desempenho médio",
    "Abaixo da média estadual"
)
```

## 🖼️ Prints do dashboard

> *(adicione aqui 2-3 imagens do seu dashboard finalizado, salvas na pasta `/imagens` do repositório)*

```markdown
![Visão geral do dashboard](imagens/dashboard_geral.png)
![Ranking de municípios](imagens/ranking.png)
```

## ⚠️ Nota metodológica

O Ideb combina a nota do Saeb (prova externa, aplicada fora da escola) com a taxa de aprovação (declarada pela própria escola no Censo Escolar). Isso reduz, mas não elimina, o risco de distorção por aprovação automática. Os dados públicos usados aqui não incluem infraestrutura, merenda escolar ou salário docente — o projeto aponta *onde* olhar com atenção, não afirma a causa exata dos resultados.

## 💡 Conclusão

Municípios pequenos não têm sucesso por serem pequenos — eles têm sucesso porque conseguem proximidade entre gestão e sala de aula. O desafio de Fortaleza não é ser pequena, é simular essa proximidade em escala — e a divisão por distritos de educação é uma ferramenta possível para isso.

## 📁 Estrutura do repositório

```
├── README.md
├── dashboard_ideb_fortaleza.pbix
├── imagens/
│   ├── dashboard_geral.png
│   └── ranking.png
└── escopo_projeto.md
```

## 👤 Autor

Projeto final do módulo de Power BI — Leonardo Costa Ferreira
