1. Jako system biletowy, chcę dostarczać aktualne dane o taryfach i typach biletów do biletomatu, aby użytkownik miał zawsze poprawne informacje.
2. Jako system biletowy, chcę umożliwiać sprawdzenie ważności biletu w czasie rzeczywistym, aby zapobiegać oszustwom.
3. Jako system biletowy, chcę rejestrować każde sprzedane bilety, aby śledzić ruch i sprzedaż w systemie.
4. Jako system biletowy, chcę współpracować z aplikacjami mobilnymi, aby użytkownik mógł uzyskać elektroniczny bilet w przypadku takiego wyboru.
# Diagramy klas
## Aktualizacja taryf 

## OPIS KLAS
### KLASY
#### SystemBiletowy
 - METODY: `aktualizujTaryfy(taryfy: Taryfa)`, `powiadomOBledach(biletomat: Biletomat, taryfy: Taryfa)`
#### AdministratorSystemu
 - METODY: `przeslijZadanieAktualizacji(taryfy: Taryfa)`
#### BazaTaryf
 - METODY: `BzapiszTaryfy(taryfy: Taryfa)`
#### Biletomat
 - METODY: `odbierzAktualizacje(taryfy: Taryfa)`, `otrzymajPowiadomienieOBledach(taryfy: Taryfa)`
#### Taryfa
- ATRYBUTY: `string id`,`string nazwa`,`Float stawka`
### Relacje
- `ADMINISTRATORSYSTEMU` JEST POWIĄZANY Z `SYSTEMBILETOWY` (ASOCJACJA): Administrator systemu wysyła żądanie aktualizacji taryf.
- `SYSTEMBILETOWY` KORZYSTA Z METODY `ZAPISZTARYFY` KLASY `BAZATARYF` DO ZAPISYWANIA TARYF.
- `SYSTEMBILETOWY` KORZYSTA Z METODY `ODBIERZAKTUALIZACJE` KLASY `BILETOMAT` DO PRZESYŁANIA AKTUALIZACJI.
- `SYSTEMBILETOWY` KORZYSTA Z METODY `OTRZYMAJPOWIADOMIENIEOBLEDACH` KLASY `BILETOMAT` DO PRZEKAZYWANIA INFORMACJI O BŁĘDACH.
- `SYSTEMBILETOWY` JEST POWIĄZANY Z `TARYFA` (ASOCJACJA): System wprowadza zmiany w taryfach.
- `BAZATARYF` JEST POWIĄZANY Z `TARYFA` (ASOCJACJA): Baza przechowuje dane taryf.
- `BILETOMAT` JEST POWIĄZANY Z `TARYFA` (ASOCJACJA): Biletomat otrzymuje aktualne taryfy.

```mermaid
classDiagram
    class SystemBiletowy {
        +aktualizujTaryfy(taryfy: Taryfa)
        +powiadomOBledach(biletomat: Biletomat, taryfy: Taryfa)
    }

    class AdministratorSystemu {
        +przeslijZadanieAktualizacji(taryfy: Taryfa)
    }

    class BazaTaryf {
        +zapiszTaryfy(taryfy: Taryfa)
    }

    class Biletomat {
        +odbierzAktualizacje(taryfy: Taryfa)
        +otrzymajPowiadomienieOBledach(taryfy: Taryfa)
    }

    class Taryfa {
        -id: String
        -nazwa: String
        -stawka: Float
    }

    AdministratorSystemu --> SystemBiletowy : wysyła żądanie aktualizacji
    SystemBiletowy --> BazaTaryf : zapisuje taryfy
    SystemBiletowy --> Biletomat : przesyła aktualizacje
    SystemBiletowy --> Biletomat : powiadamia o błędach

    end
```
 
# Diagramy Sekwencji

### Aktualizacja taryf

### weryfikacja ważności biletu
>- AKTOR: system biletowy
>- OBIEKTY: kontroler
>- Scenariusz główny:
>    - System biletowy odbiera żądanie weryfikacji ważności biletu od innego systemu 
>    - System biletowy sprawdza dane biletu w swojej bazie danych 
>    - System biletowy przesyła odpowiedź ważny do żądającego systemu 
>- Scenariusz alternatywny 1 (nieważny bilet):
>    - System biletowy odbiera żądanie weryfikacji ważności biletu od innego systemu
>    - System biletowy sprawdza dane biletu w swojej bazie danych i identyfikuje, że bilet jest nieważny 
>    - System biletowy wysyła odpowiedź o nieważnym bilecie do żądającego systemu 
>    - System biletowy rejestruje zdarzenie nieważnego biletu jako potencjalną próbę oszustwa 

```mermaid
sequenceDiagram
 
    PARTICIPANT USER AS Kontroler
    PARTICIPANT SB AS System biletowy


    USER->>SB: żądanie weryfikacji ważności biletu
    SB->>SB: sprawdzenie danych biletu
    ALT Bilet ważny
    SB-->>USER: bilet ważny
    ELSE Bilet nieważny
    SB-->>SB: zapiaznie weryfikacji nieważnego biletu
    SB-->>USER: bilet nieważny
    END

```

### Aktualizacja taryf
>- AKTOR: Administrator systemu
>- OBIEKTY: System biletowy, Baza taryf, Biletomat
>- Scenariusz główny:
>    - Aktor przesyła żądanie aktualizacji taryf
>    - System biletowy odbiera żądanie aktualizacji taryf
>    - System biletowy wprowadza aktualizacje taryf
>    - System biletowy przesyła aktualizacje taryf do Biletomatu
>- Scenariusz alternatywny 1 (Błędy taryf):
>    - Aktor przesyła żądanie aktualizacji taryf
>    - System biletowy odbiera żądanie aktualizacji taryf
>    - System biletowy wprowadza aktualizacje taryf
>    - System biletowy przesyła aktualizacje taryf do Biletomatu
>    - System biletowy powiadamia Biletomat o błedach taryf

```mermaid
sequenceDiagram
 
    PARTICIPANT USER AS Administrator Systemu
    PARTICIPANT SB AS System biletowy
    PARTICIPANT DB AS Baza taryf
    PARTICIPANT BT AS Biletomat


    USER->>SB: Żądanie aktualizacji
    SB->>SB: Odebranie żądania aktualizacji
    SB->>SB: Wprowadzenie nowych taryf
    SB->>DB: Aktualizacja bazy taryf
    SB->>BT: Aktualizuj taryfy
    ALT Błędy taryf
    SB->>BT: Powiadomienie o błędach
    END
```

# Diagramy przypadków uzycia

### Współpraca z aplikacjami mobilnymi
```mermaid
flowchart TD
    
    Aplikacja_mobilna{Aplikacja mobilna}

    Aplikacja_mobilna --> Odebranie_zadania_zakupu
    Przeslanie_biletu_elektronicznego --> Aplikacja_mobilna
    Obsluga_bledow_zakupu --> Aplikacja_mobilna
    subgraph system biletowy
    
    Odebranie_zadania_zakupu([Odebranie żądania wydania biletu])
    Generowanie_biletu_elektronicznego([Generowanie biletu elektronicznego])
    Przeslanie_biletu_elektronicznego([Przesłanie biletu])
    
    Generowanie_biletu([Generowanie biletu])
    Obsluga_bledow_zakupu([Powiadomienie o błędach])


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
    
    Biletomat{Biletomat}
    Kontroler{Kontroler}
    Biletomat --> Odebranie_zadania_weryfikacji
    Kontroler --> Odebranie_zadania_weryfikacji
    Przeslanie_wyniku_weryfikacji -->Biletomat
    Przeslanie_wyniku_weryfikacji -->Kontroler
    subgraph system biletowy
    
    Odebranie_zadania_weryfikacji([Odebranie żądania weryfikacji])
    Sprawdzenie_danych_biletu([Sprawdzenie danych biletu])
    Przeslanie_wyniku_weryfikacji([Przesłanie wyniku weryfikacji])

    Weryfikacja_bazy_danych([Weryfikacja bazy danych])
    Powiadomienie_o_oszustwie([Powiadomienie o oszustwie])

    Odebranie_zadania_weryfikacji --> Sprawdzenie_danych_biletu
    Sprawdzenie_danych_biletu --> Przeslanie_wyniku_weryfikacji

    Sprawdzenie_danych_biletu --> |include| Weryfikacja_bazy_danych
    Powiadomienie_o_oszustwie -.-> |extend| Przeslanie_wyniku_weryfikacji
    end
```

### Rejestracja tranzakcji

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
    Zapis_transakcji_w_bazie([Zapis danych transakcji])
    Potwierdzenie_rejestracji_transakcji([Potwierdzenie rejestracji transakcji])

    Zapis_danych_transakcji([Zapis transakcji])
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
    
    Aplikacja_mobilna{Aplikacja mobilna}
    Administrator_systemu{Administrator systemu}
    Biletomat{Biletomat}
    Kontroler{Kontroler}

    Aplikacja_mobilna --> Odebranie_zadania_zakupu
    Przeslanie_biletu_elektronicznego --> Aplikacja_mobilna
    Obsluga_bledow_zakupu --> Aplikacja_mobilna
    Administrator_systemu --> Odebranie_zadania_aktualizacji
    Przeslanie_zaktualizowanych_taryf --> Biletomat
    Powiadomienie_o_bledach_taryf --> Biletomat
    Biletomat --> Odebranie_zadania_weryfikacji
    Kontroler --> Odebranie_zadania_weryfikacji
    Przeslanie_wyniku_weryfikacji -->Biletomat
    Przeslanie_wyniku_weryfikacji -->Kontroler
    Biletomat --> Odebranie_danych_transakcji
    Aplikacja_mobilna --> Odebranie_danych_transakcji
    Potwierdzenie_rejestracji_transakcji -->Biletomat
    Potwierdzenie_rejestracji_transakcji -->Aplikacja_mobilna

    subgraph system biletowy
    Odebranie_zadania_zakupu([Odebranie żądania wydania biletu])
    Generowanie_biletu_elektronicznego([Generowanie biletu elektronicznego])
    Przeslanie_biletu_elektronicznego([Przesłanie biletu])
    Generowanie_biletu([Generowanie biletu])
    Obsluga_bledow_zakupu([Powiadomienie o błędach])
    Odebranie_zadania_zakupu --> Generowanie_biletu_elektronicznego
    Generowanie_biletu_elektronicznego --> Przeslanie_biletu_elektronicznego
    Generowanie_biletu_elektronicznego --> |include| Generowanie_biletu
    Obsluga_bledow_zakupu -.-> |extend| Generowanie_biletu_elektronicznego
    
    Odebranie_zadania_aktualizacji([Odebranie żądania aktualizacji])
    Wprowadzenie_nowych_taryf([Wprowadzenie nowych taryf])
    Przeslanie_zaktualizowanych_taryf([Przesłanie zaktualizowanych taryf])
    Aktualizacja_bazy_taryf([Aktualizacja bazy taryf])
    Powiadomienie_o_bledach_taryf([Powiadomienie o błędach taryf])
    Odebranie_zadania_aktualizacji --> Wprowadzenie_nowych_taryf
    Wprowadzenie_nowych_taryf --> Przeslanie_zaktualizowanych_taryf
    Wprowadzenie_nowych_taryf --> |include| Aktualizacja_bazy_taryf
    Powiadomienie_o_bledach_taryf -.-> |extend| Przeslanie_zaktualizowanych_taryf
    
    Odebranie_zadania_weryfikacji([Odebranie żądania weryfikacji])
    Sprawdzenie_danych_biletu([Sprawdzenie danych biletu])
    Przeslanie_wyniku_weryfikacji([Przesłanie wyniku weryfikacji])
    Weryfikacja_bazy_danych([Weryfikacja bazy danych])
    Powiadomienie_o_oszustwie([Powiadomienie o oszustwie])
    Odebranie_zadania_weryfikacji --> Sprawdzenie_danych_biletu
    Sprawdzenie_danych_biletu --> Przeslanie_wyniku_weryfikacji
    Sprawdzenie_danych_biletu --> |include| Weryfikacja_bazy_danych
    Powiadomienie_o_oszustwie -.-> |extend| Przeslanie_wyniku_weryfikacji
    
    Odebranie_danych_transakcji([Odebranie danych transakcji])
    Zapis_transakcji_w_bazie([Zapis danych transakcji])
    Potwierdzenie_rejestracji_transakcji([Potwierdzenie rejestracji transakcji])
    Zapis_danych_transakcji([Zapis transakcji])
    Powiadomienie_o_bledach([Powiadomienie o błędach])
    Odebranie_danych_transakcji --> Zapis_transakcji_w_bazie
    Zapis_transakcji_w_bazie --> Potwierdzenie_rejestracji_transakcji
    Zapis_transakcji_w_bazie --> |include| Zapis_danych_transakcji
    Powiadomienie_o_bledach -.-> |extend| Potwierdzenie_rejestracji_transakcji
    end
```
