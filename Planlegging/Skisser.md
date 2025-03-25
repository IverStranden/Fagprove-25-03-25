# Data Modell
![image](https://github.com/user-attachments/assets/4e4ad9db-b69d-4383-b664-97161e1bca33)
## Forklaring
  ### Routines
  - Dette skal være tabellen som inneholder rutinen. Tanken er at man skal kunne lage rutiner som man kan gjennomføre gjentatte ganger.
  ### Exercises
  - Inneholder øvelser som man kan gjøre. Øvelsene kan for eksempel være knebøy eller benkpress, og feltene "WeightIsAssisted" og "isGlobal" sier om øvelsene er maskinassisterte eller fellesøvelser for alle brukere, siden tanken er at brukerene skal kunne lage egne øvelser.
  ### Workout Exercise Routines
  - Kobler sammen rutiner med øvelser og definerer rekkefølgen (via Order) og standard antall sett per exercise.
  ### Session
  - Sessions er den faktiske treningsøkten hvor en spesifikk rutine blir gjennomført. Inneholder også tidspunkt for start og slutt.
  ### Sets
  - Inneholder info om hvert enkelt sett som ble gjort i løpet av en Workout Session, inkludert vekt og repetisjoner.
  ### Workout Persons
  - Inneholder data relatert til en spesifikk person sin treningsaktivitet, som kroppsvekt med kobling til System Persons.
    
## Views & Procedures
  ### Statistics 
  - Dette blir enten håndtert via ett view, eller så blir den håndtert via en procedure. Jeg tenker å hente dataen fra f.eks sets for å se hvor god økning det har vært basert på tid.
  ### Personal Best
  - Viser høyeste registrerte vekt og flest repetisjoner per øvelse.
  ### Projection
  - Viser anntatt styrke fremover basert på data.

[Draw SQL](https://drawsql.app/teams/iver-a-co/diagrams/training-app-fagproeve)

# Figma skisser
![image](https://github.com/user-attachments/assets/60801c4d-6911-4b73-8b6d-e3d6c2f566aa)
![image](https://github.com/user-attachments/assets/1d33c448-b1f5-4994-a6d7-d84501ef6246)
![image](https://github.com/user-attachments/assets/22fb6c67-fa87-40ce-a5c8-3c7a9f04763b)

[Figma](https://www.figma.com/design/qTi4bul6hcGCOdqzFOb1Uw/Untitled?node-id=0-1&p=f&t=kQM19UO93CqwIWt4-0)
