# protein-fold-data

This repository is a passive JSON data store. It has no code, nothing to run, and
nothing to deploy on its own. It exists purely so the Netlify Functions behind
[protein-fold-site](https://github.com/Digim0rt4l/protein-fold-site) have somewhere
to read and write shared state via the GitHub Contents API, without needing a
separate database.

## What's in here

A single file, `data/state.json`, created automatically the first time the site is
used. It holds:

- `protein` — the sequence, PDB ID, and helix regions of the protein currently being
  folded (villin headpiece subdomain, PDB 1VII)
- `phiPsi` — the current best-known conformation, as an array of backbone and
  side-chain torsion angles, one entry per residue
- `energy` and `initialEnergy` — the current best score and the score of the very
  first, randomized starting conformation
- `ensemble` — a small, capped list of the best independent results submitted so far
- `claims` — trajectories currently in progress on some visitor's device
- `stats` — running totals: completed trajectories, accepted improvements, and a
  count of contributions per anonymous device ID

## Please don't edit this file by hand

The site's Netlify Functions read and write this file using optimistic concurrency
(a version check before every write, with automatic retry on conflict). A manual
edit made through the GitHub website or app doesn't go through that same check, and
could either be silently overwritten by the next automated write or leave the file
in a shape the site doesn't expect.

## Resetting the search

To start the fold over from scratch, delete `data/state.json` from this repository.
The next request to the site will regenerate it fresh, starting from a new
randomized, unfolded conformation.
