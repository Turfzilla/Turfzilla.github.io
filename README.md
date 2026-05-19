# Turfzilla.github.io

Hello World!

Here’s a clean, recursive Python solution that counts gifts in “The Twelve Days of Christmas”—both:

1. Total gifts received over all 12 days (which should be 364), and
2. Per‑gift totals (e.g., how many “partridges”, “turtle doves”, etc., are received in total).

<details>
<summary>Show/Hide Python Code</summary>

```python
# The canonical list of gifts, indexed by day (1-based in the song; 0-based here)
GIFTS = [
    "partridge in a pear tree",      # day 1
    "turtle doves",                  # day 2
    "French hens",                   # day 3
    "calling birds",                 # day 4
    "gold rings",                    # day 5
    "geese a-laying",                # day 6
    "swans a-swimming",              # day 7
    "maids a-milking",               # day 8
    "drummers drumming",             # day 9  (note: commonly 'ladies dancing' is day 9 and 'lords leaping' day 10,
    "ladies dancing",                # day 10   'pipers piping' day 11, 'drummers drumming' day 12; ordering varies by version.
    "lords a-leaping",               # day 11   This list can be adjusted to your preferred lyrics; the math is unchanged.
    "pipers piping"                  # day 12
]

def triangular(n):
    """
    Recursively compute the triangular number T_n = 1 + 2 + ... + n.
    Base: T_0 = 0
    """
    if n <= 0:
        return 0
    return n + triangular(n - 1)

def gifts_received_on_day(n):
    """
    On day n, you receive gifts for days n, n-1, ..., 1, i.e., T_n gifts.
    """
    return triangular(n)

def total_gifts(days):
    """
    Recursively sum gifts received across all 'days' of Christmas.
    Total = Σ_{d=1..days} T_d
    """
    if days <= 0:
        return 0
    return gifts_received_on_day(days) + total_gifts(days - 1)

def per_gift_totals(days):
    """
    Recursively accumulate how many of each specific gift is received across all 'days'.
    On day d, gift k (1-based) appears if k <= d, contributing exactly 1 instance on that day.
    Thus, each gift k appears on days d = k..days -> (days - k + 1) times.
    We compute this recursively to illustrate the pattern.
    """
    def accumulate(d, counts):
        if d == 0:
            return counts
        # Add one instance for each gift up to day d (i.e., indices 0..d-1)
        for i in range(d):
            counts[GIFTS[i]] = counts.get(GIFTS[i], 0) + 1
        return accumulate(d - 1, counts)

    return accumulate(days, {})

# Example usage and a small demonstration
for d in range(1, 13):
    print(f"Day {d}: gifts today = {gifts_received_on_day(d)}")

grand_total = total_gifts(12)
print(f"\nTotal gifts over all 12 days: {grand_total}")

totals_by_gift = per_gift_totals(12)
print("\nPer-gift totals (across all 12 days):")
for gift, count in totals_by_gift.items():
    print(f"  {gift}: {count}")
```

</details>
