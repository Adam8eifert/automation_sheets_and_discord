# 🤖 RPA Integrace: Detekce HOT Leadů z Google Sheets do Discordu
### Projekt: Low-Code Business Logika a API Integrace (Make.com)

## Cíl Projektu
Cílem tohoto projektu bylo vytvořit **automatizovaný proces (RPA/Low-Code)**, který monitoruje nově přidané záznamy v databázi (simulované v Google Sheets) a okamžitě notifikuje obchodní tým na kanálu Discord **pouze v případě, že jsou splněny kritické obchodní podmínky (Filtrační logika).**

Tento scénář demonstruje práci s triggery, komplexní filtrační logikou a přímou integrací pomocí HTTP/API volání, čímž se vyhýbá standardním konektorům.

## ⚙️ Použité Technologie

| Kategorie | Nástroj / Technologie | Poznámka |
| :--- | :--- | :--- |
| **Low-Code Platforma** | [**Make.com**](https://www.make.com) (dříve Integromat) | Hosting a exekuce automatizace |
| **Trigger (Zdroj Dat)** | Google Sheets | Simulovaná databáze leadů |
| **Cíl (Notifikace)** | Discord | Obchodní kanál pro notifikace |
| **Klíčová Integrace** | **HTTP - Make a request** | **Použito pro přímé API volání** (Discord Webhook), čímž je demonstrováno pokročilé chápání API a práce s JSON payloadem. |

## 🧠 Architektura a Logika Scénáře
Scénář je postaven na čtyřech modulech:

1.  **Google Sheets - Watch New Rows (TRIGGER):**
    * Monitoruje nový řádek s nejvyšším ID.
    * *Důkaz schopností:* Konfigurace triggeru s ohledem na omezení řádků (řešení chyby 400 úpravou startovacího ID).
2.  **Router:**
    * Umožňuje větvení logiky (např. poslat HOT leada na Discord, COLD leada do CRM).
3.  **Filter (Klíčová Logika):**
    * Scénář pokračuje pouze tehdy, pokud jsou splněny **dvě AND podmínky**:
        * `Status` **se rovná** `"HOT"`
        * `Hodnota (EUR)` **je větší než** `10000`
4.  **HTTP - Make a request (API VOLÁNÍ):**
    * Vytváří a odesílá zprávu ve formátu **JSON (Embed)** do Discord Webhook URL.
    * *Důkaz schopností:* Správné nastavení hlavičky (`Content-Type: application/json`) a **dynamické mapování** dat (`{{Jméno_Leadu}}`, `{{Hodnota (EUR)}}`) přímo do JSON payloadu.

## 💾 Replikační Schopnost (Pro náboráře)

Projekt je snadno replikovatelný, i když používá můj osobní Google Disk a Discord.

### 1. Příprava Dat

* Stáhněte si **šablonu tabulky** ze složky `data/`.
* Zkopírujte obsah do nového listu Google Sheets a pojmenujte list **`Leads`**.

### 2. Konfigurace Discord Webhooku

* Vytvořte nový Discord Webhook ve svém kanálu (Nastavení kanálu > Integrace > Vytvořit Webhook).
* **Zkopírujte URL** webhooku.

### 3. Import Scénáře do Make.com

* Ve složce `scenare/` je soubor `RPA_Discord_Notification.json` (JSON export).
* V Make.com na stránce "Scenarios" klikněte na tři tečky a zvolte **"Import Blueprint"**.
* Po importu se objeví scénář, ale bude **nepřipojený**.

### 4. Nastavení Spojení

* **Google Sheets modul:** Připojte Váš Google účet a vyberte Vaši zkopírovanou tabulku. Nastavte **Trigger start od ID 20** (nebo poslední ID v šabloně).
* **HTTP modul:** Vložte Vaše **Discord Webhook URL** do pole URL. Ostatní nastavení JSON jsou již správně namapována.

## 💡 Vizualizace a Výsledek

*(Zde vložte snímek obrazovky notifikace z Discordu. Snímek by měl být uložen v `assets/discord_result.png`)*

**Snímek obrazovky výsledné notifikace:** [assets/discord_result.png]

---
**Demonstrace Filtrační Logiky:**
* Test s hodnotou 4500 EUR $\rightarrow$ **Zastaveno Filtrem.**
* Test s hodnotou 120000 EUR $\rightarrow$ **Prošlo a notifikováno.**
