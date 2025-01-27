1. Jako system biletowy, chcę dostarczać aktualne dane o taryfach i typach biletów do biletomatu, aby użytkownik miał zawsze poprawne informacje.
2. Jako system biletowy, chcę umożliwiać sprawdzenie ważności biletu w czasie rzeczywistym, aby zapobiegać oszustwom.
3. Jako system biletowy, chcę rejestrować każde sprzedane bilety, aby śledzić ruch i sprzedaż w systemie.
4. Jako system biletowy, chcę współpracować z aplikacjami mobilnymi, aby użytkownik mógł uzyskać elektroniczny bilet w przypadku takiego wyboru.

### Diagramy przypadków uzycia

### Obsługa aplikacji mobilnej
```mermaid
flowchart TD
    
    Aplikacja_mobilna{Aplikacja mobilna}

    Aplikacja_mobilna --> Odebranie_zadania_zakupu
    Przeslanie_biletu_elektronicznego --> Aplikacja_mobilna

    subgraph system biletowy
    
    Odebranie_zadania_zakupu([Odebranie żądania zakupu])
    Generowanie_biletu_elektronicznego([Generowanie biletu elektronicznego])
    Przeslanie_biletu_elektronicznego([Przesłanie biletu elektronicznego])
    
    Generowanie_biletu([Generowanie biletu])
    Obsluga_bledow_zakupu([Obsługa błędów zakupu])


    Odebranie_zadania_zakupu --> Generowanie_biletu_elektronicznego
    Generowanie_biletu_elektronicznego --> Przeslanie_biletu_elektronicznego

    Generowanie_biletu_elektronicznego --> |include| Generowanie_biletu
    Obsluga_bledow_zakupu -.-> |extend| Generowanie_biletu_elektronicznego
    end
```
### Aktualizacja Taryf

```mermaid
flowchart TD
    
    Administrator_systemu{Administrator systemu}
    Biletomat{Biletomat}

    Administrator_systemu --> Odebranie_zadania_aktualizacji
    Przeslanie_zaktualizowanych_taryf --> Biletomat
    Powiadomienie_o_bledach_taryf --> Biletomat

    subgraph system biletowy
    
    Odebranie_zadania_aktualizacji([Odebranie żądania aktualizacji])
    Wprowadzenie_nowych_taryf([Wprowadzenie nowych taryf])
    Przeslanie_zaktualizowanych_taryf([Przesłanie zaktualizowanych taryf])
    Aktualizacja_bazy_taryf([Aktualizacja bazy taryf])
    Powiadomienie_o_bledach_taryf([Powiadomienie o błędach taryf])


    Odebranie_zadania_aktualizacji --> Wprowadzenie_nowych_taryf
    Wprowadzenie_nowych_taryf --> Przeslanie_zaktualizowanych_taryf
    Wprowadzenie_nowych_taryf --> |include| Aktualizacja_bazy_taryf
    Powiadomienie_o_bledach_taryf -.-> |extend| Przeslanie_zaktualizowanych_taryf
    end
```



### Weryfikacja ważności biletu

```mermaid
flowchart TD
    system_biletowy{System Biletowy}
    weryfikacja_waznosci_biletu([Weryfikacja ważności biletu])
    sprawdzenie_danych_biletu_w_bazie_danych([Sprawdzenie danych biletu w bazie danych])
    powiadomienie_o_probie_oszustwa([Powiadomienie o próbie oszustwa w przypadku nieważnego biletu])
    
    system_biletowy --> weryfikacja_waznosci_biletu
    weryfikacja_waznosci_biletu --> |include| sprawdzenie_danych_biletu_w_bazie_danych
    weryfikacja_waznosci_biletu --> |extend| powiadomienie_o_probie_oszustwa
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
