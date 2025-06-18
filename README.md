# Fabric AutoPause / Resume - Udviklet af Atlytix

![Atlytix Logo](https://www.atlytix.dk/path-to-your-logo.png)

🚀 **Udviklet af Atlytix**  
🌐 [www.atlytix.dk](https://www.atlytix.dk)  
📧 [jtl@atlytix.dk](mailto:jtl@atlytix.dk)

---

## Introduktion
Denne løsning hjælper dig med at opsætte automatisk **pause** og valgfri **genoptagelse (resume)** af din Microsoft Fabric kapacitet, så du kan spare omkostninger uden at gå på kompromis med funktionalitet.

Denne guide guider dig fra start til slut i opsætningen – med præcise klik-vejledninger og tips til at undgå typiske fejl.

---

## Forudsætninger
✅ En Azure Subscription (med rettigheder til at deploye ressourcegrupper og Logic Apps)  
✅ Navn på din Microsoft Fabric kapacitet  
✅ Kendskab til hvilken ressourcegruppe kapaciteten ligger i  

---

## Trin-for-trin guide

### 1️⃣ Gå til Azure Custom Deployment
- Klik på dette link: [Deploy ARM Template](https://portal.azure.com/#create/Microsoft.Template)  
- Vælg **Build your own template in the editor** eller upload din JSON-template.

---

### 2️⃣ Indsæt ARM-template
- Indsæt den leverede ARM-template i editoren.
- Klik **Save**.

---

### 3️⃣ Udfyld felterne
Når formularen vises:
- **Subscription:** Vælg din subscription (fx *Atlytix Primary Subscription*)
- **Resource group:** Vælg eksisterende eller opret ny
- **Region:** Vælg fx *West Europe*
- **Fabric Capacity Name:** Indtast navnet på din kapacitet (fx `myfabric-capacity`)
- **Resource Group Name:** Indtast navnet på ressourcegruppen hvor kapaciteten ligger
- **Pause Time:** Sæt tid (UTC format, fx `17:00`)
- **Resume Time (valgfri):** Udfyld hvis du vil have auto-resume (fx `06:00`), eller lad stå tomt

---

### 4️⃣ Gennemgå og deploy
- Klik på **Review + create**
- Azure validerer templaten – klik derefter **Create**

---

## Typiske problemer og løsninger

| Problem | Årsag | Løsning |
|----------|--------|----------|
| IAM role assignment fejler | Brugeren der deployer har ikke Owner-rettigheder | Tildel rolle manuelt via Azure portal > IAM |
| Logic App starter ikke | Forkert tidspunkt eller format | Tjek at `Pause Time` / `Resume Time` er i format HH:mm |
| Resume sker ikke | Resume time tomt eller disabled | Angiv et korrekt tidspunkt i `Resume Time` |

---

## Hvad sker der når deployment er færdig?
✅ Logic App `fabricAutoPauseByAtlytix` er oprettet i den valgte ressourcegruppe  
✅ Tags er tilføjet: DevelopedBy=Atlytix, Website=https://www.atlytix.dk, Contact=jtl@atlytix.dk  
✅ Daglig pause (og evt. resume) er sat op via Logic App workflows

---

## Support
Hvis du har brug for hjælp, kontakt:  
📧 [jtl@atlytix.dk](mailto:jtl@atlytix.dk)  
🌐 [www.atlytix.dk](https://www.atlytix.dk)

---

© Atlytix – alle rettigheder forbeholdes.
