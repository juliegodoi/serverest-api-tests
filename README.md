# ServeRest API Tests

Este repositório contém testes automatizados de API para a aplicação [ServeRest](https://serverest.dev/?lang=pt-BR#/). O projeto foi estruturado utilizando o Postman para a criação e validação das requisições, e o Newman para a execução automatizada via linha de comando.

## Sumário
- [Links Relacionados](#links-relacionados)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Escopo dos Testes](#escopo-dos-testes)
- [Instalação e Execução (Newman)](#instalação-e-execução-newman)
- [Execução via Postman](#execução-via-postman)
- [Vulnerabilidade Encontrada](#vulnerabilidade-encontrada)
- [Autor](#autor)

## Links Relacionados
- **Repositório de Testes de Front-End:** [GitHub - ServeRest Front-End Tests](https://github.com/juliegodoi/serverest-front-tests)

## Estrutura do Projeto
A arquitetura do projeto é composta pelos próprios arquivos exportados do sistema do Postman, dividida nas seguintes pastas:

- **`collections/`**: Contém o arquivo `ServeRest.postman_collection.json`, que armazena todas as requisições, pastas de cenários, fluxos e scripts de validação desenvolvidos.
- **`environments/`**: Contém o arquivo `ServeRest.postman_environment.json`, que armazena as variáveis essenciais, como a URL raiz de acesso da API, além de salvar dados gerados dinamicamente ao decorrer dos testes (ids, tokens, nomes falsos).

## Escopo dos Testes
Os cenários implementados focam em validar as respostas retornadas pelo sistema (como status HTTP e mensagens), persistência de dados no formato de contratos, regras de negócio e proteções de acesso. O caminho (fluxo) principal destes testes de API foi estruturado com **foco na jornada do usuário Administrador**. O que é testado:

- **Autenticação:** Criação de usuários do zero, validações de tentativa de cadastro com e-mail duplicado, procura de usuários por banco (dados existentes e não encontrados). Além disso, há o teste do fluxo de login completo e checagem de rejeição ao informar uma senha inválida. Em especial, o tempo de validade esperado de 10 minutos para a vida útil do token também é averiguado.
- **Produtos:** Validando as permissões de rotas fechadas, garantindo que o cadastro de produtos exija o uso de validação ativa no Header com um token de Administrador. Checagem do bloqueio ao usar token inválido ou tentar cadastrar nomes de produtos repetidos, além de buscas pelos IDs dos itens registrados.

## Instalação e Execução (Newman)
Para clonar este repositório e executar os testes automatizados pelo terminal sem complicação, siga os passos a seguir:

1. **Baixe o projeto para a sua máquina:**
   ```bash
   git clone https://github.com/juliegodoi/serverest-api-tests.git
   cd serverest-api-tests
   ```

2. **Instale o Newman globalmente:**
   Certifique-se de possuir o [Node.js](https://nodejs.org/) instalado juntamente ao npm. Em seguida rode no seu terminal:
   ```bash
   npm install -g newman
   ```

3. **Inicie os testes:**
   Já nos arquivos do seu projeto raiz, utilize o seguinte comando executando exatamente os arquivos `.json` presentes nos seus diretórios (sem precisar renomear nada):
   ```bash
   newman run collections/ServeRest.postman_collection.json -e environments/ServeRest.postman_environment.json
   ```

## Execução via Postman
Caso você prefira rodar usando a Interface Gráfica da própria ferramenta nativa ou até mesmo queira editar os fluxos e realizar debbug, siga as instruções:

1. Abra o aplicativo do **Postman**.
2. Clique no ícone de **Import** (Importar), localizado próximo aos seus workspaces (geralmente lado superior esquerdo).
3. Selecione e importe os arquivos recém clonados que estão nas pastas deste projeto: `ServeRest.postman_collection.json` e `ServeRest.postman_environment.json`.
4. Defina o Environment **"ServeRest"** como o escolhido na barra de seleção do canto superior direito.
5. Sinta-se a vontade para rodar apenas chamadas individuais abertas ou clicar facilmente sobre a pasta raiz da Collection e acessar o **Run** para realizar inspeção em fluxo.

## Vulnerabilidade Encontrada
Durante o processo de avaliação dos retornos, foi identificada uma falha de segurança focada no formato do token de autenticação gerado pelo sistema. A aplicação não mascara a segurança perfeitamente com criptografia de hashes não conversíveis. Apenas com a visualização do token devolvido pela API e o uso rápido da ferramenta **Base64 Decode** para revelar informações, é possível expor dados atrelados à conta do próprio usuário da sessão (data de emissão do registro, expiração limite de uso, etc). A exposição destes dados puramente textuais de fácil resolução constitui grande risco para possíveis vazamentos e espionagem de interceptações num ambiente em nuvem.

## Autor
**Julie Godoi**

- **GitHub:** [https://github.com/juliegodoi](https://github.com/juliegodoi)
- **LinkedIn:** [https://www.linkedin.com/in/juliegodoi](https://www.linkedin.com/in/juliegodoi)
