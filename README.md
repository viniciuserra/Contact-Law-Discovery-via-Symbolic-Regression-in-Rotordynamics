# Contact Law Discovery via Symbolic Regression in Rotordynamics

This repository contains the companion code for the IFToMM Rotordynamics conference paper:

**Contact Law Discovery via Symbolic Regression in Rotordynamics**  
Vinicius S. Vianna, Tiago H. Machado, and Ilmar F. Santos

The notebook demonstrates how symbolic regression can recover an interpretable contact-force law for a rotor-bearing-impact system. The rotor motion is simulated with hydrodynamic bearing coefficients and a flat-barrier impact model. The normal force used by symbolic regression is recovered directly from the movement equations, using the rotor response as the measurement source.

## Repository Contents

- `Simb_regression_force_model_rorordynamics.ipynb`  
  Main notebook with the rotor simulation, contact-force reconstruction from Eq. (18) of the paper, and PySR symbolic regression.

- `Artigo_IFTOM__Symbolic_Regression_to_Rotor_Contact_revised.pdf`  
  Conference paper describing the model, symbolic-regression setup, and results.

- `stiffness_coeficient.png`, `damping_coeficient.png`, `Histeresis Loop Hunt and Crossley Model.png`, `T_0_0015_*.png`  
  Figures used to document the bearing coefficients, contact response, and simulation behavior.

## Main Idea

The important point in the updated notebook is the separation between:

1. **Synthetic motion generation**  
   The rotor simulation uses the Hunt-Crossley contact law to generate a controlled impact response.

2. **Force identification from the movement equations**  
   The symbolic-regression target is not recalculated from Hunt-Crossley. Instead, the notebook explicitly rearranges Eq. (18) of the paper:

   ```text
   F_N = m*e*(-theta_ddot*cos(theta) + theta_dot**2*sin(theta)) - m*g_eff - m*uz_ddot - c*uz_dot - k*(uz - z_b)
   ```

   In the notebook, `g_eff = 0` because the bearing coefficients are built from the static load and the simulated displacement is treated as the dynamic motion around that loaded state. For absolute laboratory coordinates, set `g_eff = g`.

   Eq. (17) is also evaluated as a horizontal/friction consistency check:

   ```text
   F_N,y = (m*uy_ddot + c*uy_dot + k*(uy - y_b) - m*e*(theta_ddot*sin(theta) + theta_dot**2*cos(theta))) / mu
   ```

3. **Symbolic regression**  
   PySR receives penetration, penetration rate, and approach velocity as inputs, and the Eq. (18) normal force as the target.

This keeps the identification step closer to the experimental situation: the force is inferred from measured motion and known dynamics, rather than being copied from the analytical law used to generate the synthetic data or from a comparison with a no-contact simulation.

## Requirements

The notebook was developed with Python and the scientific stack:

- `numpy`
- `pandas`
- `matplotlib`
- `scipy`
- `sympy`
- `scikit-learn`
- `pysr`

PySR also requires a working Julia installation. See the PySR documentation for the Julia setup details.

## How to Run

1. Open `Simb_regression_force_model_rorordynamics.ipynb`.
2. Run the rotor and contact simulation cells.
3. Run the section **Contact force from the motion equations** to build `contact_df`.
4. Run the symbolic-regression cells for clean and noisy measurement cases.

The PySR cells may take a long time because the search is configured for publication-quality runs. For a quick test, reduce `niterations` in `make_pysr_model(...)`.

## Citation

If you use this repository, please cite the conference paper included in this repository.
