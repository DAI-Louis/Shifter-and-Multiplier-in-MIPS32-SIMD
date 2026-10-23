# Shifter et Multiplieur MIPS32 SIMD

## À propos du projet
Ce projet a été réalisé durant l'année 2025-2026 dans le cadre du Master SESI. Il a pour objectif de concevoir et de vérifier une version "Single Instruction Multiple Data" (SIMD) d'opérateurs de décalage et de multiplication pour un processeur MIPS32 Vectoriel. 

## Architecture et Fonctionnalités
Les opérateurs ont été conçus pour effectuer des calculs en un seul cycle d'horloge tout en basculant entre 3 configurations (Word, Half, Byte), permettant de traiter jusqu'à 16 vecteurs en parallèle.

### 1. Shifter Paramétrable (Barrel Shifter)
*   **Opérations supportées :** Décalages logiques (SLL/SRL), décalages arithmétiques (SRA) et rotations (ROL/ROR).
*   **Conception matérielle :** Utilisation d'une architecture en "Barrel Shifter" 32 bits, un circuit combinatoire organisé en étages (notamment pour gérer les croisements Half et Byte).

### 2. Multiplieur Vectoriel
*   **Optimisation :** Implémentation basée sur un Arbre de Wallace utilisant des compresseurs 3:2 "Carry Save Adder" (CSA) pour un délai logarithmique.
*   **Opérations :** Prise en charge des opérandes signées et non signées, ainsi que des opérations de type Mul-Add ($R = Z + X \cdot Y$) et Mul-Sub ($R = Z - X \cdot Y$).
*   **Formatage :** Découpage et extension de signe spécifiques sur des registres étendus à 128 bits pour éviter les collisions entre les blocs de données (Word, Half, Byte) lors du calcul des produits partiels.

## Validation et Procédure de Recette
La validation des opérateurs a été effectuée en confrontant le code matériel à un modèle de référence :
*   **Description matérielle :** L'intégralité du Shifter et du Multiplieur est décrite en VHDL.
*   **Génération de tests :** Un programme en C génère un fichier contenant 10 millions de patterns de tests aléatoires avec leurs résultats de référence.
*   **Simulation :** Le programme `Simulad` injecte ces patterns dans l'opérateur VHDL et vérifie l'absence d'erreurs.

## Performances
Les synthèses ont été réalisées sur un FPGA Zybo Z7-20 :
*   Le Multiplieur Vectoriel prend 1,49 fois plus de surface matérielle (LUT) que la version scalaire, ce qui est inférieur au coût théorique d'un ajout de multiplieurs distincts pour chaque mode.

---
*Auteurs : Tifenn FABRICI, Asmar MOHAMED, Louis DAI*
