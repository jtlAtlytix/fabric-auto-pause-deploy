# 🚀 Fabric Auto-Pause Solution  

Denne løsning hjælper dig med automatisk at **pause din Microsoft Fabric kapacitet** på et valgt tidspunkt hver dag.  
Den er designet til at være enkel at bruge og sikker at implementere — uden at du behøver at skrive kode.  

---

## ⚙ Hvad løsningen gør  
✅ Opretter en **Azure Logic App** i din Azure subscription  
✅ Logic App kører dagligt på det tidspunkt du vælger (f.eks. 17:00 UTC)  
✅ Logic App sender et API-kald til Microsoft Fabric for at pause kapaciteten  
✅ (Valgfrit) Logic App kan også opsættes til at genoptage (resume) kapaciteten på et andet tidspunkt  

---

## 📌 Krav  
For at bruge løsningen skal du have:  
- En **aktiv Azure subscription**  
- En **resource group** (du kan oprette en ny under deployment, hvis du ikke har en)  
- En **Microsoft Fabric kapacitet**, som du ønsker at pause (du skal kende kapacitetens navn)  

💡 **OBS:**  
Din Azure subscription skal have ressource provideren **Microsoft.Logic** registreret (det er den der tillader Logic Apps).  
👉 Se nedenfor hvordan du får den registreret.  

---

## 🚀 Vælg din version af løsningen  

Du kan vælge mellem:  

### ✅ **Admin-version (med IAM-forsøg)**  
Denne version forsøger automatisk at give Logic App nødvendige rettigheder i Azure IAM.  
👉 Kræver, at brugeren der deployer har rettigheder til at oprette rolle-tildelinger (fx Owner på subscription).  

[![Deploy Admin Version](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FjtlAtlytix%2Ffabric-auto-pause-deploy%2Fmain%2Ffabric-auto-pause.json)  

---

### ✅ **User-version (uden IAM-forsøg)**  
Denne version opretter kun Logic App.  
En administrator skal efterfølgende give Logic App Managed Identity adgang til at pause kapaciteten.  

[![Deploy User Version](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FjtlAtlytix%2Ffabric-auto-pause-deploy%2Fmain%2Ffabric-auto-pause-user.json)  

---

## 📝 Når du deployer  
- **Resource Group** – vælg eksisterende eller opret en ny  
- **Fabric Capacity Name** – navnet på din Fabric kapacitet (f.eks. `virksomhedensfabric`)  
- **Pause Time** – hvornår kapaciteten skal pauses (default er `17:00`, UTC)  
- **Resume Time** – hvornår kapaciteten evt. skal genoptages (valgfrit, UTC)  

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
```
Dette skal kun gøres én gang pr. subscription.

💡 Hvad sker der efter deployment?
Azure opretter en Logic App i den valgte Resource Group

Logic App kører dagligt på det tidspunkt du valgte

Logic App forsøger at pause kapaciteten (og resume hvis valgt)

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
📧 jtl@atlytix.dk

✅ Eksempel på brugsscenarie
En kunde med kapacitet contosoFabric i resource group contosoRG kan:

Vælge dagligt pause kl. 17:00

Sætte Resume kl. 07:00 (valgfrit)

Deploye på få minutter — ingen yderligere kode

## 🛡 Hvordan giver man Logic App rettigheder til at pause Fabric?

Hvis du har brugt **User-versionen**, skal en administrator give Logic App Managed Identity de nødvendige rettigheder til at kunne pause (og evt. resume) Fabric kapaciteten.

### 1️⃣ Aktivér Logic App's Managed Identity
- Gå til **Resource groups > [din resource group] > Logic Apps > [din Logic App]**
- I menuen til venstre: vælg **Identity**
- Under **System assigned**, skift slider fra **Off** til **On**
- Klik **Save**
- Vent et øjeblik – nu bliver der oprettet et **Principal ID**

### 2️⃣ Giv Managed Identity adgang til Fabric kapacitet
- Gå til **Fabric kapacitetens ressource**
- Vælg **Access control (IAM)** > **Add > Add role assignment**
- Vælg:
  - **Role:** Contributor *(eller custom rolle med de nødvendige rettigheder til Fabric suspend/resume)*
  - **Assign access to:** Managed identity
  - Vælg Logic App’en

- Klik **Save**

✅ Nu har Logic App'en de nødvendige rettigheder.

---

💡 **Tip:**  
Hvis du deployer med **Admin-versionen**, forsøger ARM automatisk at tildele rollen. Hvis det fejler, brug ovenstående guide.
