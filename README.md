# SafeData 🔒

**Lokalt værktøj til pseudonymisering af børne- og medarbejderdata**

Et enkelt, browser-baseret system til pseudonymisering af persondata — uden server, uden cloud, uden installation.

---

## Hvad er dette?

SafeData er et lokalt HTML-værktøj, der hjælper dig med at:
- Erstatte rigtige navne med unikke koder i CSV-filer
- Gemme koblingen (nøglefilen) sikkert og separat
- Koble navnene tilbage igen, når du har brug for det

**Formålet:** At du kan arbejde med data i AI-værktøjer og regneark uden at sende rigtige navne afsted.

---

## ⚡ Hurtig start (3 trin)

```
1. Download filen app.html
2. Dobbeltklik på app.html — den åbner i din browser
3. Brug systemet — alt kører lokalt
```

Ingen installation. Ingen server. Ingen internetforbindelse påkrævet.

---

## Mappestruktur

```
SafeData/
│
├── app.html              ← Hele applikationen (denne fil er alt du behøver)
├── README.md             ← Denne vejledning
│
└── docs/
    ├── eksempel_input.csv     ← Eksempel på input-fil
    ├── eksempel_output.csv    ← Eksempel på pseudonymiseret output
    └── eksempel_noeglefil.json ← Eksempel på nøglefil (struktur)
```

**Dine egne filer gemmes automatisk i browseren** (localStorage). Du behøver ikke oprette mapper til dem.

---

## Funktioner

### 1. Opret person
- Skriv navn og vælg kategori (barn/medarbejder)
- Systemet genererer en unik kode: `B-2F9A` (barn) eller `M-7C3E` (medarbejder)
- Samme navn = altid samme kode
- Eksportér nøglefilen som JSON eller CSV

### 2. Pseudonymisér datasæt
- Upload en CSV-fil (semikolon- eller kommasepareret)
- Eller indsæt CSV-tekst direkte
- Vælg hvilke kolonner der indeholder navne
- Download pseudonymiseret fil (sikker til AI)

### 3. Re-identificér datasæt
- Upload en pseudonymiseret fil
- Systemet slår koderne op i nøglefilen
- Download fil med rigtige navne

### 4. Oversigt
- Se alle registrerede personer
- Søg på navn eller kode
- Slet enkeltpersoner eller ryd alle data

---

## CSV-format

Systemet understøtter standard CSV-filer. Eksempel:

**Input (med navne):**
```
navn;klasse;dato;kommentar
Emma Hansen;3A;2024-01-15;God dag
Lars Nielsen;3B;2024-01-16;Observation
```

**Output (pseudonymiseret):**
```
navn;klasse;dato;kommentar
B-2F9A;3A;2024-01-15;God dag
B-7C3E;3B;2024-01-16;Observation
```

Tip: Brug UTF-8 kodning og enten `;` eller `,` som separator.

---

## Nøglefil-format

Nøglefilen gemmes som JSON. Eksempel:

```json
{
  "meta": {
    "tool": "SafeData v1.0",
    "exported": "2024-01-15T10:00:00.000Z",
    "warning": "FORTROLIG — Indeholder rigtige navne. Del ALDRIG med AI.",
    "count": 2
  },
  "persons": {
    "emma hansen": {
      "name": "Emma Hansen",
      "type": "barn",
      "code": "B-2F9A",
      "created": "15.1.2024"
    }
  }
}
```

---

## Datahåndtering og sikkerhed

| Fil | Indhold | Sikker til AI? |
|-----|---------|---------------|
| `pseudonymiseret_data.csv` | Kun koder, ingen navne | ✅ Ja |
| `noeglefil_FORTROLIG.json` | Navne + koder | ❌ **Nej — aldrig** |
| Browser localStorage | Nøglefil gemt lokalt i browseren | ❌ Kun lokal brug |

**Teknisk:**
- Data gemmes kun i din browsers `localStorage`
- Ingen netværkskommunikation af nogen art
- Alle filer er på din enhed
- Koden kan inspiceres — den har ingen skjulte afsendelser

---

## ⚠ GDPR — Vigtige begrænsninger

**Dette system er pseudonymisering, ikke anonymisering.**

- Pseudonymiserede data tæller stadig som persondata under GDPR
- Nøglefilen udgør persondata og er underlagt databeskyttelsesreglerne
- Du er selv dataansvarlig og skal have et lovligt behandlingsgrundlag
- Systemet reducerer eksponeringsrisiko men fritager ikke for GDPR-ansvar
- Kompromitteres nøglefilen, ophæves pseudonymiseringen

**Opbevar nøglefilen:**
- Krypteret (fx i en adgangskodebeskyttet ZIP eller på krypteret drev)
- Separat fra analysedata
- Aldrig i cloud (Google Drive, OneDrive, Dropbox osv.)
- Aldrig sendt til AI-tjenester

---

## Teknisk valg — Hvorfor denne løsning?

Anbefalet stack: **Enkelt HTML + Vanilla JavaScript**

**Fordele:**
- Zero installation — åbn filen og brug den
- Zero dependencies — ingen npm, ingen Python, ingen server
- Transparent kode — én fil, nem at inspicere og forstå
- Maksimal portabilitet — virker på alle platforme med en browser
- Offline-first — kræver ikke internet

**Alternativer der blev fravalgt:**
- Python/Flask: Kræver installation og terminal
- Electron desktop-app: Kræver Node.js og build-process
- React/Vue: Unødvendig kompleksitet for dette formål

---

## Forslag til videre forbedringer

1. **Kryptering af nøglefilen** — Brug Web Crypto API til at kryptere localStorage med adgangskode
2. **Bulk-oprettelse** — Upload en liste med navne og opret alle på én gang
3. **Audit log** — Log hvornår og hvad der er pseudonymiseret
4. **Regex-baseret søgning** — Find navne i fritekst, ikke kun CSV-kolonner
5. **Electron-wrapper** — Pak appen som en rigtig desktop-app med filsystem-adgang
6. **Backup-reminder** — Påmind brugeren om at eksportere nøglefilen regelmæssigt
7. **Multiple nøglefiler** — Separat nøgle pr. projekt eller institution
8. **Offline-fonts** — Embed Google Fonts lokalt til brug uden internet

---

## Licens

MIT-licens — fri til brug, kopiering og videreudvikling.
Del gerne — men husk: nøglefiler der er genereret med systemet er fortrolige og må aldrig deles.

---

*SafeData er udviklet til lokal, ansvarlig brug med persondata i pædagogiske og administrative sammenhænge.*
