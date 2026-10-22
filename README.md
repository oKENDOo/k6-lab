# K6 Lab

Performance testing lab using Grafana K6 and QuickPizza.

## Run tests

```bash
k6 run a1-smoke.js
k6 run --out web-dashboard=export=reports/a2-average.html a2-average.js
```

## Reports

HTML reports for the average-load, stress, spike, and soak tests are in the `reports/` folder.
