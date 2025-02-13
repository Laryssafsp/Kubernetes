# Variável de ambiente

Usar variáveis de ambiente para configurar valores no Kubernetes facilita a manutenção e a personalização das configurações, além de melhorar a segurança, pois evita hardcoding de credenciais no código.

* O hardcoding de credenciais no código é o ato de inserir diretamente informações sensíveis, como senhas, chaves de API, tokens de autenticação, nomes de usuário, entre outros, diretamente no código-fonte da aplicação. Isso significa que essas informações ficam visíveis no código e podem ser acessadas por qualquer pessoa que tenha acesso a esse código.

* Resumo sobre o uso de variáveis de ambiente no Kubernetes:

É recomendável modificar a aplicação para que valores sensíveis, como client_id e password, sejam lidos através de variáveis de ambiente. Isso melhora a segurança e facilita a configuração, já que as credenciais ficam fora do código-fonte e podem ser gerenciadas de forma centralizada no ambiente de execução.

No Kubernetes, você pode definir as variáveis de ambiente para credenciais, como:

* POWERBI_CLIENT_ID
* POWERBI_PASSWORD
* 
Exemplo de como implementar:

No código Python, ao invés de hardcoding, você acessa as variáveis de ambiente da seguinte forma:

``` python
import os

client_id = os.environ["POWERBI_CLIENT_ID"]
pbi_password = os.environ["POWERBI_PASSWORD"]

body = {
  "client_id": client_id,
  "grant_type": "password",
  "resource":"https://analysis.windows.net/powerbi/api",
  "username":"xxxxx@xxxx.com",
  "password": pbi_password,
  "admin_consent":"True"
}

```

ou

``` python
import os

# Recupera a chave de API a partir de uma variável de ambiente
api_key = os.getenv('API_KEY')

# Utiliza a chave de API no código
print(f"A chave de API é: {api_key}")

```

# A diferença entre os.getenv('API_KEY') e os.environ["API_KEY"]

A diferença entre os.getenv('API_KEY') e os.environ["API_KEY"] está no comportamento quando a variável de ambiente não está definida:

## 1. os.getenv('API_KEY'):
* Comportamento: Se a variável de ambiente API_KEY não estiver definida, os.getenv('API_KEY') retorna None por padrão.
* Uso recomendado: Usado quando você pode querer tratar a falta da variável de ambiente sem gerar um erro imediatamente, permitindo que o código continue com um valor None ou outro valor default, caso a variável não exista.

```python
api_key = os.getenv('API_KEY')
if api_key is None:
    print("API_KEY não está definida!")
```

Ponto importante: Você pode fornecer um valor padrão caso a variável de ambiente não exista, como em os.getenv('API_KEY', 'default_value'), onde 'default_value' seria o valor a ser retornado se a variável não estiver definida.

## 2. os.environ["API_KEY"]:
* Comportamento: Se a variável de ambiente API_KEY não estiver definida, um erro KeyError será gerado.
* Uso recomendado: Usado quando você exige que a variável de ambiente esteja definida e não quer que o código continue se ela estiver ausente. Isso ajuda a identificar problemas de configuração.

```python
try:
    api_key = os.environ["API_KEY"]
except KeyError:
    print("API_KEY não está definida!")
```
Ponto importante: Este método é útil quando a falta da variável de ambiente deve ser tratada como um erro e a aplicação não deve continuar sem ela.

#### Resumo da diferença:
- os.getenv('API_KEY'): Retorna None se a variável não for encontrada, permitindo um tratamento mais flexível.
- os.environ["API_KEY"]: Gera um erro KeyError se a variável não for encontrada, forçando a aplicação a tratar a ausência da variável como um erro.
