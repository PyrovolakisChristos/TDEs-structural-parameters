# TDEs-structural-parameters

This repository contains only the primary TDE structural-parameter data files.

## Data files

- `r_band.csv` — structural parameters measured in the optical red band (SDSS `r` filter)
- `g_band.csv` — structural parameters measured in the optical green band (SDSS `g` filter)
- `Excluded_TDEs.csv` — objects excluded from the dataset and analysis, with reasons for exclusion when available.

## Parameter definitions and units

The structural parameters provided in the `r_band.csv` and `g_band.csv` files are derived from the GALFIT modelling of the Pan-STARRS host-galaxy images.

The main catalogue quantities are:

- `Re` — effective radius of the Sérsic component, as returned by GALFIT, in pixels.
- `Rs` — scale radius of the exponential component, when present, in pixels.
- `n` — Sérsic index of the main Sérsic component.
- `qbulge` and `qdisk` — axis ratios (`q = b/a`) of the corresponding fitted components.
- `ibulge` and `idisk` — inclinations derived from the corresponding axis ratios.
- `Magnitude` — apparent magnitude returned by GALFIT.
- `z` — spectroscopic redshift of the host galaxy.
- `Class` — optical spectral classification of the TDE.

### Inclination

The inclination values were calculated from the GALFIT axis ratio `q = b/a` as

\[
i = \arccos(q),
\]

and are reported in degrees. With this convention, `i = 0°` corresponds to a face-on orientation and `i = 90°` to an edge-on orientation.

### Conversion of radii to angular and physical units

GALFIT returns the fitted radii in pixels. For the Pan-STARRS images used in this work, we adopt a pixel scale of

\[
0.25\ {\rm arcsec\ pixel^{-1}}.
\]

Therefore, a fitted radius can be converted from pixels to arcseconds using

\[
R_{\rm arcsec} = R_{\rm pix} \times 0.25.
\]

To convert the angular radius to a physical radius in kpc, we use the angular-diameter distance at the redshift of each host. We adopt a flat ΛCDM cosmology with

\[
H_0 = 70\ {\rm km\ s^{-1}\ Mpc^{-1}}, \qquad
\Omega_m = 0.3.
\]

For an angular-diameter distance \(D_A\) in Mpc,

\[
{\rm kpc\ per\ arcsec}
=
\frac{D_A \times 1000}{206265},
\]

and therefore

\[
R_{\rm kpc}
=
R_{\rm pix}
\times 0.25
\times
\frac{D_A({\rm Mpc})\times1000}{206265}.
\]

The same conversion can be applied to both `Re` and `Rs`.

An equivalent Python implementation using `astropy` is:

```python
from astropy.cosmology import FlatLambdaCDM

pixel_scale = 0.25  # arcsec/pixel
cosmo = FlatLambdaCDM(H0=70, Om0=0.3)

Re_arcsec = Re * pixel_scale
DA_Mpc = cosmo.angular_diameter_distance(z).value
kpc_per_arcsec = (DA_Mpc * 1000) / 206265
Re_kpc = Re_arcsec * kpc_per_arcsec
