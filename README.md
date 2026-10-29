# Acolhe-te

## Sistema Distribuído de Alertas de Emergência em Rede Resiliente

Este repositório reúne o projeto interdisciplinar do **Grupo 3** do curso de
Tecnologia em Análise e Desenvolvimento de Sistemas da Fatec. O projeto é
desenvolvido em conjunto pelas disciplinas de **Inglês**, **Redes de
Computadores** e **Sistemas Distribuídos**.

## Sobre o projeto

O **Acolhe-te** propõe um protótipo de envio de mensagens para situações de
emergência em um campus universitário e em comunidades. A solução busca formar
uma rede resiliente de alertas, capaz de continuar transmitindo mensagens
mesmo quando houver falhas, congestionamento ou ausência de conexão externa,
como internet e sinal de telefonia.

Em uma situação de emergência, um nó com botão de acionamento gera o alerta.
Esse alerta pode ser retransmitido por uma rede em malha até um gateway, que o
publica em um broker MQTT. Um serviço de alertas distribui a mensagem para
painéis e dispositivos na rede local, registra as confirmações de recebimento
(ACK) e realiza reenvios quando necessário. O enlace sem fio e os nós podem ser
implementados ou simulados, permitindo testar rotas alternativas, perda de
pacotes e a indisponibilidade temporária de componentes.

O projeto relaciona um problema social real aos conteúdos do curso:

- **Redes de Computadores:** topologia, endereçamento, enlaces sem fio,
  transmissão e métricas de rede;
- **Sistemas Distribuídos:** comunicação entre componentes, troca de mensagens,
  filas, confirmação de recebimento, tolerância a falhas e disponibilidade;
- **Inglês:** documentação técnica, README, glossário e apresentação oral.

## Organização das entregas

As atividades estão separadas em pastas para facilitar a realização e a
entrega individual de cada disciplina:

- [`Redes de Computadores/`](./Redes%20de%20Computadores/): plano de projeto e
  materiais relacionados à topologia e à comunicação da rede;
- [`Sistemas Distribuidos/`](./Sistemas%20Distribuidos/): arquivos da
  simulação, evidências e documentação do laboratório de sistemas
  distribuídos;
- [`Inglês/`](./Ingl%C3%AAs/): README, glossário e demais entregas de inglês
  técnico.

## Equipe

- **Amanda Carvalho** — gestora da equipe
- **Gabriele Monteiro**
- **Juliana Melo**
- **Taina Kasai** — líder da equipe
- **Talita Sabine**

## Objetivo geral

Planejar, implementar em protótipo ou simulação e avaliar um sistema
distribuído de alertas de emergência para múltiplos usuários, com operação
resiliente em um campus ou comunidade, inclusive sem conectividade externa.

## Tecnologias e ferramentas previstas

Python, MQTT/Mosquitto, Docker ou máquinas virtuais, Cisco Packet Tracer ou
Mininet, ferramentas de análise de rede e Git. Como extensão futura, o projeto
poderá utilizar placas ESP32 com módulos LoRa, além de integração com o Projeto
Acolhe-te, aplicativo móvel, autenticação e criptografia de ponta a ponta.
