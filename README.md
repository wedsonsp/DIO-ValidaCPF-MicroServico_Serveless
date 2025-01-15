# Passos para Criação do Projeto

Seguem os passos da criação do referido projeto abaixo:

## Criação da Função

Criada conforme no **VSCode**, logado com a conta do Azure e com a **Extensão Azure Function** instalada, a função `fnwsouza-ctru002` com a conta do Azure logada no VSCode: `fnwsouza-ctru002` na região **Central US**.

### Erro Encontrado

Estava dando o erro: `You are signed in now and can close this page.` Como posso criar um nome para outra região que possui mais instâncias, como por exemplo a região Central?

Tentei um nome distinto, pois pode ser que já existia esse nome, e também troquei a região.

## Passo a Passo no Terminal

### 1. Criando o diretório para o projeto

```powershell
PS C:\Users\gabri\Documents\DIO\HandsOn Serveless Para validar CPF> mkdir httpValidaCpf

Directory: C:\Users\gabri\Documents\DIO\HandsOn Serveless Para validar CPF

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----          15/01/2025    11:33                httpValidaCpf

### 2. Acessando o diretório

PS C:\Users\gabri\Documents\DIO\HandsOn Serveless Para validar CPF> cd .\httpValidaCpf

### 3. Inicializando o projeto

PS C:\Users\gabri\Documents\DIO\HandsOn Serveless Para validar CPF\httpValidaCpf> func init --worker-runtime dotnet

### 4. Verificando os arquivos criados

PS C:\Users\gabri\Documents\DIO\HandsOn Serveless Para validar CPF\httpValidaCpf> ls

Directory: C:\Users\gabri\Documents\DIO\HandsOn Serveless Para validar CPF\httpValidaCpf

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----          15/01/2025    11:36                .vscode
d----          15/01/2025    11:36                Properties
-a---          15/01/2025    11:36           4626 .gitignore
-a---          15/01/2025    11:36            274 host.json
-a---          15/01/2025    11:36            628 httpValidaCpf.csproj
-a---          15/01/2025    11:36            163 local.settings.json

### 5. Criando a função

PS C:\Users\gabri\Documents\DIO\HandsOn Serveless Para validar CPF\httpValidaCpf> func new

Escolha a opção HttpTrigger para criar a função.

Function name: fnvalidacpf
fnvalidacpf

Creating dotnet function...
The function "fnvalidacpf" was created successfully from the "HttpTrigger" template.

### 6. Iniciando a função localmente

PS C:\Users\gabri\Documents\DIO\HandsOn Serveless Para validar CPF\httpValidaCpf> func start

A saída será algo assim:

Functions:
        fnvalidacpf: [GET,POST] http://localhost:7071/api/fnvalidacpf


Abra o Postman no método GET e cole o endereço:

http://localhost:7071/api/fnvalidacpf

This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.

Passar o parâmetro ?name=Wedson:
http://localhost:7071/api/fnvalidacpf?name=Wedson

Agora funcionou:
Hello, Wedson. This HTTP triggered function executed successfully.

### 7. Codificando a validação do CPF
No VSCode, abra o arquivo fnvalidacpf e faça as alterações necessárias para implementar a validação do CPF.

Após a implementação, teste novamente no Postman:

Se for testado com GET, será mostrado o erro 404 Not Found.

Se for testado com POST, será mostrada a mensagem:
Por favor, informe o CPF.

No Postman, coloque o JSON com a informação em Body > Raw > JSON:
{
    "cpf": "19920175064"
}

O CPF 19920175064 é válido, não consta na base de dados de fraudes e não consta na base de dados de débitos.

Publicando a Função no Azure
PS C:\Users\gabri\Documents\DIO\HandsOn Serveless Para validar CPF\httpValidaCpf> func azure functionapp publish fnwsouza-ctru002

A saída será:
Deployment completed successfully.

A URL de invocação da função será:
https://fnwsouza-ctru002.azurewebsites.net/api/fnvalidacpf

### 8. Testando a Função no Postman
No Postman, você verá o erro 401 Unauthorized. Isso acontece porque agora precisamos fornecer a chave de autenticação.

Acesse a Função no Azure, no menu lateral esquerdo, em Function > App Keys, copie a chave em default.

No Postman, em Params, no campo Key, digite code e, no campo Value, cole a chave copiada:


Agora a validação funcionará corretamente, assim como na URL local.



