# Projekta-darbs-231RDB164





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
