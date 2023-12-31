# Pipeline-dados-Ebac-44
Este repositório destina-se para realização de um Pipeline de dados em nuvem

# Introdução
O presente projeto foi desenvolvido como conclusão do curso Profissão Analista de Dados da Escola Britânica de Artes Criativas & Tecnologia EBAC e tem como objetivo a construção de um pipeline de dados em nuvem.

O pipeline de dados foi desenvolvido para ingerir, processar e armazenar mensagens de texto disparadas em um grupo no aplicativo de mensagens Telegram, junto ao grupo foi adicionado um bot, criado dentro da própria plataforma, responsável por captar as mensagens e encaminha-lás via API para a plataforma cloud Amazon Web Services AWS, onde as mensagens foram processadas, armazenadas, limpas e analisadas.

# Chat bot
O chatbot é uma aplicação de software que utiliza Inteligência Artificial (IA) e processamento de Linguagem Natural (NLP) para ajudar seus utilizadores a interagir com as aplicações ou serviços Web através de texto, imagens gráficas ou voz.

Porque usar o chatbot?Eles podem servir de agentes interativos e assistentes digitais no atendimento ao cliente, por exemplo:
Resolução de problemas de suporte a cliente e esclarecimento de dúvidas recorrentes;
Acelerar ciclos de vendas, entregando para o cliente o máximo de informação possível, gerando mais oportunidades e melhorando a fidelização dos clientes;

# Telegram
O Telegram é um aplicativo de mensagens instantâneas freeware, ou seja, distribuído gratuitamente, e em sua maioria, open source. É popular entre desenvolvedores por ser pioneiro na implantação da funcionalidade de criação de chatbots, que por sua vez, permitem a criação de diversas automações. Está disponível para smartphones e tablets (Android, IOS, Windows phone, Firefox, Ubuntu) e também aplicação Web.

# Objetivo
Realizar a análise exploratória dos dados enviados ao bot, para responder perguntas como:

Qual horário os usuários mais acionam o bot?
Qual dúvida ou problema mais recorrente?
O bot está conseguindo esclarecer as dúvidas?

# 1. Ingestão
No projeto, as mensagens capturadas pelo bot podem ser ingeridas através da API web de bots do Telegram, portanto são fornecidos no formato JSON. Como o Telegram retem mensagens por apenas 24h em seus servidores, a ingestão via streaming é a mais indicada. Para que seja possível esse tipo de ingestão, vamos utilizar um webhook (gancho web), ou seja, vamos redirecionar as mensagens automaticamente para outra API web.

Sendo assim, precisamos de um serviço da AWS que forneça um API web para receber os dados redirecionados, o AWS API Gateway. Dentre suas diversas funcionalidades permite o redirecionamento do dado recebido para outros serviços da AWS. Logo, vamos conecta-lo ao AWS Lambda, que pode sua vez, irá armazenar o dado em seu formato original (JSON) em um bucket do AWS S3.

Uma requisição HTTP com o conteúdo da mensagem em seu payload é recebia pelo AWS API Gateway que, por sua vez, as redireciona para o AWS Lambda, servindo assim como seu gatilho. Já o AWS Lambda recebe o payload da requisição em seu parâmetro event, salva o conteúdo em um arquivo no formato JSON (original, mesmo que o payload) e o armazena no AWS S3 particionado por dia.

Portanto precisamos

Criar um bucket no AWS S3;
Criar uma função no AWS Lambda;
Criar uma API web no AWS API Gateway;
Configurar o webhook da API de bots do Telegram.

# AWS Lambda
Na etapa de ingestão, o AWS Lambda tem a função de ativamente persistir as mensagens captadas pelo bot do Telegram em um bucket do AWS S3.

Para tanto vamos criar uma função que opera da seguinte forma:

Recebe a mensagem no parâmetro event;
Verifica se a mensagem tem origem no grupo do Telegram correto;
Persiste a mensagem no formato JSON no bucket do AWS S3;
Retorna uma mensagem de sucesso (código de retorno HTTP igual a 200) a API de bots do Telegram.
