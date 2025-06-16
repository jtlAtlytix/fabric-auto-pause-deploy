# 🚀 Fabric Auto-Pause Solution

Denne løsning hjælper dig med automatisk at **pause din Microsoft Fabric kapacitet** på et valgt tidspunkt hver dag.  
Den er designet til at være enkel at bruge og sikker at implementere — uden at du behøver at skrive kode.

---

## ⚙ Hvad løsningen gør
✅ Opretter en **Azure Logic App** i din Azure subscription  
✅ Logic App kører dagligt på det tidspunkt du vælger (f.eks. 17:00 UTC)  
✅ Logic App sender et API-kald til Microsoft Fabric for at pause kapaciteten  
✅ Hvis der opstår en fejl under pause, kan appen:
- Sende en e-mail notifikation
- Sende en Teams notifikation
- Sende begge dele
- Eller undlade notifikationer (du vælger)

✅ (Valgfrit) Logic App kan også opsættes til at genoptage (resume) kapaciteten på et andet tidspunkt

---

## 📌 Krav
For at bruge løsningen skal du have:
- En **aktiv Azure subscription**
- En **resource group** (du kan oprette ny under deployment, hvis du ikke har en)
- En **Microsoft Fabric kapacitet**, som du ønsker at pause (du skal kende kapacitetens navn)

💡 **OBS:**  
Din Azure subscription skal have ressource provideren **Microsoft.Logic** registreret (det er den der tillader Logic Apps).  
Hvis den ikke er registreret, vil deployment fejle, og du vil få en fejlbesked.  
👉 Se nedenfor hvordan du får den registreret.

---

## 🚀 Sådan sætter du løsningen op

1️⃣ Klik på knappen herunder for at starte deployment i Azure:

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FjtlAtlytix%2Ffabric-auto-pause-deploy%2Fmain%2Ffabric-auto-pause.json)

---

2️⃣ I Azure deployment-formularen:
- **Resource Group** – vælg eksisterende eller opret en ny
- **Fabric Capacity Name** – navnet på din Fabric kapacitet (f.eks. `atlytixfabric`)
- **Admin Email** – den e-mail som skal modtage notifikationer hvis pause fejler
- **Notification Type** – vælg `email`, `teams`, `both` eller `none`
- **Pause Time** – hvornår kapaciteten skal pauses (default er `17:00`, UTC)
- **Resume Time** – hvornår kapaciteten evt. skal genoptages (valgfrit, UTC)
- **Team ID** + **User ID** – kun hvis du vælger Teams notifikationer

---

3️⃣ Klik **Review + create** og derefter **Create**

---

## ⚠ Hvis Logic App deployment fejler (Microsoft.Logic ikke registreret)

En **Azure administrator** (med Owner eller Contributor på subscription) skal gøre følgende:

### I Azure Portal:
- Gå til **Subscriptions > [din subscription] > Resource providers**
- Søg: `Microsoft.Logic`
- Klik: `Register`

### Eller via CLI:
```bash
az provider register --namespace Microsoft.Logic

____
Dette skal kun gøres én gang pr. subscription.

💡 Hvad sker der efter deployment?
Azure opretter en Logic App i den valgte Resource Group

Logic App kører dagligt på det tidspunkt du valgte

Hvis pause fejler, sender den notifikationer som valgt

Du kan se Logic App’en i Azure Portal under Logic Apps

Du kan ændre tidspunkter og indstillinger efter behov

🔒 Sikkerhed
Løsningen deployer kun til din Azure subscription — vi har ingen adgang til din kapacitet

Der ligger ingen følsomme data i GitHub — kun template-struktur

Logic App bruger standard Azure API'er og connectorer

💸 Omkostninger
Selve løsningen (ARM template + deploy) er gratis

Logic App faktureres på forbrug — typisk ekstremt lavt (øre pr. måned ved 1-2 kald pr. dag)

📬 Kontakt os
Har du spørgsmål, eller brug for hjælp til opsætning?

👉 Kontakt vores support
Send en mail via din kontaktformular eller direkte til:
📧 jtl@atlytix.dk

✅ Eksempel på brugsscenarie
En kunde med kapacitet contosoFabric i resource group contosoRG kan:

Vælge dagligt pause kl. 17:00

Sætte Resume kl. 07:00 (valgfrit)

Modtage mail hvis pause fejler

Alt konfigureres ved deploy — ingen yderligere kode
