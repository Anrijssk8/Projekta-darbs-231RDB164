231RDB164
Projekta Nosaukums: Valūtu Konvertētājs
Projekta Uzdevums
Draugs kuram interesē tirgot valūtas iedeva man ideju izveidot valūtas konvertētāju, kas ļauj lietotājam iegūt aktuālus valūtu kursus pret norādīto pamatvalūtu. Programma nodrošina iespēju sekot vairākām valūtām un vizualizēt to attiecības, palīdzot lietotājiem plānot un analizēt finansiālās darbības.

Python Bibliotēkas un to Izmantošana
requests: Tiek izmantota, lai veiktu HTTP GET pieprasījumus API, kas nodrošina valūtu kursus. Šī bibliotēka nodrošina vienkāršu un efektīvu veidu, kā iegūt datus no attālinātas vietnes.

json: Lai apstrādātu iegūtos datus, izmantojot JSON formātu. Šī bibliotēka ļauj konvertēt JSON datu struktūras par Python datu struktūrām.

tkinter: Tiek izmantota, lai izveidotu lietotājam draudzīgu grafisko saskarni (GUI). Šī bibliotēka nodrošina līdzekļus logrīku, pogu un citu vizuālo elementu izveidei.

Programmatūras Izmantošanas Metodes
Ievadīt pamatvalūtu: Lietotājam tiek prasīts ievadīt pamatvalūtu, pret kuru viņš vēlas redzēt citu valūtu kursus.

Atlasīt vēlamās valūtas: Lietotājam ir iespēja atlasīt valūtas, kurās viņš ir ieinteresēts. Šis projekts piedāvā iespēju sekot līdzi vairākām valūtām vienlaicīgi.

Saņemt un parādīt valūtu kursus: Programma veic HTTP GET pieprasījumu uz attālinātu API, iegūstot aktuālos valūtu kursus. Dati tiek attēloti lietotājam grafiskajā interfeisā.

Analizēt rezultātus: Lietotājs var analizēt un sekot līdzi valūtu kursu izmaiņām. Programmā tiek nodrošināta vizuāla attēlojuma iespēja, kas palīdz lietotājam viegli izprast valūtu attiecības.

Papildu Iezīmes
Programmā ir implementēts drošības mehānisms, lai apstrādātu iespējamos kļūdu gadījumus, piemēram, neesošas valūtas vai atteikšanos no API.
Lietotājs var ērti iziet no programmas, ievadot "q" vai "Q" pamatvalūtas izvēlē.
Lietotāja Vadlīnija
Palaist Programmu: Palaistiet Python skriptu un sekotiet norādēm uz ekrāna.
Ievadīt Pamatvalūtu: Ievadiet pamatvalūtu, pret kuru jūs vēlaties redzēt citu valūtu kursus (piemēram: USD, EUR).
Atlasīt Citas Valūtas: Atlasiet vēlamās valūtas, atdalot tās ar komatiem (piemēram: USD, EUR, CAD).
Apskatīt Rezultātus: Skatiet rezultātus uz ekrāna, analizējiet valūtu kursu izmaiņas.
Iziet no Programmas: Lai izietu no programmas, ievadiet "q" vai "Q" pamatvalūtas izvēlē.

Koda apraksts:

# Importējam requests bibliotēku, lai veiktu HTTP pieprasījumus
import requests

# API atslēga, nepieciešama piekļuvei valūtu apmaiņas datus nodrošinošam API
API_KEY = 'fca_live_QiRD4yaE7K1aJQIAfVLojfu8J2KPxSXPYkDxxTzc'

# Pamatadrese ar iekļautu API atslēgu
BASE_URL = f"https://api.freecurrencyapi.com/v1/latest?apikey={API_KEY}"

# Saraksts ar valūtām, kuras vēlamies izseko
CURRENCIES = ["USD", "CAD", "EUR", "AUD", "CNY"]

# Funkcija, kas atgriež valūtu apmaiņas datus no API
def convert_currency(base):
    currencies = ",".join(CURRENCIES)
    url = f"{BASE_URL}&base_currency={base}&currencies={currencies}"
    try:
        # Veicam HTTP GET pieprasījumu
        response = requests.get(url)
        # Izgūstam datus JSON formātā
        data = response.json()
        # Atgriežam tikai datus no "data" atslēgas
        return data["data"]
    except:
        # Ja kaut kas neizdodas, izvadam kļūdas paziņojumu
        print("Nav pieejama valūta.")
        return None

# Galvenais cikls, kas izpildīsies, kamēr lietotājs neizvēlas iziet
while True:
    # Lietotājam tiek prasīts ievadīt valūtu
    base = input("Ievadiet valūtu piemēram: USD; EUR; CAD; CNY; AUD (q, lai iziet): ").upper()

    # Ja lietotājs ievada "Q" vai "q", cikls tiek pārtraukts
    if base == "Q":
        break

    # Izsaucam funkciju, lai iegūtu valūtu apmaiņas datus
    data = convert_currency(base)
    
    # Ja dati nav iegūti, turpinam ciklu
    if not data:
        continue

    # Izņemam pamatvalūtu, lai netiktu parādīti apmaiņas kursi pret pašu sevi
    del data[base]

    # Izdrukājam atlikušos datus
    for ticker, value in data.items():
        print(f"{ticker}: {value}")
