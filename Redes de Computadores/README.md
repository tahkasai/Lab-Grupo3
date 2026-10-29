# Plano de Projeto — Grupo 3

## Identificação

- **Curso:** Tecnologia em Análise e Desenvolvimento de Sistemas
- **Projeto:** Projeto Interdisciplinar — Inglês, Redes de Computadores e
  Sistemas Distribuídos
- **Data:** 28/10/2026

## Tema do projeto

**Grupo 3 — Mensagens para situações de emergência: rede resiliente de alertas
para campus e comunidade, sem dependência de conectividade externa.**

## Título do projeto

**Acolhe-te: Sistema Distribuído de Alertas de Emergência em Rede Resiliente
para Campus Universitário e Comunidade**

## Equipe

- **Amanda Carvalho** — gestora da equipe
- **Gabriele Monteiro**
- **Juliana Melo**
- **Taina Kasai** — líder da equipe
- **Talita Sabine**

## Motivação

O projeto baseia-se no Projeto Acolhe-te, uma plataforma voltada para denúncia
e empregabilidade de vítimas de violência doméstica. Em situações críticas de
emergência, as vítimas frequentemente encontram-se em locais sem sinal de
celular ou sem acesso à internet.

Dessa forma, surge a necessidade de integrar um módulo de hardware e uma rede
de transmissão capaz de efetuar o envio de mensagens e alertas de emergência,
seja no âmbito do campus ou comunitário, de forma resiliente. A solução deve
garantir uma comunicação rápida mesmo em condições adversas e sem a
conectividade externa padrão.

Do ponto de vista acadêmico, o tema permite aplicar de forma integrada os
conteúdos de:

- **Redes de Computadores:** topologia, endereçamento, enlaces sem fio e
  métricas;
- **Sistemas Distribuídos:** componentes independentes, troca de mensagens,
  tolerância a falhas e disponibilidade;
- **Inglês técnico:** README, glossário e apresentação.

Assim, o projeto conecta a formação em Análise e Desenvolvimento de Sistemas a
um problema social real.

## Introdução

Sistemas de alerta de emergência dependem, em geral, da infraestrutura de
telefonia celular e da internet. Quando essa infraestrutura está indisponível,
congestionada ou fora de alcance, a mensagem não chega a quem poderia ajudar.

Redes em malha de baixa potência e longo alcance, como as baseadas em LoRa, e
protocolos leves de mensageria, como o MQTT, permitem construir soluções de
comunicação que continuam operando de forma local, sem depender de serviços
externos.

Neste projeto, será desenvolvido um protótipo, com partes implementadas e
partes simuladas, de um sistema de mensagens para situações de emergência em
um campus universitário conectado. Nós de emergência com botão de acionamento
enviam alertas por uma rede em malha até um gateway, que os publica em um
broker MQTT. Um serviço de alertas distribui as mensagens a vários usuários,
como painel web e aplicativos na rede local, e, quando houver internet, pode
realizar o envio de SMS ou notificações push.

O tema relaciona-se diretamente ao curso de Tecnologia em Análise e
Desenvolvimento de Sistemas, pois envolve:

- projeto de arquitetura distribuída;
- desenvolvimento de software, incluindo gateway, serviço de alertas e painel;
- configuração de redes;
- documentação técnica em inglês.

O sistema deverá demonstrar também como reage quando um serviço ou uma conexão
fica indisponível.

## Objetivos

### Objetivo geral

Planejar, implementar em protótipo ou simulação e avaliar um sistema
distribuído de envio de alertas de emergência para múltiplos usuários, capaz de
operar de forma resiliente em um campus ou comunidade, inclusive sem
conectividade externa.

### Objetivos específicos

1. Projetar a topologia de rede, composta por nós de emergência, repetidores em
   malha, gateway, broker e clientes, com endereçamento e identificação dos
   nós.
2. Implementar um fluxo completo de comunicação: acionamento do alerta,
   transmissão pela malha, publicação no broker, distribuição aos usuários e
   confirmação de recebimento (ACK).
3. Testar o funcionamento normal e ao menos uma condição de falha, como a
   queda de um repetidor, do gateway ou do enlace, registrando métricas de
   latência, entrega e perda de mensagens.
4. Documentar o trabalho em relatório técnico em português, README e glossário
   técnico em inglês, além de realizar uma apresentação oral em inglês.

## Descrição do projeto

Será construído o protótipo de uma rede de alertas de emergência com quatro
camadas:

1. **Nós de emergência:** possuem botão físico de acionamento e geram a
   mensagem.
2. **Nós repetidores em malha:** retransmitem as mensagens por rotas
   diferentes quando houver falha.
3. **Gateway:** converte os pacotes recebidos em mensagens MQTT.
4. **Serviços na rede local:** incluem broker, serviço de alertas e painel web,
   que distribuem o aviso a vários usuários e registram a confirmação de
   leitura.

O alerta contém:

- identificador do nó;
- tipo de ocorrência;
- localização aproximada, identificada pelo ponto ou bloco;
- data e hora;
- número de sequência.

O serviço de alertas mantém uma fila com reenvio até receber o ACK dos
destinatários. Esse mecanismo de armazenar e encaminhar (*store-and-forward*)
evita que as mensagens sejam perdidas quando um serviço ou conexão fica
temporariamente indisponível.

### Falhas previstas nos testes

- Desligamento de um repetidor: a malha deve utilizar uma rota alternativa.
- Interrupção do gateway ou do broker: as mensagens ficam em fila e são
  entregues após a recuperação.
- Perda de pacotes no enlace: será medida pela taxa de entrega e pela latência
  fim a fim.

### Componentes implementados

- gateway;
- broker MQTT;
- serviço de alertas com fila e ACK;
- painel web;
- scripts de teste e de injeção de falhas em contêineres ou máquinas virtuais.

### Componentes simulados

- enlace LoRa;
- comportamento da rede em malha, emulado por software com atraso e perda de
  pacotes configuráveis;
- nós de emergência, caso não haja hardware disponível.

### Possibilidades futuras

- nós físicos com ESP32 e LoRa;
- integração com o Projeto Acolhe-te;
- aplicativo móvel;
- autenticação;
- criptografia de ponta a ponta;
- cobertura comunitária em maior escala.

## Diagrama de blocos e descrição funcional

| Componente | Função |
|---|---|
| **N1/N4 — Nós de emergência** | Geram o alerta ao acionar o botão e enviam o pacote pela malha. |
| **N2/N3 — Repetidores** | Retransmitem pacotes, controlam duplicidade e TTL e oferecem uma rota alternativa. |
| **GW — Gateway** | Utiliza o endereço `192.168.10.10`, recebe os pacotes da malha e os publica no broker MQTT. |
| **Broker MQTT** | Utiliza o endereço `192.168.10.20` e a porta `1883`; intermedeia a comunicação por publicação e assinatura. |
| **Serviço de alertas** | Utiliza o endereço `192.168.10.30`, valida, enfileira, distribui aos usuários e reenvia até receber o ACK. |
| **Clientes** | Incluem painel web e dispositivos na rede local. SMS e notificações push dependem de conexão com a internet. |

### Fluxo principal

1. O botão do nó N1 é acionado.
2. O pacote é retransmitido por N2 até o gateway.
3. O gateway publica a mensagem no tópico MQTT de alertas.
4. O serviço de alertas, assinante do tópico, distribui o aviso aos clientes.
5. Cada cliente devolve o ACK.
6. Sem ACK, o serviço reenvia a mensagem.

Se N2 falhar, o pacote segue por N3, utilizando a rota alternativa.

## Metodologia

A metodologia seguirá as etapas do Projeto Integrado:

1. **Planejamento:** definição do problema, dos usuários e da divisão de
   tarefas.
2. **Desenho:** elaboração do diagrama de rede e da arquitetura distribuída.
3. **Construção:** desenvolvimento do protótipo e da simulação, com registro
   das decisões e dificuldades.
4. **Testes:** execução de um cenário normal e de um cenário de falha, com
   produção de evidências.
5. **Comunicação:** elaboração do relatório, README, glossário e apresentação.

### Materiais e ferramentas

- Python para scripts de nós simulados, gateway e serviço de alertas;
- Mosquitto como broker MQTT;
- Docker, Docker Compose ou máquinas virtuais;
- Cisco Packet Tracer ou Mininet para a topologia;
- `ping`, `tc/netem` e Wireshark para medir latência e perda;
- Git para versionamento;
- placas ESP32 com módulo LoRa, opcionalmente, caso estejam disponíveis.

Os testes serão registrados em uma tabela contendo condição inicial, ação,
resultado esperado, resultado observado e evidência.

## Exequibilidade

O projeto é viável com os recursos existentes no Campus: laboratórios de
informática com computadores, acesso à rede local e possibilidade de uso de
máquinas virtuais e contêineres.

Como o enunciado admite protótipo ou simulação, não é necessário hardware
específico. O enlace LoRa e os nós podem ser emulados por software, com atraso
e perda de pacotes configuráveis.

O escopo foi delimitado a um fluxo principal e a um teste de falha, sendo
compatível com o cronograma de 28/10 a 19/11/2026. O uso de placas ESP32 e
LoRa fica como extensão, conforme a disponibilidade.

## Bibliografia básica

- KUROSE, J. F.; ROSS, K. W. *Redes de computadores e a internet: uma
  abordagem top-down*. 8. ed. São Paulo: Pearson, 2021.
- TANENBAUM, A. S.; VAN STEEN, M. *Sistemas distribuídos: princípios e
  paradigmas*. 2. ed. São Paulo: Pearson Prentice Hall, 2007.
- OASIS. *MQTT Version 3.1.1*. OASIS Standard, 2014. Disponível em:
  <https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/>. Acesso em: 28 out. 2026.
- BRASIL. *Lei nº 11.340, de 7 de agosto de 2006 (Lei Maria da Penha)*.
  Brasília, DF: Presidência da República, 2006.

## Atividades previstas

1. Plano de projeto.
2. Progressos no trabalho — diagrama de rede e arquitetura do sistema
   distribuído.
3. Progressos no trabalho — protótipo e arquivos técnicos, plano e resultados
   dos testes, relatório técnico em português, README técnico em inglês e
   glossário técnico em inglês.
4. Apresentação e demonstração.
5. Entrega do relatório.

## Cronograma

| Atividade | 28/10/2026 | 04 e 05/11/2026 | 11 e 12/11/2026 | 19/11/2026 |
|---|:---:|:---:|:---:|:---:|
| (1) Plano de projeto | X |  |  |  |
| (2) Diagrama de rede / arquitetura |  | X |  |  |
| (3) Protótipo, testes e documentação |  |  | X |  |
| (4) Apresentação e demonstração |  |  |  | X |
| (5) Entrega do relatório |  |  |  | X |

