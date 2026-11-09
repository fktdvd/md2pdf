# md2pdf

Markdown → A4 PDF egy paranccsal, macOS-en. Pandoc-kal HTML-t csinál, a fej nélküli
Chrome-mal PDF-et ír belőle — nincs LaTeX, nincs plusz telepítés a Chrome-on kívül.
Nyomtatás külön lépés (`-p`-vel egyből megy az alap nyomtatóra).

## Telepítés

    git clone https://github.com/fktdvd/md2pdf.git ~/Documents/GitHub/md2pdf
    ln -s ~/Documents/GitHub/md2pdf/md2pdf ~/.local/bin/md2pdf

Kell hozzá:

- **pandoc** a PATH-ban. Homebrew-val csak akkor, ha arm64-en natív prefix van
  (`/opt/homebrew`). Intel prefix alatt (`/usr/local`) a brew forrásból kezdi
  fordítani (GHC + cabal + pandoc), ami nagyon lassú — ilyenkor a hivatalos
  előre fordított binárist használd:
  https://github.com/jgm/pandoc/releases (`pandoc-X.Y-arm64-macOS.zip`, a `bin/pandoc` megy `~/.local/bin`-be)
- **Google Chrome** (`/Applications/Google Chrome.app`), fej nélküli PDF-hez

## Használat

    md2pdf jegyzet.md             # -> jegyzet.pdf ugyanabban a mappában
    md2pdf a.md b.md c.md         # több fájl egyszerre
    md2pdf -o nev.pdf jegyzet.md
    md2pdf -p jegyzet.md          # PDF + azonnali nyomtatás (lp, alap nyomtató)
    md2pdf -k jegyzet.md          # a köztes HTML megtartása (hibakeresés)

| opció | jelentés |
|-------|----------|
| `-o FÁJL` | kimeneti fájlnév (csak egy bemenetnél) |
| `-p` | nyomtatás is az alapértelmezett nyomtatóra |
| `-k` | HTML megtartása |
| `-h` | súgó |

## Miért két eszköz

1. **pandoc** — a markdown jeleit HTML-lé alakítja (`#` cím, `-` felsorolás, `**szó**` félkövér).
2. **Chrome** — a HTML-t A4-es lapra rendezi és PDF-et ír belőle. Ugyanaz a motor, ami a Cmd+P-nél.

A markdown → PDF közvetlen út LaTeX-et kérne (~1–2 GB); a HTML → Chrome ingyen van, mert a Chrome már fent van.

## Megjegyzések

- A felsorolásokat `lists_without_preceding_blankline`-nal olvassa, mert a jegyzetekben
  gyakran nincs üres sor a bevezető sor és a `- ` lista között; enélkül egy bekezdésbe folyna.
- A `stilus.css` a szkript mellett van; a szkript symlinken át is megtalálja. Saját stílus:
  `MD2PDF_CSS=/ut/hozza/stilus.css md2pdf jegyzet.md`
- A képeket a `--embed-resources` miatt beágyazza, ha a markdown-hoz képest relatív az útvonal.
