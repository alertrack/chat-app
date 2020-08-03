# Desafio

O desafio consiste em desenvolver um aplicativo android de chat (Como o telegram e WhatsApp) fazendo o uso das seguintes tecnologias:

* Kotlin / Java ou Dart (Flutter)
* Persistência de dados (SharedPreferences)
* Comunicação com APIs

Bibliotecas recomendadas:

Flutter -> https://pub.dev/packages/shared_preferences</br>
Kotlin / Java -> https://square.github.io/retrofit/ (Requisições)</br>
Flutter -> https://pub.dev/packages/http (Requisições)

### Funcionamento

O aplicativo será composto de três telas, seu fluxo será: 

###### Autenticação -> Lista de conversas -> Conversa

<img src="https://raw.githubusercontent.com/alertrack/alertrack_teste_mobile/master/fluxo.png" width="auto" height="500">

##### - Autenticação -

<img src="https://raw.githubusercontent.com/alertrack/alertrack_teste_mobile/master/login.png" width="auto" height="500">

Para ter acesso ao resto da aplicação o usuário terá que efetuar uma autenticação com seu login e senha. Para isso será feita uma requisição do tipo POST enviando os dados da autenticação como no exemplo abaixo:

###### POST -> http://alertrack.com.br/api/teste_mobile/auth.php
```
{
  "login": "meulogin",
  "senha": "123"
}
```

###### Resposta
```
{
  "status": true,
  "message": "Autenticado com sucesso",
  "token": "9bdf52a4830779a1383ac24f1b3ed054",
  "user": {
    "avatar": "http://www.alertrack.com.br/api/teste_mobile/img/perfil_.png",
    "name": "Meu nome",
    "email": "meuemail@alertrack.com.br"
  }
}
```

Após a resposta deverá ser persistido os dados do usuário autenticado usando SharedPreferences para não ter a necessidade de efetuar a autenticação novamente ao iniciar a aplicação, tornando a tela de lista de conversas como a principal. Os dados enviados para requisição são fictícios não existindo nenhum tipo de validação real.

##### - Lista de conversas -

<img src="https://raw.githubusercontent.com/alertrack/alertrack_teste_mobile/master/home.png" width="auto" height="500">

O usuário terá uma lista de conversas para continuar conversando com seus contatos, a lista será composta do nome do contato, imagem de perfil e a última mensagem desta conversa. Nesta tela deverá conter o nome do usuário logado junto sua imagem de perfil. O usuário poderá também efetuar o logout, limpando todos os dados persistidos tendo a necessidade de efetuar novamente sua autenticação. Para popular esta lista deverá ser feito uma requisição do tipo POST passando o token do usuário autenticado como no exemplo abaixo:


###### POST -> http://alertrack.com.br/api/teste_mobile/chats.php
```
{
  "token": "9bdf52a4830779a1383ac24f1b3ed054"
}
```

###### Resposta
```
{
  "status": true,
  "chat": [
    {
      "contact": {
        "id": 0,
        "avatar": "http://www.alertrack.com.br/api/teste_mobile/img/perfil1_.png",
        "name": "Contato 0"
      },
      "last_msg": "mensagem 0"
    },
    {
      "contact": {
        "id": 1,
        "avatar": "http://www.alertrack.com.br/api/teste_mobile/img/perfil1_.png",
        "name": "Contato 1"
      },
      "last_msg": "mensagem 1"
    },
    {
      "contact": {
        "id": 2,
        "avatar": "http://www.alertrack.com.br/api/teste_mobile/img/perfil1_.png",
        "name": "Contato 2"
      },
      "last_msg": "mensagem 2"
    }
  ]
}
```

##### - Conversa -

<img src="https://raw.githubusercontent.com/alertrack/alertrack_teste_mobile/master/chat.png" width="auto" height="500">

Cada conversa terá seu histórico de mensagens, esse histórico será uma lista de mensagens que serão obtidas por requisição GET passando como parâmetro o id do contato em conversa, é necessário também pessar o "token" pelo header. Vejamos o exemplo abaixo:


###### GET -> http://alertrack.com.br/api/teste_mobile/msgs.php?contact_id=1

###### Resposta
```
{
  "status": true,
  "msgs": [
    {
      "id": 0,
      "from_me": true,
      "message": "mensagem 0",
      "timestamp": 1594736520
    },
    {
      "id": 1,
      "from_me": false,
      "message": "mensagem 1",
      "timestamp": 1594736700
    },
    {
      "id": 2,
      "from_me": false,
      "message": "mensagem 2",
      "timestamp": 1594736760
    }
  ]
}
```

Para diferenciar se a mensagem é do tipo enviada pelo contato ou usuário utilizamos o atributo "from_me", onde true se foi enviada pelo usuário e false pelo contato.
O usuário poderá enviar mensagens também, estas mensagens serão apenas para ser populada na lista de mensagens não havendo, portanto, a necessidade de ser feita qualquer tipo de requisição ou armazenamento, sendo removidas a cada vez que sair da tela da conversa.
