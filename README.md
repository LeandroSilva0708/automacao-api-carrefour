Automação de Testes de API - Desafio Carrefour

Este repositório contém a automação de testes de backend (API) baseada na documentação do [ServeRest](https://serverest.dev/), 
desenvolvida como parte do desafio técnico para QA no Carrefour. 
O projeto contempla testes de ponta a ponta, controle de variáveis de ambiente e uma esteira de Integração Contínua (CI/CD) automatizada no GitHub Actions.

*Análise Crítica de Requisitos (Observação de QA)*

Durante a fase de planejamento e mapeamento dos testes, foi identificada uma divergência entre o documento de requisitos do desafio e a implementação real da API:

O que foi solicitado no descritivo: Realizar operações nos endpoints `/users` (ex: `POST /users`, `GET /users`, etc.).
Comportamento real do sistema: A documentação oficial do Swagger e as rotas ativas do ServeRest utilizam o endpoint em português: `/usuarios`.

Ação tomada: Para garantir o funcionamento da automação contra o ambiente real e validar a regra de negócio, os testes foram desenvolvidos utilizando a rota oficial `{{url_base}}/usuarios`. 
Esta divergência serve como um alerta de qualidade (Bug de Documentação/Requisito).


🛠️ Tecnologias Utilizadas

Postman Criação dos scripts de teste, asserções e manipulação de variáveis.
Newman: Execução dos testes via linha de comando (CLI).
Newman HTML Extra: Geração de relatórios visuais ricos em HTML.
itHub Actions: Pipeline de CI/CD para execução automatizada na nuvem.
JavaScript: Para scripts de validação e testes lógicos dentro do Postman.

EndPoints gerados e Testados:

1. POST /usuarios: Cria um novo usuário dinâmico e valida a criação (Status 201).
2. POST /login: Autentica com o usuário criado e salva o Token JWT no ambiente (Status 200).
3. GET /usuarios/{_id}: Busca o usuário específico validando seus dados (Status 200).
4. PUT /usuarios/{_id}: Atualiza as informações do usuário criado (Status 200).
5. GET /usuarios: Lista todos os usuários cadastrados no banco (Status 200).
6. DELETE /usuarios/{_id}: Exclui o usuário validando a limpeza da base de dados (Status 200).

Atenção ao Rate Limit: O desafio estabelece um limite de 100 requisições por minuto. 
Para garantir que a automação não sofra bloqueios de segurança (Status 429), um delay de `500ms` foi implementado entre cada requisição via Newman.


⚙️ Como Executar os Testes Localmente

Pré-requisitos
* Ter o [Node.js](https://nodejs.org/) instalado.
* Ter o Postman instalado (opcional, para visualização).

Via Interface (Postman)
1. Clone este repositório.
2. Abra o Postman e importe os arquivos `collection.json` e `environment.json` localizados na pasta `/postman`.
3. Selecione o ambiente importado no canto superior direito.
4. Execute a Collection utilizando o recurso Runner.

Via Linha de Comando (Newman CLI)
1. Abra o terminal na raiz do projeto.
2. Instale o Newman e o gerador de relatórios globalmente:
   ```bash
   npm install -g newman newman-reporter-htmlextra
   newman run ./postman/collection.json -e ./postman/environment.json --delay-request 500 -r htmlextra,cli

Como eu executei o Teste:
Para garantir a integração contínua do projeto, eu configurei uma pipeline automatizada utilizando o GitHub Actions.

Estruturei a automação para que, sempre que um novo código for enviado para a branch main, a pipeline realize automaticamente as seguintes etapas:

1-Prepara o ambiente instalando o Node.js e as dependências do Newman.
2-Executa a coleção de testes que desenvolvi, respeitando rigorosamente o Rate Limit exigido.
3-Gera o relatório visual de execução.
4-Disponibiliza o relatório em formato .html como um Artefato da pipeline. 
Configurei essa etapa para salvar a evidência independentemente de os testes passarem ou falharem, garantindo a rastreabilidade em qualquer cenário.
5-Acesse a aba Actions aqui do repositório.
6-Clique na execução mais recente (marcada com o check verde).
7-Role até o final da página e baixe o arquivo comprimido na seção Artifacts.
8-Extraia o arquivo e abra o documento HTML no seu navegador web padrão para visualizar o dashboard com os resultados detalhados.
