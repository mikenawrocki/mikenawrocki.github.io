+++
date = '2025-02-24T18:09:27-05:00'
title = 'Tipping Plates'
[params]
  math = true
+++

If you've been in the gym long enough, you've probably seen it happen (maybe even to you) -
the set is over, the plates are being unloaded from the barbell, and suddenly the
remainder go flying. It's easy enough to avoid, but I wanted a way to determine exactly how
unbalanced the plate count on either side of the barbell can be before catastrophy strikes.

It's not a particularly difficult physics problem, but I didn't see previous solutions so mine
follows.

## Model
View the barbell as a lever and the J-cups as two pivots. A number of weight plates are loaded on
either side of the barbell, and of course the barbell itself has mass.

For tipping to occur, the torque applied by the plates on one side of the barbell must exceed the
torque applied by the plates on the other side. This allows a rotation and, as plates slide down
the barbell sleeve, the unbalanced torque will grow still more.

For convenience, assume that the left side has fewer plates than the right side and tipping will
occur towards the right if the torque is unbalanced.

Knowing this, we need to find the torque values on each side of the pivot and balance them.

## Barbell Torque
Because the pivots are not at the center of mass of the barbell, the torque applied on each side
from it will factor into the final computation.

Assume that the barbell has uniform density across its length. I can't easily determine linear
density for the sleeves (obviously greater than the bar itself) so this approximation will need to
do. Let this linear density be \(\rho_b=\frac{m_b}{l}\) where \(m_b\) is the mass of the barbell
(typicall 20kg) and \(l\) is the length of the bar. Let \(d\) be the distance from the pivot to
the end of the barbell for whichever side of the pivot torque is being determined.

Since torque \(\tau\) is force times distance, we need to determine the force and the distance.
The force is gravity only, \(G\), and the distance from the pivot ranges from zero until the
end of the barbell (up to \(d\)),
so a definite integral can be used to compute the total torque value:

$$
\tau = \int_{0}^{d} G\rho_bxdx
$$

This simplifies to:

$$
\tau = \frac{G\rho_b}{2}d^2
$$

Let the torque on the left be

$$
\tau_l = \frac{G\rho_b}{2}d_l^2
$$

and the torque on the right be

$$
\tau_r = \frac{G\rho_b}{2}d_r^2
$$

where \(d_l\) is the length of the barbell to the left of the pivot and \(d_r\) to the right.

This will feed into the total torque calculations.

## Plate Torque
Plate torque is similar to barbell torque, but because the number of plates \(\in \mathbb{N}\),
a summation works best for calculating plate torque. We use the variables from above and
introduce a few new ones: let \(w\) be the width of a weight plate, \(n\) be the number of
plates on the side being calculated, \(m_p\) be the mass of the plate,
and \(d_p\) be the distance from the pivot to the edge of the first plate.
Assume the center of mass is at the midpoint of the plate. Then:

$$
\tau = \sum_{i=1}^{n} Gm_p(d_p+w\frac{2i-1}{2})
$$

This simplifies to:

$$
\tau = \frac{1}{2}Gm_pn(2d_p+nw)
$$

Let the torque on the left be

$$
\tau_l = \frac{1}{2}Gm_pn_l(2d_{pl}+n_lw)
$$

and the torque on the right be

$$
\tau_r = \frac{1}{2}Gm_pn_r(2d_{pr}+n_rw)
$$

where \(d_{pl}\) is the distance from the pivot to the first plate on the left,
\(d_{pr}\) is the distance from the pivot to the first plate on the right,
\(n_l\) is the number of plates on the left, and \(n_r\) is the number of plates
on the right.

## Tipping Inequality
We can frame tipping as an inequality with the torques, again remembering that the left side has
fewer plates than the right and tipping will proceed towards the right (if it does). In other
words, when \(\tau_l < \tau_r\). Combining the above calculations, we begin with:

$$
\frac{G\rho_b}{2}d_l^2 + \frac{1}{2}Gm_pn_l(2d_{pl}+n_lw) <  \frac{G\rho_b}{2}d_r^2 + \frac{1}{2}Gm_pn_r(2d_{pr}+n_rw)
$$

This can simplify a little to:

$$
\rho_b(d_l^2 - d_r^2) + m_p(n_l(2d_{pl}+n_lw) - n_r(2d_{pr}+n_rw)) < 0
$$

## Worked Example
I've taken measurements from my home setup to plug in here, which are as follows:
* Barbell mass \(m\): 20kg
* Barbell length \(l\): 2.2 meters
* Barbell linear density \(\rho_b = \frac{m}{l} \approx 9.1 \frac{\text{kg}}{m}\)
* Barbell length to left of pivot \(d_l\): 1.65 meters
* Barbell length to right of pivot \(d_r\): 0.55 meters 
* Distance from pivot to first plate on left \(d_{pl}\): 1.24 meters
* Distance from pivot to first plate on right \(d_{pr}\): 0.14 meters
* Plate mass \(m_p\): 20.4kg
* Width of plate \(w\): 0.04 meters

For this example, assume one plate is on the left, so \(n_l = 1\). How many plates can it
balance (i.e., what is \(n_r\))? Well, plugging it in:

$$
9.1 (1.65^2 - 0.55^2) + 20.4(1(2\cdot 1.24+0.04) - n_r(2\cdot 0.14 +n_r\cdot 0.04)) < 0
$$

which simplifies to:

$$
-0.186n_r^2 - 5.712n_r + 73.43 < 0
$$

Solving for the roots, we get \(n_r \approx 6.6\), so 6 plates should be safe.

Empirically, this is correct: in my configuration, one plate balances 6!
Just be certain the barbell is centered.
