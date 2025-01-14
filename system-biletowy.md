1. Jako system biletowy, chcę dostarczać aktualne dane o taryfach i typach biletów do biletomatu, aby użytkownik miał zawsze poprawne informacje.
2. Jako system biletowy, chcę umożliwiać sprawdzenie ważności biletu w czasie rzeczywistym, aby zapobiegać oszustwom.
3. Jako system biletowy, chcę rejestrować każde sprzedane bilety, aby śledzić ruch i sprzedaż w systemie.
4. Jako system biletowy, chcę współpracować z aplikacjami mobilnymi, aby użytkownik mógł uzyskać elektroniczny bilet w przypadku takiego wyboru.

## Diagramy przypadków uzycia

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