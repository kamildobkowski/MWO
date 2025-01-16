@startuml
title Zarządzanie Ofertą przez Administratora

actor Administrator

participant "System Centralny" as Centralny
participant "Biletomat ZTM" as Biletomat
participant "System Biletowy" as Biletowy

Administrator -> Centralny: Zaloguj się do systemu
activate Centralny
Centralny --> Administrator: Potwierdzenie logowania
deactivate Centralny

Administrator -> Centralny: Przejdź do zarządzania ofertą
activate Centralny
Centralny -> Administrator: Wyświetl aktualną ofertę
deactivate Centralny

Administrator -> Centralny: Edytuj ofertę (bilety, promocje, taryfy)
activate Centralny
Centralny -> Centralny: Aktualizuj dane oferty
Centralny --> Administrator: Potwierdzenie aktualizacji
deactivate Centralny

Centralny -> Biletomat: Prześlij zaktualizowaną ofertę
deactivate Centralny

activate Biletomat
Biletomat -> Biletowy: Synchronizuj z ofertą centralną
activate Biletowy
Biletowy --> Biletomat: Potwierdzenie synchronizacji
deactivate Biletowy
Biletomat --> Centralny: Potwierdzenie aktualizacji na urządzeniu
deactivate Biletomat

Centralny --> Administrator: Potwierdzenie zarządzania ofertą
@enduml

@startuml
title Dokonywanie płatności z obsługą wielokrotnych prób

actor User

participant "Biletomat ZTM" as Biletomat
participant "System Transakcyjny" as Transakcyjny

User -> Biletomat: Wybierz opcję płatności
activate Biletomat

Biletomat -> User: Poproś o dane płatności
deactivate Biletomat

loop Do wprowadzenia danych płatności
    activate User
    User -> Biletomat: Wprowadź dane płatności
    deactivate User

    activate Biletomat
    Biletomat -> Transakcyjny: Przetwórz płatność
    activate Transakcyjny

    Transakcyjny --> Biletomat: Status transakcji (sukces/niepowodzenie)
    deactivate Transakcyjny

    alt Sukces transakcji
        Biletomat -> User: Potwierdzenie płatności
        break
    else Niepowodzenie transakcji
        Biletomat -> User: Komunikat o błędzie
    end
    deactivate Biletomat
end
@enduml
@startuml
title Zdalna aktualizacja oprogramowania biletomatu

actor "Administrator" as A
participant "System Zarządzania" as SZ
participant "Biletomat" as BM

A->>SZ: Zlecenie aktualizacji oprogramowania
activate SZ
SZ-->>A: Potwierdzenie przyjęcia zlecenia

SZ->>BM: Rozpoczęcie procesu aktualizacji
activate BM
BM-->>SZ: Potwierdzenie odbioru żądania

alt Aktualizacja przebiega poprawnie
    SZ->>BM: Wysłanie pakietu aktualizacji
    BM-->>SZ: Potwierdzenie instalacji i restart urządzenia
    SZ-->>A: Informacja o sukcesie aktualizacji
else Błąd w trakcie aktualizacji
    BM-->>SZ: Informacja o błędzie podczas aktualizacji
    SZ-->>A: Powiadomienie o problemie i wymaganej interwencji
end
@startuml
title Zdalna aktualizacja oprogramowania biletomatu

actor "Administrator" as A
participant "System Zarządzania" as SZ
participant "Biletomat" as BM

A->>SZ: Zlecenie aktualizacji oprogramowania
activate SZ
SZ-->>A: Potwierdzenie przyjęcia zlecenia

SZ->>BM: Rozpoczęcie procesu aktualizacji
activate BM
BM-->>SZ: Potwierdzenie odbioru żądania

alt Aktualizacja przebiega poprawnie
    SZ->>BM: Wysłanie pakietu aktualizacji
    BM-->>SZ: Potwierdzenie instalacji i restart urządzenia
    SZ-->>A: Informacja o sukcesie aktualizacji
else Błąd w trakcie aktualizacji
    BM-->>SZ: Informacja o błędzie podczas aktualizacji
    SZ-->>A: Powiadomienie o problemie i wymaganej interwencji
end

deactivate BM
deactivate SZ
@enduml
deactivate BM
deactivate SZ
@enduml
