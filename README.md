# Detecção de Pistas Clandestinas via Imagens de Satélite e Visão Computacional

Iniciação Científica desenvolvida no CS&I Lab (INATEL), com o objetivo de detectar
automaticamente pistas de pouso clandestinas na Amazônia a partir de imagens de
satélite, utilizando segmentação de instâncias com modelos YOLO. O projeto visa,
futuramente, comparar imagens ópticas com imagens de Radar de Abertura Sintética
(SAR) e embarcar o modelo final em um drone para detecção em tempo real (Edge AI).

## Motivação

Pistas de pouso clandestinas estão associadas a atividades ilegais na Amazônia,
como garimpo e desmatamento. A detecção automática a partir de imagens de
satélite pode apoiar ações de fiscalização ambiental, reduzindo a dependência de
inspeção manual de grandes áreas.

## Metodologia

Organização do dataset → segmentação manual das imagens → conversão das
anotações → treinamento dos modelos (YOLOv8) → validação → avaliação por
métricas → experimentos de data augmentation.

## Dataset

- Imagens de satélite (Google Earth), anotadas manualmente em formato de
  segmentação de instâncias (polígonos).
- Dataset consolidado atualmente com 661 imagens (classe única: `pistas`),
  versionado no Roboflow.
- Experimentos de data augmentation isolados (uma técnica por vez): Flip
  (horizontal + vertical) e Brightness (±15%).

## Modelos e métricas

- **Modelo base:** YOLOv8n-seg (Ultralytics), segmentação de instâncias.
- **Métricas oficiais:** Precision, Recall, mAP50, mAP50-95 (via `model.val()`).
- **Avaliação complementar própria:** pareamento guloso de predições com o
  ground truth por IoU (limiar 0,5), reportando TP/FP/FN e IoU médio separado
  para caixa delimitadora (bbox) e máscara de segmentação.

## Resultados principais

| Experimento | Precision (bbox) | Recall (bbox) | IoU (bbox) |
|---|---|---|---|
| Baseline (sem augmentation) | 65,6% | 83,6% | 0,7825 |
| Flip (batch 8) | 60,2% | 97,3% | 0,8299 |
| Flip (batch 16) | 66,4% | 97,3% | 0,8005 |
| Brightness (batch 8) | 63,2% | 75,3% | 0,8014 |
| Brightness (batch 16) | 78,3% | 74,0% | 0,8250 |

> Detalhes completos, incluindo métricas de máscara e mAP, estão em
> [`resultados/experimentos.md`](resultados/experimentos.md).

## Orientadores

Felipe Augusto Pereira de Figueiredo e Evandro Cesar Vilas Boas — WAI Lab & CS&I Lab, INATEL.
