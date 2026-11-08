# prospector-sprite-template

A GitHub Actions workflow that builds the firmware of a
[Prospector Dongle](https://github.com/akira-toriyama/zmk-beacon) with one
sprite embedded, from the sprite's number, in your own repository's run. The
firmware is [zmk-beacon](https://github.com/akira-toriyama/zmk-beacon)'s, built
by its reusable workflow; this repository holds nothing but the workflow that
calls it.

## Use

1. **Use this template** (the button above) to make a repository of your own,
   and make it **private**: the run's artifact holds the picture, and anyone
   can download the artifacts of a public repository.
2. In your repository: **Actions** → **prospector-sprite** → **Run workflow**.
   Give the sprite's number (its National Pokédex number) and the language of
   the name shown above the HP bar: `en`, the English name, or `ja`, the
   katakana name when it fits the bar, else the English one. The run takes
   about four minutes.
3. Download the run's artifact **prospector-sprite** and flash the
   `prospector-sprite.uf2` inside it: double-tap the Prospector Dongle's reset
   button and copy the file onto the `XIAO-SENSE` volume that appears
   (zmk-beacon's README, "Flashing", has the details and the no-hands way).

Another sprite is another run. GitHub keeps an artifact for 90 days.

## What the image does

The sprite plays above the HP bar; the bar shows your keyboard's battery once
the keyboard's split central runs zmk-beacon's broadcaster (zmk-beacon's README,
"Using the module in a ZMK config"). Until then the bar reads `--/100`, and
the screen stays lit anyway (this image never turns it off by itself; add
`CONFIG_BEACON_SCREEN_OFF_AFTER_S=300` under `extra_conf` in the workflow for
the five-minute timeout).

This image takes its sprite from the build only: it has no BLE receiver, so it
neither advertises nor connects, and zmk-beacon's sprite sender cannot swap
its sprite. To show another sprite, run the workflow again and flash again.

## The pictures and the names

The workflow fetches the GIF from
[PokeAPI/sprites](https://github.com/PokeAPI/sprites) (the Black/White animated
sprites) and the name from
[PokeAPI/pokeapi](https://github.com/PokeAPI/pokeapi)'s species names CSV,
both at commits pinned in zmk-beacon's `tools/sprite_fetch.py`, straight into
your run. They never enter this repository, yours, or zmk-beacon's.

The sprites are © The Pokémon Company (PokeAPI/sprites' `LICENCE.txt`); the
names come from a BSD-3-Clause data set; Pokémon and the Pokémon character
names are trademarks of Nintendo. Build the image for your own device and
your own use. Do not publish the artifact, the firmware or anything made from
the pictures, and keep the repository private.

Another source of `<number>.gif` files, or of names, goes in the workflow's
commented `with:` lines (a repository other than the default needs its commit
given too); a private one needs a token in the repository's secrets, named in
the commented `secrets:` lines.

## When the run fails

The log says where: `sprite_fetch:` lines for a GIF or CSV that could not be
fetched (a number with no GIF, a repository without its commit); CMake for a
GIF that does not convert or does not fit the LVGL pool; a `static assertion`
for one wider or taller than the sprite box; `region 'FLASH' overflowed` for a
pack too large for the flash. zmk-beacon's README, "The sprite", lists the
limits.

If your repository's settings require actions to be pinned by SHA, the tag
references inside zmk-beacon's reusable workflow (`actions/checkout@v7`) may
be refused; this has not been tried.

## License

The workflow and this text: MIT (`LICENSE`). The pictures and the names are
not part of this repository and come under their own terms, above.
