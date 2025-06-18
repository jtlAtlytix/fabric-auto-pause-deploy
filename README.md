# 🚀 Fabric Auto-Pause / Resume Solution - Udviklet af Atlytix

![Atlytix Logo](https://raw.githubusercontent.com/jtlAtlytix/fabric-auto-pause-deploy/main/5.png)

🌐 [www.atlytix.dk](https://www.atlytix.dk)  
📧 [jtl@atlytix.dk](mailto:jtl@atlytix.dk)

## 👉 [Download komplet deployment guide som PDF](https://github.com/jtlAtlytix/fabric-auto-pause-deploy/raw/main/AutoPauseGuide.pdf)

---

## ⚙ Hvad løsningen gør  
✅ Opretter en **Azure Logic App** i din Azure subscription  
✅ Logic App kører dagligt på det tidspunkt du vælger (f.eks. 17:00 UTC)  
✅ Logic App sender et API-kald til Microsoft Fabric for at pause kapaciteten  
✅ (Valgfrit) Logic App kan også opsættes til at genoptage (resume) kapaciteten på et andet tidspunkt  

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
- En **resource group** (du kan oprette en ny under deployment)  
- En **Microsoft Fabric kapacitet** (du skal kende kapacitetens navn)  

💡 **OBS:**  
Din Azure subscription skal have ressource provideren **Microsoft.Logic** registreret (det er nødvendigt for Logic Apps).  
👉 Se længere nede hvordan du registrerer den.

---

## 🚀 Vælg din version  

### ✅ **Admin-version (med IAM-forsøg)**  
Denne version forsøger automatisk at give Logic App nødvendige rettigheder i Azure IAM.  
👉 Kræver, at brugeren der deployer har rettigheder til at tildele roller (fx Owner på subscription).

[![Deploy Admin Version](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FjtlAtlytix%2Ffabric-auto-pause-deploy%2Fmain%2Ffabric-auto-pause.json)

---

### ✅ **User-version (uden IAM-forsøg)**  
Denne version opretter kun Logic App.  
👉 En administrator skal bagefter give Logic App Managed Identity adgang til at pause kapaciteten.

[![Deploy User Version](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FjtlAtlytix%2Ffabric-auto-pause-deploy%2Fmain%2Ffabric-auto-pause-user.json)

---

## 📝 Når du deployer  
- **Resource Group** – vælg eksisterende eller opret en ny  
- **Fabric Capacity Name** – navnet på din kapacitet (f.eks. `myfabric-capacity`)  
- **Pause Time** – tidspunkt for daglig pause (default: `17:00`, UTC)  
- **Resume Time** – valgfrit tidspunkt for resume (f.eks. `07:00`, UTC)  

---

## ⚠ Hvis Logic App deployment fejler (Microsoft.Logic ikke registreret)  
En Azure administrator skal registrere provideren:

### I Azure Portal  
- Gå til **Subscriptions > [din subscription] > Resource providers**  
- Søg: `Microsoft.Logic`  
- Klik: `Register`  

### Eller via CLI  
```bash
az provider register --namespace Microsoft.Logic
```

💡 Hvad sker der efter deployment?

✅ Logic App fabricAutoPauseByAtlytix bliver oprettet.

✅ Dagligt pause/resume kører automatisk. 

✅ Du kan se og ændre Logic App’en i Azure Portal. 


## **🛡 Sådan giver du Logic App rettigheder til at pause kapaciteten**
Hvis du har brugt User-versionen, skal en admin tildele adgang manuelt:

1️⃣ Aktivér Logic App's Managed Identity
Gå til Resource groups > [din resource group] > Logic Apps > [din Logic App]

Vælg Identity > skift System assigned til On > klik Save

2️⃣ Giv Managed Identity adgang
Gå til Fabric kapacitetens ressource > Access control (IAM)

Klik Add > Add role assignment

Vælg Role: Contributor

Assign access to: Managed identity

Vælg din Logic App > Save

✅ Logic App’en har nu de nødvendige rettigheder.

💸 Omkostninger
ARM template + deployment = gratis
Logic App faktureres pr. kald (typisk meget lavt, øre pr. måned)

📬 Support
Har du spørgsmål?
📧 jtl@atlytix.dk


