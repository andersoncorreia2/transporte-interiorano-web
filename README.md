Documentação do Software: Transporte Interiorano
1. Visão Geral do Projeto
O Transporte Interiorano é uma plataforma de mobilidade urbana projetada para conectar motoristas e passageiros, focando no transporte intermunicipal e rotas no interior. O sistema oferece uma solução híbrida que atende a duas necessidades distintas: planejamento antecipado de viagens e solicitações de corridas em tempo real.

O ecossistema é composto por um aplicativo nativo Android, um portal Web Progressivo e uma API RESTful robusta.

2. Arquitetura e Tecnologias (Tech Stack)
O projeto segue uma arquitetura Cliente-Servidor (Client-Server), com processamento centralizado na nuvem e interfaces distribuídas.

Aplicativo Mobile (Android): Desenvolvido nativamente em Kotlin utilizando o moderno toolkit de UI Jetpack Compose.

Aplicação Web (PWA): Construída com HTML5, JavaScript Vanilla e estilizada com Tailwind CSS.

Backend (API REST): Desenvolvido em Python utilizando o microframework Flask.

Banco de Dados: PostgreSQL relacional hospedado na nuvem.

Hospedagem e Deploy: Frontend Web via GitHub Pages e Backend via Render.

3. Módulos e Funcionalidades Principais
3.1. Gestão de Identidade e Contas
Perfis Dinâmicos: Um único usuário pode atuar como Passageiro ou Motorista, ativando a chave "Ofertar Corridas" em seu perfil.

Cadastro Abrangente: Coleta de dados essenciais (Nome, CPF, Data de Nascimento, Gênero, Endereço completo) e dados do veículo (Tipo, Modelo, Placa e Vagas).

Recuperação de Senha: Envio de código OTP de 6 dígitos via e-mail (Token expirável em 10 minutos).

Validação de Segurança: Verificação de existência prévia de CPF, e-mail e geração de sugestões de nomes de usuário (Username) caso o desejado esteja indisponível.

3.2. Modalidade: Viagens Programadas (Caronas)
Focada no agendamento antecipado de rotas (ex: viagens intermunicipais ou eventos específicos).

Criação de Eventos: Motoristas publicam rotas definindo Origem, Destino, Data/Hora, Vagas e Valor.

Reserva de Vagas: Passageiros solicitam entrada na carona. O motorista tem a autonomia de "Aceitar" ou "Recusar" cada pedido.

Gestão de Prazos: Acompanhamento dinâmico do status da solicitação (Pendente, Aceito Integral, Reserva Paga, Finalizado).

3.3. Modalidade: Corridas Emergenciais (Estilo "Uber")
Focada em solicitações em tempo real com rastreamento GPS.

Radar do Motorista: Motoristas ativam a modalidade "Emergencial" para começarem a receber chamados próximos através de long polling.

Geocodificação: Conversão de endereços em coordenadas de latitude e longitude.

Tracking em Tempo Real: Sincronização contínua da localização do motorista até o encontro com o passageiro.

Controle de Estados: Transição dinâmica de status: Procurando -> Aceita -> Em Viagem -> Finalizada.

3.4. Motor Financeiro e Antifraude
Integração com Gateway: Geração de Pix Copia e Cola (Checkout Transparente).

Taxa de Reserva vs. Pagamento Integral: Passageiros podem pagar uma taxa parcial (ex: R$ 5,00) para segurar a vaga por 24 horas, ou quitar o valor total.

Bloqueio de Inadimplência (Calote): Se uma corrida emergencial for finalizada pelo motorista sinalizando falta de pagamento, o passageiro é bloqueado automaticamente em todo o ecossistema até a quitação do débito pendente.

4. Estrutura do Banco de Dados (Schema)
O banco de dados relacional é estruturado nas seguintes tabelas centrais:

usuarios: Armazena dados pessoais, credenciais, endereço, métricas de corridas e flags de segurança (bloqueado, identidade_validada).

caronas: Registra os eventos programados ofertados pelos motoristas.

solicitacoes: Relaciona passageiros às caronas programadas, controlando status de aceitação e prazos de pagamento.

corridas_emergentes: Registra viagens em tempo real, armazenando coordenadas GPS (Origem, Destino, Posição do Motorista).

debitos_passageiros: Tabela de auditoria financeira para rastrear corridas não pagas e gerenciar bloqueios.

codigos_recuperacao: Tabela temporária para gerenciar tokens de redefinição de senha.

5. Integrações de Terceiros (APIs)
Mercado Pago: Geração de transações via Pix e verificação de status de pagamento.

Mapbox API: Geocodificação reversa e auto-sugestão de endereços precisos baseados na proximidade do GPS atual.

Firebase Cloud Messaging (FCM): Motor de envio de notificações Push de alta prioridade (ex: "Motorista a caminho").

Brevo API (Antigo Sendinblue): Disparo de e-mails transacionais utilizando protocolo SMTP customizado para o domínio do aplicativo.