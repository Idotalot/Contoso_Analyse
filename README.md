# Deliverable 2 — Contoso-analyse in Python

Jordy Perret · HBO-ICT, Avans Hogeschool · Leerjaar 3, semester 1
Data Engineering, leeruitkomst 1

Deze repository bevat de notebook die de tien visualisaties uit deliverable 1
in Python realiseert, plus een Dev Container zodat de notebook op elke
computer draait zonder dat er pakketten geïnstalleerd hoeven te worden.

## Inhoud

```
deliverable2/
├── .devcontainer/
│   └── devcontainer.json      containerdefinitie voor VS Code
├── data/                      brongegevens (niet in Git, zie hieronder)
├── output/                    grafieken die de notebook wegschrijft
├── analyse_contoso.ipynb      de notebook
├── requirements.txt           vastgezette pakketversies
└── .gitignore
```

## Draaien

### Benodigd op de computer

* [Docker Desktop](https://www.docker.com/products/docker-desktop/)
* [Visual Studio Code](https://code.visualstudio.com/)
* De VS Code-extensie **Dev Containers** (`ms-vscode-remote.remote-containers`)

Meer niet. Python, pandas en matplotlib komen in de container te staan, niet
op de computer zelf.

### Stappen

1. Clone de repository.
2. Zet de CSV-bestanden in de map `data/` (zie hieronder welke).
3. Open de map in VS Code.
4. VS Code toont rechtsonder de melding *Reopen in Container*. Klik daarop.
   Gebeurt dat niet, druk dan op `F1` en kies **Dev Containers: Reopen in
   Container**.
5. De eerste keer duurt het bouwen een paar minuten. Daarna is het enkele
   seconden, omdat de image lokaal bewaard blijft.
6. Open `analyse_contoso.ipynb` en kies **Kernel → Python 3.12** uit de
   container.
7. **Run All**.

### Brongegevens

De CSV's staan bewust niet in Git. `Sales.csv` is 203 MB en GitHub weigert
bestanden boven 100 MB; daarnaast is de Contoso-dataset lesmateriaal dat via
Brightspace beschikbaar is.

Zet deze tien bestanden in `data/`:

| Bestand | Inhoud |
|---|---|
| `Sales.csv` | feitentabel, 2.282.482 transacties |
| `Calendar.csv` | datumdimensie, 2005–2018 |
| `Channel.csv` | verkoopkanalen |
| `Geography.csv` | land, stad, continent |
| `Stores.csv` | winkels, inclusief vloeroppervlak |
| `Product.csv` | producten en merken |
| `ProductSubcategory.csv` | subcategorieën |
| `ProductCategory.csv` | categorieën (sub-dimensie) |
| `Promotion.csv` | promoties en kortingspercentages |
| `publicHolidays.csv` | eigen dataset, feestdagen (Kaggle) |

Heten de bestanden anders, pas dan de dictionary `BESTANDEN` in paragraaf 1.2
van de notebook aan. Dat is de enige plek in de notebook waar paden staan.

## Waarom een Dev Container

Zonder container moet op elke computer Python worden geïnstalleerd, een
virtual environment worden aangemaakt en de pakketten worden geïnstalleerd —
en dan nog kan een andere pandas-versie ander gedrag geven. De container legt
het besturingssysteem, de Python-versie en de pakketversies vast in twee
bestanden die meegaan in Git. Wie de repository cloont krijgt exact dezelfde
omgeving.

De versies in `requirements.txt` staan vast met `==`. Zonder dat zou `pip`
de nieuwste versie pakken, en dan is de reproduceerbaarheid die de container
oplevert alsnog weg.

## Zonder VS Code

De notebook draaien via Docker zonder VS Code kan ook, met Jupyter in de
browser:

```bash
docker run --rm -it \
  -v "$(pwd)":/werk -w /werk \
  -p 8888:8888 \
  python:3.12 \
  bash -c "pip install -r requirements.txt jupyterlab && \
           jupyter lab --ip=0.0.0.0 --allow-root --no-browser"
```

Open daarna de link met het token die in de terminal verschijnt.