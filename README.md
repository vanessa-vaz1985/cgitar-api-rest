# Sobre o projeto
Estas foram as anotações que fiz durante o "Curso Gratuito de Introdução aos Testes de API Rest", ministrado por Júlio de Lima.
O curso pode ser acessado gratuitamente em https://www.youtube.com/watch?v=VqVQ7vHY32o&list=PLf8x7B3nFTl17WeEVj405tHlstiq1kNBX.

## API - O que é?
- Interface de programação de aplicações, que tem o objetivo de expor um serviço, sem precisar que os consumidores conheçam sua arquitetura.
- API é feita para que outros softwares comuniquem com ela.
- É importante que as API's sejam desenvolvidas utilizando padrões estabelecidos pela comunidade de software, para que ela tenha uma boa interoperabilidade com outros softwares.
- Para garantir uma boa interoperabilidade, a API pode ser desenvolvida utilizando modelos de arquitetura, como Rest, ou protocolos, como Soap.

## Arquitetura de uma API Rest
### Services
Camada da aplicação que geralmente armazena as regras de negócio.

### Repository
Camada da aplicação responsável por trafegar as informações entre as regras de negócio e o banco de dados, por exemplo.

### Controller
Intermediador entre quem chama a API (software terceiro) e os serviços de repositório criados na API.

## Instalando uma API Rest de Exemplo para Testes (Aula 5)
1. Baixar e instalar Java JDK 8. Para validar a instalação, executar o comando no cmd:
    java -version
2. Baixar e instalar Intellij IDEA, versão Community.
3. Baixar e instalar Maven Java.
    - Baixar o arquivo .zip.
    - Descompactar e recortar a pasta > Colar em C:\Program Files\Java.
    - Colocar o caminho C:\Program Files\Java\apache-maven-3.9.2\bin nas variáveis de ambiente.
    - Para validar a instalação, executar o comando no cmd:
        mvn -v
4. Baixar e instalar Git.
5. Acessar o repositório da API e copiar o caminho:
    https://github.com/AntonioMontanha/gerenciador-viagens
6. Criar uma pasta no seu computador, onde possa salvar o projeto.
7. Dentro da pasta criada no passo anterior, clicar com o botão direito do mouse e selecionar "Git Bash Here".
8. Na tela aberta no passo anterior executar o comando:
    git clone https://github.com/AntonioMontanha/gerenciador-viagens.git
9. Abrir o projeto no Intellij.
10. Após aguardar instalação de todas as dependência, executar o seguinte comando no Maven, dentro do Intellij:
        mvn clean install
11. Após receber a mensagem de BUILD SUCCESS, executar este novo comando no Maven, dentro do Intellij:
        mvn spring-boot:run
12. Verificar se a mensagem abaixo foi exibida no terminal do Intellij, pois isso indicará que o serviço está de pé e pode ser usado:
        Tomcat started on port(s): 8089 (http)
13. Executar o comando abaixo no terminal do Git Bash. Espera-se receber a mensagem "Aplicação está funcionando corretamente":
        curl -X GET http://localhost:8089/api/v1/status

## Como usar Headers, Swagger e Verbos em APIs Rest (Aula 6)
### Headers
- Faz parte de uma Request, onde colocamos informações técnicas para ajudar o servidor a processar a requisição.
- Nele podemos enviar informações como o Content Type (json ou xml, por exemplo), a Autenticação (um token, por exemplo).

### Swagger
- Documentação da interface que estamos interagindo.
- Para abrir o swagger deste serviço, acessar o endereço no browser:
    http://localhost:8089/api/swagger-ui.html

### Verbos em APIs Rest
- Post: Utilizado para criar um recurso.
- Get: Consultar recursos existentes.
- Put: Alterar um recurso que está no servidor.
- Delete: Apaga um recurso que está no servidor.

- Executar o comando abaixo no terminal do Git Bash:
    curl -X POST -i "http://localhost:8089/api/v1/auth" -H "accept: */*" -H "Content-Type: application/json" -d "{ \"email\": \"admin@email.com\", \"senha\": \"654321\"}"

## Autenticação e Autorização em APIs Rest (Aula 7)
- Autenticação está relacionada a quem você é, seu perfil.
- Autorização está relacionada ao que você pode ou não fazer.

- Executar o comando abaixo no terminal do Git Bash:
    curl -X POST -i "http://localhost:8089/api/v1/viagens" -H "accept: application/json" -H "Authorization: eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbkBlbWFpbC5jb20iLCJyb2xlIjoiUk9MRV9BRE1JTiIsImNyZWF0ZWQiOjE2ODYxNjcxNDAzODgsImV4cCI6MTY4NjI2NzEzOX0.4U51DMj1T9ShB7ppDD-kSJw_rgiizNGTvyK7i8R6D8aMVXOfaTE-ElYmKFZlXEfG__B2fCDxCLAgSeIfYaGRLg" -H "Content-Type: application/json" -d "{ \"acompanhante\": \"Acompanhante teste 1\", \"dataPartida\": \"2023-06-08\", \"dataRetorno\": \"2023-06-11\", \"localDeDestino\": \"Rio de Janeiro\", \"regiao\": \"Sudeste\"}"

## Header, Query, Path e Body: Como usar parâmetros em APIs Rest? (Aula 8)
- Header: -H
- Query: após o endpoint, informar ? seguido pelo nome do parâmetro. Se existir mais de um parâmetro, utilizar &
- Path: após o endpoint, informar / seguido pelo valor do parâmetro.
- Body: -d

- Comando para autenticar como usuário:
    curl -X POST -is http://localhost:8089/api/v1/auth -d '{"email": "usuario@email.com", "senha": "123456"}' -H 'Content-Type: application/json'

- Comando para consultar todas as viagens cadastradas, lembrando de substituir o token abaixo pelo token gerado no passo anterior:
    curl -X GET -is http://localhost:8089/api/v1/viagens -H 'Authorization: eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ1c3VhcmlvQGVtYWlsLmNvbSIsInJvbGUiOiJST0xFX1VTVUFSSU8iLCJjcmVhdGVkIjoxNjg2MTcyOTQyOTczLCJleHAiOjE2ODYyNzI5NDF9.ClB_uj_PO_cv8srvwURxY2YTDC7cMYxLcY6iW4OSWvZFfSZIciHvlI9vvnXSpCRFaGDEGUOXWgRwgbVkKeeudw'

- Comando para consultar todas as viagens cadastradas de determinada região, passando a região por query:
    curl -X GET -is http://localhost:8089/api/v1/viagens?regiao=Sudeste -H 'Authorization: eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ1c3VhcmlvQGVtYWlsLmNvbSIsInJvbGUiOiJST0xFX1VTVUFSSU8iLCJjcmVhdGVkIjoxNjg2MTcyOTQyOTczLCJleHAiOjE2ODYyNzI5NDF9.ClB_uj_PO_cv8srvwURxY2YTDC7cMYxLcY6iW4OSWvZFfSZIciHvlI9vvnXSpCRFaGDEGUOXWgRwgbVkKeeudw'

- Comando para consultar todas as viagens cadastradas de determinada região, passando a região por path:
    curl -X GET -is http://localhost:8089/api/v1/viagens/1 -H 'Authorization: eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ1c3VhcmlvQGVtYWlsLmNvbSIsInJvbGUiOiJST0xFX1VTVUFSSU8iLCJjcmVhdGVkIjoxNjg2MTcyOTQyOTczLCJleHAiOjE2ODYyNzI5NDF9.ClB_uj_PO_cv8srvwURxY2YTDC7cMYxLcY6iW4OSWvZFfSZIciHvlI9vvnXSpCRFaGDEGUOXWgRwgbVkKeeudw'

## Como usar o Insomnia para Testar APIs Rest (Aula 9)
1. Baixar e instalar o Insomnia, versão Core.

## Como usar o Postman para Testar APIs Rest (Aula 10)
1. Baixar e instalar o Postman.

## Como automatizar testes de API Rest com RestAssured (Aula 11)
1. Incluir a dependência do JUnit 4.12 no arquivo pom.xml do serviço.
    https://mvnrepository.com/artifact/junit/junit/4.12

2. Incluir a dependência do RestAssured 3.0.0 no arquivo pom.xml do serviço.
    https://mvnrepository.com/artifact/io.rest-assured/rest-assured/3.0.0

3. Após incluir a dependência do RestAssured, possivelmente ficará destacado em vermelho. Para resolver este problema, clique com o botão direito no arquivo pom.xml > Maven > Reload project.

## Como fazer estratégias de Testes de API Rest (Aula 12)
- Uma das opções para definir a estratégia de teste é o Mapa Mental. Alguns ferramentas que possibilitam a criação de Mapa Mental:
    - XMind
    - Miro
    - MindMaps
- Nele podemos definir:
    - Massas: 1 viagem registrada, 1 email e senha de um usuário comum (não administrador)
    - Integrações: API do Tempo
    - Notas: Um dos desenvolvedores comentou que normalmente a API do Tempo fica fora do ar
    - Riscos: A API do Tempo não estar disponível durante o desenvolvimento, prejudicando as estimativas de data que foram fornecidas
    - Ferramentas: Postman, RestAssured - JUnit, Intellij, Maven
    - Abordagens: Testes Baseados em Riscos - Projetar testes para mitigar Riscos - Priorizo a execução dos testes relacionados aos riscos com maior impacto e probabilidade de acontecer
                    Testes Exploratórios
    - Técnicas: Partição de Equivalência - Viagem - Partições - Existente/Inexistente
                                        - Credenciais de Usuário - Partições - Usuário/Administrador/Inválida
                Permutação: Combinar todas as possibilidades das partiçoes de equivalência
    - Tipos: Funcionais - Olhar para os requisitos e a conformidade com eles
            Não Funcionais - Performance/Compatibilidade/Segurança - ISO 25010 - Características de Qualidade
            Estruturais - Arquitetura da API Rest está de acordo com as recomendações gerais do estilo de Arquitetura Rest

## Como medir a Cobertura de Testes de API Rest? (Aula 13)
Sugestões de leitura para entender melhor sobre o assunto:
- Workshop Alberto Martin-Lopez, Sergio Segura, Antonio Ruiz-Cortés
    https://personal.us.es/amarlop/wp-content/uploads/2019/09/Test_Coverage_Criteria_for_RESTful_Web_APIs.pdf

- Artigo da Nayara Crema
    https://medium.com/revista-dtar/como-verificar-a-cobertura-de-testes-da-api-rest-9e2f745564b

## Como fazer Testes Funcionais em API Rest? (Aula 14)
É muito importante identificar quais as funcionalidades definidas na Regras de Negócio, para levantar todos os Testes Funcionais necessários.

## Como fazer testes de performance em API Rest? (Aula 15)
1. Baixar o Apache JMeter 5.2.1.
2. Extrair o arquivo e colocar no diretório desejado.
3. Localizar o arquivo "jmeter" do tipo batch, dentro da pasta bin, e executá-lo para abrir o Jmeter.
4. Sugestão de leitura do artigo:
    https://medium.com/revista-tspi/testes-de-performance-n%C3%A3o-s%C3%A3o-t%C3%A3o-simples-quanto-usar-o-jmeter-2beedf224344

## Como fazer testes de compatibilidade em API Rest? (Aula 16)
1. Baixar a versão v1.2.2 do swagger-diff e salvar no diretório desejado:
    https://github.com/Sayi/swagger-diff
2. No diretório onde o arquivo estiver, executar o seguinte comando no Git Bash. Este comando vai mostrar as diferenças entre os arquivos no próprio terminal:
    java -jar swagger-diff.jar -old swagger-antigo.json -new swagger-novo.json
3. Para exportar as diferenças entre os arquivos para um arquivo, executar o comando:
    java -jar swagger-diff.jar -old swagger-antigo.json -new swagger-novo.json -output-mode html > diff.html

## Como criar Mock para testes de API Rest? (Aula17)
1. Baixar o jar do WireMock e salvar no diretório desejado:
    https://wiremock.org/docs/download-and-installation/
2. No diretório onde o arquivo estiver, executar o seguinte comando no Git Bash.
    java -jar wiremock-jre8-standalone-2.35.0.jar

## Como validar a estrutura do corpo da requisição com JsonSchema (Aula 18)
1. Acessar o JsonSchema:
    https://jsonschema.net/app/schemas/0
2. Copiar o Response do cadastro de viagem, já validado, e colar no JsonSchema.
3. Submeter o Response acima e copiar o Schema retornado.

## Como ler logs de APIs Rest? (Aula 19)
Alguns níveis de log:
- DEBUG: Usado para depurar, tempo de desenvolvimento.
- TRACE: Usado para depurar, tempo de produção.
- INFO: Informações importantes sobre o progresso do uso.
- WARN: Situação potencialmente perigosa.
- ERROR: Erros que não impedem o uso da aplicação.
- FATAL: Erros que impedem o uso da aplicação.
