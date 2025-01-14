1. Jako biletomat, chcę automatycznie aktualizować listę dostępnych biletów i ich cen, aby zapewnić zgodność z polityką przewoźnika.
2. Jako biletomat, chcę rejestrować wszystkie transakcje i wysyłać raporty do systemu centralnego, aby umożliwić monitoring i kontrolę operacji.
3. Jako biletomat, chcę posiadać czytelny ekran dotykowy, aby użytkownik mógł łatwo nawigować po interfejsie.
4. Jako biletomat, chcę być wyposażony w różne metody płatności (terminal kart, czytnik gotówki, NFC), aby obsługiwać różnorodne transakcje.
5. Jako biletomat, chcę wydawać resztę w gotówce, jeśli użytkownik zapłaci nadmiarowo, aby transakcja była zgodna z oczekiwaniami.

```mermaid
flowchart TD
    Biletomat{Biletomat}
    Obsluga_wyboru_jezyka([Obsługa wybotu języka])
    Generowanie_eiletu_elektronicznego([Generowanie biletu elektronicznego])
    Powiadomienie_o_bledach_w_procesie([Powiadomienie o błędach w procesie])

    Biletomat --> Obsluga_wyboru_jezyka
    Obsluga_wyboru_jezyka --> |include| Generowanie_eiletu_elektronicznego
    Obsluga_wyboru_jezyka --> |include| Powiadomienie_o_bledach_w_procesie
```
