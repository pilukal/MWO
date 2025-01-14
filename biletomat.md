1. Jako biletomat, chcę automatycznie aktualizować listę dostępnych biletów i ich cen, aby zapewnić zgodność z polityką przewoźnika.
2. Jako biletomat, chcę rejestrować wszystkie transakcje i wysyłać raporty do systemu centralnego, aby umożliwić monitoring i kontrolę operacji.
3. Jako biletomat, chcę posiadać czytelny ekran dotykowy, aby użytkownik mógł łatwo nawigować po interfejsie.
4. Jako biletomat, chcę być wyposażony w różne metody płatności (terminal kart, czytnik gotówki, NFC), aby obsługiwać różnorodne transakcje.
5. Jako biletomat, chcę wydawać resztę w gotówce, jeśli użytkownik zapłaci nadmiarowo, aby transakcja była zgodna z oczekiwaniami.

## Diagramy przypadków uzycia

### Wyświetlenie dostępnych biletów

```mermaid
flowchart TD
    biletomat{Biletomat}
    wyswietlenie_dostepnych_biletow([Wyświetlenie dostępnych biletów])
    pobranie_listy_dostepnych_biletow([Pobranie listy dostępnych biletów])
    ostrzezenie_o_braku_aktualnych_danych([Ostrzeżenie o braku aktualnych danych])
    
    biletomat --> wyswietlenie_dostepnych_biletow
    wyswietlenie_dostepnych_biletow --> |include| pobranie_listy_dostepnych_biletow
    wyswietlenie_dostepnych_biletow --> |extend| ostrzezenie_o_braku_aktualnych_danych
```