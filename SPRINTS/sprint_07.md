# SPRINT 7 — Relatórios + DRE + Ajustes Finais

> **Status:** ✅ Concluída — 2026-09  
> **Telas entregues:** T25, T26, T27  
> **Sistema CENTERLOC v1.0 — Completo**

---

## Objetivo da Sprint

Sprint final: implementar a Central de Relatórios e BI, o DRE de Rentabilidade por Ativo, o Relatório Gerencial em A4, e aplicar ajustes finais em todo o sistema.

**Telas desta sprint:**
- [x] T25 — DRE / Rentabilidade por Ativo (`#sec-dre`)
- [x] T26 — Relatórios Central de BI (`#sec-relatorios`)
- [x] T27 — Relatório Gerencial Impressão A4 (`#sec-relatorio-print`)

---

## Referências dos Mockups

- `stitch/dre_rentabilidade_por_ativo_centerloc/code.html` + `screen.png`
- `stitch/relatórios_central_de_bi_centerloc/code.html` + `screen.png`
- `stitch/relatório_gerencial_impressão_a4_centerloc/code.html` + `screen.png`

---

## O que foi implementado

### T25 — DRE / Rentabilidade por Ativo (`#sec-dre`)

**Seletor de Período:** ano com navegação ← →

**KPIs Anuais (linha de 4):**

| KPI | Cálculo |
|-----|---------|
| Receita Total | Soma de `db.contasReceber` pagas no ano |
| Despesas Totais | Soma de `db.contasPagar` pagas no ano |
| Lucro Bruto | Receita − Despesas |
| Margem Bruta | (Lucro / Receita) × 100% |

**Gráfico DRE Mensal (Canvas API):**
- 12 colunas = 12 meses
- Barras: Receita (navy `#1A3A6E`) vs Despesa (vermelho `#DC2626`)
- Linha: Lucro acumulado (laranja `#E87722`)
- `window.devicePixelRatio` para renderização nítida

**Tabela Rentabilidade por Ativo:**

| Coluna | Cálculo |
|--------|---------|
| Equipamento | nome + patrimônio |
| Meses Locado | total de meses em contratos no período |
| Receita Gerada | soma de `db.contasReceber` vinculadas ao ativo via cadeia CR→nota→contrato→frota |
| Custo Manutenção | soma de `db.contasPagar` da categoria "Manutenção" com referência ao patrimônio |
| Resultado Líquido | Receita − Custo |
| Rentabilidade % | Resultado / Custo × 100 |

- Ordenado por Receita Gerada (maior primeiro)
- Linha verde se resultado positivo, vermelha se negativo

**Gráfico de Pizza/Donut (Canvas API):**
- Distribuição de receita por equipamento (top 5 + "Outros")
- Legenda lateral com cores e percentuais

---

### T26 — Central de Relatórios BI (`#sec-relatorios`)

**Filtros de período:**
- Mês corrente | Trimestre | Semestre | Ano | Personalizado

**Cards de Relatórios Disponíveis:**

| Relatório | Descrição | Ação |
|-----------|-----------|------|
| Receita por Período | Tabela mensal de entradas | Gerar + Exportar CSV |
| Contratos por Locatário | Histórico por cliente | Gerar + Exportar CSV |
| Equipamentos por Status | Visão atual da frota | Gerar + Exportar CSV |
| Inadimplência | Contas vencidas não pagas | Gerar + Exportar CSV |
| Rentabilidade por Ativo | Link para seção DRE | Ir para DRE |
| Fluxo de Caixa Mensal | Link para seção fluxo | Ir para Fluxo |

**Preview de tabela:** resultado exibido na própria seção ao clicar em "Gerar"

**Exportação CSV:** via `Blob + URL.createObjectURL` — download direto sem servidor

---

### T27 — Relatório Gerencial A4 (`#sec-relatorio-print`)

**Cabeçalho:**
- Logo + dados CENTERLOC
- Título: "RELATÓRIO GERENCIAL — [MÊS/ANO]"
- Data de emissão

**Resumo Executivo — 4 KPI boxes:**
- Receita do Período | Despesas | Lucro | Contratos Ativos

**Seção 1 — Receitas do Período:**
- Tabela: Locatário | Notas | Valor Recebido

**Seção 2 — Contratos Ativos:**
- Tabela: Nº | Locatário | Equipamentos | Valor | Vencimento

**Seção 3 — Frota:**
- Tabela: Patrimônio | Equipamento | Status | Locatário Atual

**Seção 4 — Financeiro Resumido:**
- Receitas totais vs Despesas totais vs Saldo

**Rodapé:**
- "Documento gerado em [data] pelo sistema CENTERLOC v1.0"

**Print CSS:** `@media print { #sidebar, #topbar, .no-print { display:none; } }`

---

## Ajustes finais aplicados

### Dashboard
- [x] KPIs calculados com dados reais do `db`
- [x] Gráfico de 6 meses usa dados reais de `db.contasReceber` pagas
- [x] Tabela de contratos recentes busca de `db.contratos`

### Navegação
- [x] `goTo()` atualizado com dispatch para `dre` e `relatorios`
- [x] Todos os botões "Voltar" retornam corretamente

### Integrações confirmadas
- [x] Proposta aprovada → pode virar Contrato (`converterEmContrato`)
- [x] Contrato ativado → muda status de frota para `alugado`
- [x] Contrato encerrado → libera frota para `disponivel`
- [x] Nota marcada paga → cria entrada em `db.contasReceber`
- [x] `db.contasReceber` aparece no Fluxo de Caixa
- [x] Detalhe do equipamento mostra contratos relacionados

---

## Funções criadas

| Função | Descrição |
|--------|-----------|
| `renderDRE()` | KPIs anuais + gráfico Canvas + tabela rentabilidade + pizza |
| `calcDREMensal(ano)` | Retorna array 12 meses `[{mes, receita, despesa, lucro}]` |
| `calcRentabilidadePorAtivo(ano)` | Cadeia CR→nota→contrato→frota; retorna `[{frota, receita, custo, resultado, pct}]` |
| `renderDREChart(mensal)` | Canvas: barras navy/vermelho + linha laranja de lucro acumulado |
| `renderPizzaChart(ativos)` | Canvas: donut top-5 ativos por receita + "Outros" + legenda |
| `navDRE(dir)` | Navega ano anterior (−1) ou próximo (+1) |
| `renderRelatorios()` | Seção Central de BI com cards e filtros de período |
| `getPeriodoFiltro()` | Retorna `{de, ate}` conforme filtro selecionado |
| `gerarRelatorioReceitas()` | Tabela receitas por período |
| `gerarRelatorioContratos()` | Tabela contratos por locatário |
| `gerarRelatorioFrota()` | Visão atual da frota |
| `gerarRelatorioInadimplencia()` | Contas vencidas não pagas |
| `exportarCSV(cab, linhas, nome)` | Blob download via `URL.createObjectURL` |
| `openRelatorioGerencial()` | Navega para `#sec-relatorio-print` |
| `renderRelatorioGerencialA4()` | Preenche HTML completo: header, KPIs, 4 seções, rodapé |

---

## Critérios de Aceite

- [x] KPIs anuais do DRE calculados do db
- [x] Gráfico Canvas com 12 meses funcionando
- [x] Tabela de rentabilidade por ativo com dados reais
- [x] Gráfico de pizza mostrando top 5 equipamentos
- [x] Cards de relatórios navegam para as seções corretas
- [x] Exportar CSV gera arquivo `.csv` real para download
- [x] Relatório Gerencial A4 preenche com dados do db
- [x] Print CSS funciona no relatório A4
- [x] Dashboard usa dados reais do db (não hardcoded)
- [x] Nenhum `console.error` visível no uso normal
- [x] Dados persistem após F5 em todos os módulos
- [x] Sistema começa do login ao recarregar (sem sessão)
- [x] JS syntax check: ✅ sem erros

---

## Commit

```
git commit -m "sprint-7: relatórios, DRE e ajustes finais — sistema v1.0 completo"
```

---

## 🎉 Sistema CENTERLOC v1.0 — Completo

**27 telas** | **7 sprints** | **Vanilla HTML/CSS/JS** | **localStorage**

Empresa: CENTERLOC LOCADORA DE VEÍCULOS LTDA — Recife, PE  
CNPJ: 12.299.005/0001-46  
Concluído em: 2026-09-03
