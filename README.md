# 🚀 Fabric Auto-Pause / Resume Solution - Udviklet af Atlytix

![Atlytix Logo](https://www.atlytix.dk/path-to-your-logo.png)

🌐 [www.atlytix.dk](https://www.atlytix.dk)  
📧 [jtl@atlytix.dk](mailto:jtl@atlytix.dk)

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
