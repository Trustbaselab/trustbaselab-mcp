# Verifying this dataset yourself

This repository's only unusual claim is: **you don't have to trust us.**

Here is exactly how to check that claim.

---

## Step 1 — get the data and the verifier

```bash
curl -O https://trustbaselab.com/upload/data.json
curl -O https://trustbaselab.com/verify/verify_trustbase.py
```

## Step 2 — run it

```bash
python3 verify_trustbase.py data.json
```

Expected output ends with:

```
✅ No hard mismatches found (hash / snapshot / fields)
```

Options:

```bash
# sample only, fast
python3 verify_trustbase.py data.json --sample 300

# also verify snapshot bytes (download /snapshots/ first)
python3 verify_trustbase.py data.json --snapshots ./snapshots
```

---

## What it checks

| # | Check | Method |
|---|---|---|
| ① | `record_hash` recomputes | sha256 over the 5-field canonical body |
| ①b | `body_hash` recomputes | sha256 over the full record minus `evidence` |
| ② | Snapshot bytes intact | gunzip → sha256 == filename |
| ③ | Record → snapshot pointer valid | `evidence.snapshot_url` resolves |
| ④ | `r_level` present | in `{R1, R1-own, R2, R3}` |
| ⑤ | Required fields present | schema-aware across 3 record shapes |

---

## The recipes

```python
import json, hashlib

def canonical(o):
    return json.dumps(o, ensure_ascii=False, sort_keys=True, separators=(",", ":"))

def record_hash(rid, lst, rec):
    body = {"id": rid, "list": lst,
            "public": rec.get("public"),
            "sourced": rec.get("sourced"),
            "description": rec.get("description")}
    return "sha256:" + hashlib.sha256(canonical(body).encode("utf-8")).hexdigest()

def body_hash(rec):
    body = {k: v for k, v in rec.items() if k != "evidence"}
    return "sha256:" + hashlib.sha256(canonical(body).encode("utf-8")).hexdigest()
```

`list` is the table name the record lives in (`policies`, `domain_benchmarks`,
`trade_flows`, …).

---

## Why `evidence` is excluded

`evidence` is *about* the record (source URLs, snapshot pointers, hashes), not
the record's content. Excluding it means evidence can be backfilled or corrected
**without invalidating the data hash** — which is what makes progressive
provenance work possible.

`body_hash` therefore covers everything else: `public`, `sourced`, `as_of`,
`type`, `title`, `facts`, trade values, and any future field.

---

## Snapshot verification

Snapshots are addressed by content hash:

```
https://trustbaselab.com/snapshots/<sha[0:2]>/<sha>.gz
```

To verify a snapshot:

```bash
curl -O https://trustbaselab.com/snapshots/b5/b56adf52....gz
gunzip -c b56adf52....gz | sha256sum
# must equal b56adf52...
```

That proves the byte-for-byte content of the source page **at fetch time**,
independent of whether the upstream page still says the same thing today.

---

## Reproduction tip

`/llms-full.txt` gives you the whole catalogue in one file. If you find a record
whose hash looks wrong, open an issue with the record `id` — that's the fastest
way to make this dataset better.
