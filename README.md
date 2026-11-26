# Loja com Flask e PostgreSQL

Sistema acadêmico de vendas com cadastro de clientes e produtos, baixa de estoque, descontos por faixa de valor e relatórios consolidados.

[![Sintaxe Python](https://github.com/guilhermeestrela10/loja-flask-postgresql/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/guilhermeestrela10/loja-flask-postgresql/actions/workflows/ci.yml)

[Execução](#executar-localmente) · [Banco de dados](#regras-no-banco) · [Estrutura](#estrutura) · [Limitações](#limitações)

## Contexto

Projeto de Banco de Dados · Guilherme Henrique Macedo Estrela · Professor Anderson Soares.

O Flask apresenta as telas. O PostgreSQL concentra a validação do estoque, o cálculo de descontos e a gravação da venda.

## Executar localmente

Requisitos: Python 3, PostgreSQL 11+ e utilitários `createdb` e `psql` disponíveis no terminal. Execute na raiz do repositório:

```shell
git clone https://github.com/guilhermeestrela10/loja-flask-postgresql.git
cd loja-flask-postgresql
python -m venv .venv
```

No PowerShell:

```powershell
.\.venv\Scripts\python.exe -m pip install -r src/requirements.txt
createdb -U postgres loja
psql -U postgres -d loja -f database/setup.sql
$env:DB_PASSWORD = "SUA_SENHA_LOCAL"
Set-Location src
..\.venv\Scripts\python.exe app.py
```

Abra [localhost:5000](http://127.0.0.1:5000). No Linux/macOS, use `.venv/bin/python` e exporte as variáveis no seu shell.

O script SQL inclui estrutura e dados de exemplo; execute somente em um banco de estudo preparado para este projeto.

### Configuração

| Variável | Uso | Padrão do código |
| --- | --- | --- |
| `DB_HOST` | Servidor PostgreSQL | `localhost` |
| `DB_PORT` | Porta | `5432` |
| `DB_NAME` | Banco | `loja` |
| `DB_USER` | Usuário | `postgres` |
| `DB_PASSWORD` | Senha local | Defina explicitamente |

O [`.env.example`](.env.example) documenta essas variáveis. O código usa `os.getenv` e não carrega `.env` automaticamente.

## Regras no banco

| Objeto | Responsabilidade |
| --- | --- |
| `clientes`, `produtos`, `vendas`, `itens_venda` | Dados da aplicação |
| `vw_relatorio_vendas` | Consolidação de vendas e itens |
| `fn_calcular_desconto(subtotal)` | Desconto de 5% a partir de R$ 200 e 10% a partir de R$ 500 |
| `sp_realizar_venda` | Validação de estoque, gravação e baixa em uma transação |

## Estrutura

```text
src/app.py          Rotas e acesso ao PostgreSQL
src/templates/      Interface em Jinja2
src/requirements.txt Dependências Python
database/           Esquema, dados de exemplo, função, procedure e view
docs/               Proposta de integração futura
```

## Verificações

O GitHub Actions verifica a sintaxe Python. Não executa operações de venda nem substitui testes de integração com PostgreSQL.

## Limitações

O servidor de desenvolvimento inicia com debug habilitado, não há autenticação e a chave de sessão é demonstrativa. Execute localmente com dados fictícios. Supabase e Redis aparecem em uma [proposta de integração](docs/integracao-planejada.md), ainda não implementada no código atual.
