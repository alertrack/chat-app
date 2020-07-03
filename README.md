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

##### - Autenticação -

Para ter acesso ao resto da aplicação o usuário terá que efetuar uma autenticação com seu login e senha. Para isso será feita uma requisição do tipo POST enviando os dados da autenticação como no exemplo abaixo:

###### POST -> http://www.alertrack.com.br/auth
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
    "avatar": "http://www.alertrack.com.br/avatar.jpg",
    "name": "Meu nome",
    "email": "meuemail@alertrack.com.br"
  }
}
```

Após a resposta deverá ser persistido os dados do usuário autenticado usando SharedPreferences para não ter a necessidade de efetuar a autenticação novamente ao iniciar a aplicação, tornando a tela de lista de conversas como a principal. Os dados enviados para requisição são fictícios não existindo nenhum tipo de validação real.

##### - Lista de conversas -

O usuário terá uma lista de conversas para continuar conversando com seus contatos, a lista será composta do nome do contato, imagem de perfil e a última mensagem desta conversa. Nesta tela deverá conter o nome do usuário logado junto sua imagem de perfil. O usuário poderá também efetuar o logout, limpando todos os dados persistidos tendo a necessidade de efetuar novamente sua autenticação. Para popular esta lista deverá ser feito uma requisição do tipo POST passando o token do usuário autenticado como no exemplo abaixo:


###### POST -> http://www.alertrack.com.br/chats
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
        "avatar": "http://www.alertrack.com.br/avatar0.jpg",
        "name": "Contato 0"
      },
      "last_msg": "mensagem 0"
    },
    {
      "contact": {
        "id": 1,
        "avatar": "http://www.alertrack.com.br/avatar1.jpg",
        "name": "Contato 1"
      },
      "last_msg": "mensagem 1"
    },
    {
      "contact": {
        "id": 2,
        "avatar": "http://www.alertrack.com.br/avatar2.jpg",
        "name": "Contato 2"
      },
      "last_msg": "mensagem 2"
    }
  ]
}
```

##### - Conversa -

Cada conversa terá seu histórico de mensagens, esse histórico será uma lista de mensagens que serão obtidas por requisição GET passando como parâmetro o id do contato em conversa. Vejamos o exemplo abaixo:


###### GET -> http://www.alertrack.com.br/msgs/id_do_contato

###### Resposta
```
{
  "status": true,
  "msgs": [
    {
      "id": 0,
      "from_me": true,
      "message": "mensagem 0"
    },
    {
      "id": 1,
      "from_me": false,
      "message": "mensagem 1"
    },
    {
      "id": 2,
      "from_me": false,
      "message": "mensagem 2"
    }
  ]
}
```

Para diferenciar se a mensagem é do tipo enviada pelo contato ou usuário utilizamos o atributo "from_me", onde true se foi enviada pelo usuário e false pelo contato.
O usuário poderá enviar mensagens também, estas mensagens serão apenas para ser populada na lista de mensagens não havendo, portanto, a necessidade de ser feita qualquer tipo de requisição ou armazenamento, sendo removidas a cada vez que sair da tela da conversa.
