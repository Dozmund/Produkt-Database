# Produktdatabase – VVS-komponenter (Markdown)

## Struktur
- `produsenter/<id>.md` – én fil per produsent
- `komponenter/<id>.md` – én fil per komponent; strukturerte felt øverst (YAML), notater og innstillingstabell under
- `INDEKS.md` – oversiktstabell (oppdateres når oppføringer endres)

## Regler (gir enkel migrering senere)
- Filnavn = `id` (små bokstaver, bindestrek). Endre aldri en id.
- `produsent:` må matche en fil i `produsenter/`.
- Feltnavn uten æøå (for database-kompatibilitet); verdier kan ha æøå.
- Faste verdier i lister:
  - protokoller: BACnet MS/TP, BACnet/IP, Modbus RTU, Modbus TCP, MP-Bus, KNX, EXOline, N2, SLC
  - analog_io: 0-10V, 2-10V, 4-20mA, 3-punkt, analog
  - styremodus: VAV, CAV, CAV trinn, posisjon, DCV, varme/kjøling
- Innstillinger i tabellen: Navn | Område | Enhet | Standard.
- `kilde` og `verifisert` på alle oppføringer.

## Søk og redigering nå
Åpne mappen i **Obsidian** (gratis) som hvelv. Fritekstsøk, og med Dataview-utvidelsen kan du filtrere på feltene:
```dataview
TABLE produsent, protokoller FROM "komponenter"
WHERE type = "VAV-regulator" AND contains(protokoller, "Modbus RTU") AND contains(analog_io, "0-10V")
```

## Oppgradering til ekte database
1. **Nå:** Markdown med strukturerte felt.
2. **Migrering:** et skript leser feltene inn i tabeller: `produsenter`, `komponenter`, `komponent_protokoller`, `komponent_io`, `komponent_styremodus`, `innstillinger (komponent_id, navn, omrade, enhet, standard)`.
3. **Mål:** Postgres (Supabase) eller Directus/NocoDB – flerbruker-redigering, søkegrensesnitt og API mot SD/BMS.
4. Skraping skriver da direkte til databasen.
