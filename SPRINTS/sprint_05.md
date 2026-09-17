# SPRINT 5 — Propostas de Locação + Notas de Débito

> **Status:** ✅ Concluída — 2026-09  
> **Telas entregues:** T14, T15, T16, T17, T18, T19

---

## Objetivo da Sprint

Implementar dois módulos fiscais/comerciais: Propostas de Locação (documento pré-contratual) e Notas de Débito (faturamento), ambos com impressão A4 fiel ao formato real da empresa.

**Telas desta sprint:**
- [x] T14 — Lista Propostas (`#sec-propostas`)
- [x] T15 — Nova/Editar Proposta (`#sec-nova-proposta`)
- [x] T16 — Proposta Impressão A4 (`#sec-proposta-print`)
- [x] T17 — Lista Notas de Débito (`#sec-notas`)
- [x] T18 — Nova/Editar Nota de Débito (`#sec-nova-nota`)
- [x] T19 — Nota de Débito Impressão A4 (`#sec-nota-print`)

---

## Referências dos Mockups

- `stitch/propostas_de_locação_centerloc/code.html` + `screen.png`
- `stitch/nova_proposta_de_locação_centerloc/code.html` + `screen.png`
- `stitch/proposta_de_locação_impressão_a4_centerloc/code.html` + `screen.png`
- `stitch/notas_de_débito_centerloc/code.html` + `screen.png`
- `stitch/nova_nota_de_débito_centerloc/code.html` + `screen.png`
- `stitch/nota_de_débito_impressão_a4_centerloc/code.html` + `screen.png`

---

## O que foi implementado

### T14 — Lista Propostas (`#sec-propostas`)

**Tabela:**

| Coluna | Tipo |
|--------|------|
| Nº Proposta | PR-XXXX |
| Locatário | nome |
| Data Emissão | dd/mm/yyyy |
| Validade | dd/mm/yyyy (vermelho se vencida) |
| Valor Total | R$ |
| Status | pill: Pendente / Aprovada / Rejeitada |
| Ações | Ver \| Imprimir \| Converter em Contrato \| Rejeitar |

- "Converter em Contrato" → copia dados da proposta para novo contrato como rascunho

---

### T15 — Formulário Nova/Editar Proposta (`#sec-nova-proposta`)

**Identificação:**
- Nº Proposta (PR-XXXX automático via `gerarNumeroProposta()`)
- Locatário * (dropdown com busca em `db.locatarios`)
- Data de Emissão * (default: hoje)
- Validade (data)
- Referência / Objeto da locação

**Tabela de Itens:**

| Campo | Descrição |
|-------|-----------|
| Nº Ord. | sequencial |
| Detalhamento | descrição do equipamento |
| Qtde | quantidade |
| Mês (horas) | período |
| P. Unit. | valor unitário |
| MOB-DESMOB | mobilização (R$) |
| Total | Qtde × Meses × P.Unit + MOB |

**Observações padrão (checkboxes):**
- Valores sujeitos a alteração sem aviso prévio
- Proposta válida por 15 dias
- Preço inclui operador
- Preço não inclui combustível
- Manutenção por conta da CENTERLOC
- Transporte por conta do CLIENTE

**Totais:** Subtotal de locação | Total MOB/DESMOB | Valor Total

---

### T16 — Proposta Impressão A4 (`#sec-proposta-print`)

**Cabeçalho:**
- Logo CENTERLOC + dados da empresa
- Título "PROPOSTA DE LOCAÇÃO DE EQUIPAMENTOS"
- Nº da Proposta + Data

**Destinatário:**
- A/C: [Contato], [Empresa], [Cidade-UF]

**Tabela de Itens:**
- Nº Ord | Detalhamento | Qtde | Meses | P.Unit | MOB-DESMOB | Total

**Totais + Valor por extenso** via `numExtenso()`

**Observações:** lista das condições marcadas no form

**Assinatura:** CENTERLOC + local/data via `gerarDataExtenso()`

---

### T17 — Lista Notas de Débito (`#sec-notas`)

**Tabela:**

| Coluna | Tipo |
|--------|------|
| Nº Nota | ND-XXXX |
| Locatário | nome + CNPJ |
| Referência | ex: "Agosto/2024 — Contrato CT-0002" |
| Data Emissão | |
| Vencimento | vermelho se vencida |
| Valor | R$ |
| Status | pill: Aberta / Paga / Vencida |
| Ações | Ver \| Imprimir \| Marcar Pago |

- Status "Vencida" é **derivado** em runtime: `status==='aberta' && dataVencimento < hoje`
- "Marcar como Pago" → abre modal de baixa + cria entrada em `db.contasReceber`

---

### T18 — Formulário Nova/Editar Nota de Débito (`#sec-nova-nota`)

**Identificação:**
- Nº Nota (ND-XXXX automático via `gerarNumeroNota()` — série histórica, inicia em ND-0533)
- Sacado (locatário) * → dropdown com CNPJ preenchido automaticamente
- Contrato de referência (dropdown de `db.contratos` do locatário)
- Data de Emissão *, Data de Vencimento *

**Tabela de Discriminação:**
- Descrição do serviço/equipamento
- Período de referência (ex: 01/08 a 31/08/2024)
- Valor

**Totais:** Valor Total da Nota

**Base Legal:** "Conforme Lei Complementar 116/03, item 3.01 – Serviços de locação de bens móveis."

**Dados de Pagamento:**
- Chave PIX (de `db.empresa.pix`)
- Dados bancários

---

### T19 — Nota de Débito Impressão A4 (`#sec-nota-print`)

Formato exato do documento da empresa:

```
[LOGO]    CENTERLOC LOCADORA DE VEÍCULOS LTDA
          CNPJ: 12.299.005/0001-46
          Av. Portuária, 1200 — Suape — Ipojuca/PE

══════════════════════════════════════════
              NOTA DE DÉBITO Nº XXXX
══════════════════════════════════════════

SACADO: [Razão Social]
CNPJ:   [CNPJ formatado]
FATURA: [Referência do contrato]

DISCRIMINAÇÃO:
┌─────────────────────────────────┬──────────┐
│ [Descrição + Período]           │ R$ XXXXX │
└─────────────────────────────────┴──────────┘

TOTAL: R$ XX.XXX,XX
       [Valor por Extenso]

BASE LEGAL: Lei Complementar 116/03, item 3.01

PAGAMENTO VIA PIX: 12.299.005/0001-46
Banco Bradesco | Ag. 3214-5 | C/C 00123456-7

[Assinatura CENTERLOC]    [Assinatura Locatário]
```

**Print CSS:** oculta sidebar e topbar, renderiza apenas o documento

---

## Funções criadas

| Função | Descrição |
|--------|-----------|
| `gerarNumeroProposta()` | Retorna PR-XXXX automático |
| `renderPropostas()` | Tabela paginada com filtro de status |
| `openNovaProposta(id)` | Abre form (null=novo, id=editar) |
| `adicionarItemProposta()` | Adiciona linha na tabela de itens |
| `removeItemProposta(idx)` | Remove linha da tabela |
| `calcTotalProposta()` | Recalcula totais ao alterar itens |
| `saveProposta(status)` | Valida e salva no db |
| `converterEmContrato(id)` | Copia proposta para novo contrato como rascunho |
| `renderPropostaA4(id)` | Preenche A4 com itens, totais e `numExtenso()` |
| `gerarNumeroNota()` | Retorna ND-XXXX automático (série ND-0533+) |
| `renderNotas()` | Tabela paginada; status vencida derivado em runtime |
| `openNovaNota(id)` | Abre form (null=novo, id=editar) |
| `adicionarItemNota()` | Adiciona linha de discriminação |
| `calcTotalNota()` | Recalcula total ao alterar itens |
| `saveNota()` | Valida e salva no db |
| `marcarNotaPago(id)` | Abre modal de baixa; cria entrada em `db.contasReceber` |
| `renderNotaA4(id)` | A4 com discriminação, PIX, base legal LC 116/03 |

---

## Dados inicializados

```javascript
db.propostas = [
  { id:"PR0001", numero:"PR-0001", locatarioId:"L0003",
    itens:[
      { descricao:"Gerador 150 KVA", qtde:1, meses:12,
        valorUnit:4500, mobDesob:800, total:54800 }
    ],
    dataEmissao:"2024-07-15", validade:"2024-08-15",
    valorTotal:54800, status:"aprovada",
    obs:["Preço inclui operador","Manutenção por CENTERLOC"] }
];
db.nextIds.proposta = 2;

db.notas = [
  { id:"ND0531", numero:"ND-0531", locatarioId:"L0001",
    contratoId:"CT0001", dataEmissao:"2024-07-01",
    dataVencimento:"2024-07-31",
    itens:[{ descricao:"Retroescavadeira Case 580N — Julho/2024", valor:9800 }],
    valorTotal:9800, status:"paga" },
  { id:"ND0532", numero:"ND-0532", locatarioId:"L0002",
    contratoId:"CT0002", dataEmissao:"2024-09-01",
    dataVencimento:"2024-09-30",
    itens:[
      { descricao:"Retroescavadeira Case 580N — Setembro/2024", valor:9800 },
      { descricao:"Caminhão Basculante Ford Cargo — Setembro/2024", valor:5500 }
    ],
    valorTotal:15300, status:"aberta" }
];
db.nextIds.nota = 533;   // próxima será ND-0533
```

---

## Critérios de Aceite

- [x] Lista propostas com filtro de status e busca
- [x] Form nova proposta: tabela de itens com cálculo automático de totais
- [x] Observações padrão exibem como checkboxes
- [x] Proposta A4 renderiza fiel ao layout com `numExtenso()`
- [x] "Converter em Contrato" cria rascunho pré-preenchido
- [x] Lista notas com badges de status corretos (vencida em vermelho)
- [x] Form nova nota: sacado seleciona locatário e preenche CNPJ
- [x] "Marcar como Pago" cria entrada em `db.contasReceber`
- [x] Nota A4 renderiza com discriminação + valor por extenso + PIX + assinatura
- [x] Print CSS funciona para Proposta e Nota

---

## Commit

```
git commit -m "sprint-5: propostas e notas de débito"
```
