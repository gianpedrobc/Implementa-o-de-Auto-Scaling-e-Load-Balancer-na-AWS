# Projeto: Launch Template, Auto Scaling Group e Load Balancer na AWS

## Visão Geral
Este projeto teve como objetivo implementar e compreender os serviços **Launch Template, Auto Scaling Group e Load Balancer** na AWS. A implementação seguiu a seguinte ordem lógica:  

**VPC → EC2 → AMI → Launch Template → Auto Scaling Group → Load Balancer → Regras de Scaling**

A infraestrutura começa com a configuração de rede, incluindo **VPC, subnets e gateways**. Em seguida, cria-se uma instância EC2 com os serviços necessários, a partir da qual é gerada uma **AMI**. Esta AMI é utilizada no **Launch Template**, servindo de base para que o **Auto Scaling** gerencie automaticamente as instâncias conforme a demanda.

---

## Auto Scaling
O **Auto Scaling** ajusta automaticamente a quantidade de servidores para manter desempenho estável e controlar custos.Imagine que sua aplicação é uma loja e os servidores são os caixas: quando a loja enche, o Auto Scaling “chama mais caixas” (instâncias) e, quando o movimento diminui, “dispensa alguns” para economizar dinheiro, mantendo sempre o número certo de servidores.

### Principais funções
- **Monitoramento:** acompanha métricas como CPU, tráfego de rede e número de conexões.
- **Ajuste automático:** adiciona (**scaling out**) ou remove (**scaling in**) instâncias conforme a demanda.

### Passo a passo básico
1. Crie um **Auto Scaling Group** definindo:
   - Tipo de instância
   - Sistema operacional
   - Configurações básicas
2. Defina os limites de instâncias:
   - **Mínimo**: quantidade mínima de instâncias ativas
   - **Desejado**: quantidade de instâncias que você quer inicialmente
   - **Máximo**: limite máximo de instâncias
3. Configure políticas de escalonamento:
   - Exemplo: “Se a CPU ultrapassar 70% por 5 minutos, adicione uma nova instância”.  

**Exemplo prático:**  
- Monitoramento: CPU média > 70% por 5 minutos → adiciona 1 instância.  
- Monitoramento: CPU média < 30% por 5 minutos → remove 1 instância.

---
**Application Load Balancer (ALB)**  
  ![ALB](https://images.openai.com/thumbnails/url/bJGxC3icu1mSUVJSUGylr5-al1xUWVCSmqJbkpRnoJdeXJJYkpmsl5yfq5-Zm5ieWmxfaAuUsXL0S7F0Tw4s9i1JK4xMDA40LPbzK3MzNUmpSglMM_fJzUkKy3LxKUrOdDNMjjf0zwjyjTRyiXcJMDfIzk8yTXUudVQrBgAmhCmt)
---
## Load Balancer
O **Load Balancer** distribui o tráfego entre várias instâncias EC2, garantindo **alta disponibilidade** e **desempenho estável**.Pense nele como o porteiro de um prédio, que direciona os visitantes para diferentes elevadores para evitar sobrecarga.

### Principais funções
- Distribui o tráfego de forma inteligente entre as instâncias.
- Monitora a saúde de cada instância, enviando requisições apenas para servidores ativos.

### Passo a passo básico
1. Escolha o tipo de Load Balancer:
   - **Application Load Balancer (ALB)**
   - **Network Load Balancer (NLB)**
   - **Gateway Load Balancer (GLB)**  
   *(ALB é o mais comum para aplicações web)*
2. Crie um **Target Group** e adicione as instâncias que receberão o tráfego.
3. Configure o **Listener** (porta e protocolo, ex.: HTTP:80) para receber requisições.

**Exemplo prático:**  
- Tipo de Load Balancer: ALB  
- Target Group: adiciona instâncias EC2 do Auto Scaling Group  
- Listener: HTTP na porta 80, direcionando para o Target Group

---

## Regras de Scaling (Scaling Up e Scaling Down)
As regras de **Scaling** determinam quando o Auto Scaling deve adicionar ou remover instâncias.

### Conceitos
- **Scaling Up:** aumenta o número de instâncias quando a demanda cresce (como adicionar mais caixas em uma loja cheia).  
- **Scaling Down:** reduz o número de instâncias quando a demanda diminui (como liberar caixas extras em uma loja vazia).

### Passo a passo básico
1. Escolha a métrica que acionará o escalonamento (ex.: CPU, tráfego de rede).  
2. Defina os limites:
   - **Upper Limit (Scaling Up):** ex.: CPU > 70%, adicionar X instâncias  
   - **Lower Limit (Scaling Down):** ex.: CPU < 30%, remover Y instâncias  
3. Ajuste o período de observação para evitar reações a picos temporários.

**Exemplo prático:**  
- Métrica: CPU > 70% por 5 minutos → adiciona 1 instância  
- Métrica: CPU < 30% por 5 minutos → remove 1 instância

---

## Integração com CloudWatch
O **Amazon CloudWatch** monitora métricas e cria alarmes que podem ser usados para:

- Acionar políticas do Auto Scaling automaticamente.
- Enviar notificações via **SNS** (e-mail ou SMS) para alertar administradores.

### Exemplo de fluxo
1. CloudWatch monitora a CPU de uma instância.  
2. Se a CPU ultrapassar 70% por 5 minutos, dispara um alarme.  
3. O alarme aciona o Auto Scaling para criar uma nova instância e envia uma notificação, mantendo a aplicação estável.

**Exemplo prático:**  
- CloudWatch monitora a CPU  
- CPU > 70% por 5 minutos → alarme dispara  
- Alarme aciona Auto Scaling para criar instância adicional  
- SNS envia notificação para administradores

---

## Observações Finais
Este projeto foi realizado com o objetivo de **aprender na prática** como configurar e integrar serviços de escalabilidade e balanceamento na AWS, garantindo:

- **Alta disponibilidade**  
- **Desempenho estável**  
- **Controle de custos**

