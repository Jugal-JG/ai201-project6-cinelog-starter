# PR Response Doc — CineLog Watchlist Feature

## AI Usage

<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 — Rename

**What I did:** Renamed `save_to_watchlist()` to `add_to_watchlist()` in `services/watchlist_service.py`. I searched the repository for the old name and updated the one call site in `routes/watchlist/watchlist.py`, including its import.

**How I verified:** A follow-up project-wide search returned no `save_to_watchlist` references. I then ran `pytest tests/ -v`; all four existing tests passed.

## Comment 2 — Deduplication

**What I did:** Added `AlreadyInWatchlistError` and a `WatchlistEntry.query.filter_by(user_id=user_id, film_id=film_id).first()` check in `add_to_watchlist()`. The check runs after confirming the film exists and before constructing or committing a new entry, matching `add_to_collection()`'s deduplication flow.

**How I verified:** In an isolated in-memory database, I added the same film twice for one user. The second call raised `AlreadyInWatchlistError`, and the database still contained exactly one matching entry. I also ran `pytest tests/ -v`; all four existing tests passed.

## Comment 3 — Missing test

**What I did:** Created `tests/test_watchlist.py` with `test_add_to_watchlist_nonexistent_film_raises`. I modeled its isolated in-memory database fixture, sample-user fixture, fake ID, and `pytest.raises(FilmNotFoundError)` assertion on `test_add_to_collection_nonexistent_film_raises`.

**How I verified:** `pytest tests/test_watchlist.py -v` passed the new test. I then ran `pytest tests/ -v`; all five tests passed.

## Comment 4 — Default visibility

**My position:**

**Reasoning:**

**Tradeoff acknowledged:**

## Comment 5 — Sort order

**My position:**

**Reasoning:**

**Engagement with reviewer's point:**

## Comment 6 — Rebase

**What conflicted:**

**How I resolved it:**

**How I verified no conflict remains:**

## PR Description

<!-- Written at the end — feature overview, design decisions, manual testing steps -->
