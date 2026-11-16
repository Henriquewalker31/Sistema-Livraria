# Sistema de Livraria

Sistema de gerenciamento de livraria desenvolvido em **PHP puro** com **MySQL**, empacotado em **Docker**. Permite cadastrar, listar, buscar, editar e excluir livros por meio de uma interface responsiva com Bootstrap.

Projeto acadêmico do curso de Análise e Desenvolvimento de Sistemas, mantido como portfólio.

## Funcionalidades

- Cadastro de livros com título, autor, ano, editora, categoria, quantidade, preço e data.
- Listagem completa do acervo.
- Busca por título com `LIKE` e `prepared statements`.
- Edição e exclusão de registros.
- Mensagens de sucesso e erro via sessão.
- Interface responsiva com Bootstrap 5 e Bootstrap Icons.

## Stack

| Camada | Tecnologia |
|---|---|
| Backend | PHP 8.2 |
| Banco | MySQL 8.0 |
| Frontend | HTML, Bootstrap 5, CSS e JavaScript |
| Infra | Docker e Docker Compose |
| Administração do banco | phpMyAdmin |

## Como rodar

Pré-requisitos: **Docker** e **Docker Compose**.

```bash
git clone https://github.com/Henriquewalker31/Sistema-Livraria.git
cd Sistema-Livraria
docker compose up -d --build
```

Depois de subir os containers:

| Serviço | URL |
|---|---|
| Aplicação | <http://localhost:8080> |
| phpMyAdmin | <http://localhost:8081> |

O banco `livraria` é criado automaticamente pelo container do MySQL. Falta apenas criar a tabela `livros` — use o phpMyAdmin ou o comando abaixo:

```bash
docker compose exec db mysql -uroot livraria -e "
CREATE TABLE IF NOT EXISTS livros (
  id INT AUTO_INCREMENT PRIMARY KEY,
  titulo VARCHAR(255) NOT NULL,
  autor VARCHAR(255) NOT NULL,
  ano_publicacao INT NULL,
  editora VARCHAR(255) NOT NULL,
  categoria VARCHAR(120) NOT NULL,
  quantidade INT NOT NULL,
  preco DECIMAL(10, 2) NULL,
  data_cadastro DATE NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
"
```

Para parar os containers:

```bash
docker compose down
```

Para apagar também os dados do banco:

```bash
docker compose down -v
```

## Estrutura do projeto

```
Sistema-Livraria/
├── app/
│   ├── actions/acoes.php       # criação, atualização e exclusão
│   └── config/conexao.php      # conexão MySQL (host "db" no Docker)
├── public/
│   ├── index.php               # listagem e busca
│   ├── livro-create.php        # formulário de cadastro
│   ├── livro-edit.php          # formulário de edição
│   ├── livro-view.php          # detalhes do livro
│   ├── acoes.php               # ponte para app/actions/acoes.php
│   ├── navbar.php
│   ├── mensagem.php
│   ├── css/style.css
│   └── JS/script.js
├── docker-compose.yml
└── Dockerfile
```

## Configuração

A conexão fica em `app/config/conexao.php`:

```php
$servername = "db";      // nome do serviço no docker-compose
$username   = "root";
$password   = "";
$dbname     = "livraria";
```

Para rodar fora do Docker (XAMPP, Laragon ou MySQL local), troque `db` por `localhost` e ajuste usuário, senha e porta conforme o seu ambiente.

## Segurança e limitações

Este é um projeto acadêmico, não pronto para produção. Antes de usar em ambiente real:

- Definir senha para o usuário `root` do MySQL e mover a configuração para variáveis de ambiente.
- Adicionar proteção CSRF nos formulários.
- Validar e tipar os dados no servidor (hoje a validação é basicamente `mysqli_real_escape_string`).
- Restringir o acesso ao phpMyAdmin.
- Criar a tabela `livros` via migration ou script versionado.

## Autor

Feito por [Henrique Walker](https://github.com/Henriquewalker31).
