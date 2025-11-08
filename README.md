# R&D AI — Parametric Curve Parameter Estimation

## Problem
Estimate unknown parameters θ, M, X of the parametric curve:
x(t) = t*cos(θ) - exp(M*|t|)*sin(0.3*t)*sin(θ) + X
y(t) = 42 + t*sin(θ) + exp(M*|t|)*sin(0.3*t)*cos(θ)
for 6 ≤ t ≤ 60 using provided xy_data.csv.

## Method
1. Load and clean data (remove NaNs, duplicates, outliers via MAD).
2. Define the model function `model_xy(t, θ, M, X)`.
3. Use alternating projection – optimization:
   - Project each observed (x,y) to the nearest point on a finely-sampled model curve to estimate t_i.
   - Compute X analytically as `mean(x_obs - x_model_without_X)`.
   - Optimize θ and M via `scipy.optimize.least_squares` with `loss='soft_l1'`.
   - Repeat until convergence.
4. Use coarse grid + multi-start for robust initialization.
5. Evaluate final fit by sampling predicted curve and computing L1 distance (sum of distances from predicted samples to nearest observed points).

## Results
- θ (deg): 31.062545
- θ (rad): 0.542144
- M: 0.050000
- X: 24.347685
- L1 distance: 25979.0026

## Desmos
x(t) = t*cos(0.542144) - e^(0.050000*abs(t))*sin(0.3*t)*sin(0.542144) + 24.347685  
y(t) = 42 + t*sin(0.542144) + e^(0.050000*abs(t))*sin(0.3*t)*cos(0.542144)  
Domain: 6 ≤ t ≤ 60

Desmos link: https://www.desmos.com/calculator?x1(t)=t*cos(0.542144)-e^(0.050000*abs(t))*sin(0.3*t)*sin(0.542144)+24.347685&y1(t)=42+t*sin(0.542144)+e^(0.050000*abs(t))*sin(0.3*t)*cos(0.542144)

## Files to submit
- `xy_data.csv` (input)
- `flam.ipynb` / `main.ipynb` (notebook with code + printed diagnostics)
- `rd_ai_results.csv` (final parameter values)
- `README.md` (this file)
- Screenshot(s) of final plot or Desmos view

## Notes & possible improvements
- M hit the boundary (0.05). Consider expanding the search range if allowed to see whether the fit improves.
- Increasing t-grid density (more samples) may reduce projection error.
- Multi-start with more diverse seeds helps avoid local minima.
<img width="950" height="703" alt="flam" src="https://github.com/user-attachments/assets/73123eee-440f-4f3c-8d01-34c270217276" />
