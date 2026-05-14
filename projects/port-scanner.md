# Port Scanner

## Eesmärk
TCP port scanner mis kontrollib antud IP aadressi pordivahemikku ja kuvab millised pordid on avatud. Ehitasin selle et õppida kuidas võrguühendused ja socket'id töötavad.

## Tööriistad
- Python 3
- `socket` — TCP ühenduste loomiseks

## Kasutus
```bash
python3 port_scanner.py 192.168.1.1 1 1024
```

## Mida õppisin
- Kuidas TCP kolmekäeline handshake töötab (SYN → SYN-ACK → ACK)
- Miks suletud port annab `Connection refused` ja ei vaiki lihtsalt
- Socket timeout tähtsus — ilma selleta jääb skript kinni filtreeritud portidel
- Erinevus TCP ja UDP vahel skannimisel

## Probleemid ja lahendused
**Probleem:** Skannimine oli algselt liiga aeglane
**Lahendus:** Lisasin `socket.settimeout()` — vähendas ooteaja 30s → 1s pordi kohta

**Probleem:** Kood viskas koguaeg errori üless, siis avastasin, et connect() on see süüdlane
**Lahendus:** Asendasin connect() -> connect_ex() ja see lahendas probleemi

## Kood
```python
import socket

ip = input("Sisesta IP: ")
for port in range(1, 1025):
    s = socket.socket()
    s.settimeout(0.5)
    tulemus = s.connect_ex((ip, port))
    if tulemus == 0:
        print(f"Port: {port} - LAHTI")
    s.close()
```


---
*Valminud: 2025 · Kuupäev*
