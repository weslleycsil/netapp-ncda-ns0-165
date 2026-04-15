# Arquitetura ONTAP

## Conceitos-base

- **Cluster**: conjunto de nodes administrados como uma única entidade.
- **Node**: controlador individual.
- **HA pair**: dois nodes que se protegem mutuamente.
- **SVM**: camada lógica de tenant/serviço.

## Ideia central

O ONTAP não deve ser visto apenas como “um storage”, mas como uma plataforma distribuída que organiza capacidade, rede e serviços de dados de forma lógica.

## O que revisar

- relação entre cluster, node e HA pair;
- acesso indireto a dados via cluster interconnect;
- separação entre plano de dados e administração.
