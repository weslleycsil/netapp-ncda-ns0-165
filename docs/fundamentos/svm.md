# SVM e multitenancy

**SVM (Storage Virtual Machine)** é a entidade lógica que entrega serviços de dados.

Cada SVM pode ter:

- volumes;
- LIFs;
- protocolos como NFS, SMB e iSCSI.

## Ideia prática

Pense na SVM como um tenant lógico com rede, namespace e serviços próprios.

## Ponto de prova

A SVM não é o cluster, e também não é o aggregate. Ela consome recursos do cluster para entregar acesso aos dados.
