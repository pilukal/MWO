1. Jako biletomat, chcę automatycznie aktualizować listę dostępnych biletów i ich cen, aby zapewnić zgodność z polityką przewoźnika.
2. Jako biletomat, chcę rejestrować wszystkie transakcje i wysyłać raporty do systemu centralnego, aby umożliwić monitoring i kontrolę operacji.
3. Jako biletomat, chcę posiadać czytelny ekran dotykowy, aby użytkownik mógł łatwo nawigować po interfejsie.
4. Jako biletomat, chcę być wyposażony w różne metody płatności (terminal kart, czytnik gotówki, NFC), aby obsługiwać różnorodne transakcje.
5. Jako biletomat, chcę wydawać resztę w gotówce, jeśli użytkownik zapłaci nadmiarowo, aby transakcja była zgodna z oczekiwaniami.

## Diagramy przypadków uzycia

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
