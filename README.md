# Banco API Tests

Projeto de automação de testes de API REST para serviços bancários, desenvolvido com foco em boas práticas de QA, arquitetura modular, validações de regras de negócio e geração de relatórios de execução.

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Mocha](https://img.shields.io/badge/Mocha-8D6748?style=for-the-badge&logo=mocha&logoColor=white)
![Chai](https://img.shields.io/badge/Chai-A30701?style=for-the-badge&logo=chai&logoColor=white)
![SuperTest](https://img.shields.io/badge/SuperTest-41B883?style=for-the-badge&logo=vuedotjs&logoColor=white)
![Mochawesome](https://img.shields.io/badge/Reports-Mochawesome-blue?style=for-the-badge)

---

## Visão Geral

Este repositório contém a suíte de testes de integração e contrato para a [Banco API](https://github.com/juliodelimas/banco-api) (desenvolvida por [Julio de Lima](https://github.com/juliodelimas)), cobrindo fluxos críticos como autenticação de usuários e operações de transferência de valores (criação, regras de validação e consultas com paginação).

### Tecnologias Utilizadas

- **[Node.js](https://nodejs.org/)**: Ambiente de execução JavaScript.
- **[Mocha](https://mochajs.org/)**: Framework e executor de testes (*Test Runner*).
- **[Chai](https://www.chaijs.com/)**: Biblioteca de asserções (*BDD/TDD* com sintaxe `expect`).
- **[SuperTest](https://github.com/ladjs/supertest)**: Cliente HTTP para realizar requisições e asserções nos endpoints.
- **[Mochawesome](https://github.com/adamgruber/mochawesome)**: Gerador de relatórios visuais em HTML e JSON.
- **[Dotenv](https://github.com/motdotla/dotenv)**: Gerenciamento de variáveis de ambiente de forma segura.

---

## Estrutura do Projeto

A arquitetura do projeto foi organizada visando facilidade de manutenção e desacoplamento de responsabilidades:

```text
banco-api-tests/
├── fixtures/                    # Massas de dados estáticas para requisições (payloads)
│   ├── postLogin.json           # Dados mockados para login
│   └── postTransferencias.json  # Dados mockados para transferências
├── helpers/                     # Funções utilitárias e reaproveitáveis
│   └── autenticacao.js          # Helper responsável por obter token de autenticação
├── mochawesome-report/          # Relatórios de execução gerados automaticamente (HTML/JSON)
├── test/                        # Suíte de testes automatizados
│   ├── login.test.js            # Cenários de teste do endpoint /login
│   └── transferencia.test.js    # Cenários de teste dos endpoints /transferencias
├── .env                         # Variáveis de ambiente locais (ignorado no git)
├── .env.example                 # Exemplo do arquivo de variáveis de ambiente
├── .gitignore                   # Arquivos e diretórios ignorados pelo Git
├── package.json                 # Metadados do projeto e scripts de execução
└── README.md                    # Documentação do projeto
```

---

## Cenários de Teste Cobertos

### Autenticação (`/login`)
| Método | Endpoint | Cenário | Status Esperado |
| :--- | :--- | :--- | :---: |
| `POST` | `/login` | Deve retornar status 200 com token em string ao informar credenciais válidas | `200 OK` |

### Transferências (`/transferencias`)
| Método | Endpoint | Cenário | Status Esperado |
| :--- | :--- | :--- | :---: |
| `POST` | `/transferencias` | Deve criar uma transferência quando o valor for igual ou superior a R$ 10,00 | `201 Created` |
| `POST` | `/transferencias` | Deve rejeitar a transferência com erro de validação quando o valor for inferior a R$ 10,00 | `422 Unprocessable Entity` |
| `GET` | `/transferencias/{id}` | Deve retornar os detalhes da transferência correspondente ao ID informado | `200 OK` |
| `GET` | `/transferencias` | Deve retornar a lista paginada respeitando os limites informados via query params (`page` e `limit`) | `200 OK` |

---

## Como Executar o Projeto

### Pré-requisitos
- **Node.js** instalado (versão 18 ou superior recomendada)
- **npm** (incluso com o Node.js)
- A **API do Banco** em execução localmente ([banco-api do Julio de Lima](https://github.com/juliodelimas/banco-api))

### 1. Clonar o repositório
```bash
git clone https://github.com/thaliamariana/banco-api-tests.git
cd banco-api-tests
```

### 2. Instalar as dependências
```bash
npm install
```

### 3. Configurar variáveis de ambiente
Crie um arquivo `.env` na raiz do projeto a partir do `.env.example`:

```bash
# No Windows PowerShell:
Copy-Item .env.example .env

# No Linux/macOS ou Git Bash:
cp .env.example .env
```

Abra o arquivo `.env` e confirme a URL base da API alvo:
```env
BASE_URL="http://localhost:3000"
```

### 4. Executar os testes
Para rodar toda a suíte de testes com geração do relatório:
```bash
npm test
```

Para rodar apenas um arquivo de teste específico:
```bash
# Executar apenas testes de login
npx mocha test/login.test.js

# Executar apenas testes de transferências
npx mocha test/transferencia.test.js
```

---

## Relatórios de Execução (Mochawesome)

Após rodar `npm test`, o Mochawesome gera automaticamente um relatório interativo detalhado na pasta `mochawesome-report/`.

Para visualizar o relatório no navegador:

- **Windows (PowerShell):**
  ```powershell
  Start-Process mochawesome-report/mochawesome.html
  ```
- **Linux:**
  ```bash
  xdg-open mochawesome-report/mochawesome.html
  ```
- **macOS:**
  ```bash
  open mochawesome-report/mochawesome.html
  ```

Ou abra o arquivo `mochawesome-report/mochawesome.html` diretamente em seu navegador preferido.
