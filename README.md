# Basketball Free Throw – Speed vs Angle Maps

This project explores basketball shot trajectories (no air resistance) and visualizes relationships between launch speed and angle under official dimensions. It also computes optimal (minimal speed) angles for various heights and distances.

## Prompt for GPT5
Prompt:

Write a Python script that visualizes basketball shooting trajectories with the following requirements:

Assume a person of height xx meters is standing yy meters away from the hoop.
The person throws a basketball at an angle zz (degrees) and with an initial speed vv (m/s).
Both the basketball and the hoop have standard official radii:
Basketball radius: 0.12 m
Hoop radius: 0.23 m
The hoop height: 3.05 m (standard)
The script should plot a figure where:
The x-axis is the initial speed (vv, m/s)
The y-axis is the launch angle (zz, degrees)
Draw three curves:
The combinations of speed and angle that allow the ball's center to just pass through the front edge of the hoop.
The combinations for the center of the hoop.
The combinations for the back edge of the hoop.
Use matplotlib for plotting. Assume no air resistance.
The script should let me change the thrower's height (xx) and the distance to the hoop (yy) easily at the top of the script.
Please provide full code with comments.

## Contents

- `main.ipynb` – Interactive notebook that:
  - Plots speed–angle curves for three hoop alignments of the ball center:
    - Front edge plane
    - Rim center plane
    - Back edge plane
  - Shows, for a fixed height and multiple distances, the minimal-speed angle and overlays the optimal points on one figure
  - Generates a table of optimal angle/speed for multiple heights and distances

## Assumptions and constants

- 2D vertical plane, no lateral offset
- No air resistance and no spin effects
- Gravity: g = 9.81 m/s²
- Standard sizes:
  - Ball radius: 0.12 m
  - Hoop inner radius: 0.23 m
  - Rim height: 3.05 m

The ball center must pass a vertical plane located at the rim front/center/back, adjusted horizontally to account for ball radius at front/back clearance.

## How it works (physics)

For launch point at height `xx` and a horizontal target plane at `x_target`, the trajectory without drag is:

- y(x) = xx + x tanθ − g x² / (2 v² cos²θ)

To pass exactly through hoop height H_rim at x = x_target, the minimal speed for angle θ is derived from:

- v² = g x_target² / [2 cos²θ (xx + x_target tanθ − H_rim)]

Feasibility requires the denominator to be positive; otherwise, no solution exists for that θ.

<img width="790" height="590" alt="output" src="https://github.com/user-attachments/assets/12377fde-82f8-4846-8c4b-2923a1a062c6" />

Release height xx = 1.9 m, Distance yy = 4.6 m
Targets x (m): front=4.49, center=4.60, back=4.71

## Using the notebook

Open `main.ipynb` and run cells in order. The key editable parameters appear at the top of relevant code cells:

1) Speed–angle curves (front/center/back)
- Edit:
  - `xx`: release height (m)
  - `yy`: horizontal distance to rim center (m)
  - `angles_deg`: (optional) angle sweep range (deg)
- Output: A figure with three curves (x = speed, y = angle) corresponding to front/center/back planes.

2) Same height, multiple distances (one figure)
- Edit:
  - `xx`: release height (m)
  - `yy_values`: list of distances (m)
  - `alignment`: 'front' | 'center' | 'back'
  - `angles_deg`: (optional) angle sweep range (deg)
- Output: Curves for each distance with annotated minimal-speed points.

3) Optimal table: multiple heights and distances
- Edit:
  - `xx_values`: list of heights (m)
  - `yy_values`: list of distances (m)
  - `alignment`: 'front' | 'center' | 'back'
  - `angles_deg`: (optional) angle sweep range (deg)
- Output: An ASCII table and a Python list `optimal_table` with dict rows.

4) Fixed height, optimal vs distance
- Edit:
  - `xx`: fixed release height (m)
  - `yy_values`: distances to scan (m)
  - `alignment`: 'front' | 'center' | 'back'
  - `angles_deg`: (optional) angle sweep range (deg)
- Output: Two subplots (v_min vs distance, θ_opt vs distance) and a parametric scatter (v_min vs θ_opt) colored by distance.

## Environment

The notebook uses numpy and matplotlib. Most Python environments already include them, but if needed, install via pip:

```
pip install numpy matplotlib
```

## Tips

- Angle range: If you don’t see feasible solutions, widen or shift `angles_deg`.
- Distances: Ensure the computed target plane is in front of the shooter. For front/back planes, this means `yy − R_rim + R_ball > 0` (front) and `yy + R_rim − R_ball > 0` (back).
- Real-world shots: Air drag and spin are neglected, so actual required speeds may be higher.

## License

This project is provided for educational purposes. Use at your own discretion.
