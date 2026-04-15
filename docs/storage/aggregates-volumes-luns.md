# Aggregates, Volumes e LUNs

## Hierarquia

```text
Discos -> RAID groups -> Aggregate -> Volume -> LUN / Files
```

## Aggregate

É a camada física de capacidade. Fornece o espaço base para os volumes.

## Volume

É a camada lógica onde vivem dados NAS, LUNs e snapshots.

## LUN

É a entidade de bloco exposta ao host em cenários SAN.

## Regra simples

- aggregate fornece espaço;
- volume organiza;
- LUN entrega ao host.

## Pegadinhas

- snapshot acontece no **volume**;
- o host não consome o aggregate diretamente;
- LUN não é criada no aggregate, e sim dentro do volume.
