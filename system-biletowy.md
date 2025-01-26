1. Jako system biletowy, chcę dostarczać aktualne dane o taryfach i typach biletów do biletomatu, aby użytkownik miał zawsze poprawne informacje.
2. Jako system biletowy, chcę umożliwiać sprawdzenie ważności biletu w czasie rzeczywistym, aby zapobiegać oszustwom.
3. Jako system biletowy, chcę rejestrować każde sprzedane bilety, aby śledzić ruch i sprzedaż w systemie.
4. Jako system biletowy, chcę współpracować z aplikacjami mobilnymi, aby użytkownik mógł uzyskać elektroniczny bilet w przypadku takiego wyboru.

### Diagramy przypadków uzycia

### Aktualizacja Taryf

```mermaid
flowchart TD
    System_biletowy{System biletowy}
    Akualizacja_taryf([Aktualizacja taryf])
    Wprowadzenie_danych_do_bazy_taryf([Wprowadzenie danych do bazy taryf])
    Powiadomienie_biletomatów_o_niezgodności_taryf([Powiadomienie biletomatów o niezgodności taryf])

    System_biletowy --> Akualizacja_taryf
    Akualizacja_taryf --> |include| Wprowadzenie_danych_do_bazy_taryf
    Akualizacja_taryf --> |include| Powiadomienie_biletomatów_o_niezgodności_taryf
```

### Weryfikacja ważności biletu

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

### Rejestracja tranzakcji sprzedazy

```mermaid
flowchart TD
    
    Biletomat{Biletomat}
    Aplikacja_mobilna{Aplikacja mobilna}
    Biletomat --> Odebranie_danych_transakcji
    Aplikacja_mobilna --> Odebranie_danych_transakcji
    Potwierdzenie_rejestracji_transakcji -->Biletomat
    Potwierdzenie_rejestracji_transakcji -->Aplikacja_mobilna
    subgraph system biletowy
    
    Odebranie_danych_transakcji([Odebranie danych transakcji])
    Zapis_transakcji_w_bazie([Zapis transakcji w bazie])
    Potwierdzenie_rejestracji_transakcji([Potwierdzenie rejestracji transakcji])

    Zapis_danych_transakcji([Zapis danych transakcji])
    Powiadomienie_o_bledach([Powiadomienie o błędach])


    Odebranie_danych_transakcji --> Zapis_transakcji_w_bazie
    Zapis_transakcji_w_bazie --> Potwierdzenie_rejestracji_transakcji

    Zapis_transakcji_w_bazie --> |include| Zapis_danych_transakcji
    Powiadomienie_o_bledach -.-> |extend| Potwierdzenie_rejestracji_transakcji
    end
```

### Diagram wspólny

```mermaid
flowchart TD
    system_biletowy{System Biletowy}
    weryfikacja_waznosci_biletu([Weryfikacja ważności biletu])
    sprawdzenie_danych_biletu_w_bazie_danych([Sprawdzenie danych biletu w bazie danych])
    powiadomienie_o_probie_oszustwa([Powiadomienie o próbie oszustwa w przypadku nieważnego biletu])
    akualizacja_taryf([Aktualizacja taryf])
    wprowadzenie_danych_do_bazy_taryf([Wprowadzenie danych do bazy taryf])
    powiadomienie_biletomatów_o_niezgodności_taryf([Powiadomienie biletomatów o niezgodności taryf])


    system_biletowy --> weryfikacja_waznosci_biletu
    weryfikacja_waznosci_biletu --> |include| sprawdzenie_danych_biletu_w_bazie_danych
    weryfikacja_waznosci_biletu --> |extend| powiadomienie_o_probie_oszustwa
    system_biletowy --> akualizacja_taryf
    akualizacja_taryf --> |include| wprowadzenie_danych_do_bazy_taryf
    akualizacja_taryf --> |include| powiadomienie_biletomatów_o_niezgodności_taryf
```