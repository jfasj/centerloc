# SPRINT 4 — Contratos de Locação

> **Status:** ✅ Concluída — 2026-09  
> **Telas entregues:** T11, T12, T13

---

## Objetivo da Sprint

Implementar o módulo de Contratos de Locação: listagem, novo contrato com itens de frota, e visualização formatada para impressão A4.

**Telas desta sprint:**
- [x] T11 — Lista Contratos (`#sec-contratos`)
- [x] T12 — Novo/Editar Contrato (`#sec-novo-contrato`)
- [x] T13 — Contrato Impressão A4 (`#sec-contrato-print`)

---

## Referências dos Mockups

- `stitch/contratos_de_locação_centerloc/code.html` + `screen.png`
- `stitch/novo_contrato_de_locação_centerloc/code.html` + `screen.png`
- `stitch/contrato_de_locação_impressão_a4_centerloc/code.html` + `screen.png`

---

## O que foi implementado

### T11 — Lista Contratos (`#sec-contratos`)

**KPI Cards (linha de 4):**

| KPI | Descrição | Cor |
|-----|-----------|-----|
| Contratos Ativos | Total em vigor | `#1A3A6E` |
| Valor Mensal Total | Soma dos ativos | `#E87722` |
| Vencendo em 30 dias | Alerta | `#F59E0B` |
| Encerrados (mês) | Histórico | `#64748B` |

**Filtros:**
- Status: Todos | Ativo | Encerrado | Suspenso
- Período: mês/ano
- Busca: por número ou locatário

**Tabela de Contratos:**

| Coluna | Tipo |
|--------|------|
| Nº Contrato | código (ex: CT-0001) |
| Locatário | nome + CNPJ pequeno |
| Equipamentos | nomes resumidos |
| Início | data |
| Fim | data (badge vermelho se vencido) |
| Valor/Mês | R$ |
| Status | pill |
| Ações | Ver \| Imprimir \| Encerrar |

---

### T12 — Formulário Novo/Editar Contrato (`#sec-novo-contrato`)

Página full (não modal):

**Seção 1 — Identificação:**
- Nº do Contrato (gerado: CT-XXXX)
- Locatário * → dropdown com busca em `db.locatarios` ativos
- Data de Início *, Data de Fim *
- Local de Execução

**Seção 2 — Itens de Frota:**
- Tabela de itens com botão "+ Adicionar Equipamento"
- Cada item: Equipamento (dropdown `db.frota`), Quantidade, Período (meses), Valor Unit., Total calculado
- Rodapé: Subtotal | Mobilização/Desmobilização | Total do Contrato

**Seção 3 — Condições Comerciais:**
- Prazo de Pagamento
- Forma de Pagamento: Boleto | PIX | TED
- Reajuste (% anual)
- Multa por Rescisão Antecipada (%)

**Seção 4 — Observações e Cláusulas:**
- Textarea livre
- Checkbox "Inclui cláusula de exclusividade"
- Checkbox "Inclui seguro contra danos"

**Rodapé de ações:**
- Botão "Cancelar" → volta para lista
- Botão "Salvar Rascunho"
- Botão "Ativar Contrato" (laranja) → muda status da frota para `alugado`

---

### T13 — View Impressão A4 (`#sec-contrato-print`)

**Cabeçalho:**
- Logo CENTERLOC
- Dados da empresa (razão, CNPJ, endereço, contato)
- Título: "CONTRATO DE LOCAÇÃO DE EQUIPAMENTOS"
- Nº do Contrato + Data de Emissão

**Identificação das Partes:**
- LOCADORA: dados da CENTERLOC
- LOCATÁRIO: razão social, CNPJ, endereço

**Objeto do Contrato:**
- Tabela de equipamentos: Descrição | Qtde | Meses | Valor Unit. | Total

**Valores:**
- Subtotal, Mobilização/Desmobilização
- Valor Total por extenso via `numExtenso()`

**Condições:**
- Prazo e forma de pagamento, reajuste, multa, observações

**Assinatura:**
- Dois blocos: LOCADORA e LOCATÁRIO
- Cidade e data por extenso via `gerarDataExtenso()`

**Print CSS:** `@media print { #sidebar, #topbar, .no-print { display:none; } }`

---

## Funções criadas

| Função | Descrição |
|--------|-----------|
| `renderContratos()` | Renderiza tabela com KPIs e filtros |
| `filtrarContratos(query, status)` | Filtra em tempo real |
| `calcKPIsContratos()` | Calcula 4 KPIs do db.contratos |
| `openNovoContrato(id)` | Abre form (null=novo, id=editar) |
| `renderFormContrato(id)` | Preenche form para edição |
| `adicionarItemContrato()` | Adiciona linha de equipamento na tabela |
| `removeItemContrato(idx)` | Remove linha da tabela |
| `calcTotalContrato()` | Recalcula total ao alterar itens |
| `saveContrato(status)` | Salva com status `rascunho` ou `ativo` |
| `encerrarContrato(id)` | Muda status + libera equipamentos (`disponivel`) |
| `viewContrato(id)` | Abre seção de impressão A4 |
| `renderContratoA4(id)` | Preenche HTML do contrato formatado |
| `gerarDataExtenso(d)` | ISO → "Recife, 03 de setembro de 2026" |

---

## Dados inicializados

```javascript
db.contratos = [
  { id:"CT0001", numero:"CT-0001", locatarioId:"L0001",
    itens:[
      { frotaId:"F0001", descricao:"Retroescavadeira Case 580N",
        qtde:1, meses:3, valorUnit:9800, total:29400 }
    ],
    dataInicio:"2024-06-01", dataFim:"2024-08-31",
    valorTotal:29400, mobDesob:1500, valorFinal:30900,
    prazoPagemento:"30 dias após emissão", formaPagamento:"PIX",
    status:"encerrado", criadoEm:"2024-05-28" },
  { id:"CT0002", numero:"CT-0002", locatarioId:"L0002",
    itens:[
      { frotaId:"F0001", descricao:"Retroescavadeira Case 580N",
        qtde:1, meses:6, valorUnit:9800, total:58800 },
      { frotaId:"F0003", descricao:"Caminhão Basculante Ford Cargo",
        qtde:1, meses:6, valorUnit:5500, total:33000 }
    ],
    dataInicio:"2024-09-01", dataFim:"2025-02-28",
    valorTotal:91800, mobDesob:3000, valorFinal:94800,
    prazoPagemento:"15 dias após emissão", formaPagamento:"Boleto",
    status:"ativo", criadoEm:"2024-08-20" },
  { id:"CT0003", numero:"CT-0003", locatarioId:"L0003",
    itens:[
      { frotaId:"F0004", descricao:"Gerador 150 KVA Stemac",
        qtde:1, meses:12, valorUnit:4500, total:54000 }
    ],
    dataInicio:"2024-08-01", dataFim:"2025-07-31",
    valorTotal:54000, mobDesob:800, valorFinal:54800,
    prazoPagemento:"30 dias após emissão", formaPagamento:"TED",
    status:"ativo", criadoEm:"2024-07-25" }
];
db.nextIds.contrato = 4;
```

---

## Critérios de Aceite

- [x] Lista renderiza com KPIs calculados do db
- [x] Filtro por status e busca funcionam
- [x] Dropdown de locatário busca em `db.locatarios`
- [x] Tabela de itens permite adicionar/remover equipamentos
- [x] Total recalcula automaticamente ao alterar quantidade/meses/valor
- [x] Salvar contrato persiste no db e volta para lista
- [x] Ativar contrato muda status dos equipamentos para `alugado`
- [x] Encerrar contrato libera equipamentos (status → `disponivel`)
- [x] View A4 renderiza com todos os dados do contrato
- [x] `numExtenso()` exibe valor por extenso no contrato
- [x] Print CSS oculta sidebar e topbar ao imprimir

---

## Commit

```
git commit -m "sprint-4: contratos de locação"
```
