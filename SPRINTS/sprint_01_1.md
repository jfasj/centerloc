# SPRINT 1 — Foundation: Auth + Layout + Dashboard + Configurações

> **Status:** ✅ Concluída — 2026-09  
> **Telas entregues:** T01, T02, T03

---

## Objetivo da Sprint

Criar a estrutura base da SPA: shell HTML, autenticação simulada, layout com sidebar + topbar, Dashboard com KPIs e gráfico, e tela de Configurações da Empresa.

**Telas desta sprint:**
- [x] T01 — Login (`#login-page`)
- [x] T02 — Dashboard Principal (`#sec-dashboard`)
- [x] T03 — Configurações da Empresa (`#sec-configuracoes`)

---

## Referências dos Mockups

- `stitch/login_centerloc/code.html` + `screen.png`
- `stitch/dashboard_centerloc/code.html` + `screen.png`
- `stitch/configurações_da_empresa_centerloc/code.html` + `screen.png`
- `stitch/centerloc_fleet_equipment_design_system/DESIGN.md`

---

## O que foi implementado

### Shell HTML (`index.html`)

Arquivo único com:

- **Google Fonts:** Barlow Condensed (600, 700, 800, 900) + Inter (400, 500, 600, 700)
- **Material Symbols Outlined** via CDN
- Todos os estilos CSS inline (sem arquivo externo)
- Variáveis CSS com tokens do design system

**Estrutura de seções:**

```html
<section id="login-page">           → tela de login
<aside id="sidebar">                → navegação lateral fixa
<header id="topbar">                → barra superior fixa
<section id="sec-dashboard">        → dashboard
<section id="sec-configuracoes">    → configurações
[demais sections preenchidas nas sprints 2–7]
```

---

### T01 — Login

- Fundo dividido: esquerda `#1A3A6E` com logo + tagline, direita branca com form
- Campos: E-mail, Senha
- Botão "Entrar" laranja `#E87722`
- Autenticação simulada: qualquer e-mail/senha válidos → `goTo('dashboard')`
- Usuário padrão: `admin@centerloc.com.br` / `centerloc2024`
- Sessão salva em `sessionStorage`

---

### Sidebar (layout global)

- Fundo `#1A3A6E`, largura `260px`, fixa
- Logo CENTERLOC no topo
- Grupos de navegação:
  - **Principal:** Dashboard
  - **Cadastros:** Locatários, Veículos/Equip., Clientes/Forneced.
  - **Comercial:** Propostas, Contratos
  - **Fiscal:** Notas de Débito
  - **Financeiro:** Contas a Receber, Contas a Pagar, Fluxo de Caixa
  - **Relatórios:** Relatórios BI, DRE
  - **Sistema:** Configurações
- Item ativo: fundo `#E87722`, texto branco
- Item hover: fundo `#12294E`
- Rodapé: CNPJ + versão `v1.0.0`

---

### Topbar (layout global)

- Fixa, altura `64px`, fundo branco, `border-bottom: 1px solid #D4DFEE`
- Esquerda: breadcrumb dinâmico (ex: "Principal > Dashboard")
- Centro: campo de busca global (placeholder)
- Direita: botão "Novo" laranja + avatar + botão "Sair"
- Botão "Sair": limpa `sessionStorage` e volta para login

---

### T02 — Dashboard Principal

**KPI Cards (linha de 4):**

| Card | Cor stripe |
|------|-----------|
| Receita do Mês | `#E87722` |
| Contratos Ativos | `#1A3A6E` |
| A Receber | `#F59E0B` |
| Equipamentos em Campo | `#16A34A` |

- Valores calculados do `db` em tempo real (atualizado na Sprint 7)
- Top-stripe 4px, número grande Barlow Condensed 800, variação percentual

**Gráfico de Barras — Receita 6 meses:**
- Canvas API nativo (sem biblioteca)
- Últimos 6 meses calculados de `db.contasReceber` pagas (atualizado na Sprint 7)
- Barras arredondadas navy → gradiente laranja
- Valores acima de cada barra, eixo Y em R$

**Tabela "Contratos Recentes":**
- Colunas: Nº, Locatário, Equipamento, Início, Fim, Valor, Status
- Dados reais do `db.contratos`
- Badges de status coloridos

---

### T03 — Configurações da Empresa

**Seções:**
1. **Dados da Empresa:** Razão Social, CNPJ, Inscrição Estadual, Endereço, Cidade, UF, CEP
2. **Contato:** Telefone, E-mail
3. **Dados Financeiros:** Chave PIX, Banco, Agência, Conta
4. **Logomarca:** Upload de imagem → base64 → `db.empresa.logomarca`

- Botão "Salvar Configurações" → `saveDB()` + toast de sucesso
- Pré-carregado com dados da CENTERLOC

---

## Funções criadas

| Função | Descrição |
|--------|-----------|
| `goTo(section)` | SPA routing — oculta/mostra sections |
| `updateBreadcrumb(section)` | Atualiza breadcrumb no topbar |
| `updateSidebarActive(section)` | Marca item ativo no sidebar |
| `login()` | Valida form e inicia sessão |
| `logout()` | Limpa sessionStorage e volta ao login |
| `checkAuth()` | Verifica sessão ativa ao carregar |
| `loadDB()` | Carrega db do localStorage ou inicializa padrão |
| `saveDB()` | Persiste db no localStorage |
| `initDefaultData()` | Popula dados de exemplo iniciais |
| `renderDashboard()` | Renderiza KPIs + gráfico + tabela |
| `renderChart()` | Desenha gráfico Canvas de barras |
| `numExtenso(n)` | Número → extenso PT-BR |
| `formatBRL(n)` | Número → R$ 1.234,56 |
| `formatDate(d)` | ISO → dd/mm/yyyy |
| `gerarId(prefix, n)` | Gera ID sequencial formatado (ex: L0001) |
| `showToast(msg, tipo)` | Notificação temporária (sucesso/erro/info) |
| `saveConfiguracoes()` | Salva dados da empresa no db |
| `loadConfiguracoes()` | Carrega form com dados do db |

---

## Dados inicializados

```javascript
db.empresa = {
  razaoSocial: "CENTERLOC LOCADORA DE VEÍCULOS LTDA",
  cnpj: "12.299.005/0001-46",
  inscricaoEstadual: "0123456-78",
  endereco: "Av. Portuária, 1200 — Complexo Suape",
  cidade: "Ipojuca",
  uf: "PE",
  cep: "55590-000",
  telefone: "(81) 3432-5100",
  email: "operacoes@centerloc.com.br",
  pix: "12.299.005/0001-46",
  banco: "Bradesco",
  agencia: "3214-5",
  conta: "00123456-7"
};
```

---

## Critérios de Aceite

- [x] Login funcional (credenciais corretas → entra, erradas → alerta)
- [x] Sidebar renderiza com todos os itens de menu
- [x] Item ativo no sidebar destaca ao navegar
- [x] Topbar mostra breadcrumb correto por seção
- [x] Dashboard carrega KPIs com valores do `db`
- [x] Gráfico Canvas renderiza com 6 meses de dados
- [x] Tabela de contratos recentes exibe badges de status
- [x] Configurações carrega dados da empresa e salva ao clicar
- [x] Toast aparece ao salvar configurações
- [x] `localStorage` persiste entre recarregamentos

---

## Commit

```
git commit -m "sprint-1: foundation — login, dashboard, configurações"
```

---

## Decisões técnicas registradas

| Decisão | Motivo |
|---------|--------|
| Vanilla JS (sem framework) | Sem dependências, roda em qualquer ambiente |
| localStorage chave `centerloc_v1` | Simples, sem servidor |
| `<dialog>` nativo para modais | API moderna, acessível, sem lib extra |
| Google Fonts CDN | Design system Stitch (Barlow Condensed + Inter) |
| Material Symbols Outlined (CDN) | Ícones consistentes com mockup |
| `@media print` para documentos | Impressão limpa sem sidebar/topbar |
