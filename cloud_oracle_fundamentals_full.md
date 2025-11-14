# Cloud Oracle Fundamentals

cloud infrastructure platform

- Infraestrutura

	Servidores , storages , containers, redes e etc

- Databases

	Oracle, MySQL, Autonomous DB , JSON , Exadata

- Aplicações

	Servless, SaaS, Integrações.
	
- Analytics

	Analytics Cloud, Fusion Analytics
	
- Dados e IA

	Big Data, mensageria, Stream
	
- Serviços de desenvolvedor

	Low Code (APEX), APIS, IaC (infrastructure as code)
	
- Governança e Administração

<br>

## 01 - OCI Arquitertura

- Regian (Região geografica - ex: estado de São Paulo)

- Availability Domains (Data Centers dentro de uma região ex: vinehdo e guarulhos)

- Fault Domains (Espelhamento de Hardware dentro do mesmo Data Center)


Como escolher uma região:

- Escolha a região mais proxima pra pegar baixa latencia
- Requisitos de residência e conformidade dos dados com legislação 
- Disponibilidade do serviço


<br>


## 02 - IAM (identity and acess Management)

O que é?

- Mecanismo de controle de acesso aos recursos da OCI
- Alta capacidade de modularização / granuldaridade

AuthN: Identifica /autoriza o usuário.

"Quem é"

AuthZ: Define quais permissões o usuário tem.

"O que pode Fazer"


Fluxo de gereciamento de permissões:

Usuários -> Grupos -> politicas -> Compartments -> serviços


Compartments:

São divisões lógicas para a alocação dos recursos

	- São uteis para organização e gereciamento de recursos
	- Separação por equipes (ex: redes , servidores, banco de dados etc).
	
Cada recurso pode pertencer apenas a um compartments.
"Pertencer" a um compartments não significa está inacessivel a outro. 

* Acesso aos recursos
* Interações entre compartments (ex: VNC -> database -> Block Storage)
* Movimentação de recursos (ex: comp hml (recurso) -> comp prd)
* Multi Region 


- Compartments aninhados (até seis niveis)
- Cotas e budget
	* Quantidade de recursos
	* Finaceiros


<br>

# 03 - Network (Rede)

VCN

O que é VCN

- Virtual cloud Network (rede virtual na nuvem)
- Uma rede de computadores, porém 100%  virtual
- Permite a criação de sub-redes
- Permite a conexão entre os recursos da nuvem
- Permite conexões internas e externas

Serviços de Network

- Internet Gateway
	*Habilita conexões para a internet (saida e entrada)
	
- NAT Gateway
	*Permite conexões para a internet
	* Apenas saida
	* Entrada não permitida
	
- Service Gateway
	*Fornece conexão privada
	*Sem expor o tráfico na intenet
	*Ex: a partir do on-prem.
	
- Dynamic Routing Gateway
	*Um roteador virtual
	*É possivel criar rotas entre:
		- on-prem e a OCI
		- VNC e VCN
		- Region e Regian
		- OCI e AWS, GCP, Azure...
	*VPN
	*Oracle Fast Connect	
	
### Roteamento

- Route Table (tabela de roteamento)
	* Regras para rotear o tráfego de subnets
	* Através de "gateways" ou instacias especiamente configuradas
	* Toda VNC tem um "route table"
	* Por padrão vazia
	* você pode incluir suas regras nessa "route table"
	
	
	Rede            	|       Alvo de roteamento
	0.0.0.0/0     	| Nat Gateway
	192.168.0.0/16  | Dynamic Routing Gateway

### Peering

- Roteamento entre VCNs.
- Local peering (VCNs dentro da mesma região)
	* Usando o LPG (local peering gateway)

- Remote Peering (VCNs em regiãos distintas)
	* Usando o DRG (dynamic route gateway)

### Roteamento (DRG v2)

- Faz o mesmo trabalho que o DRG padrão
- Elimina a necessidade de varias peers
- É capaz de conectar até 300 VCNs.
- Se for preciso pode se utilizar o DRG v2 adicionais

### Segurança

- Security List
	* Atuam exatamente como um fireway
	* É definidouma subnet
	* Uma porta de origem e uma porta de destino
	* É definido o sentido do tráfego
		*Ingress (entrada)
		*Engress (Saida)
		* OU ambos

- Network Security Group
	* Atua como security list
	* É permitido apenas subnets (CIDR)
	* Network Security Groups

### Load Balancer

- Equilibrar o tráfego entre servidores
- Layer 7 (HTTPS)
- Pode ser:
	* Público
	* Privado

Obs: pode ser usando quando se precisa mais interligencia (Por exemplo: inpesação de pacotes ao chegar dados na rede)

- Network Load Balancer
	*Layer 4
		*TCP
		* UDP

Obs: poder ser usando quando se precisar velocidade no load Balancer


<br>

## 04 - Compute

### Tipos de maquinas

- Compute = Instance = computadores = servidores

- Detro da OCI existem três tipos de maquinas

	* Virtual Machine (maquinas virtuais)
	* Bare Metal (servidor fisico)
	* Dedicated Host (Servidor fisico dedicado para a virtualização)
	
### Configuração Flexível

- Memoria
- CPU (quantidade e arquitertura)
- Preemptibl VMs( uso pontual temporario )
	sobe -> processa -> desliga
	
### Processadores ARM com NGINX

- Para aplicações mobile (smartphones)
- Maior perfomance
	
### Como funciona

- Antes de qualquer coisa, um servidor dentro da OCI precisa:
	* Um compartment
	* Uma VCN
	* Um disco de boot (default)
	* um disco para os dados


- Um servidor no modo VM pode de maneira 100% transparente passar por um "live migration"
	* Trocar fisicamente de servidor em caso de falhas


### Scaling

- É possivel fazer o processo de scaling de duas maneiras:

- Vertical 
	* Adicionamento mais hardware (CPU, Memoria)
	* Troca de shape
	* mesma arquitertura
	* Requer downtime
	
- Horizontal 
	* Adicionamento novo servidores
	
	
### Auto scaling

- Processo de adicionar novas máquinas virtuais 
- Pode crescer e diminuir automaticamente.
	* De acordo com as regras configuradas

- Configurando:
	* Tenha uma instacia de templates (em execução)
	* Crie o modelo de scaling (config)
		* Diferentes Availability Domains
		* Start stop
	* Define o minimo e o maximo de instancias, justamente com as regras de scale "up" ou "down"
		

### Containers

### Orquestração

- Kubernetes , plataforma de orquestração
	* Open Source
	* Executa app em containers sem downtime
	* Auto corrigivel
	* Auto escalavel
	* Deploy simplicado


### Orquestração: OKE

- Oracle Kubernetes Engine
	* Core
		* 100% escalavel
		* Serviço com alta Disponibilidade

	* Engine
		* Open Source
		* Kubernetes

	* Operação:
		* Console
		* API

	* Acesso:
		* Linha de comando( kubectl )
		* Kubernetes Dashboard
		* Kubernetes

	
### Containers Clusters

- Worker Node:
	* Máquinas onde o Kubernetes está instalado
	* Máquina onde os containers são iniciados

- Pod
	* Grupo de containers que compartilham a mesma Infraestrutura.
		* storage, rede , cpu, memoria etc

- Gestão dos "Worker Nodes" é através do "control plane"
	* Ferrementas disponibilizada peça Oracle
	* Com alta disponibilidade
	* Sem Custos

### Tipos de Clusters

- Basic Clusters 
	* todas as funcionalidades básicas
		* Kubernetes
		* Dorker
	* Garante o SLO
	* Não Garante o SLA

- Enchanceed Clusters:
	* Todas as funcionalidades
	* Incluido o SLA

- Gerenciamento
	* Virtual Nodes:
		* Servless
		* Atualização, patchs de Segurança instalados automaticamente  sem downtime
		* Apenas para enchanced Clusters

- Managed Nodes
	* Você é 100% responsavel pelo:
	* Gerenciamento dos Nodes
	* Atualizações


###  Container Instances

###  OCI Functions | Functions as service

- Função "as service".
- Arquitertura "Event Driven"
- 100% integrada com a OCI
- Container Native
- Open Source

- Pago apenas pela execução
- Plataforma auto-scaling
-  Não requer servidores


<br>

## 05 - Storage

- Dentro da OCI existem basicamente dois tipos de storage:
	* Persistente e não persistentes

- O que define isso?
	* Tipo de dado (foto, video , texto e etc )
	* Perfomance (IOPS , throughput )
	* Durabilidade (número de copias )
	* Conectividade ( local ou via rede )
	* Protocolo (blocos, arquivos , http)



### Tipos de storage na OCI

- Local NVMe
- Block Volume
- File Storage
- Object Storage

### Serviço de Migração

- Data Transfer Disk
- Data Transfer Appliance
- Storage Gateway

### Object Storage

- Alta perfomance
- Gerenciamento dos dados como objetos
- Ideal para dados não estruturados
- Serviço regionalizado e publico
- Multiplos tiers ( Niveis de armazenamento )
	* HOT:
		* Acesso rápido e imediato
		* recuperação instantânea
		* Pode sofrer downgrade
	* Cool
		* Acesso infrequente
		* Menor Custo
		* Renteção mínima 31 dias
	* Cold:
		* acesso raro
		* retenção minima 90 dias
		* objeto precisa ser restaurado antes do download (1 hora)
		* disponibilidade por 24 horas para o download

- Acesso privado a partir de recursos da OCI


- É conhecido "chamando" de bucket
- Tem uma "unique name" dentro de um tenacy
- Hierarquia flexivel 
	* Namespace
	* Bucket
	* Object


- Objeto Storage: Auto Tier
	* Processo de movimentação automática entre os tiers
	* Sem taxas de recuperação e armazenamento propocional



### Segurançã & Acesso

- Todos os dados são encriptados por default. Porém você pode aplicar uma criptografia adicional, caso deseja.

- Uma vez que os dados são armazenados, você pode acessá-los facilmente via API ou até mesmo atráves do browser.


### Block Volume

- Disco "Persistentes" que são anexados a intancias (compute).
- vocẽ pode:
	* Cria e anexar um disco ( Block volume ).
	* Desanexar e excluir ( deletar um disco ).
	* Manter o disco e seus dados, mesmos após excluir a instancias.

- São compatilhados entres as instacias
	* Para escritas e leitura

- São redimensionais
	* Podem ser aumentados (apenas).

- Podem ser replicaveis entre regiões
	* Recuperação de desastres
	* Migração

### File Storage

- Apresentado para servidores para armazenamento de arquivos
- Organizado em diretorios 
- Compatível com NFS e Samba.
- Compatível com Linux e Windows
- Não necessita de ferramentas adicionais


<br>

## 06 - Database

### Banco de dados Oracle na OCI

- Banco de dados Oracle como serviço
	* Conhecido como "DB as service" ou "DBCS"

- Exadata Database com infaestrutura dedica

- Autonomous Database:
	* Infraestrutura compatilhada
	* Infraestrutura dedicada.

- Cloud at Customer (data center do cliente)
	* Exadata Database service
	* Autonomous Database

### Autonomous Database

- Um banco de dados gerenciado 100% automatico:
- Ele "sozinho" vai executar as atividades de :

	- Tunning & perfomance.
	- Segurança
	- Backups
	- Patchs, Updates e Upgrades
	- Inclui as opções: RAC. Data Guard, Database Vault, In-memory, Multi Tenart etc.

	infaestrutura Exadata + Automação & ML Ops = 100% da Gestão automatizado  


- Opção de deploy:
	* Infraestrutura compatilhada

		* Você provisiona e gerencia o banco de dados (Autonomous database). A Oracle gerencia a infrastutura da Exadata, está opção está disponivel
		  para as opções "autonomous transaction process" e "autonomous data warehouse"

	*Infraestrutura dedicada

		* Você provisiona e gerencia o banco de dados (autonomous database) em um infrastutura dedicada , está opção está disponivel para as opções
		  "autonomous transaction process" e "autonomous data warehouse".

	
- Otimização de workloads
- É possivel criar instâncias do autonomous database de acordo com as seguintes caracteristicas:
	- Autonomous Data Warehouse (ADW)
	- Autonomous transaction process (ATP)
	- Autonomous JSON (ADJ) - banco em formato JSON (ex: mongo db e etc)
	- APEX service

### MY SQL & OCI

- Rápido provisionamento de instâncias.
	* Fácil para teste, Aplicação e rolback de patchs

- Escalabilidade rápido , barata e eficiente.
	* Rápido e fácil processo de aumento e diminuição de recursos (CPU & memória).
	* Expansão do storage de acordo com o crescimento dos dados.
	* Replicação elástica, adicione e remova instâncias  de acordo com o workloads

- Redução de custo
	* Menor custo se comparado com o processo de construção e manutenção se comparado com o ambiente on-promisse.


### MY SQL Alta disponibilidade

- Na OCI o My SQL oferece:
	* Maior Uptime
	* Fault-Tolerant
	* Failover automatico
	* Zero data-loss
	* RPO & RTO (minutos)

- Tudo isso com um clique

### My SQL HeatWave

- "full managed database service" mecanismo que acelera o poder de processamento dos comandos sql, através do macanismo in-memory

- Ideal para: real time analytics , data warehouse, data lake & machine learning

- Pode ser para aplicações : OLTP e OLAP

- Sem a necessidade de alteração de código

### Oracle NoSQL Database

- Completamente auto gerenciável
- Elástico (crescimento de acordo com o workload)
- Alta perfomance (latência de milissegundos)
- Modelagem de dados flexivel (Document, Key/Value. fixed-schema).
- Baixo custo (pague apenas pelo throughput e storage)
- Hybrid cloud (interoperabilidade on-promisse OCI e outros cloud providers)
- Fácil de utilizar (acesso via API e ferramentas de desenvolvimento)

<br>

## 07 - Security

### Visão geral 

- On-Premisse (Resposabilidade de você)
	* Dados
	* Dispositivos 
	* Contas de Usúarios 
	* Aplicações
	* Rede
	* Sistema operacional
	* virtualização
	* Hosts Fisicos
	* Rede Fisica
	* Data center

- OCI (Resposabilidade de você)
	* Dados
	* Dispositivos 
	* Contas de Usúarios 
	* Aplicações
	* Rede
	* Sistema operacional

- OCI (Resposabilidade de você)
	* virtualização
	* Hosts Fisicos
	* Rede Fisica
	* Data center


### Ferramentas de segurança

- Infraestrutura
	* DDos protection
	* WAS
	* Security list
	* Fireway

- IAM
	* IAM
	* Auditing

- Proteção de dados
	* Data Safe
	* Key Vault
	* Certificates

- Sistema operacional
	* Bastion
	* Shield Instance
	* Dedicated Host

- Detecção & correção
	* Cloud Guard
	* Vulnerability
	* Threat Intelligence
	* Security Zone

### Cloud Guard

Exibição unificada da camada na Oracle Cloud infrastructure, junto com o Threat Detector , detecta recursos configurados incorretamente,
atividade não segura e atividades de ameaça maliciosa além de fornecer aos administradores de segurança a visibilidade para fazer
triagem e resolver os problemas de segurança na nuvem.


- Uma visão geral 

Target           |         Detectors           |         Problems               |  Responders 
_________________|_____________________________|________________________________|__________________
Examina analise  |Indetifica problemas         |Risco de segurança              | Ações corretivas


### Security Zone

Security Zone impõem a postura de segurança no compartimentos de nuvem da OCI e impedem ações que possam esfraquecer
de segurança de um cliente.

As politicas podem ser aplicadas a vários tipos de infrastutura de nuvem (rede, computação, armazenamento, banco de dados e etc)
para garantir que os recursos da nuvem permaneçam seguros e impeçam configurações incorretas de segurança.

Os Usuários determinam quais politicas são apropriadas para suas necessidades definindo conjuntos de politicas
de security zones personalizadas


### Security Advisor

O security Advisor suporta e reforça as melhorias práticas de segurança da Oracle, incluido os requisitos de Configuração
para recursos na security zones. Ele combina e simplifica os workflows existentes para criar de forma eficiente recursos
que atendem aos requisitos de segurança de linha de base desde o inicio.

### Criptografia / Encryption

- Criptografia (Encryption):

	Transforma dados visiveis em dados embaralhados
 	
- Descriptografia (decryption):

	Torna os dados embaralhados em dados visiveis
	
- Cheve (Key):

	Uma String (letras e números) que combinados com um algoritmo , fazem a criptografia e Descriptografia dos dados.
	
- Par de chaves: (Key Pair):

	Uma String gerados por um algoritmo e que pode ser usado para criptografar so dados ou assinar digitalmente.


* Dados + chave + algoritmo = dados criptografados

* dados criptografados + chave + algoritmo = dados

### Médotos de Criptografia

- Encryption at rest

	* Dados com criptografia "at rest" são aqueles dados que estão criptografados em sem estado de repouso.
	Mesmo se alguém acessar o local desses dados se não tiver  a chave não conseguirá ler os dados de maneira integra.

- Encryption in-transit

	*A criptografia " in-transit" é a criptografia de dados enquanto eles estão trafegando entre sua origem e um destino,
	por exemplo uma requisição HTTPS.


### criptografia Simétrica e Assimétrica

- Simétrica:
	Na criptografia de chave Simétrica, uma unica chave é utilizada para criptografar e decriptografar os dados

- Assimétrica:
	Na criptografia de chave Assimétrica, é necessário o uso de duas chaves. Uma chave pública e outra chave privada

## HSM - Hardware Security Model 

  OCI HSM é um serviço de gerenciamnento de chaves (KMS) totalmente gerenciado que oferece armazenamento e gereciamento
  seguros de chaves de criptografia para proteger dados armazenados na Oracle Cloud infrastructure.


  OCI HSM usa HSMs (hardware security modules) certificados FIPS 140-2 level 3 para armazenamento chaves de criptografia
  de forma segura.

  São dispositivos fisicos projetados para proteger chaves de criptografia de acesso não autorizado

  ## OCI Vault

- OCI Vault é um serviço gerenciado que oferecem uma variedade de recursos para ajudar você a proteger seus dados,incluido:]

* Gerenciamento de chaves:

	OCI Vault permite que você crie, gerencie e roteie chave de criptografia. Você tambem pode importar chaves de criptografia
	de fonte externas.

* Gerenciamento de segredos: 

	OCI Vault permite que você armazena e gerencia segredos como senhas e certificados.

* Controle de acesso:

	OCI Vault oferecem controle de acesso granular ás chaves e segredos.
	Você pode definir politicas para restrigir o acesso a chaves e segredos especificos para usúarios e grupos especificos.

* Relatorios :

	OCI Vault  oferecem relatorios abrangentes sobre o uso de chaves e segredos. Você pode usar esse relatorios para monitorar
	o uso de chaves e segredos e identificar possiveis problemas de segurança.
