1. Jako biletomat, chcę automatycznie aktualizować listę dostępnych biletów i ich cen, aby zapewnić zgodność z polityką przewoźnika.
2. Jako biletomat, chcę rejestrować wszystkie transakcje i wysyłać raporty do systemu centralnego, aby umożliwić monitoring i kontrolę operacji.
3. Jako biletomat, chcę posiadać czytelny ekran dotykowy, aby użytkownik mógł łatwo nawigować po interfejsie.
4. Jako biletomat, chcę być wyposażony w różne metody płatności (terminal kart, czytnik gotówki, NFC), aby obsługiwać różnorodne transakcje.
5. Jako biletomat, chcę wydawać resztę w gotówce, jeśli użytkownik zapłaci nadmiarowo, aby transakcja była zgodna z oczekiwaniami.

# Diagramy klas

## Wyświetlenie dostępnych biletów

### KLASY

#### Biletomat
- **ATRYBUTY**: brak
- **METODY**:
  - `VOID uruchomEkranPowitalny()`: Uruchamia ekran powitalny w biletomacie, który jest wyświetlany użytkownikowi po włączeniu urządzenia.
  - `VOID wyslijZapytanie()`: Wysyła zapytanie do systemu centralnego, aby uzyskać listę dostępnych biletów.
  - `VOID wyswietlBilety()`: Wyświetla użytkownikowi listę dostępnych biletów wraz z ich szczegółami.
  - `VOID wyswietlBlad()`: Wyświetla użytkownikowi komunikat o błędzie, np. w przypadku braku dostępnych biletów lub problemu z połączeniem z systemem centralnym.

#### SystemCentralny
- **ATRYBUTY**: brak
- **METODY**:
  - `void zapytajOBilety(): Bilet[]`: Odpowiada na zapytanie biletomatu o dostępność biletów. Może zwrócić listę biletów lub wyjątek - komunikat o błędzie.

#### Bilet
- **ATRYBUTY**:
  - `String numer`: Numer biletu, który identyfikuje konkretny bilet.
  - `String nazwa`: Nazwa biletu (np. bilet normalny, ulgowy).
  - `String kategoria`: Kategoria biletu (np. bilety na podróż, bilety na wydarzenia).
  - `String status`: Status biletu (np. dostępny, wyprzedany).

### RELACJE:

- **Biletomat** KORZYSTA Z **SystemuCentralnego**: Biletomat wysyła zapytanie o dostępność biletów, a system centralny zwraca listę dostępnych biletów.
- **SystemCentralny** KORZYSTA Z **Bilet**: System centralny przechowuje dane dotyczące biletów (numer, nazwa, kategoria, status).
- **Biletomat** KORZYSTA Z **Bilet**: Biletomat wyświetla listę dostępnych biletów użytkownikowi, korzystając z danych przechowywanych w obiektach klasy `Bilet`.

```mermaid
classDiagram
    class Biletomat {
        +uruchomEkranPowitalny()
        +wyslijZapytanie()
        +wyswietlBilety()
        +wyswietlBlad()
    }

    class SystemCentralny {
        +zapytajOBilety()
        +zwracajListeBiletow()
        +zwracajBlad()
    }

    class Bilet {
        +numer: String
        +nazwa: String
        +kategoria: String
        +status: String
    }

    Biletomat --> SystemCentralny : pozyskuje listę dostępnych biletów
    SystemCentralny --> Bilet : przechowuje listę dostępnych
    Biletomat --> Bilet : wyswietla listę biletów
```

# Diagramy sekwencji

## Obsługa wyboru języka
> - Aktor: Uzytkownik
> - Obiekty: Biletomat
> - Scenariusz główny:
>    - Uzytkownik wybiera opcję wyboru języka
>    - Biletomat pobiera listę dostępnych języków
>    - Biletomat wyświetla ekran wyboru języka
>    - Uzytkownik wybiera język
>    - Biletomat zamyka ekran wyboru języka
>    - Biletomat rejestruje wybór języka
>    - Biletomat dostosowuje interfejs do wybranego języka
> - Scenariusz alternatywny (brak wyboru uzytkownika przez 30 sekund):
>    - Biletomat uruchamia ekran powitalny (Uruchomienie ekranu powitalnego).
>    - Uzytkownik wybiera opcję wyboru języka
>    - Biletomat pobiera listę dostępnych języków
>    - Biletomat wyświetla ekran wyboru języka
>    - Biletomat zamyka ekran wyboru języka
>    - Biletomat dostosowuje interfejs do języka domyślnego

```mermaid
sequenceDiagram
 
    PARTICIPANT USER AS Użytkownik
    PARTICIPANT BT AS Biletomat

    USER->>BT: Wybież opcję wyboru języka
    BT->>BT: Pobierz listę dostępnych języków
    BT->>USER: Wyświetl ekran wyboru języka
    ALT wybrano język
    USER->>BT: Wybierz język
    BT->>BT: Zamknij ekran wyboru języka
    BT->>BT: Zarejestruj wybór języka
    BT->>BT: Dostosuj interfejs do wybranego języka
    ELSE nie wybrano jezyka przez 30 sekund
    BT->>BT: Zamknij ekran wyboru języka
    BT->>BT: Dostosuj interfejs do języka domyślnego
    
    END
```

## wyświetlenie dostępnych biletów

> - Aktor: Biletomat Uzytkownik
> - Obiekty: system centralny 
> - Scenariusz główny:
>    - Biletomat uruchamia ekran powitalny
>    - Biletomat wysyła zapytanie o listę dostępnych biletów do systemu centralnego
>    - System centralny zwraca listę dostępnych biletów.
>    - Biletomat wyświetla kategorię biletów oraz szczegóły dotyczące ich dostępności
> - Scenariusz alternatywny (brak aktualnych danych):
>    - Biletomat uruchamia ekran powitalny (Uruchomienie ekranu powitalnego).
>    - Biletomat wysyła zapytanie o listę dostępnych biletów do systemu centralnego.
>    - System centralny zwraca komunikat o błędzie, np. awaria sieci.
>    - Biletomat wyświetla ostrzeżenie o braku aktualnych danych.
```mermaid
sequenceDiagram
 
    PARTICIPANT USER AS Użytkownik
    PARTICIPANT BI AS Biletomat
    PARTICIPANT CS AS System centralny

    BI->>BI: Uruchomienie ekranu powitalnego
    BI->>CS: Zapytanie o listę dostępnych biletów
    ALT Lista biletów dostępna
    CS-->>BI: lista dostępnych biletów
    BI-->>USER: wyświetlenie dostępnych biletów
    ELSE Lista biletów niedostępna
    CS-->>BI: komunikat o błędzie
    BI-->>USER: wyświetlenie informacji o braku aktualnych danych
    END

```
# Diagramy przypadków uzycia

## Generowanie potwierdzenia zakupu

```mermaid
flowchart TD
    
    System_transakcyjny{System transakcyjny}
    Uzytkownik{Użytkownik}
    System_transakcyjny --> Potwierdzenie_zakonczenia_transakcji
    Oczekiwanie_na_odbior --> Uzytkownik

    subgraph biletomat
    
    Potwierdzenie_zakonczenia_transakcji([Potwierdzenie zakończenia transakcji])
    Generowanie_potwierdzenia([Generowanie potwierdzenia])
    Informacja_o_potwierdzeniu([Informacja o potwierdzeniu])
    Oczekiwanie_na_odbior([Oczekiwanie na odbiór])

    Generowanie_biletu([Generowanie biletu])
    Blad_generowania([Błąd generowania])


    Potwierdzenie_zakonczenia_transakcji --> Generowanie_potwierdzenia
    Generowanie_potwierdzenia --> Informacja_o_potwierdzeniu
    Informacja_o_potwierdzeniu --> Oczekiwanie_na_odbior

    Generowanie_potwierdzenia --> |include| Generowanie_biletu
    Blad_generowania -.-> |extend| Generowanie_potwierdzenia
    end
```

### Obsługa wyboru języka

```mermaid
flowchart TD
    Uzytkownik{Użytkownik}
    Uzytkownik --> Wyświetlenie_opcji_jezykowych
    Dostosowanie_interfejsu --> Uzytkownik
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
    
    Uzytkownik{Użytkownik}
    Oczekiwanie_na_wybor_uzytkownika --> Uzytkownik

    subgraph biletomat
    
    Uruchomienie_ekranu_powitalnego([Uruchomienie ekranu powitalnego])
    Pobranie_listy_biletow([Pobranie listy biletów])
    Wyswietlenie_biletow([Wyświetlenie biletów])
    Oczekiwanie_na_wybor_uzytkownika([Oczekiwanie na wybór użytkownika])

    Aktualizacja_biletow([Aktualizacja biletów])
    Ostrzezenie_o_braku_danych([Ostrzeżenie o braku danych])


    Uruchomienie_ekranu_powitalnego --> Pobranie_listy_biletow
    Pobranie_listy_biletow --> Wyswietlenie_biletow
    Wyswietlenie_biletow --> Oczekiwanie_na_wybor_uzytkownika

    Pobranie_listy_biletow --> |include| Aktualizacja_biletow
    Ostrzezenie_o_braku_danych -.-> |extend| Pobranie_listy_biletow
    end
```

### Wyswietlenie podsumowania tranzakcji

```mermaid
flowchart TD
    
    Uzytkownik{Użytkownik}
    Uzytkownik --> Gromadzenie_danych_o_transakcji
    Oczekiwanie_na_decyzje_uzytkownika --> Uzytkownik

    subgraph biletomat
    
    Gromadzenie_danych_o_transakcji([Gromadzenie danych o transakcji])
    Wyswietlenie_podsumowania([Wyświetlenie podsumowania])
    Oczekiwanie_na_decyzje_uzytkownika([Oczekiwanie na decyzję użytkownika])

    Podsumowanie_transakcji([Podsumowanie transakcji])
    Obsluga_anulowania([Obsługa anulowania])


    Gromadzenie_danych_o_transakcji --> Wyswietlenie_podsumowania
    Wyswietlenie_podsumowania --> Oczekiwanie_na_decyzje_uzytkownika

    Wyswietlenie_podsumowania --> |include| Podsumowanie_transakcji
    Obsluga_anulowania -.-> |extend| Oczekiwanie_na_decyzje_uzytkownika
    end
```


### Diagram wspólny


```mermaid
flowchart TD
    
    System_transakcyjny{System transakcyjny}
    Uzytkownik{Użytkownik}
    System_transakcyjny --> Potwierdzenie_zakonczenia_transakcji
    Oczekiwanie_na_odbior --> Uzytkownik

    Uzytkownik --> Wyświetlenie_opcji_jezykowych
    Dostosowanie_interfejsu --> Uzytkownik

    Oczekiwanie_na_wybor_uzytkownika --> Uzytkownik

    Uzytkownik --> Gromadzenie_danych_o_transakcji
    Oczekiwanie_na_decyzje_uzytkownika --> Uzytkownik

    subgraph biletomat
    
    Potwierdzenie_zakonczenia_transakcji([Potwierdzenie zakończenia transakcji])
    Generowanie_potwierdzenia([Generowanie potwierdzenia])
    Informacja_o_potwierdzeniu([Informacja o potwierdzeniu])
    Oczekiwanie_na_odbior([Oczekiwanie na odbiór])
    Generowanie_biletu([Generowanie biletu])
    Blad_generowania([Błąd generowania])
    Potwierdzenie_zakonczenia_transakcji --> Generowanie_potwierdzenia
    Generowanie_potwierdzenia --> Informacja_o_potwierdzeniu
    Informacja_o_potwierdzeniu --> Oczekiwanie_na_odbior
    Generowanie_potwierdzenia --> |include| Generowanie_biletu
    Blad_generowania -.-> |extend| Generowanie_potwierdzenia
    
    
    Wyświetlenie_opcji_jezykowych([Wyświetlenie opcji językowych])
    Rejestracja_wyboru_jezyka([Rejestracja wyboru języka])
    Dostosowanie_interfejsu([Dostosowanie interfejsu])
    Opcje_jezykowe([Opcje językowe])
    Powrot_do_jezyka_domyslnego([Powrót do języka domyślnego])
    Wyświetlenie_opcji_jezykowych --> Rejestracja_wyboru_jezyka
    Rejestracja_wyboru_jezyka --> Dostosowanie_interfejsu
    Wyświetlenie_opcji_jezykowych --> |include| Opcje_jezykowe
    Powrot_do_jezyka_domyslnego -.-> |extend| Wyświetlenie_opcji_jezykowych
    

    Uruchomienie_ekranu_powitalnego([Uruchomienie ekranu powitalnego])
    Pobranie_listy_biletow([Pobranie listy biletów])
    Wyswietlenie_biletow([Wyświetlenie biletów])
    Oczekiwanie_na_wybor_uzytkownika([Oczekiwanie na wybór użytkownika])
    Aktualizacja_biletow([Aktualizacja biletów])
    Ostrzezenie_o_braku_danych([Ostrzeżenie o braku danych])
    Uruchomienie_ekranu_powitalnego --> Pobranie_listy_biletow
    Pobranie_listy_biletow --> Wyswietlenie_biletow
    Wyswietlenie_biletow --> Oczekiwanie_na_wybor_uzytkownika
    Pobranie_listy_biletow --> |include| Aktualizacja_biletow
    Ostrzezenie_o_braku_danych -.-> |extend| Pobranie_listy_biletow

    Gromadzenie_danych_o_transakcji([Gromadzenie danych o transakcji])
    Wyswietlenie_podsumowania([Wyświetlenie podsumowania])
    Oczekiwanie_na_decyzje_uzytkownika([Oczekiwanie na decyzję użytkownika])
    Podsumowanie_transakcji([Podsumowanie transakcji])
    Obsluga_anulowania([Obsługa anulowania])
    Gromadzenie_danych_o_transakcji --> Wyswietlenie_podsumowania
    Wyswietlenie_podsumowania --> Oczekiwanie_na_decyzje_uzytkownika
    Wyswietlenie_podsumowania --> |include| Podsumowanie_transakcji
    Obsluga_anulowania -.-> |extend| Oczekiwanie_na_decyzje_uzytkownika
    end
```
