# EM3d

This is a package that input your complex permittivity and permeability to generate plots of reflection loss as functions of thickness and frequency.
Steps to prepare your data and to use this package:

### Prepare your data

Arrange your raw data in the following sequence: frequency (either GHz or Hz), real part of complex permittivity (ε'), imaginary part of complex permittivity (ε"), real part of complex permeability (μ'), and imaginary part of complex permeability (μ").

### How to use this package

1.  Calculate the variations of tan δ~m~ (magnetic loss tangent), tan δ~e~ (dielectric loss tangent), and Reflection loss (RL) as functions of thickness and frequency using the function `calcultion_EM_data`.
    The data are defaulted calculated using thickness of between 0.01 (`d1`) and 10 mm (`d2`) with a `step` of 0.1 mm.

    $$
    tan\delta_e=\epsilon'/\epsilon"
    $$

    $$
    tan\delta_m=\mu'/\mu"
    $$

    $$
    RL=20log|\frac{Z-Z_0}{Z+Z_0}|
    $$

    $$
    Z_0=\sqrt{\mu_0/\epsilon_0}
    $$

    $$
    Z=Z_0\sqrt{\frac{\mu}{\epsilon}}tanh(i\frac{2\pi fd}{c})\sqrt{\mu \epsilon}
    $$

    where *f* is frequency (Hz), *d* is thickness (m), *c* is the light speed.

2.  You can use save the generated data frame for subsequently visualization.

3.  To calculate the bandwidth of RL \< -10 dB at each thickness, using the function `EM_bandwidth`.
    The default RL is -10 dB, however, you could assign the RL to any value.

4.  To calculate the maximum RL at each thickness, using the function `EM_maxRL`.
