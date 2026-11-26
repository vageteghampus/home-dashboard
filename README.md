# Hemma – dashboard för Homey Pro

En sida för väggplatta eller mobil som visar väder (SMHI), elpris och förbrukning (Tibber) och styr lampor, Sonos och stämningar via din Homey Pro.

- Rum och lampor hämtas automatiskt från Homeys zoner. Lägger du till en lampa i Homey dyker den upp på sidan.
- Sidan fungerar bara när enheten är på ditt hemma-wifi.
- API-nyckel och Tibber-token sparas bara i den enhetens webbläsare, aldrig i filerna.
- Utan inställningar visas exempeldata, så du kan titta på sidan direkt.

Filer: `index.html` (själva sidan), `manifest.webmanifest` och ikonerna (för "Lägg till på hemskärmen").

---

## 1. Lägg upp sidan på GitHub Pages

1. Gå till github.com och skapa ett nytt repo, t.ex. `hemma`. Välj **Public** (GitHub Pages är gratis för publika repon; inga hemligheter ligger i filerna).
2. I repot: **Add file → Upload files**. Dra in alla filer från mappen och tryck **Commit changes**.
3. Gå till **Settings → Pages**. Under *Build and deployment* väljer du **Deploy from a branch**, branch **main** och mapp **/ (root)**. Spara.
4. Efter en minut finns sidan på `https://DITT-ANVÄNDARNAMN.github.io/hemma/`.

Öppna länken i mobilen. Du ska se dashboarden med exempeldata.

## 2. Skapa en API-nyckel i Homey

1. Öppna Homeys webbapp (my.homey.app) och gå till **Inställningar → API-nycklar → Ny API-nyckel**.
2. Ge den ett namn, t.ex. "Väggplatta", och kryssa i behörigheterna för att:
   - **visa enheter** och **styra enheter**
   - **visa zoner**
   - **visa Flows** och **starta Flows**
3. Kopiera nyckeln direkt (den visas bara en gång).

Ta inte med fler behörigheter än så. När låset kopplas in senare kan sidan bara **låsa**, aldrig låsa upp.

Ta också reda på Homeys IP-adress (finns i Homey-appen under Inställningar, eller i routerns lista över anslutna enheter). Ge gärna Homey en **fast IP-adress** i routern så att den inte ändras.

## 3. Hämta en Tibber-token

1. Logga in på developer.tibber.com med ditt Tibber-konto.
2. Öppna **Access token** och kopiera din token.

Förbrukningen ("Förbrukat idag") kräver att Tibber får mätvärden, t.ex. via Tibber Pulse. Annars står det "Ingen data än".

## 4. Skapa Flows för stämningarna

Knapparna Morgon, Kväll, Hemma och Borta startar Flows i Homey med **exakt samma namn**.

- **Vanligt Flow:** välj startkortet *"Detta Flow startas"* under Logik, och lägg sedan till vad som ska hända (tänd/släck lampor, starta en Mood osv).
- **Avancerat Flow:** använd startkortet *Start*.

Knappar utan ett matchande Flow är gråa, och sidan talar om vilka som saknas. Vill du ha andra namn ändrar du dem under Inställningar.

## 5. Koppla ihop på mobilen eller plattan

1. Öppna sidan och tryck på **kugghjulet**.
2. Fyll i Homeys IP-adress, API-nyckeln och Tibber-token. Latitud/longitud är förifyllda för Sundsvall.
3. Tryck **Spara**.

**Fullskärm som en app:**
- iPhone/iPad: Safari → Dela → **Lägg till på hemskärmen**. Obs: appen på hemskärmen har egna inställningar, så fyll i kugghjulet en gång till när du öppnat den därifrån.
- Android: Chrome → meny → **Lägg till på startskärmen** / **Installera app**.

## Inställningar

| Fält | Vad det gör |
|---|---|
| Homeys IP-adress | T.ex. `192.168.1.50`. Sidan pratar med `https://192-168-1-50.homey.homeylocal.com`. |
| Homey API-nyckel | Från steg 2. Tomt = exempelenheter. |
| Tibber-token | Från steg 3. Tomt = exempelpriser. |
| Latitud / Longitud / Ortnamn | Var vädret hämtas för. |
| Stämningar | Namnen på dina Flows, upp till fyra. |
| Favoriter | Enhetsnamn som ska ligga som snabbknappar. Tomt = fasaden och Sonos väljs automatiskt. |
| Rumsordning | T.ex. `Hall, Kök, Vardagsrum`. Rum som inte nämns hamnar sist i bokstavsordning. |

## Felsökning

**"Kunde inte nå Homey"**
- Är enheten på hemma-wifi (inte mobildata eller gästnätverk)?
- Stämmer IP-adressen?
- **Webbläsaren kan fråga** om sidan får nå enheter i ditt lokala nätverk – svara *Tillåt*.
- **Routerns DNS-rebinding-skydd** kan blockera adresser som `…homey.homeylocal.com`. Testa att öppna `https://192-168-1-50.homey.homeylocal.com` (med din IP) direkt i webbläsaren. Går det inte, lägg till `homeylocal.com` som undantag för DNS-rebinding i routerns inställningar.

**"Homey godkände inte API-nyckeln"** – Kontrollera att nyckeln är rätt kopierad och har behörigheterna i steg 2. Skapa annars en ny.

**Stämningsknapparna är gråa** – Det finns inget Flow med exakt det namnet, eller så är Flowet avstängt.

**Vädret saknas** – SMHI svarar inte just nu. Sidan försöker igen var 30:e minut.

## Bra att veta

- Enheterna uppdateras var 5:e sekund, elpriset var 15:e minut och vädret var 30:e minut.
- Sidan håller skärmen vaken medan den är öppen och laddar om sig själv kl 03 varje natt.
- På en väggplatta: använd en kioskwebbläsare (t.ex. Fully Kiosk Browser på Android) eller Guidad åtkomst på iPad.
- Väderdata: SMHI, licens CC BY 4.0.
