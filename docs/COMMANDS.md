# Command Reference

All commands start with `/tx`. Only admins (GUID in `admins.txt`) can run most commands.  
VIP commands require the GUID to be in `vips.txt`.

---

## Grid Format

Two formats are accepted:

- **6 digits:** `XXXZZZ` → X = XXX × 100m, Z = ZZZ × 100m  
  Example: `038088` → X 3800, Z 8800
- **8 digits:** `XXXXZZZZ` → X = XXXX × 10m, Z = ZZZZ × 10m  
  Example: `03800880` → X 3800, Z 8800

Without a grid, missions spawn at the player's position.

---

## Admin Commands

### Kill Missions

```text
/tx kill 3 infected 038088
/tx kill 5 military 082004
/tx kill 2 policia 050105
