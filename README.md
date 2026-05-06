# Contact Law Discovery via Symbolic Regression in Rotordynamics

This repository contains the companion code for the IFToMM Rotordynamics conference paper:

**Contact Law Discovery via Symbolic Regression in Rotordynamics**  
Vinicius S. Vianna, Tiago H. Machado, and Ilmar F. Santos

The notebook demonstrates how symbolic regression can recover an interpretable contact-force law for a rotor-bearing-impact system. The rotor motion is simulated with hydrodynamic bearing coefficients and a flat-barrier impact model. The normal force used by symbolic regression is then estimated from the equations of motion, using the rotor response as the measurement source.

## Repository Contents

- `Simb_regression_force_model_rorordynamics.ipynb`  
  Main notebook with the rotor simulation, contact-force estimation from the motion-equation residual, and PySR symbolic regression.

- `Artigo_IFTOM__Symbolic_Regression_to_Rotor_Contact_revised.pdf`  
  Conference paper describing the model, symbolic-regression setup, and results.

- `stiffness_coeficient.png`, `damping_coeficient.png`, `Histeresis Loop Hunt and Crossley Model.png`, `T_0_0015_*.png`  
  Figures used to document the bearing coefficients, contact response, and simulation behavior.

## Main Idea

The important point in the updated notebook is the separation between:

1. **Synthetic motion generation**  
   The rotor simulation uses the Hunt-Crossley contact law to generate a controlled impact response.

2. **Force identification from motion**  
   The symbolic-regression target is not recalculated from Hunt-Crossley. Instead, the notebook estimates `normal_force` from the vertical equation of motion:

   ```text
   normal_force = disk_mass * (acceleration_without_contact_terms - measured_vertical_acceleration)
   ```

3. **Symbolic regression**  
   PySR receives penetration, penetration rate, and approach velocity as inputs, and the motion-derived normal force as the target.

This keeps the identification step closer to the experimental situation: the force is inferred from measured motion and known dynamics, rather than being copied from the analytical law used to generate the synthetic data.

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

