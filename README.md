# Desenvolvendo um Cliente de E-mail com API: Envio e Recebimento de E-mails


Este projeto implementa um cliente de e-mail de página única usando Django no backend e JavaScript no frontend. Os usuários podem enviar, receber e arquivar mensagens, todas armazenadas em um banco de dados local.

![image](https://github.com/user-attachments/assets/d48767a4-34af-4691-a47f-08742d8976d7)

## Project Overview

Este projeto implementa um cliente de e-mail de página única usando Django no backend e JavaScript no frontend. Os usuários podem enviar, receber e arquivar mensagens, todas armazenadas em um banco de dados local. A configuração inicial envolve fazer e aplicar migrações para o projeto e, em seguida, executar o servidor web com o comando python manage.py runserver. Após abrir o servidor web no navegador, os usuários podem se registrar com qualquer endereço de e-mail e senha, pois os e-mails enviados e recebidos são armazenados localmente.

A interface do usuário é altamente interativa, graças ao uso de JavaScript. Após o login, os usuários são direcionados para a página da Caixa de Entrada, que inicialmente estará em branco. Eles podem navegar entre as caixas de correio Enviadas e Arquivadas usando botões, e clicar no botão "Escrever" para acessar um formulário de composição de e-mail. Este aplicativo é uma página única, onde o JavaScript controla a exibição e ocultação das seções de visualização de e-mails e composição.

No backend, as rotas da API são definidas para obter, enviar e atualizar e-mails. Por exemplo, a rota GET /emails/<mailbox> obtém todos os e-mails em uma caixa de correio específica, enquanto POST /emails envia um novo e-mail. O código Django para essas rotas inclui funções que interagem com o banco de dados para recuperar e salvar e-mails.

No frontend, o arquivo JavaScript inbox.js é responsável por anexar ouvintes de eventos aos botões e manipular a interface do usuário. Quando um botão é clicado, funções JavaScript são chamadas para carregar a caixa de correio correspondente ou exibir o formulário de composição. A função load_mailbox faz uma solicitação fetch para a API para obter os e-mails e atualizar a interface do usuário. A função compose_email exibe o formulário de composição e limpa os campos de entrada.

Este projeto demonstra como criar um cliente de e-mail funcional e interativo usando Django e JavaScript. A integração com a API permite enviar, receber e gerenciar e-mails de forma eficiente, proporcionando uma experiência de usuário fluida e responsiva. 🚀


