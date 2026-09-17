# SPRINT 3 — Frota: Veículos e Equipamentos

> **Status:** ✅ Concluída — 2026-09  
> **Telas entregues:** T08, T09, T10

---

## Objetivo da Sprint

Implementar o módulo de Frota: cadastro e gestão de todos os veículos e equipamentos da empresa, com status operacional, detalhe por ativo e troca de status.

**Telas desta sprint:**
- [x] T08 — Lista Veículos/Equipamentos (`#sec-frota`)
- [x] T09 — Cadastro/Edição Equipamento (`#modal-frota`)
- [x] T10 — Detalhe do Equipamento (`#sec-frota-detalhe`)

---

## Referências dos Mockups

- `stitch/veículos_e_equipamentos_centerloc/code.html` + `screen.png`
- `stitch/cadastro_de_equipamento_centerloc/code.html` + `screen.png`
- `stitch/detalhe_do_equipamento_centerloc/code.html` + `screen.png`

---

## O que foi implementado

### T08 — Lista Frota (`#sec-frota`)

**KPI Cards no topo (4):**

| KPI | Cor |
|-----|-----|
| Total de Ativos | `#1A3A6E` |
| Disponíveis | `#16A34A` |
| Alugados/Campo | `#2563EB` |
| Em Manutenção | `#F59E0B` |

**Filtros:**
- Segmented control: Todos | Veículos | Equipamentos
- Filtro de status: Todos | Disponível | Alugado | Manutenção | Inativo
- Campo de busca: por descrição, placa, patrimônio, chassi

**Tabela de Frota:**

| Coluna | Tipo |
|--------|------|
| Patrimônio | tag navy monospaced (ex: `CLR-0042`) |
| Tipo | ícone + texto (Veículo / Equipamento) |
| Descrição | texto principal |
| Modelo / Fabricante | secundário |
| Ano | |
| Placa/Chassi | badge monospaced |
| Valor/Mês | R$ formatado |
| Status | pill colorida |
| Ações | Ver detalhe + Editar + Mudar Status |

---

### T09 — Modal Cadastro/Edição Equipamento (`#modal-frota`)

**Seção Identificação:**
- Tipo *: radio "Veículo" / "Equipamento"
- Patrimônio (gerado automaticamente: `CLR-XXXX`)
- Descrição *, Modelo *, Fabricante *, Ano de Fabricação

**Seção Técnica (condicional por tipo):**
- Se Veículo: Placa, Chassi, Cor, Combustível, Capacidade
- Se Equipamento: Nº Série, Potência, Peso, Dimensões (A/C/L)

**Seção Comercial:**
- Valor de Locação/Mês (R$) *
- Status inicial: Disponível

**Seção Manutenção:**
- Data da última revisão
- Próxima revisão programada
- Horas de operação acumuladas

**Observações:** textarea livre

---

### T10 — Detalhe do Equipamento (`#sec-frota-detalhe`)

Seção full dentro do layout (não modal):

**Header:**
- Tag de patrimônio grande
- Descrição + Modelo
- Badge de status com botão "Mudar Status"
- Botões: Editar | Voltar para lista

**Grid de informações (4 cards):**
- Dados de Identificação
- Especificações Técnicas
- Dados Comerciais (valor/mês)
- Histórico de Manutenção

**Tabela: Contratos Relacionados**
- Colunas: Nº Contrato, Locatário, Período, Status
- Alimentada por `getContratosDoEquipamento(frotaId)`

**Modal Mudar Status (`#modal-status-frota`):**
- Opções: Disponível | Alugado | Manutenção | Inativo
- Campo observação (obrigatório para Manutenção)

---

## Funções criadas

| Função | Descrição |
|--------|-----------|
| `renderFrota()` | Renderiza tabela com KPIs e filtros ativos |
| `filtrarFrota(query, tipo, status)` | Filtra em tempo real |
| `calcKPIsFromFrota()` | Calcula 4 KPI cards do db.frota |
| `openModalFrota(id)` | Abre modal (null=novo, id=editar) |
| `saveFrota()` | Valida e salva no db |
| `toggleCamposFrota(tipo)` | Mostra/oculta campos condicionais por tipo |
| `gerarPatrimonio()` | Gera CLR-XXXX sequencial |
| `viewFrotaDetalhe(id)` | Abre seção de detalhe via goTo |
| `renderFrotaDetalhe(id)` | Preenche conteúdo do detalhe |
| `openModalMudarStatus(id)` | Abre modal de mudança de status |
| `salvarNovoStatus(id, status, obs)` | Persiste novo status no db |
| `getContratosDoEquipamento(frotaId)` | Retorna contratos que incluem o ativo |

---

## Dados inicializados

```javascript
db.frota = [
  { id:"F0001", tipo:"equipamento", patrimonio:"CLR-0001",
    descricao:"Retroescavadeira Case 580N", modelo:"580N",
    fabricante:"Case", anoFabricacao:2019, serie:"JHHE0586KAKL00123",
    valorLocacao:9800, status:"alugado",
    ultimaRevisao:"2024-06-15", proximaRevisao:"2024-12-15",
    horasOperacao:3240 },
  { id:"F0002", tipo:"equipamento", patrimonio:"CLR-0002",
    descricao:"Compactador de Solo Wacker Neuson", modelo:"RD27",
    fabricante:"Wacker Neuson", anoFabricacao:2021, serie:"WN2021RD27X001",
    valorLocacao:2400, status:"disponivel",
    ultimaRevisao:"2024-08-01", proximaRevisao:"2025-02-01",
    horasOperacao:890 },
  { id:"F0003", tipo:"veiculo", patrimonio:"CLR-0003",
    descricao:"Caminhão Basculante Ford Cargo", modelo:"2429",
    fabricante:"Ford", anoFabricacao:2020, placa:"ABC-1234",
    chassi:"9BFZE5AH4MB123456", cor:"Branco",
    valorLocacao:5500, status:"manutencao",
    ultimaRevisao:"2024-07-10", proximaRevisao:"2024-10-10",
    horasOperacao:1560 },
  { id:"F0004", tipo:"equipamento", patrimonio:"CLR-0004",
    descricao:"Gerador 150 KVA Stemac", modelo:"GTA150",
    fabricante:"Stemac", anoFabricacao:2022, serie:"ST2022GTA150X004",
    valorLocacao:4500, status:"alugado",
    ultimaRevisao:"2024-05-20", proximaRevisao:"2024-11-20",
    horasOperacao:2100 }
];
db.nextIds.frota = 5;
```

---

## Critérios de Aceite

- [x] Lista de frota renderiza com KPIs calculados do db
- [x] Filtros de tipo e status funcionam em tempo real
- [x] Campo de busca filtra por descrição, placa e patrimônio
- [x] Modal abre com campos condicionais corretos por tipo
- [x] Patrimônio gerado automaticamente no cadastro (CLR-XXXX)
- [x] Tela de detalhe abre ao clicar em "Ver" na tabela
- [x] Detalhe exibe todos os dados do equipamento em cards
- [x] Modal "Mudar Status" altera status e persiste no db
- [x] Tabela de contratos relacionados no detalhe
- [x] Botão "Voltar" do detalhe retorna para `#sec-frota`

---

## Commit

```
git commit -m "sprint-3: frota — veículos e equipamentos"
```
