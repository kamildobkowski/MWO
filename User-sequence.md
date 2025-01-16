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
