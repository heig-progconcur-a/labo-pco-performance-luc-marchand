# Labo prerformance

## Informations du cache

### Questions :

1. **Combien de niveaux de cache avez-vous sur votre ordinateur ?**
   Un ordinateur dispose de 3 niveaux de cache : L1, L2 et L3.

2. **Quelle est la taille d'une ligne de cache en bytes ?**
   La taille d'une ligne de cache est de 64 bytes.

3. **Quels sont les tailles des caches pour chaque niveau en KiB ?**
    - L1 : - LEVEL1_ICACHE_SIZE = 32768 → 32 KB     (Instruction cache)
           - LEVEL1_DCACHE_SIZE = 49152 → 48 KB     (Data cache)
  
    - L2 : - LEVEL2_CACHE_SIZE = 1310720 → ~1.25 MB
    - L3 : - LEVEL3_CACHE_SIZE = 12582912 → 12 MB 

## Expérience n°1 : Prédiction d'embranchements

### Questions :

En rapport avec branch-mispredictor.cpp

1. **Quelle est la différence entre les deux exécutions ?**
   La différence entre les deux exécutions est que la première exécution (avec l'argument 0) traite un tableau non trié, tandis que la seconde exécution (avec l'argument 1) traite un tableau trié.

2. **Pouvez-vous expliquer pourquoi ?**
   La différence de performance entre les deux exécutions s'explique par la prédiction d'embranchements. Dans le cas du tableau trié, les embranchements sont plus prédictibles, ce qui permet au processeur de mieux anticiper les instructions à exécuter, réduisant ainsi les pénalités de misprediction. En revanche, dans le cas du tableau non trié, les embranchements sont moins prévisibles, ce qui entraîne un taux de misprediction plus élevé et une performance réduite.

3. **Reportez le ratio `branch-misses / branches` pour les deux exécutions.**

```bash
liveuser@localhost-live:~/Documents/labo-pco-performance-luc-marchand$ perf stat -e branches,branch-misses ./branch-misprediction 0
1532

 Performance counter stats for './branch-misprediction 0':

       597,398,343      cpu_atom/branches/u                                                     (0.31%)
       882,825,752      cpu_core/branches/u                                                     (99.69%)
       117,260,856      cpu_atom/branch-misses/u                                                (0.31%)
       209,987,336      cpu_core/branch-misses/u                                                (99.69%)

       1.649458841 seconds time elapsed

       1.614495000 seconds user
       0.031874000 seconds sys


liveuser@localhost-live:~/Documents/labo-pco-performance-luc-marchand$ perf stat -e branches,branch-misses ./branch-misprediction 1
192

 Performance counter stats for './branch-misprediction 1':

       123,122,151      cpu_atom/branches/u                                                     (1.68%)
     1,844,255,935      cpu_core/branches/u                                                     (98.32%)
        22,980,342      cpu_atom/branch-misses/u                                                (1.68%)
        36,615,767      cpu_core/branch-misses/u                                                (98.32%)

       0.715485354 seconds time elapsed

       0.684595000 seconds user
       0.028880000 seconds sys
```
   - Pour l'exécution avec le tableau non trié (argument 0) :
     - Branches : 597,398,343 + 882,825,752 = 1,480,224,095
     - Branch-misses : 117,260,856 + 209,987,336 = 327,248,192
     - Ratio : 327,248,192 / 1,480,224,095 ≈ 0.221 (22.1%)

   - Pour l'exécution avec le tableau trié (argument 1) :
     - Branches : 123,122,151 + 1,844,255,935 = 1,967,378,086
     - Branch-misses : 22,980,342 + 36,615,767 = 59,596,109
     - Ratio : 59,596,109 / 1,967,378,086 ≈ 0.030 (3.0%)

   Le ratio de mispredictions est significativement plus élevé pour le tableau non trié (22.1%) par rapport au tableau trié (3.0%), ce qui explique la différence de performance entre les deux exécutions.


   4. **Que pensez-vous de la question StackOverflow Why is it faster to process a sorted array than an unsorted array? et du nombre de upvotes ?**
   La question sur StackOverflow "Why is it faster to process a sorted array than an unsorted array?" a reçu de nombreux upvotes car elle touche à un concept fondamental en informatique : la prédiction d'embranchements. Les développeurs et les étudiants en informatique sont souvent confrontés à des problèmes de performance liés à la manière dont les données sont organisées en mémoire. Un tableau trié permet au processeur de mieux prédire les embranchements, ce qui améliore les performances. La question est pertinente et aide à expliquer un phénomène qui peut sembler contre-intuitif pour ceux qui ne sont pas familiers avec les microarchitectures des processeurs, donc elle a suscité beaucoup d'intérêt et de discussions, d'où le nombre élevé d'upvotes.


   ## Expérience n°2 : Latences de la SDRAM

   ### Questions :

   