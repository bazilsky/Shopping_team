# Example output

This is the output of the 2026-09-26 run for the locked brief in `brief.lock`: an external USB-C SSD, 2TB or 4TB, delivered in the UK, selling price under £200. Soft preferences, withheld from both Shopper passes: 4TB over 2TB, then higher advertised sequential read, then higher advertised sequential write.

Two Shopper passes ran. One covered amazon.co.uk and ebay.co.uk. The other covered manufacturer and independent retailers. The script sorted the union. The Fit auditor checked one tied row. The run stops on a near-tie, so there is no single purchase recommendation.

## Result

No 4TB external SSD under £200 turned up on either route. Every row that cleared the brief is 2TB. The speed sort then ties.

## Shortlist

Sorted by the locked soft preferences. Sponsored rows would sort after an unsponsored tie. None of these rows were sponsored.

| Model | Advertised read / write | Price | Stock | List origin | Where |
| --- | --- | --- | --- | --- | --- |
| GiGimundo M20 | 2000 / 1800 MB/s | £145.99 | In stock | Marketplace | [Amazon](https://www.amazon.co.uk/dp/B0DFQ6VKP6) |
| KingSpec MemoStone US5 | 2000 / 1800 MB/s | £169.99 | 3 left | Marketplace | [Amazon](https://www.amazon.co.uk/dp/B0DP4PZ7HM) |
| Fikwot FP70 | 1050 / 1000 MB/s | £152.99 | In stock | Marketplace | [Amazon](https://www.amazon.co.uk/dp/B0DBQ984MC) |
| Crucial X9 (used) | 1050 / 1000 MB/s | £159.99 plus postage | Last one | Marketplace | [eBay](https://www.ebay.co.uk/itm/307196415985) |
| SanDisk Extreme Portable, older model | 1050 / 1000 MB/s | £183.99 | In stock | Marketplace | [Amazon](https://www.amazon.co.uk/dp/B0C59G53GS) |
| Kioxia EXCERIA PLUS G2 | 1050 / 1000 MB/s | £189.99 | Fewer than 10 | Independent | [eBuyer](https://www.ebuyer.com/kioxia-kioxia-exceria-plus-g2-portable-ssd-2tb-703355#colcode=70335503) |
| Netac ZSLIM | 550 / 480 MB/s | £168.99 | In stock | Marketplace | [Amazon](https://www.amazon.co.uk/dp/B08BHX922Y) |
| SureFire Pyrodrive | 460 / 440 MB/s | £134.14 | 1 left | Marketplace | [Amazon](https://www.amazon.co.uk/dp/B0DH812SNC) |

SureFire Pyrodrive also appeared on eBay at £156.78. That listing is the same brand and model, so the script kept one row and the lower in-stock Amazon offer.

## Near-tie

GiGimundo M20 and KingSpec MemoStone US5 tie on both soft preferences. Both are 2TB, both advertise 2000 MB/s read and 1800 MB/s write, and neither listing is sponsored.

The next band, 1050 / 1000 MB/s, is also a tie: Fikwot FP70, Crucial X9, SanDisk Extreme Portable, and Kioxia EXCERIA PLUS G2.

## Audit

The Fit auditor re-fetched the KingSpec row before the tie with GiGimundo was settled.

- Verdict: accept
- Source label: corroborated
- Fresh price: £169.99
- Fresh stock: 3 left
- Marketplace page: https://www.amazon.co.uk/dp/B0DP4PZ7HM
- Manufacturer page: https://www.kingspec.com/product/us5-ussd.html

SSD, 2TB, and USB-C agree on both pages. Price, stock, and UK delivery come from the Amazon page. The GiGimundo row has not been audited.

## Mac speed note

Those two headline speeds are USB 3.2 Gen 2×2 (20Gb/s). The M5 Air’s ports are Thunderbolt 4 / USB4. The GiGimundo listing says a USB4 or Thunderbolt 3/4 computer cannot reach 20Gb/s on this drive, only 10Gb/s. On this Air they should land near the 1050 / 1000 MB/s group, not at 2000 MB/s.

## Stop

A near-tie waits for you. The run does not check out. Open tie-breaks:

- Lowest price among the two 2000 MB/s listings (GiGimundo M20), then audit that row.
- Keep the KingSpec row already accepted.
- Speed this Mac can actually use, then a second rule you name (price, new-only, or a brand).
