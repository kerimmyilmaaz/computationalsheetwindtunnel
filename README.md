# computationalsheetwindtunnel
# Wind Tunnel 1D Aerodynamic Sizing & Power Estimation

This repository contains a comprehensive 1D computational MATLAB script developed for the **ASTRAL Project**. The script performs aerodynamic sizing, pressure loss distribution, and fan power estimation for an open-circuit low-speed wind tunnel prior to the integration of icing spray nozzles.

The mathematical model strictly adheres to the empirical formulas and methodologies outlined in **"Low-Speed Wind Tunnel Testing" by Barlow, Rae, & Pope (1999)**.

## 🚀 Features
* **Component-wise Pressure Loss Calculation:** Evaluates individual losses for the test section, nozzle, diffuser, settling chamber, exit dump, and model drag.
* **Compressibility Check:** Dynamic assessment of the Mach number to ensure the validity of incompressible flow assumptions.
* **Sensitivity Analysis:** Generates flow and power sensitivity tables across a range of target velocities (5 m/s to 60 m/s).
* **Automated Plotting:** Outputs a 6-panel performance dashboard (Flow Rate, Dynamic Pressure, Reynolds Number, Nozzle Geometry, Stacked Pressure Loss, and Required Fan Power).

## 📚 Aerodynamic Loss Models & Equations

The script calculates the total pressure drop ($\Delta p_{total}$) by summing the non-dimensional loss coefficients ($K$) of each component, referenced to the dynamic pressure at the test section ($q_{test}$).

### 1. Test Section Loss ($K_{test}$)
Friction losses in the test section are determined using the **Prandtl Universal Friction Law** for smooth pipes to iteratively solve for the Darcy friction factor ($f$):
* $\frac{1}{\sqrt{f}} = 2 \log_{10}(Re \sqrt{f}) - 0.8$
* $K_{test} = f \frac{L_{test}}{D_{h, out}}$

### 2. Nozzle / Contraction Loss ($K_{nozzle}$)
The contraction cone loss is estimated using **Wattendorf's empirical formula**, which accounts for the average friction factor across the contracting geometry:
* $K_{nozzle} = 0.32 \times f_{avg} \frac{L_{nozzle}}{D_{h, out}}$

### 3. Diffuser Loss ($K_{diffuser}$)
Diffuser losses are split into wall friction ($K_f$) and expansion losses ($K_e$), adapting **Barlow Eq. 3.24 - 3.29**. The equivalent cone angle ($\theta_e$) is derived from the area ratio ($AR$) and length.
* $K_f = \left(1 - \frac{1}{AR^2}\right) \frac{f_{diff}}{8 \sin(\theta_e)}$
* $K_{ex} = K_e \left(\frac{AR - 1}{AR}\right)^2$ (where $K_e$ is derived from Barlow's empirical polynomials based on $\theta_e$)
* $K_{diffuser} = K_f + K_{ex}$

### 4. Settling Chamber Loss ($K_{settling}$)
Accounts for the pressure drop across the honeycomb flow straighteners and mesh screens, referenced back to the test section via the Contraction Ratio ($CR$). Uses **Barlow Eq. 3.36 - 3.43**:
* **Honeycomb:** $K_h = \lambda_h \left(\frac{L_h}{D_h} + 3\right) \left(\frac{1}{\beta_h}\right)^2 + \left(\frac{1}{\beta_h} - 1\right)^2$
* **Screens:** $K_{screen} = K_{mesh} K_{Rn} \sigma_s + \frac{\sigma_s^2}{\beta_s^2}$ 
* $K_{settling} = \frac{K_h + N_{screens} \times K_{screen}}{CR^2}$

### 5. Exit Dump Loss ($K_{exit}$)
Models the kinetic energy lost when the flow exits the diffuser into the ambient environment (Barlow Section 3.3):
* $K_{exit} = \frac{1}{AR_{diffuser}^2}$

### 6. Fan Power Estimation
The total mechanical power required by the primary fan is calculated by combining all losses and factoring in the fan's mechanical efficiency ($\eta_{fan}$):
* $\Delta p_{total} = (K_{nozzle} + K_{test} + K_{diffuser} + K_{settling} + K_{exit} + K_{model}) \times q_{test}$
* $P_{fan} = \frac{\Delta p_{total} \times Q}{\eta_{fan}}$ (Watts)

## 🛠️ How to Run
1. Open `ruzgar_tuneli_hesap.m` in MATLAB.
2. Run the script. No additional toolboxes are required.
3. When prompted in the Command Window, enter your target test section velocity (m/s), ambient temperature (°C), and ambient pressure (Pa).
4. View the generated tabular data in the Command Window and the 6-panel performance figure.
