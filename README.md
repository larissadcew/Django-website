# Desenvolvendo um Cliente de E-mail com API: Envio e Recebimento de E-mails


Este projeto implementa um cliente de e-mail de página única usando Django no backend e JavaScript no frontend. Os usuários podem enviar, receber e arquivar mensagens, todas armazenadas em um banco de dados local.

![image](https://github.com/user-attachments/assets/d48767a4-34af-4691-a47f-08742d8976d7)

## Project Overview

Este projeto implementa um cliente de e-mail de página única usando Django no backend e JavaScript no frontend. Os usuários podem enviar, receber e arquivar mensagens, todas armazenadas em um banco de dados local. A configuração inicial envolve fazer e aplicar migrações para o projeto e, em seguida, executar o servidor web com o comando python manage.py runserver. Após abrir o servidor web no navegador, os usuários podem se registrar com qualquer endereço de e-mail e senha, pois os e-mails enviados e recebidos são armazenados localmente.

A interface do usuário é altamente interativa, graças ao uso de JavaScript. Após o login, os usuários são direcionados para a página da Caixa de Entrada, que inicialmente estará em branco. Eles podem navegar entre as caixas de correio Enviadas e Arquivadas usando botões, e clicar no botão "Escrever" para acessar um formulário de composição de e-mail. Este aplicativo é uma página única, onde o JavaScript controla a exibição e ocultação das seções de visualização de e-mails e composição.

No backend, as rotas da API são definidas para obter, enviar e atualizar e-mails. Por exemplo, a rota GET /emails/<mailbox> obtém todos os e-mails em uma caixa de correio específica, enquanto POST /emails envia um novo e-mail. O código Django para essas rotas inclui funções que interagem com o banco de dados para recuperar e salvar e-mails.

# views.py

```python
from django.http import JsonResponse
from .models import Email

def get_emails(request, mailbox):
    emails = Email.objects.filter(user=request.user, mailbox=mailbox)
    return JsonResponse([email.serialize() for email in emails], safe=False)

def send_email(request):
    data = json.loads(request.body)
    email = Email(
        user=request.user,
        recipients=data["recipients"],
        subject=data["subject"],
        body=data["body"],
        mailbox="sent"
    )
    email.save()
    return JsonResponse({"message": "Email sent successfully."}, status=201)
```

Enviar uma solicitação para GET /emails/<mailbox>, onde <mailbox> é inbox, sent ou archive, retornará uma lista de todos os e-mails nessa caixa de correio, em ordem cronológica inversa. Por exemplo, se você enviar uma solicitação para GET /emails/inbox, poderá obter uma resposta JSON como a abaixo (representando dois e-mails):
```python

[
    {
        "id": 100,
        "sender": "foo@example.com",
        "recipients": ["bar@example.com"],
        "subject": "Hello!",
        "body": "Hello, world!",
        "timestamp": "Jan 2 2020, 12:00 AM",
        "read": false,
        "archived": false
    },
    {
        "id": 95,
        "sender": "baz@example.com",
        "recipients": ["bar@example.com"],
        "subject": "Meeting Tomorrow",
        "body": "What time are we meeting?",
        "timestamp": "Jan 1 2020, 12:00 AM",
        "read": true,
        "archived": false
    }
]
```


Observe que cada e-mail especifica seu id (um identificador exclusivo), sender (um endereço de e-mail), uma matriz de recipients, uma string para subject, body, e timestamp, bem como dois valores booleanos indicando se o e-mail foi read e se o e-mail foi archived.

No frontend, o arquivo JavaScript inbox.js é responsável por anexar ouvintes de eventos aos botões e manipular a interface do usuário. Quando um botão é clicado, funções JavaScript são chamadas para carregar a caixa de correio correspondente ou exibir o formulário de composição. A função load_mailbox faz uma solicitação fetch para a API para obter os e-mails e atualizar a interface do usuário. A função compose_email exibe o formulário de composição e limpa os campos de entrada

```python
// inbox.js
document.addEventListener('DOMContentLoaded', function() {
    document.querySelector('#inbox').addEventListener('click', () => load_mailbox('inbox'));
    document.querySelector('#sent').addEventListener('click', () => load_mailbox('sent'));
    document.querySelector('#archived').addEventListener('click', () => load_mailbox('archive'));
    document.querySelector('#compose').addEventListener('click', compose_email);
});

function load_mailbox(mailbox) {
    fetch(`/emails/${mailbox}`)
        .then(response => response.json())
        .then(emails => {
            // Print emails
            console.log(emails);
        
            // ... do something else with emails ...
        });
}

function compose_email() {
    document.querySelector('#compose-view').style.display = 'block';
    document.querySelector('#emails-view').style.display = 'none';
    document.querySelector('#compose-recipients').value = '';
    document.querySelector('#compose-subject').value = '';
    document.querySelector('#compose-body').value = '';
}
```

No frontend, o arquivo JavaScript inbox.js é responsável por anexar ouvintes de eventos aos botões e manipular a interface do usuário. Quando um botão é clicado, funções JavaScript são chamadas para carregar a caixa de correio correspondente ou exibir o formulário de composição. A função load_mailbox faz uma solicitação fetch para a API para obter os e-mails e atualizar a interface do usuário. A função compose_email exibe o formulário de composição e limpa os campos de entrada.

Este projeto demonstra como criar um cliente de e-mail funcional e interativo usando Django e JavaScript. A integração com a API permite enviar, receber e gerenciar e-mails de forma eficiente, proporcionando uma experiência de usuário fluida e responsiva. 🚀


