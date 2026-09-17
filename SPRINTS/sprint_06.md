# SPRINT 6 — Financeiro: Contas a Receber + Contas a Pagar + Fluxo de Caixa

> **Status:** ✅ Concluída — 2026-09  
> **Telas entregues:** T20, T21, T22, T23, T24

---

## Objetivo da Sprint

Implementar o módulo financeiro completo: gestão de Contas a Receber (integrada com Notas de Débito), Contas a Pagar (lançamentos manuais) e Fluxo de Caixa (visão temporal consolidada).

**Telas desta sprint:**
- [x] T20 — Contas a Receber (`#sec-contas-receber`)
- [x] T21 — Baixa de Pagamento (`#modal-baixa-receber`)
- [x] T22 — Contas a Pagar (`#sec-contas-pagar`)
- [x] T23 — Novo Lançamento a Pagar (`#modal-nova-pagar` + `#modal-pagar-conta` baixa rápida)
- [x] T24 — Fluxo de Caixa (`#sec-fluxo-caixa`)

---

## Referências dos Mockups

- `stitch/contas_a_receber_centerloc/code.html` + `screen.png`
- `stitch/contas_a_pagar_centerloc/code.html` + `screen.png`
- `stitch/fluxo_de_caixa_centerloc/code.html` + `screen.png`

---

## O que foi implementado

### T20 — Contas a Receber (`#sec-contas-receber`)

**KPI Cards (linha de 4):**

| KPI | Cor |
|-----|-----|
| Total a Receber | `#E87722` |
| Recebido no Mês | `#16A34A` |
| Vencidas (atraso) | `#DC2626` |
| Previsão próx. 30 dias | `#1A3A6E` |

**Filtros:**
- Status: Todos | Pendente | Pago | Vencido
- Período: mês/ano selector
- Busca: por locatário ou descrição

**Tabela:**

| Coluna | Tipo |
|--------|------|
| Nº | código sequencial |
| Locatário | nome |
| Nota/Referência | link para nota |
| Vencimento | negrito vermelho se vencida |
| Valor | R$ |
| Data Pagamento | — ou data quando pago |
| Forma Pagamento | PIX / Boleto / TED / — |
| Status | pill |
| Ações | Baixar \| Ver Nota \| Excluir |

- Status "Vencido" é **derivado** em runtime: `status==='pendente' && dataVencimento < hoje`

---

### T21 — Modal Baixa de Pagamento (`#modal-baixa-receber`)

Disparado pelo botão "Baixar" na tabela:

- Data de Pagamento * (default: hoje)
- Valor Recebido * (pré-preenchido com valor original, editável)
- Forma de Pagamento *: PIX | Boleto | TED | Dinheiro | Cheque
- Observações

Ação: muda status para `pago`, salva data e forma, atualiza `db.notas` se tiver `notaId`

---

### T22 — Contas a Pagar (`#sec-contas-pagar`)

**KPI Cards (linha de 4):**

| KPI | Cor |
|-----|-----|
| Total a Pagar | `#DC2626` |
| Pago no Mês | `#64748B` |
| Vencidas | `#F59E0B` |
| Próximas (7 dias) | `#2563EB` |

**Tabela:**

| Coluna | Tipo |
|--------|------|
| Nº | código CP-XXXX |
| Fornecedor/Descrição | nome ou descrição livre |
| Categoria | Manutenção / Combustível / Pessoal / etc. |
| Vencimento | data |
| Valor | R$ |
| Status | pill |
| Ações | Pagar \| Editar \| Excluir |

**Categorias disponíveis:**
Manutenção | Combustível | Peças e Insumos | Pessoal | Administrativo | Impostos e Taxas | Transporte | Outros

---

### T23 — Modais de Lançamento a Pagar

**Modal Novo Lançamento (`#modal-nova-pagar`):**
- Fornecedor (dropdown de `db.clientes` tipo fornecedor ou campo livre)
- Descrição *, Categoria *, Valor *, Data de Vencimento *
- Observações

**Modal Baixa Rápida (`#modal-pagar-conta`):**
- Data de Pagamento * (default: hoje)
- Forma de Pagamento *: PIX | TED | Boleto | Dinheiro | Cheque
- Observações

---

### T24 — Fluxo de Caixa (`#sec-fluxo-caixa`)

**Seletor de período:** mês/ano com navegação ← →

**KPIs do período:**

| KPI | Descrição |
|-----|-----------|
| Saldo Inicial | Saldo do início do mês (0 por padrão) |
| Total Entradas | Soma de pagamentos recebidos no período |
| Total Saídas | Soma de pagamentos efetuados no período |
| Saldo Final | Inicial + Entradas − Saídas |

**Gráfico de Barras (Canvas API):**
- Barras: Entradas (verde) vs Saídas (vermelho) por semana
- Linha de saldo acumulado
- Usa `window.devicePixelRatio` para renderização nítida

**Tabela do Fluxo:**

| Coluna |
|--------|
| Data |
| Descrição |
| Tipo (Entrada/Saída) |
| Categoria |
| Valor |
| Saldo Acumulado |

- Linha verde para entradas, vermelha para saídas
- Saldo acumulado em negrito (vermelho se negativo)

**Resumo por Categoria:**
Mini-tabela lateral com total de cada categoria de despesa

---

## Funções criadas

| Função | Descrição |
|--------|-----------|
| `renderContasReceber()` | Tabela paginada com KPIs; status vencido derivado |
| `filtrarContasReceber(status, periodo, query)` | Filtra em tempo real |
| `calcKPIsReceber()` | Calcula 4 KPIs do db.contasReceber |
| `openModalBaixaReceber(id)` | Abre modal de baixa pré-preenchido |
| `salvarBaixaReceber()` | Persiste pagamento e atualiza nota de débito vinculada |
| `renderContasPagar()` | Tabela paginada com filtro de categoria e período |
| `filtrarContasPagar(status, periodo, categoria)` | Filtra em tempo real |
| `calcKPIsPagar()` | Calcula 4 KPIs do db.contasPagar |
| `openModalNovaPagar(id)` | Abre modal (null=novo, id=editar) |
| `salvarContaPagar()` | Cria ou edita lançamento em db.contasPagar |
| `confirmarPagamentoConta()` | Baixa rápida de conta a pagar |
| `deletarContaPagar(id)` | Remove com confirmação |
| `renderFluxoCaixa()` | Consolida db.contasReceber + db.contasPagar do período |
| `renderFluxoChart()` | Canvas: barras entradas×saídas por semana + linha saldo |
| `renderTabelaFluxo(data)` | Tabela de lançamentos com saldo acumulado |
| `renderResumoCategorias()` | Mini-tabela lateral com totais por categoria |
| `navFluxo(direcao)` | Navega mês anterior (−1) ou próximo (+1) |

---

## Dados inicializados

```javascript
db.contasReceber = [
  { id:"CR0001", descricao:"ND-0531 — ENGEMAN Retroescavadeira Julho/2024",
    locatarioId:"L0001", notaId:"ND0531", valor:9800,
    dataVencimento:"2024-07-31", dataPagamento:"2024-08-05",
    formaPagamento:"PIX", status:"pago", obs:"" },
  { id:"CR0002", descricao:"ND-0532 — CLC Retroescavadeira + Caminhão Set/2024",
    locatarioId:"L0002", notaId:"ND0532", valor:15300,
    dataVencimento:"2024-09-30", dataPagamento:null,
    formaPagamento:null, status:"pendente", obs:"" },
  { id:"CR0003", descricao:"CT-0003 — Gerador 150 KVA Agosto/2024",
    locatarioId:"L0003", notaId:null, valor:4500,
    dataVencimento:"2024-08-31", dataPagamento:null,
    formaPagamento:null, status:"pendente", obs:"" }
];
db.nextIds.contaReceber = 4;

db.contasPagar = [
  { id:"CP0001", descricao:"Revisão Retroescavadeira CLR-0001 — 6.000h",
    fornecedorId:"C0001", categoria:"Manutenção", valor:3200,
    dataVencimento:"2024-09-15", dataPagamento:"2024-09-15",
    formaPagamento:"TED", status:"pago", obs:"" },
  { id:"CP0002", descricao:"Combustível — Frota Agosto/2024",
    fornecedorId:null, categoria:"Combustível", valor:1850,
    dataVencimento:"2024-09-20", dataPagamento:null,
    formaPagamento:null, status:"pendente", obs:"" },
  { id:"CP0003", descricao:"INSS + FGTS — Folha Agosto/2024",
    fornecedorId:null, categoria:"Pessoal", valor:4200,
    dataVencimento:"2024-09-07", dataPagamento:null,
    formaPagamento:null, status:"vencido", obs:"Competência 08/2024" },
  { id:"CP0004", descricao:"Seguro Frota — Apólice Anual",
    fornecedorId:null, categoria:"Administrativo", valor:6800,
    dataVencimento:"2024-10-01", dataPagamento:null,
    formaPagamento:null, status:"pendente", obs:"Renovação anual" }
];
db.nextIds.contaPagar = 5;
```

---

## Critérios de Aceite

- [x] Lista de contas a receber com KPIs calculados do db
- [x] Filtros de status, período e busca funcionam
- [x] Modal de baixa salva pagamento e atualiza status da nota vinculada
- [x] "Marcar como Pago" na Sprint 5 cria entrada em db.contasReceber
- [x] Lista de contas a pagar com categorias corretas
- [x] Modal de novo lançamento salva em db.contasPagar
- [x] Fluxo de caixa consolida receber+pagar do período selecionado
- [x] KPIs do fluxo calculam corretamente (saldo inicial/final)
- [x] Gráfico Canvas mostra entradas vs saídas por período
- [x] Navegação de mês (← →) funciona no fluxo de caixa
- [x] Tabela do fluxo exibe saldo acumulado (vermelho se negativo)

---

## Commit

```
git commit -m "sprint-6: financeiro — contas e fluxo de caixa"
```
