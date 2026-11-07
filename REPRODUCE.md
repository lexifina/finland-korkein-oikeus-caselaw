# Reproducing this repository

Input: the decision pages of finlex.fi listed in its sitemaps (court types under /fi/oikeuskaytanto/korkein-oikeus/), fetched on
2026-11-07 by `tools/fetch.py` (at most 4 requests a second) and read by `tools/flight.py`; finland-law `main` for
the overlay (`tools/fi_index.py`).

```
python tools/fetch.py <fetched>
python tools/build.py korkein-oikeus <fetched> <empty folder> --today 2026-11-07 --retrieved 2026-11-07
```

The 6,866 precedents Finlex gives no decision day for were first left undated (decisions/undated/) and were
appended on 2026-11-07 by `tools/backfill_year_only.py`: one commit each after the build's, dated by `build.dating`
(31 December of the year given, `In-force-basis: year only ...`), then a documentation commit removing
decisions/undated/. A fresh `tools/build.py` run dates them the same way but commits them in date order.

```
python tools/backfill_year_only.py <this repo> --page-dates <url to page day JSON>
```

Overlay rows for the Constitution cited in the genitive ("perustuslain 21 §") were added by `tools/constitution_rows.py`
for decisions built before `tools/fi_index.py` knew that form; later builds read it themselves.
