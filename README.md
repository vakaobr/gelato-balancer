# gelato-balancer

Balanceador de Gelato & Sorbet. A single-file web app that builds and live-balances
gelato, American ice cream, sorbet, and frozen yogurt recipes, then prints a full
prep sheet for a home compressor machine (tuned for the H.Koenig HF340).

Live: https://gelato.andersonleite.me/

## What it does

- Pick a **base** (gelato, ice cream, sorbet, frozen yogurt), a **flavor**, a **diet**,
  a batch size, and a freezer temperature.
- The recipe is balanced live against literature targets, with gauges for fat, protein,
  total solids, sweetening power (POD), and anti-freezing power (PAC).
- **Auto-balance** drives the formula to the targets at any batch size by adjusting
  sugar, cream, milk powder, maltodextrin, and filler.
- Optional **Funcional** mode adds hydrolyzed whey for a protein boost.
- Optional **Estabilizante (neutro)** toggle adds a neutral stabilizer to dairy bases
  (sorbet uses xanthan instead).
- Diets: normal, lactose-free, sugar-free (erythritol + inulin), and low-calorie.
- A nutrition table per 100 g of mix, per 100 mL of finished product (overrun aware),
  and per serving, plus a copy-to-clipboard prep sheet.

## The balance model

Targets and ingredient coefficients follow the Italian gelato framework
(Corvitto, Caviezel), with sucrose as the reference for both metrics:

| Metric | Meaning |
|--------|---------|
| Gor | Fat (gordura) |
| PRT | Protein (proteína) |
| ST  | Total solids (sólidos totais) |
| POD | Sweetening power, sucrose = 100 |
| PAC | Anti-freezing power, sucrose = 100 |

Targets are expressed per 1000 g of mix and scaled to the chosen batch size, so the
gauges and the auto-balancer stay correct at any total amount.

The pistache gelato uses a tested, validated recipe (flagged "validado"). Every other
combination starts from literature ranges and should be checked against the fat and
solids of your own ingredients (flagged "verificar"). Ingredient nutrition values come
from USDA and literature; fruit varies with brand and ripeness.

## Stack

- Plain HTML, CSS, and vanilla JavaScript in one file ([index.html](index.html)).
  No build step, no dependencies, no backend, no external resources.
- Deployed as static assets on Cloudflare Workers.

## Local use

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

## Deploy

Static assets via Cloudflare. Config is in [wrangler.toml](wrangler.toml):

```bash
npx wrangler deploy
```

Security and caching response headers are set in [_headers](_headers). The
[.assetsignore](.assetsignore) file keeps the repo, config, license, and docs out of
the served assets.

## License

MIT. See [LICENSE](LICENSE).
