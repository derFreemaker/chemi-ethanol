---
theme: seriph
title: Ethanol
class: text-center
drawings:
  persist: true
transition: slide-left
mdc: true
---

# Ethanol

<style>
.slidev-layout::before {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(rgba(0, 0, 0, 0.333), rgba(0, 0, 0, 0.533)),
    url("https://science.lu/sites/default/files/styles/max_1300x1300/public/2025-01/AdobeStock_366077518.webp?itok=B8IwAubA") center/cover no-repeat;
  filter: blur(4px);
  z-index: -1;
}
</style>

---
layout: default
---

# Strukturformel

<div
  v-click
>

<EthanolModel :size=350 />

</div>

<div
  v-click
  v-motion
    :initial="{ x: 50, y: 50 }"
    :enter="{ x: 0, y: 0 }"
>

**Summenformel:** $C_{2}H_{6}O$

</div>

---
layout: default
---

# Eigenschaften

- **Aggregatzustand:** Flüssig bei 20°C
- **Siedepunkt:** 78,23°C
- **Schmelzpunkt:** -114,1°C
- **Dichte:** 0,789g/cm³ (bei 20°C)
- **Löslichkeit:** Vollständig mischbar mit Wasser und vielen organischen Lösungsmitteln
- **Geruch:** Alkoholgeruch
- **Brennbarkeit:** Leicht entzündlich

## Weitere Namen:

- Ethylalkohol
- Alkohol
- Methylcarbinol

---
layout: default
---

# Gefahrenpotential

- Leicht entzündlich (H225): Flüssigkeit und Dampf
- Augenreizung (H319)

## Gesundheitsgefahren:

- **Bei Einnahme:** Rauschzustände, in hohen Dosen toxisch
- **Chronischer Konsum:** Organschäden, Störung auf das Zentralnervensystem
- **Brand- und Explosionsgefahr:** Dämpfe können mit Luft explosive Gemische bilden

---
layout: default
---

# Verwendung

- **Getränke:** Bier, Wein, Spirituosen
- **Desinfektionsmittel:** Händedesinfektion
- **Lösungsmittel:** Industrie, Pharmazie, Kosmetik
- **Kraftstoff:** Bioethanol als Benzinzusatz oder E85
- **Chemische Synthese:** Ausgangsstoff für Ether, Ester, andere Chemikalien
- **Konservierungsmittel:** Medizin, Biologie

---
layout: default
---

# Herstellung

## Alkoholische Gärung
Anaerober Abbau von Zucker durch Hefepilze (Saccharomyces cerevisiae)

- **Gleichung:** $C_{6}H_{12}O_{6} \to 2 C_{2}H_{6}O + 2 CO_{2}$
- **Rohstoffe:** Getreide, Obst, Zuckerrohr, Kartoffeln
- **Prozess:** Vergärung bei 25-30 °C, anschließende Destillation zur Konzentrierung

## Industrielle Synthese
Hydratisierung von Ethen: C₂H₄ + H₂O → C₂H₅OH

- **Bedingungen:** Katalysator (Phosphorsäure), hohe Temperatur und Druck
- **Vorteil:** Große Mengen, hohe Reinheit

---
layout: default
---

# Besonderheiten

- **Psychoaktive Wirkung:** Einziger legal zum Konsum zugelassener Alkohol für Menschen
- **Nachwachsender Rohstoff:** Bioethanol als regenerative Energiequelle
- **Amphiphile Eigenschaften:** Sowohl hydrophil (OH-Gruppe) als auch lipophil (C₂H₅-Rest)
- **Historische Bedeutung:** Älteste vom Menschen hergestellte psychoaktive Substanz

## Azeotrop mit Wasser:

Bei 95,6 Vol.-% bildet Ethanol ein azeotropes Gemisch mit Wasser, das durch normale Destillation nicht weiter getrennt werden kann.

## Vergällung:

Technisches Ethanol wird mit Zusätzen ungenießbar gemacht, um Steuern zu vermeiden.


---
layout: center
---

<div
  class="fade-delay-thank-you text-center"
  v-motion
    :initial="{ y: 0 }"
    :enter="{ y: -190, transition: { delay: 4000, duration: 1000 } }"
>

# Thank you for listening

<PoweredBySlidev/>

</div>

<div
  class="fade-delay-sources"
  style="
    position: absolute;
    left: 3rem;
    top: 10rem;
  "
  v-motion
    :initial="{ x: 1000 }"
    :enter="{ x: 0, transition: { delay: 4500, duration: 1500 } }"
>

# Sources

- https://chemie-schule.de/KnowHow/Ethanol
- https://internetchemie.info/substanz/Ethanol.php
- https://gestis.dguv.de/data?name=010420
- https://www.lernhelfer.de/schuelerlexikon/chemie-abitur/artikel/ethanol-eigenschaften-und-verwendung
- https://www.chemieunterricht.de/dc2/r-oh/alk-synt.htm

</div>

<style>
.fade-delay-thank-you {
  opacity: 0;
  animation: fadeIn 1s cubic-bezier(0.281, 0.776, 0.26, 1) forwards;
  animation-delay: 1s;
}

.fade-delay-sources {
  opacity: 0;
  animation: fadeIn 1s cubic-bezier(0.281, 0.776, 0.26, 1) forwards;
  animation-delay: 5s;
}

@keyframes fadeIn {
  to { opacity: 1; }
}
</style>
