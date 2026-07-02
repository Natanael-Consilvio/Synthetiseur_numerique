Pour que les codes fonctionnent, il faut s'assurer du bon branchement physique mais également des bons ports numériques déclarés sur MCUXpresso. 

D'abord, voici les SDK à activer AVANT de créer un projet (lors de la création du projet) : 
<img width="1117" height="917" alt="image" src="https://github.com/user-attachments/assets/6081cdcf-ab60-4ec8-957a-dcab331e3441" />

Ici vous chercherez et activerez : 
fsl_lpuart ; fsl_sai ; fsl_edma ; fsl_dmamux ; fsl_port / fsl_gpio ; fsl_clock / fsl_reset / fsl_common ; CMSIS ; LVGL 

Pour la communication il se peut qu'il soit nécessaire d'aller chercher des fichiers sur GUI_Guider. C'est un logiciel à installer nous permettant d'obtenir des librairies permettant la communication entre la carte et l'écran. Une fois que vous aurez crée un projet, vous pourrez aller chercher certains fichiers que vous collerez directement dans votre projet MCUXpresso. Voici les fichiers que nous avions récupérés et collé dans le projet MCXN :lvgl_support.c ; lvgl_support_board.h

Commençons par les branchements physiques : 
  ECRAN sur le port J8 de la carte MCXN, il s'emboite directement sur la section J8 de la carte MCXN.
  CARTE SON : SCK : GND 
              BCK : J3[15] (section J3, port 15)
              DIN : J3[7]
              RCK : J3[13]
              GND : GND
              VIN : 3V3
              FLT : GND
              DEMP : GND
              XSMT : GND
              FMT : GND

 CIRCUIT ELECTRONIQUE : RX : J9[28]


Voici le montage du circuit électrique
 <img width="957" height="263" alt="image" src="https://github.com/user-attachments/assets/b78de2c6-ba31-431f-a5a8-8f51de974028" />
 
Sortant du MIDI femelle le port 4 est le cable vert et le port 5 le cable blanc

Pour ce qui est de la configuration des broches sur MCUXpresso :

Déclaration dans PINS : Aller dans PINS puis dans peripherials signal chercher: FLEXCOMM3 et activer P0 puis [D2] (broche RX)
                                                                                FLEXOMM4 et activer P0 puis [A1] et P1 puis [B1]
                                                                                SAI0 et activer TX_BLCK, TX_SYNC et TXD0 et le premier à chaque fois (carte son)

Activation des horloges : On cherche ici à allumer les horloges des ports que l'on vient d'ouvrir. 
Dans Clock, dans la fenêtre clock table, aller dans clock output puis chercher:
FLEXCOMM 1 <img width="483" height="343" alt="image" src="https://github.com/user-attachments/assets/41108d87-b6db-469e-921b-0903b1d23923" />

FLEXIOCLK <img width="481" height="347" alt="image" src="https://github.com/user-attachments/assets/844b480a-b6af-4df1-b833-6e6dd301c35f" />

FRO_12_clock <img width="472" height="210" alt="image" src="https://github.com/user-attachments/assets/0ee97dd1-7c40-42bb-8078-f0d0ee8d54b2" />

FRO_HF_clock <img width="441" height="235" alt="image" src="https://github.com/user-attachments/assets/704eeaed-74e4-4178-a93d-bb09f86f0e9f" />

PLL0_CLK_clock <img width="505" height="530" alt="image" src="https://github.com/user-attachments/assets/df74ea98-c51a-4ab4-aa5f-69c190e30f9f" />


Vous passerez surment pas mal de temps à réussir à faire communiquer le tout, c'est selon moi la chose la plus embêtante car il y a toujours un petit soucis. Il se peut que vous y passier plusieurs heures/jours mais il ne faut pas désesperer. Bon courage <3

Suite à la compilation du CODE 2 sur la carte, voici le roles des différentes molettes : 
Molette noire 
•	Rotation → séléctionne l'oscillateur  (OSC 1/2/3)
•	Clic  → change  l'oscillateur sélectionné
8 molettes blanches 
•	M1 → Detune de l'oscillateur sélectionné
•	M2 → VCF Cutoff global (20 Hz → 10 kHz)
•	M3 → VCF Résonance globale (0 → 0.95)
•	M4→ LFO Rate global (0,1 → 20 Hz)
•	M5 → LFO → Pitch global 
•	M6→ LFO → Filtre global
•	M7→ LFO → Gain global
•	M8→ Volume de l'oscillateur sélectionné
4 faders (F1 à F4)
•	F1→ EQ Basses
•	F2→ EQ Médiums 
•	F3→ EQ Aigus
•	F4→ Volume Master 

