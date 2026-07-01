# DealRadar - Plataforma de Comparacao de Precos

**DealRadar** e uma aplicacao web desenvolvida em ASP.NET Core/Blazor para o trabalho pratico de Engenharia de Software II 2024/2025, no ambito do **Tema E - Plataforma de Comparacao de Precos**.

O projecto implementa uma plataforma para registo, consulta e comparacao de precos de produtos em diferentes lojas/supermercados. A aplicacao permite consultar produtos e precos, gerir entidades principais como produtos, lojas e categorias, confirmar ou actualizar precos e gerar relatorios associados a lojas e produtos.

## Funcionalidades principais

- Consulta publica de produtos e precos, sem obrigar a autenticacao.
- Registo e autenticacao de utilizadores com ASP.NET Core Identity.
- Tres niveis de permissao configurados no arranque: `Admin`, `UserManager` e `User`.
- Criacao automatica de um utilizador administrador inicial quando a aplicacao arranca sem esse utilizador.
- Pesquisa de produtos por nome, categoria ou loja.
- Filtros e ordenacao de resultados por preco, nome e data.
- Visualizacao de detalhe de produto com imagens, categoria, melhor preco e lojas associadas.
- Historico de precos por produto, com visualizacao grafica.
- Confirmacao de precos por utilizadores autenticados.
- Edicao de precos existentes por utilizadores autenticados.
- Calculo de credibilidade do preco atraves de estrategias baseadas em confirmacoes e antiguidade do registo.
- Gestao de produtos, lojas e categorias a partir dos paineis de administracao/gestao.
- Associacao de produtos a lojas e registo de precos por loja.
- Listagem e gestao de utilizadores, incluindo estado activo/inactivo e role.
- Consulta de actividade de utilizadores.
- Envio de mensagens para utilizadores registados.
- Relatorio geral de lojas com localizacao e numero de produtos por categoria.
- Relatorio especifico de loja com produtos e precos mais recentes.
- Relatorio especifico de produto com lojas e precos mais recentes.
- Exportacao de relatorios em PDF.
- API REST para entidades como produtos, lojas, categorias, precos, imagens, mensagens, relatorios e utilizadores.
- Swagger disponivel em ambiente de desenvolvimento.

## Tecnologias usadas

- **.NET 8 / ASP.NET Core**: base da aplicacao web.
- **Blazor Server / Razor Components**: interface web interactiva.
- **C#**: linguagem principal da aplicacao.
- **ASP.NET Core Identity**: autenticacao, utilizadores e roles.
- **Entity Framework Core**: acesso e persistencia de dados.
- **PostgreSQL**: base de dados relacional.
- **Npgsql.EntityFrameworkCore.PostgreSQL**: provider PostgreSQL para EF Core.
- **Swagger / Swashbuckle**: documentacao e teste da API em desenvolvimento.
- **QuestPDF**: geracao de relatorios PDF.
- **ScottPlot / Chart.js via JavaScript**: suporte a visualizacoes/graficos de precos.
- **Bootstrap**: estilos e componentes visuais.
- **Leaflet / LeafletBlazor**: suporte a mapas/localizacao de lojas.

## Arquitectura

O projecto segue uma estrutura monolitica ASP.NET Core com separacao por responsabilidades:

- **Components**: paginas Blazor, layout, rotas e componentes de conta/autenticacao.
- **Controllers**: endpoints REST para acesso a entidades principais.
- **Services**: regras de aplicacao e operacoes sobre produtos, lojas, categorias, precos, utilizadores, relatorios e mensagens.
- **Services/Strategies**: estrategias para calculo de credibilidade de precos.
- **Models**: entidades de dominio persistidas na base de dados.
- **DTOs**: objectos de transferencia usados em listagens, pesquisas e respostas.
- **Data**: `ApplicationDbContext` e configuracao das relacoes Entity Framework.
- **Migrations**: migracoes EF Core para criacao/evolucao da base de dados.
- **wwwroot**: ficheiros estaticos, CSS, JavaScript, imagens e Bootstrap.

### Entidades principais

- `Product`: produto pesquisavel e comparavel.
- `Store`: loja/supermercado com localizacao e URL Google Maps.
- `Price`: preco de um produto numa loja, com data e grau de confianca.
- `PriceConfirmation`: confirmacao de preco feita por um utilizador.
- `Category`: categoria de produto.
- `User`: utilizador Identity com estado activo/inactivo.
- `Message`: mensagem enviada para utilizadores.
- `Report` / `GeneratedReport`: suporte a relatorios.

## Estrutura de pastas

```text
.
|-- Components/          # Paginas Blazor, layouts, rotas e area de conta
|-- Controllers/         # Controladores REST da API
|-- Data/                # ApplicationDbContext
|-- DTOs/                # Data Transfer Objects
|-- Migrations/          # Migracoes Entity Framework Core
|-- Models/              # Modelos de dominio
|-- Properties/          # Perfis de arranque
|-- Services/            # Servicos de aplicacao e estrategias
|-- wwwroot/             # Assets estaticos, CSS, JS, imagens e Bootstrap
|-- appsettings.json     # Configuracao da aplicacao
|-- ESIID42025.csproj    # Projecto ASP.NET Core
|-- esiid42025.sln       # Solucao Visual Studio/.NET
|-- global.json          # SDK .NET preferencial
|-- Program.cs           # Configuracao da app, DI, Identity, Swagger e pipeline HTTP
```

## Como executar

### Pre-requisitos

- .NET SDK 8.x
- PostgreSQL
- Ferramenta `dotnet-ef`, se for necessario aplicar migracoes pela linha de comandos:

```bash
dotnet tool install --global dotnet-ef
```

### Configurar a base de dados

O ficheiro `appsettings.json` contem a connection string `DefaultConnection` para PostgreSQL:

```json
"DefaultConnection": "Host=localhost;Database=ES2-d4;Username=postgres;Password=d3rp1234"
```

Antes de executar, confirma que o PostgreSQL esta activo e que a connection string corresponde ao teu ambiente local.

### Restaurar dependencias

```bash
dotnet restore
```

### Aplicar migracoes

```bash
dotnet ef database update
```

### Executar a aplicacao

```bash
dotnet run --project ESIID42025.csproj
```

Perfis de arranque configurados:

- HTTP: `http://localhost:5147`
- HTTPS: `https://localhost:7083`

Em ambiente de desenvolvimento, o Swagger devera ficar disponivel em:

```text
/swagger
```

### Utilizador administrador inicial

O `Program.cs` cria automaticamente as roles `Admin`, `UserManager` e `User`. Tambem cria um administrador inicial se este ainda nao existir:

```text
Email: admin@example.com
Password: Admin@123
```

Estas credenciais devem ser alteradas ou removidas antes de qualquer utilizacao fora de ambiente academico/local.

## Variaveis de ambiente

Nao foram identificadas variaveis de ambiente obrigatorias no codigo. A configuracao principal esta em `appsettings.json`.

Em alternativa ao ficheiro de configuracao, a connection string pode ser fornecida atraves do mecanismo normal de configuracao do ASP.NET Core, por exemplo:

```bash
ConnectionStrings__DefaultConnection="Host=localhost;Database=ES2-d4;Username=postgres;Password=..."
```

Variaveis/perfis relevantes:

- `ASPNETCORE_ENVIRONMENT`: definido como `Development` nos perfis de `launchSettings.json`.
- `ConnectionStrings__DefaultConnection`: alternativa recomendada para configurar a base de dados sem guardar credenciais no repositorio.

## Screenshots

Nao foi encontrada uma pasta `docs/screenshots` neste repositorio. Por isso, este README nao inclui capturas de ecra.

## Estado do projecto

Projecto academico desenvolvido para Engenharia de Software II. A implementacao presente no repositorio cobre a base funcional de uma plataforma de comparacao de precos: autenticacao, roles, produtos, lojas, categorias, precos, confirmacoes, relatorios e paineis de administracao.

De forma prudente, este README descreve apenas funcionalidades observadas no codigo. Nao foi identificada uma pasta de testes unitarios no repositorio, apesar de o enunciado geral referir testes como elemento esperado da entrega.

## Autores

Autores/contribuidores identificados no historico Git do repositorio:

- [Simao Sousa](https://github.com/simaosousa10)
- Luis Felipe Flores
- [Pedro Cruz](https://github.com/pedrojcruz)
- Deerpyyy
- [Daniel Alves](https://github.com/DanielA1ves)
