# Coaster Credit Tracker — Final Analysis

**File:** `index.html` (3,614 lines)
**Data:** 269 coasters across 6 years (2021–2026), 305 deduplicated comments, 269 base entries

---

## 🔴 Critical: Delete-by-Name Collision

**The single most dangerous bug in the app.** `saveDeleted()` stores only the ride name, and `filterDeleted()` matches on `c.Ride` alone — no park qualifier.

```js
saveDeleted(rideName) {
    deleted.add(rideName);  // just the name
}

filterDeleted() {
    AppState.coasters = AppState.coasters.filter(c => !deleted.has(c.Ride));  // matches ALL parks
}
```

**14 ride names are shared across multiple parks (29 entries at risk):**

| Name | Parks |
|------|-------|
| Batman The Ride | Great Adventure (m=67), Magic Mountain (m=79) |
| Thunderbolt | Kennywood (m=88), SFNE (m=129), Coney Island (m=182) |
| Space Mountain | Magic Kingdom (m=139), Disneyland (m=266) |
| Viper | Magic Mountain (m=3), Darien Lake (m=161) |
| Vortex | Canada's Wonderland (m=19), Carowinds (m=244) |
| Flight of Fear | Kings Dominion (m=193), Kings Island (m=194) |
| Canyon Blaster | Adventuredome (m=78), Great Escape (m=219) |
| Backlot Stunt Coaster | Kings Dominion (m=158), Kings Island (m=159) |
| Runaway Mine Train | Alton Towers (m=184), Great Adventure (m=201) |
| Big Thunder Mountain Railroad | Disneyland (m=199), Magic Kingdom (m=202) |
| Thunder Run | Canada's Wonderland (m=140), Kentucky Kingdom (m=206) |
| Woodstock Express | Kings Dominion (m=177), Kings Island (m=214) |
| Comet | Hersheypark (m=176), Waldameer (m=229) |
| Wildcat | Lake Compounce (m=242), Hersheypark (m=259, defunct) |

**Impact:** Deleting any one of these rides silently removes all rides sharing that name. This is invisible to the user — the extra rides just vanish on the next reload.

**Compound effect — ranking gap on reload:** At delete time, only the clicked coaster is removed and rankings shift correctly. But on reload, `filterDeleted` removes the *other* same-named rides **without** a ranking shift, creating a permanent gap in the ranking sequence.

**Compound effect — weighted ranking miscalculation:** `recalculateWeightedRankings` uses `yearData2026.length - deleted.size` as the denominator. `deleted.size` is 1 (one name stored), but 2+ rides are actually removed. The denominator is therefore too high, inflating all weighted rankings by a small additional amount.

**Fix:** Use `Ride + '|||' + Park` as the key everywhere — `saveDeleted()`, `loadDeleted()`, and `filterDeleted()`.

---

## 🟠 High Priority

### 1. Data Truncation — `"lr": "204"` on Two Entries

Two coaster base entries have `"lr": "204"` instead of a full year:

- **m=88:** Thunderbolt (Kennywood) — should likely be `"2024"` or `"2025"`
- **m=239:** Cedar Creek Mine Ride (Cedar Point) — should likely be `"2024"` or `"2025"`

This is baked into `_B`, so it's a source data error. Any filter or display referencing "Last Ride" will show `204` for these two.

### 2. coasterBase Mutation Bleeds Into Past Year Views

`applyToCoasterBase()` directly mutates the shared `coasterBase` array with 2026 overrides (Status, Last Ride, Type, Opening Year). Since `mergeWithBase()` spreads from the same `coasterBase` object, switching to any past year shows the mutated 2026 values for those fields.

**Example:** If a user edits Anaconda's status to "Defunct" in 2026 (which it actually is now — `_B[118].s` is already `1`), then switches to 2021, Anaconda would show as "Defunct" in 2021 even though it was operating that year.

**Init order that causes this:**
1. `init()` → `applyToCoasterBase()` mutates `coasterBase`
2. User switches to 2021 → `mergeWithBase()` spreads from mutated `coasterBase`
3. 2021 view now shows 2026 Status/Type values for any overridden rides

The original base values are never preserved for rollback. `snapshotBase()` captures them in `_originalBase` for the purpose of change detection, but they're never used to *restore* the base when switching years.

---

## 🟡 Medium Priority

### 3. Category Trailing Whitespace

`_C[20]` is `"Wing Coaster "` (trailing space). This causes subtle mismatches: filter dropdowns may show both `"Wing Coaster"` (trimmed in some paths) and `"Wing Coaster "` (raw), or chart groupings may split what should be one category into two.

Only affects m=124 (Gatekeeper @ Cedar Point) since it's the only entry with `cat: 20`.

### 4. Import Lacks Structural Validation

`importData()` only checks for `payload._version` before writing directly to localStorage:

```js
if (!payload._version) { alert('Invalid file...'); return; }
// Then immediately writes to localStorage
localStorage.setItem(this.KEY, JSON.stringify(payload.userAdded));
localStorage.setItem(this.OVERRIDES_KEY, JSON.stringify(payload.overrides));
```

No validation that:
- `m` indices in overrides exist in the current `coasterBase`
- Field names are from the expected set
- Rating/Ranking values are valid numbers
- The export was generated from the same version of the app

A corrupted or hand-edited JSON file could inject invalid data.

### 5. Weighted Ranking Denominator Includes 8 Unrated Placeholders

`recalculateWeightedRankings` uses `yearData2026.length` (269) as the current total. But 8 entries (m=261–268) have no rating or ranking — they're "do not remember" placeholders with `ci: 0`. The effective coaster count is 261.

This inflates the WR multiplier by ~3.1%:
- Current: `269 / 117` = 2.299 for 2021
- Correct: `261 / 108` = 2.417 for 2021 (if also excluding unrated in 2021)

The direction of the skew depends on how you define "correct" — excluding unrated from both numerator and denominator would actually increase the multiplier, not decrease it. Either way, the current formula mixes rated and unrated counts inconsistently.

### 6. Legacy Migration Uses Ride-Only Lookup

```js
const entry = yearData2026.find(e => coasterBase[e.m].Ride === ride);
```

If migrating from the old `coasterRankings2026` format, this finds the *first* match by ride name only, silently ignoring duplicates. For the 14 duplicate-name groups, the wrong park's entry could receive the migrated ranking.

### 7. Over-Time Chart Shows Deleted Rides

`filterDeleted()` removes rides from `AppState.coasters` but **not** from `yearData2026`. The over-time chart reads from `YearData.datasets[year]` directly, so deleted rides still appear in:
- The top N candidates in `getFilteredTopRides()`
- Individual ride data via `getRideDataByIndex()`
- Yearly averages via `getYearlyAverages()` (ratings/rankings still counted)

A deleted ride can still be selected in the over-time chart's ride search and will display data as if it still exists.

---

## 🔵 Low Priority

### 8. Rankings Can Exceed Coaster Count

No clamping prevents a user from entering a ranking like `999` on a dataset of 269. The delete ranking shift logic also doesn't validate bounds.

### 9. Export Lacks coasterBase Fingerprint

Exports contain no indicator of which version of `coasterBase` they were generated against. Importing an export from a different version (where `m` indices may have shifted) could silently apply overrides to the wrong rides.

### 10. Silent localStorage Error Swallowing

All `localStorage` operations are wrapped in `try/catch` blocks that silently ignore errors:
```js
try { localStorage.setItem(...); } catch (e) { /* ignore */ }
```

If localStorage is full, disabled, or throws, the user gets no feedback that their changes weren't saved.

---

## ✅ What's Working Well

- **Ride+Park keying** is correctly used in `registerUserRide()`, `unregisterUserRide()`, `syncRideToYearData()`, `save()`, and `_originalBaseNames`. The delete flow is the sole exception.
- **No duplicate `m` values** in any yearData array.
- **All `ci` values** are within `_CL` bounds (0–304).
- **All `m` values** are within `coasterBase` bounds (0–268).
- **Comment deduplication** via `_CL` / `ci` references is clean — no entries have both `ci` and `c` fields simultaneously.
- **Year data consistency:** 2025 and 2026 share the same 269 `m` values with no orphans.
- **Ranking shift on delete** correctly adjusts all ranks above the deleted ride (at delete time).
- **snapshotBase / applyOverrides cycle** correctly detects and persists user edits across reloads.

---

## Recommended Fix Priority

1. **Delete-by-name → Ride+Park key** (critical, prevents data loss)
2. **Fix `lr: "204"`** on m=88 and m=239 (trivial data fix)
3. **Snapshot and restore coasterBase** per-year to prevent mutation bleed (architectural)
4. **Trim `_C[20]`** whitespace (trivial)
5. **Add import validation** (safety net)
6. **Exclude unrated entries** from WR denominator (correctness)
