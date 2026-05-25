# Grupp 5 - Deep Learning: Fashion MNIST & Data Augmentation

Det här projektet är en examinerande gruppuppgift i kursen **Deep Learning**. Vi har byggt en CNN-modell som klassificerar bilder från datasetet **Fashion MNIST** och undersökt hur **data augmentation** påverkar modellens resultat och generalisering.

## Syfte

Målet var inte bara att få hög accuracy, utan att förstå hur olika val påverkar modellen. Gruppens fokusområde var **Data Augmentation**.

Vi ville undersöka:

- om augmentation kan minska overfitting
- vilka typer av augmentation som fungerar bäst för Fashion MNIST
- om starkare augmentation alltid ger bättre resultat
- hur resultaten kan förklaras med träningskurvor, test accuracy och overfitting-gap

## Dataset

Vi använde **Fashion MNIST** från Keras.

Datasetet innehåller gråskalebilder av klädesplagg.

| Del | Antal bilder |
|---|---:|
| Träningsdata | 60 000 |
| Testdata | 10 000 |
| Bildstorlek | 28 x 28 pixlar |
| Antal klasser | 10 |

Klasserna är:

1. T-shirt/top
2. Trouser
3. Pullover
4. Dress
5. Coat
6. Sandal
7. Shirt
8. Sneaker
9. Bag
10. Ankle boot

## Gruppens arbetsfördelning

| Person | Huvudansvar |
|---|---|
| Irfan | Dataförståelse och preprocessing |
| Salam | Baseline-modell och träning |
| Yunus | Data augmentation och experiment |
| Elza | Visualisering, analys och slutsatser |
| Eliesa | Presentation och README |

Alla i gruppen ska kunna förstå helheten, men varje person hade ett tydligt huvudansvar.

## Preprocessing

Innan modellen tränades gjorde vi följande:

- laddade Fashion MNIST via Keras
- kontrollerade klasser och antal bilder
- normaliserade pixelvärden från `0-255` till `0-1`
- ändrade bildformen till `(28, 28, 1)` så att CNN-modellen kan läsa gråskalebilder

## Modell: CNN

Vi använde en enkel CNN-modell som baseline.

Modellen består av:

- Input-lager: `(28, 28, 1)`
- Conv2D med 32 filter
- MaxPooling2D
- Conv2D med 64 filter
- MaxPooling2D
- Flatten
- Dense-lager med 128 neuroner
- Output-lager med 10 neuroner och Softmax

Baseline-modellen tränades utan data augmentation och användes som jämförelsepunkt.

## Fokusområde: Data Augmentation

**Data augmentation** betyder att man skapar små realistiska variationer av träningsbilderna under träningen.

Exempel:

- bilden roteras lite
- bilden zoomas lite in eller ut
- bilden flyttas lite i sidled eller höjdled

Etiketten ändras inte. En sneaker är fortfarande en sneaker även om bilden är lite roterad eller förflyttad.

Syftet är att modellen inte bara ska memorera exakta träningsbilder, utan lära sig mer generella mönster. Det kan minska overfitting och göra modellen bättre på ny data.

## Experiment

Vi tränade samma CNN-arkitektur flera gånger. Den enda skillnaden var augmentation-lagret.

| Experiment | Augmentation |
|---|---|
| Baseline | Ingen augmentation |
| Rotation 5% | `RandomRotation(0.05)` |
| Zoom 10% | `RandomZoom(0.10)` |
| Shift 10% | `RandomTranslation(0.10, 0.10)` |
| Mild kombinerad | Rotation + zoom + shift i låg nivå |
| Stark kombinerad | Rotation + zoom + shift i starkare nivå |

Detta gjorde jämförelsen mer rättvis eftersom modellen var densamma i alla experiment.

## Resultat

| Experiment | Test accuracy | Tolkning |
|---|---:|---|
| Shift 10% | 90.35% | Bäst resultat och bra generalisering |
| Zoom 10% | 89.82% | Lite bättre än baseline |
| Rotation 5% | 89.42% | Lite bättre än baseline och mindre overfitting |
| Baseline | 89.32% | Bra accuracy men tydlig overfitting |
| Mild kombinerad | 87.22% | Mindre overfitting men lägre accuracy |
| Stark kombinerad | 78.37% | För stark augmentation skadade resultatet |

## Analys

Baseline-modellen fick hög träningsaccuracy men lägre validation accuracy. Det betyder att modellen började överanpassa sig till träningsdatan.

Shift 10% blev bäst i våra experiment. Den gav högst test accuracy och minskade overfitting-gapet tydligt jämfört med baseline.

Stark kombinerad augmentation gav sämst resultat. Det visar att mer augmentation inte alltid är bättre. Eftersom Fashion MNIST-bilderna är mycket små kan för stark rotation, zoom eller förflyttning göra att viktig information försvinner.

## Slutsatser

Vi lärde oss att:

- data augmentation kan hjälpa modellen att generalisera bättre
- små realistiska förändringar fungerar bättre än starka förändringar
- test accuracy räcker inte som enda mått
- träningskurvor och overfitting-gap hjälper oss förstå modellen bättre
- vissa klasser är svåra eftersom de liknar varandra, till exempel Shirt, T-shirt/top, Pullover och Coat

## Begränsningar

Projektet har några begränsningar:

- Fashion MNIST har låg upplösning: 28 x 28 pixlar
- vissa klasser är visuellt väldigt lika
- vi tränade modellerna i 10 epoker
- resultaten kan variera lite mellan olika körningar

## Möjliga förbättringar

Nästa steg hade kunnat vara att testa:

- EarlyStopping
- Dropout
- L2-regularisering
- fler augmentation-nivåer
- fler epoker
- upprepade körningar och medelvärde av resultat

## Projektstruktur

```text
DL-grupp5-augmentation/
├── images/              # Sparade grafer och exempelbilder
├── models/              # Sparade Keras-modeller
├── notebook/            # Jupyter Notebook
├── presentation/        # Presentation
├── README.md            # Projektbeskrivning
├── requirements.txt     # Python-paket
└── LICENSE
```

## Installation

Skapa och aktivera en virtuell miljö.

```bash
python -m venv .venv
```

Windows PowerShell:

```bash
.venv\Scripts\Activate.ps1
```

Installera paket:

```bash
pip install -r requirements.txt
```

## Köra notebooken

Starta Jupyter Notebook:

```bash
jupyter notebook
```

Öppna sedan notebooken i mappen `notebook/` och kör cellerna uppifrån och ner.

## Requirements

En enkel `requirements.txt` kan se ut så här:

```text
tensorflow
keras
numpy
pandas
matplotlib
scikit-learn
seaborn
jupyter
```

## Kort sammanfattning

Data augmentation gjorde att modellen fick se mer varierad träningsdata. Det hjälpte särskilt när variationen var realistisk, som med Shift 10%. Däremot visade våra resultat att för stark augmentation kan göra bilderna svårare att tolka och försämra modellen.
