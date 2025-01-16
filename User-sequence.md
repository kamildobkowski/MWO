@startuml
title Zakup biletu z uwzględnieniem płatności gotówką, reszty oraz monitorowania systemu

actor "Użytkownik" as U
participant "Biletomat" as BM
participant "System Transakcyjny" as TS
participant "System Biletowy" as SB
participant "System Wsparcia Technicznego" as SWT

U->>BM: Wybór rodzaju biletu

activate BM
BM-->>U: Wyświetlenie opcji płatności
deactivate BM

opt Płatność kartą
    U->>BM: Wybór płatności kartą
    activate BM
    BM-->>U: "Przyłóż kartę do terminala"
    deactivate BM

    U->>TS: Autoryzacja płatności
    activate TS
    alt Płatność zaakceptowana
        TS-->>BM: Pozytywna autoryzacja
        deactivate TS

        activate BM
        BM-->>SB: Rejestracja sprzedanego biletu
        activate SB
        SB-->>BM: Potwierdzenie rejestracji
        deactivate SB

        BM-->>U: "Dziękujemy za zakup" + wydanie biletu
        deactivate BM
    else Brak środków
        TS-->>BM: Odmowa autoryzacji
        deactivate TS

        activate BM
        BM-->>U: "Płatność odrzucona. Spróbuj ponownie."
        deactivate BM
    else Awaria terminala
        TS-->>BM: Brak odpowiedzi
        deactivate TS

        activate BM
        BM-->>SWT: Alert o awarii terminala
        activate SWT
        SWT-->>BM: Potwierdzenie odbioru zgłoszenia
        deactivate SWT

        BM-->>U: "Terminal niedostępny. Wybierz inną metodę płatności."
        deactivate BM
    end
else Płatność gotówką
    U->>BM: Wybór płatności gotówką
    activate BM
    BM-->>U: "Proszę włożyć gotówkę"
    U->>BM: Włożenie gotówki
    alt Kwota wystarczająca
        BM-->>SB: Rejestracja sprzedanego biletu
        activate SB
        SB-->>BM: Potwierdzenie rejestracji
        deactivate SB

        BM-->>U: "Dziękujemy za zakup" + wydanie biletu
    else Nadpłata
        BM-->>U: Wydanie biletu + reszta
    else Brak środków
        BM-->>U: "Za mało środków. Proszę dopłacić."
    end
    deactivate BM
end

BM-->>SWT: Automatyczne monitorowanie stanu (np. brak papieru)
activate SWT
SWT-->>BM: Potwierdzenie monitorowania
deactivate SWT
@enduml
