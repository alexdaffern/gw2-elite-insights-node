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

- [x] Broad benchmark: baseline (stock EI defaults) vs ours
      (`ParseExtensions=false` + `EI_PLAYER_FILTER` to the first player)
      across up to 10 logs from each of all 60 boss folders in a real
      personal log library (568 valid logs, 9 excluded as genuine EI
      parse failures - "No Targets found", unrelated to our changes).
      Aggregate: time 1422.7s -> 1087.2s (-23.6%), size 5487.9MB ->
      1138.5MB (-79.3%). Scales with squad size as expected: solo/golem
      logs ~0% savings (nothing to filter), 5-player content 65-72%
      size, full 10-player squads 83-88% size / 8-34% time. Caveat:
      "first player" is a stand-in for whatever the real per-request
      player-selection logic ends up being - savings scale with how many
      players get excluded, so a different selection strategy would
      shift these numbers, though the mechanism/magnitude should hold.
      (Note: first attempt at this benchmark had a script bug - stale
      output files got re-read across iterations due to a fixed
      filename + a `rm` that ran without shell glob expansion, silently
      no-op-ing cleanup. Rewrote with per-iteration isolated directories
      and re-verified before trusting the numbers above.)

- [ ] Revisit `IndentJSON=true` in `gw2ei.conf` once the downstream log
      processing pipeline is built - it's on now for readability during
      development, at the cost of larger output files. Turning it off
      (and/or turning `CompressRaw` on) is a pure size win once
      readability while building stops mattering.
