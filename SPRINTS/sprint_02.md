# SPRINT 2 — Cadastros: Locatários + Clientes/Fornecedores

> **Status:** ✅ Concluída — 2026-09  
> **Telas entregues:** T04, T05, T06, T07

---

## Objetivo da Sprint

Implementar o módulo de cadastro de pessoas: Locatários (clientes que alugam) e Clientes/Fornecedores (parceiros gerais), com listagem, criação, edição e exclusão (CRUD completo).

**Telas desta sprint:**
- [x] T04 — Lista Locatários (`#sec-locatarios`)
- [x] T05 — Cadastro/Edição Locatário (`#modal-locatario`)
- [x] T06 — Lista Clientes/Fornecedores (`#sec-clientes`)
- [x] T07 — Cadastro/Edição Cliente/Fornecedor (`#modal-cliente`)

---

## Referências dos Mockups

- `stitch/locatários_centerloc/code.html` + `screen.png`
- `stitch/cadastro_de_locatário_centerloc/code.html` + `screen.png`
- `stitch/clientes_e_fornecedores_centerloc/code.html` + `screen.png`
- `stitch/cadastro_cliente_e_fornecedor_centerloc/code.html` + `screen.png`

---

## O que foi implementado

### T04 — Lista Locatários (`#sec-locatarios`)

**Header da seção:**
- Título "Locatários" (Barlow Condensed 800)
- Breadcrumb: Cadastros > Locatários
- Botão "+ Novo Locatário" (laranja)
- Campo de busca por nome/CNPJ

**Tabela de Locatários:**

| Coluna | Tipo |
|--------|------|
| ID | código (ex: L0001) |
| Razão Social | texto principal |
| CNPJ | `font-variant-numeric: tabular-nums` |
| Contato | nome do responsável |
| Telefone | |
| Status | pill: Ativo / Inativo |
| Ações | Editar (lápis) + Desativar (toggle) |

- Busca filtra em tempo real por razão social e CNPJ
- Linha selecionada: borda esquerda 4px `#E87722`
- Paginação: 15 itens por página

---

### T05 — Modal Cadastro/Edição Locatário (`#modal-locatario`)

Implementado com `<dialog>` nativo.

**Campos obrigatórios (*):**
- Razão Social *
- CNPJ * (máscara: 00.000.000/0000-00)
- Inscrição Estadual

**Campos opcionais:**
- Nome do Contato
- Cargo/Função
- E-mail
- Telefone (máscara: (00) 0 0000-0000)
- Endereço completo (Rua, Nº, Bairro, Cidade, UF, CEP)
- Observações (textarea)

**Rodapé do modal:**
- Botão "Cancelar" (outline)
- Botão "Salvar Locatário" (laranja)

**Validações:**
- CNPJ: formato válido
- E-mail: formato válido
- Campos obrigatórios bloqueiam salvar se vazios

---

### T06 — Lista Clientes/Fornecedores (`#sec-clientes`)

Mesma estrutura da lista de locatários, com coluna extra:

| Coluna adicional | Valores |
|------------------|---------|
| Tipo | pill: "Cliente" / "Fornecedor" / "Ambos" |

- Filtro de tipo: tabs (Todos | Clientes | Fornecedores)

---

### T07 — Modal Cadastro/Edição Cliente/Fornecedor (`#modal-cliente`)

Igual ao de Locatário, adicionando:
- Campo "Tipo": dropdown com opções Cliente / Fornecedor / Ambos
- Campo "Categoria" (ex: Manutenção, Combustível, Peças, Serviços, Outros)

---

## Funções criadas

| Função | Descrição |
|--------|-----------|
| `renderLocatarios()` | Renderiza tabela filtrada com paginação |
| `openModalLocatario(id)` | Abre modal (id=null → novo, id=x → editar) |
| `saveLocatario()` | Valida e salva no db |
| `toggleLocatario(id)` | Ativa/desativa locatário |
| `filtrarLocatarios(query)` | Filtra por texto em tempo real |
| `renderClientes()` | Renderiza tabela de clientes/fornecedores |
| `openModalCliente(id)` | Abre modal (id=null → novo, id=x → editar) |
| `saveCliente()` | Valida e salva no db |
| `toggleCliente(id)` | Ativa/desativa cliente |
| `filtrarClientes(query, tipo)` | Filtra por texto e tipo |
| `aplicarMascaraCNPJ(input)` | Aplica máscara 00.000.000/0000-00 ao digitar |
| `aplicarMascaraTel(input)` | Aplica máscara (00) 0 0000-0000 ao digitar |

---

## Dados inicializados

```javascript
// Locatários (inicializados na Sprint 1)
db.locatarios = [
  { id:"L0001", razaoSocial:"ENGEMAN SERVIÇOS TÉCNICOS LTDA",
    cnpj:"08.769.549/0006-61", contato:"Carlos Engenheiro",
    email:"engeman@email.com", telefone:"(81) 9999-0001", ativo:true },
  { id:"L0002", razaoSocial:"CLC ENGENHARIA E CONSTRUÇÃO LTDA",
    cnpj:"11.222.333/0001-44", contato:"Julio Cesar",
    email:"clc@email.com", telefone:"(81) 9999-0002", ativo:true },
  { id:"L0003", razaoSocial:"TAUÁ HOTEL RECIFE",
    cnpj:"22.333.444/0001-55", contato:"Ana Gerente",
    email:"taua@email.com", telefone:"(81) 9999-0003", ativo:true }
];
db.nextIds.locatario = 4;

// Clientes/Fornecedores
db.clientes = [
  { id:"C0001", tipo:"fornecedor", categoria:"Manutenção",
    razaoSocial:"MANUTEC SERVIÇOS INDUSTRIAIS LTDA",
    cnpj:"33.444.555/0001-11", contato:"Ricardo Técnico",
    email:"manutec@email.com", telefone:"(81) 9888-0001", ativo:true },
  { id:"C0002", tipo:"cliente",
    razaoSocial:"PORTO DE SUAPE S.A.",
    cnpj:"44.555.666/0001-22", contato:"Direção Operacional",
    email:"porto@suape.com.br", telefone:"(81) 3527-5000", ativo:true },
  { id:"C0003", tipo:"ambos", categoria:"Peças",
    razaoSocial:"DISTRIBUIDORA NORDESTE LTDA",
    cnpj:"55.666.777/0001-33", contato:"Marcos Distribuidora",
    email:"nordeste@dist.com", telefone:"(81) 9777-0003", ativo:true }
];
db.nextIds.cliente = 4;
```

---

## Critérios de Aceite

- [x] Lista de locatários renderiza com busca funcional em tempo real
- [x] Modal abre vazio para novo locatário
- [x] Modal abre preenchido ao editar locatário existente
- [x] Salvar locatário atualiza a lista sem recarregar
- [x] Desativar locatário muda pill de status para "Inativo"
- [x] Máscara de CNPJ e telefone funcionam ao digitar
- [x] Validação bloqueia salvar com campos obrigatórios vazios
- [x] Lista de clientes filtra por tipo (tabs Todos/Clientes/Fornecedores)
- [x] Toast confirma operações de sucesso/erro
- [x] Dados persistem no localStorage após recarregar

---

## Commit

```
git commit -m "sprint-2: cadastros — locatários e clientes/fornecedores"
```
