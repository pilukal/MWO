1. Jako system biletowy, chcę dostarczać aktualne dane o taryfach i typach biletów do biletomatu, aby użytkownik miał zawsze poprawne informacje.
2. Jako system biletowy, chcę umożliwiać sprawdzenie ważności biletu w czasie rzeczywistym, aby zapobiegać oszustwom.
3. Jako system biletowy, chcę rejestrować każde sprzedane bilety, aby śledzić ruch i sprzedaż w systemie.
4. Jako system biletowy, chcę współpracować z aplikacjami mobilnymi, aby użytkownik mógł uzyskać elektroniczny bilet w przypadku takiego wyboru.
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
