# TODO

- [x] Benchmark `ComputeMechanics=true` vs `false` in `gw2ei.conf`.
      Tested against a real 10-player Dhuum log (2 runs each): true =
      3353ms/3620ms, 39,265,726 B; false = 3350ms/3440ms, 39,221,002 B.
      Difference (~90ms, ~0.1% size) is within run-to-run noise -
      negligible.
      Re-tested against the largest The Dragonvoid log (11m33s fight, 40
      distinct mechanic types present, 10 players, 2 runs each): true =
      8564ms/7925ms, 150,629,484 B; false = 8300ms/8010ms, 150,335,402 B.
      Still negligible (~90ms, ~0.2% size), smaller than the in-group
      run-to-run spread. Confirmed on the heaviest-mechanics case
      available, not just a short fight - left on (the default) since
      it's free and the data may be useful later.

- [x] Benchmark `EI_PLAYER_FILTER` end-to-end against a real 10-player
      Dhuum log: unfiltered = 39,265,726 B / 3353-3620ms; filtered to 1
      account = 8,583,706 B / 2332ms. ~78% smaller, ~32% faster on this
      log - confirms the player-filter approach works as designed.

- [ ] Revisit `IndentJSON=true` in `gw2ei.conf` once the downstream log
      processing pipeline is built - it's on now for readability during
      development, at the cost of larger output files. Turning it off
      (and/or turning `CompressRaw` on) is a pure size win once
      readability while building stops mattering.
