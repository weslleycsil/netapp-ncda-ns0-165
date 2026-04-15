# Arquitetura ONTAP

## Conceitos-base

- **Cluster**: conjunto de nodes administrados como uma única entidade.
- **Node**: controlador individual.
- **HA pair**: dois nodes que se protegem mutuamente.
- **SVM**: camada lógica de tenant/serviço.

## Visão Geral

O NetApp ONTAP é uma plataforma de dados distribuída que transforma hardware físico em uma malha lógica de armazenamento, rede e serviços. Ele permite alta disponibilidade, mobilidade de dados e isolamento multi-tenant, enquanto mantém transparência para clientes e aplicações.

## Fundamentos da Arquitetura

A arquitetura do ONTAP organiza recursos físicos e lógicos de forma a isolar falhas, escalar sob demanda e simplificar o gerenciamento.

### Componentes Físicos

- **Nodes**: controladores que executam o software ONTAP. Cada node dispõe de CPU, memória, interfaces de rede e adaptadores de storage.
- **Shelves de disco**: conjuntos de discos conectados ao controlador via SAS ou NVMe, dependendo da plataforma.
- **Cluster Interconnect**: rede privada usada internamente para comunicação entre nodes, troca de estado, failover e acesso indireto a dados.

### Componentes Lógicos

- **WAFL (Write Anywhere File Layout)**: sistema de arquivos que grava blocos em qualquer lugar do storage, permitindo snapshots, clones e eficiência de dados.
- **RAID**: proteção de discos. No ONTAP, o padrão é o **RAID-DP** (dual parity) e, em ambientes críticos, é usado **RAID-TEC** (triple parity).
- **Aggregate**: pool de discos organizados em RAID groups. O aggregate representa a capacidade física disponível para volumes.
- **Volume (FlexVol / FlexGroup)**: unidade lógica de armazenamento criada sobre um aggregate. Os volumes armazenam dados de arquivos e LUNs.
- **LIF (Logical Interface)**: interface de rede lógica que expõe serviços de dados e tráfego de cliente.
- **SVM (Storage Virtual Machine)**: instância lógica que fornece isolamento de tenant. Cada SVM possui sua própria rede, security style, políticas de autenticação e serviços.

### Hierarquia de Storage

```text
Discos → RAID Group → Aggregate → Volume → LUN / File
```

## Modelo de Acesso a Dados

O acesso no ONTAP é sempre feito através de **LIFs**, que podem migrar entre nodes sem afetar o cliente.

- Cliente acessa dados via LIF.
- A LIF pode estar ligada a qualquer node do cluster.
- Se o dado estiver em outro node, o **cluster interconnect** faz o acesso indireto.

Isso proporciona:

- mobilidade de dados;
- balanceamento de carga entre nodes;
- transparência para aplicações.

## Alta Disponibilidade (HA)

O ONTAP usa HA pairs para proteger nodes localmente.

- **Takeover**: um node assume o trabalho do outro em falha.
- **Giveback**: retorno ao node original após recuperação.

Vantagens:

- failover transparente para dados e gestão;
- sincronização constante entre nodes do HA pair;
- compartilhamento de discos com múltiplos caminhos.

> Observação: HA pair é proteção local. O **cluster** é o que permite escala horizontal e operação de múltiplos nodes.

## Separação de Planos

O ONTAP separa o plano de controle do plano de dados.

- **Plano de Controle**: configuração e gestão do cluster, SVMs, políticas, autenticação e segurança. Usado por CLI, API e System Manager.
- **Plano de Dados**: tráfego de cliente que passa pelas LIFs e serviços de dados (NFS, SMB, iSCSI, FC).

Essa separação garante que ações administrativas não interrompam o fluxo de dados, além de permitir operações independentes de gestão e I/O.

## Escalabilidade

### Escala horizontal (scale-out)

- adicionar nodes ao cluster.
- aumenta capacidade, desempenho e resiliência.

### Escala vertical (scale-up)

- adicionar discos ou expandir aggregates.
- aumenta capacidade dentro do mesmo node ou aggregate.

## Multi-tenancy

O isolamento no ONTAP é realizado usando:

- **SVMs**: separação lógica de workloads e políticas;
- **IPspaces**: segmentação de rede interna;
- **Broadcast domains**: isolamento de tráfego no cluster.

Isso permite múltiplos clientes/serviços no mesmo cluster sem misturar dados ou configurações.

## Diferença entre Aggregate e Volume

- **Aggregate** = capacidade física agrupada por RAID.
- **Volume** = objeto lógico onde os dados são realmente armazenados e acessados.

## Pontos-chave para revisar

- relação entre cluster, node e HA pair;
- acesso indireto a dados via cluster interconnect;
- separação entre plano de dados e administração;
- diferença entre aggregate e volume;
- papel das SVMs no isolamento de ambientes;
- funcionamento do HA (takeover/giveback);
- importância das LIFs para mobilidade e disponibilidade.
