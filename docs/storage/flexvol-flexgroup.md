# FlexVol x FlexGroup

## FlexVol

É o volume tradicional do ONTAP.

Usado para:
- NAS;
- SAN;
- snapshots;
- SnapMirror.

## FlexGroup

É um volume distribuído entre múltiplos constituents, pensado para escala horizontal e workloads massivos de arquivos.

## Comparação rápida

| Item | FlexVol | FlexGroup |
|---|---|---|
| Escopo | mais tradicional | distribuído |
| SAN/LUN | sim | não é o foco |
| NAS | sim | sim |
| Escala | vertical | horizontal |

## Ponto de atenção

Para a prova, o mais importante é entender que **FlexVol é o padrão** e **FlexGroup é voltado a scale-out de NAS**.
