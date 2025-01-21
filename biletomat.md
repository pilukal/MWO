1. Jako biletomat, chcę automatycznie aktualizować listę dostępnych biletów i ich cen, aby zapewnić zgodność z polityką przewoźnika.
2. Jako biletomat, chcę rejestrować wszystkie transakcje i wysyłać raporty do systemu centralnego, aby umożliwić monitoring i kontrolę operacji.
3. Jako biletomat, chcę posiadać czytelny ekran dotykowy, aby użytkownik mógł łatwo nawigować po interfejsie.
4. Jako biletomat, chcę być wyposażony w różne metody płatności (terminal kart, czytnik gotówki, NFC), aby obsługiwać różnorodne transakcje.
5. Jako biletomat, chcę wydawać resztę w gotówce, jeśli użytkownik zapłaci nadmiarowo, aby transakcja była zgodna z oczekiwaniami.

## Diagramy przypadków uzycia

### Obsługa wyboru języka

```mermaid
flowchart TD
    User{User}
    User --> Wyświetlenie_opcji_jezykowych

    subgraph biletomat
    
    Wyświetlenie_opcji_jezykowych([Wyświetlenie opcji językowych])
    Rejestracja_wyboru_jezyka([Rejestracja wyboru języka])
    Dostosowanie_interfejsu([Dostosowanie interfejsu])
    Opcje_jezykowe([Opcje językowe])
    Powrot_do_jezyka_domyslnego([Powrót do języka domyślnego])

    Wyświetlenie_opcji_jezykowych --> Rejestracja_wyboru_jezyka
    Rejestracja_wyboru_jezyka --> Dostosowanie_interfejsu
    Wyświetlenie_opcji_jezykowych --> |include| Opcje_jezykowe
    Powrot_do_jezyka_domyslnego -.-> |extend| Wyświetlenie_opcji_jezykowych

    end
```

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

### Diagram wspólny


```mermaid
flowchart TD
    biletomat{Biletomat}
    wyswietlenie_dostepnych_biletow([Wyświetlenie dostępnych biletów])
    pobranie_listy_dostepnych_biletow([Pobranie listy dostępnych biletów])
    ostrzezenie_o_braku_aktualnych_danych([Ostrzeżenie o braku aktualnych danych])
    obsluga_wyboru_jezyka([Obsługa wyboru języka])
    generowanie_biletu_elektronicznego([Generowanie biletu elektronicznego])
    powiadomienie_o_bledach_w_procesie([Powiadomienie o błędach w procesie])
    
    biletomat --> wyswietlenie_dostepnych_biletow
    wyswietlenie_dostepnych_biletow --> |include| pobranie_listy_dostepnych_biletow
    wyswietlenie_dostepnych_biletow --> |extend| ostrzezenie_o_braku_aktualnych_danych
    biletomat --> obsluga_wyboru_jezyka
    obsluga_wyboru_jezyka --> |include| generowanie_biletu_elektronicznego
    obsluga_wyboru_jezyka --> |include| powiadomienie_o_bledach_w_procesie
```
