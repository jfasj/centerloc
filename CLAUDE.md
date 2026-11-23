# CENTERLOC — Sistema de Gestão de Locação

Sistema web de gestão da CENTERLOC (locadora de veículos e equipamentos pesados, Complexo de Suape/PE):
frota, locatários, clientes e fornecedores, propostas, contratos, notas de débito/faturas, declarações de
faturamento, contas a pagar e a receber, fluxo de caixa com conciliação bancária (OFX/PDF), importação de
XML de NF-e, plano de contas, DRE por ativo e relatórios.

## Arquitetura
- **Arquivo único: `index.html`** (~8.700 linhas, com HTML, CSS e JS vanilla juntos). Não há build nem dependências locais.
  Bibliotecas via CDN: `@supabase/supabase-js@2` e `pdf.js`.
- Cada tela é uma `section` (`#sec-<nome>`) renderizada por funções `render<Tela>()`. As rotas e os breadcrumbs ficam em `BREADCRUMBS`.
- **Persistência:** todo o estado fica num objeto JS `db`, salvo como **um único JSON** na linha `id = 1` da tabela
  `company_data` do Supabase (`loadDB()` / `saveDB()`). Coleções principais: `empresa`, `frota`, `locatarios`, `clientes`,
  `contratos`, `propostas`, `notasDebito`, `declaracoesFaturamento`, `lancamentosBancarios`, `planoDeContas`, `nextIds`.
  - Ao adicionar coleções ou campos, atualize `initDefaultData()` **e** a migração em `ensureSchemaV2()`, porque o banco já existe.
  - Gravação é "último a salvar vence". Dois usuários editando ao mesmo tempo podem sobrescrever um ao outro.
- **Login:** Supabase Auth (e-mail e senha). Os perfis ficam na tabela `profiles` (`admin` | `operacional`).
  O perfil operacional não vê as seções em `RESTRICTED_SECTIONS_OPERACIONAL` (restrição só de interface).
  - O auto-cadastro foi removido da tela em 23/11/2026. Novos usuários são criados no painel do Supabase
    (Authentication → Users → Add user). No primeiro login, o sistema cria o profile como `operacional`.

## Deploy
- **GitHub Pages, servido direto da branch `main`** → https://jfasj.github.io/centerloc/
  **Todo push em `main` vai para produção imediatamente.** Teste antes de enviar.
- Para testar localmente, abra o `index.html` num servidor estático (ex.: `python -m http.server`). Ele conecta ao
  Supabase de **produção**.

## ⚠️ O repositório é PÚBLICO
- Nunca commite dados internos da empresa (frota com valores, clientes, pesquisas de preço, planilhas).
- Pesquisas e relatórios ficam no repositório **privado** `jfasj/centerloc-pesquisas`
  (pasta irmã `../centerloc-pesquisas`). `reports/` e `research_notes/` estão no `.gitignore`.
- A chave `sb_publishable_…` no código é pública por design. A proteção dos dados depende das policies RLS no Supabase.

## Outras pastas
- `SPRINTS/`: especificações das sprints 1 a 7 (v1.0 concluída em 09/2026). Hoje as evoluções são feitas direto, sem sprint.
- `stitch_centerloc_rental_management_system/`: mockups das telas (HTML e PNG), usados como referência visual e design system.

## Convenções
- Interface, código de negócio e mensagens de commit em português.
- Escape de HTML com `esc()` em tudo que vem do usuário (já houve correção de XSS armazenado).
- Documentos de impressão A4 (contrato, proposta, nota, declaração, relatório gerencial) têm funções `render*A()` próprias.
