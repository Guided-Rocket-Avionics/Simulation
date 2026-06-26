# Design Decisions
Angle of attack: α = angle between the rocket centerline and the velocity vector
Fins produce a moment to correct this, here the aerodynamic force becomes perpedicular to the axis of the rocket.

<img width="771" height="216" alt="image" src="https://github.com/user-attachments/assets/03965f2f-c040-4831-889e-79e26c3dfae9" />

**The point on which the total forceacts is defined as the center of pressure or the rocket.**
Stability Margin = Distance between CP and CG (Calibers)
*1 Caliber = Max.body diameter of the rocket*
### Formulas

1. **Normal Force Coefficient**

$$
C_N = \frac{N}{\frac{1}{2}\rho v_0^2 A_{ref}}
$$

2. **Pitch Moment Coefficient**

$$
C_m = \frac{m}{\frac{1}{2}\rho v_0^2 A_{ref} d}
$$

3. **CP Location (Barrowman)**

$$
X = \frac{C_{m_\alpha}}{C_{N_\alpha}} d
$$
                        
