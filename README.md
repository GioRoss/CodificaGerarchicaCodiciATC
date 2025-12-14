# Analisi degli Embedding Gerarchici dei Codici ATC

Questa repository contiene l’implementazione e l’analisi di embedding gerarchici per i codici ATC (Anatomical Therapeutic Chemical), progettati per catturare esplicitamente la struttura multilivello del sistema di classificazione farmacologica.

## Descrizione generale

Ogni codice ATC viene rappresentato come un vettore denso di 88 dimensioni, ottenuto combinando informazioni provenienti da tutti e cinque i livelli gerarchici (ATC1–ATC5).

L’obiettivo del progetto è apprendere una rappresentazione vettoriale che:
- Preservi la gerarchia semantica dei codici ATC
- Rifletta la similarità farmacologica tra sostanze
- Sia valutabile tramite metriche quantitative e analisi geometriche

## Metodologia

- *Pre-processing dei codici ATC*  
  I codici vengono normalizzati e scomposti nei prefissi canonici  
  ([1, 3, 4, 5, 7] caratteri → ATC1–ATC5).

- *Modello di embedding gerarchico*  
  Ogni livello ATC è rappresentato da un embedding indipendente con dimensioni  
  [8, 16, 16, 16, 32].  
  Gli embedding vengono concatenati per ottenere un vettore finale di 88 dimensioni.

- *Classificazione gerarchica*  
  Un MLP condiviso elabora gli embedding e alimenta tre teste di classificazione:
  - ATC1 (macro-categoria anatomica)
  - ATC2 (sottogruppo terapeutico)
  - ATC3 (sottogruppo farmacologico)

- *Funzione di loss*
  - Cross-Entropy multilivello pesata
  - Triplet loss gerarchica basata sulla condivisione del prefisso ATC3
  - Loss totale:  
    L = L_class + λ · L_triplet

## Valutazione

La qualità degli embedding viene analizzata tramite:
- Accuracy di classificazione (ATC1, ATC2, ATC3)
- Metriche di clustering (Silhouette, Davies–Bouldin, Calinski–Harabasz)
- Coerenza semantica locale (K-NN consistency)
- Analisi delle distanze intra/inter categoria
- Test gerarchico delle distanze
- Visualizzazione dello spazio degli embedding tramite t-SNE

I risultati mostrano:
- Accuratezza elevata (>95% fino ad ATC3)
- Coerenza semantica locale eccellente (~98%)
- Un gradiente di distanze coerente con la gerarchia ATC

## Output

L’output finale è una matrice di embedding di dimensione *(6440, 88)*, in cui ogni riga rappresenta un codice ATC come vettore continuo.
