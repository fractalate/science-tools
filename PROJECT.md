# Science Tools

Science Tools is a static reference page for scientific constants, formulas, and calculators. It covers chemistry, spectroscopy, classical mechanics, electromagnetism, atomic physics, quantum physics, and trigonometry.

## Files and setup

- `conversions.html` contains the application, styles, and JavaScript.
- `allascii.txt` contains the NIST 2022 CODATA constants listing.
- `DISCIPLINES.md` defines subject scopes and content assignments.
- `README.md` provides the project entry point.

Open `conversions.html` directly in a browser. The application has no build step, framework, or backend. Calculators work offline. MathJax 4.0.0 loads from jsDelivr for TeX/SVG rendering and requires an internet connection.

## Content and filtering

The application contains 18 constants, 21 shared formula rows, a circular-motion panel, a trigonometric-identities panel, and three calculators. Formulas appear before constants and tools. SI prefixes and a precision note follow the tools.

Chemistry is selected initially. Multiple selected disciplines display the union of their content without duplicate rows. Show all selects every discipline. With no selection, reference content is hidden and a selection prompt appears. Filtering hides existing elements, preserving input values and unit choices.

Constants and formula rows carry `disciplines` arrays. Reference panels and calculators use `data-disciplines` attributes. Calculators also carry `data-tool`, which distinguishes them from formula panels when determining the tools empty state. Subject assignments are listed in [DISCIPLINES.md](DISCIPLINES.md).

## Constants and precision

The `constants` array provides SI values, significant-digit counts, available units, and optional standard uncertainties. The initial units are SI. Reset to SI resets all constant selectors; calculator inputs and output units retain their values.

Unit entries normally use `[label, multiplierFromSI]`. Independently tabulated values use `[label, null, value, uncertainty]`, including electron and proton masses in atomic mass units. Use their tabulated uncertainties rather than deriving them from separate kilogram and atomic-mass-constant uncertainties.

Exact derived constants use their definitions: ℏ = h/(2π), R = N_A k_B, and the Pauling energy-scale coefficient k = N_A × (1 eV). The Pauling coefficient is 96485.3321233100184 J/mol in ΔE = k(χ_A − χ_B)², with dimensionless electronegativity χ. The scale definition is exact; the bond-energy relation is empirical.

Measured constants include standard uncertainties. Standard gravity, displayed as Earth’s typical gravity, is the exact conventional reference 9.80665 m/s²; local gravitational acceleration varies.

`scientific()` displays a mantissa multiplied by a power of ten. An exponent of 1 is displayed as 0 with the mantissa adjusted accordingly. Main calculator outputs use 10 significant digits. Display precision does not specify measurement precision.

## Calculators

### Energy conversion

The energy converter uses 1 eV = 1.602176634 × 10⁻¹⁹ J. Its default input is 1 J. It accepts finite positive, zero, or negative energies and reports empty/nonfinite inputs and overflow.

### Light conversion

The light converter relates wavelength, frequency, and photon energy in a vacuum:

- f = c/λ and λ = c/f.
- E = hf = hc/λ per photon.
- E_mol = N_A hf per mole of photons.
- Energies in eV are energies in joules divided by the elementary charge expressed in coulombs.

It uses exact defining values c = 299792458 m/s, h = 6.62607015 × 10⁻³⁴ J s, and N_A = 6.02214076 × 10²³ mol⁻¹.

The default input is 550 nm, with wavelength output in nm, frequency output in THz, and energy output in J/photon. Inputs accept positive decimal or scientific-notation numbers; units are selected separately.

| Quantity | Available units |
| --- | --- |
| Wavelength | m, cm, mm, μm, nm, pm, fm, km |
| Frequency | Hz, kHz, MHz, GHz, THz, PHz, EHz |
| Energy | J/photon, eV/photon, J/mol photons, eV/mol photons |

Switching between wavelength and frequency converts a valid input to preserve the physical quantity, using nm or THz as the input unit. Changing an input unit reinterprets the entered number. Output unit changes affect the displayed results.

Empty, invalid, nonpositive, nonfinite, underflowing, or overflowing calculations produce an error. Supported ranges are limited by JavaScript floating-point arithmetic.

### Motion calculation

The motion calculator uses F = ma, p = mv, and K = ½mv². It assumes one-dimensional motion of a constant mass in an inertial frame, well below the speed of light. Velocity and acceleration are signed along the motion axis. Mass must be positive; all inputs and calculated outputs must be finite. Inputs use kg, m/s, and m/s².

## Spectrum and color conventions

`spectrum(nm)` uses these approximate wavelength bands:

| Band | Wavelength interval |
| --- | --- |
| Gamma rays | < 0.01 nm |
| X-rays | ≥ 0.01 nm and < 10 nm |
| Ultraviolet | ≥ 10 nm and < 380 nm |
| Visible light | 380–700 nm inclusive |
| Infrared | > 700 nm and < 1 mm |
| Microwaves | ≥ 1 mm and < 10 cm |
| Radio waves | ≥ 10 cm |

These boundaries are conventions. Microwaves also belong to the broader radio spectrum. Wavelengths are rounded to 12 significant digits before classification to limit floating-point boundary errors.

`visibleColor(nm)` uses a piecewise RGB approximation with edge-intensity fading and exponent 0.8. Color names divide the visible band at 450, 495, 570, 590, and 620 nm. The swatch represents an approximate screen color for a single wavelength, not a colorimetric measurement. Outside the visible band, the swatch is neutral and the page reports no visible color.

## Scientific assumptions

Each formula specifies its variables, units, and conditions. Most quantities use SI; spectroscopic wavenumber is expressed in cm⁻¹ when wavelength is in cm.

Bohr formulas assume a single-electron atom or ion, infinite nuclear mass, and nonrelativistic motion. Bohr orbits are model trajectories. Their angular-momentum rule differs from modern quantum orbital-angular-momentum quantization. Rydberg energy is the infinite-nuclear-mass scale hcR∞, rather than the precise ionization energy of real hydrogen.

Atomic emission uses integers n_i > n_f ≥ 1 so the emitted photon energy is positive. Absorption reverses the level difference. The Schrödinger equation describes nonrelativistic wavefunction evolution; the de Broglie row assumes a free particle with definite nonzero momentum.

Born–Landé gives molar ionic lattice formation energy with a negative binding convention. Dissociation reverses the sign. The model uses point charges and short-range repulsion proportional to r⁻ⁿ, with n > 1.

Classical motion formulas require nonrelativistic speeds. The mgh expression assumes constant gravity near Earth’s surface and a chosen zero height. Circular-motion formulas describe a fixed-radius circle in the xy-plane, with time-dependent radial and tangential unit vectors. Trigonometric identities apply wherever their expressions are defined; half-angle square-root signs depend on the quadrant.

## Layout and accessibility

At widths up to 740px, light controls and result cards stack, and reference tables scroll horizontally. The SI prefix chart has six columns on wide screens, four at widths up to 900px, and three at widths up to 540px. It lists all 24 prefixes from quetta to quecto, with case-sensitive symbols and powers of ten.

Clearing or invalidating the light input must preserve the result area's space. `.light-display` reserves the layout. On error, `#light-output` uses `visibility: hidden`, retains its content, becomes `inert`, and receives `aria-hidden="true"`. `#light-error` overlays that space. Recovery restores visibility and interactivity, removes `aria-hidden`, and hides the error. Valid outputs may have different heights when their text wraps differently.

Controls have labels and visible keyboard focus indicators. Results use live announcements. The light input references its error message through `aria-describedby`. Color names accompany swatches.

## Documentation

Documentation describes the current setup, behavior, scientific conventions, and content organization. Keep dated updates, session narratives, review results, and change histories out of project documentation.

## Sources

- [NIST constants](https://physics.nist.gov/cuu/Constants/) and [2022 CODATA listing](https://physics.nist.gov/cuu/Constants/Table/allascii.txt).
- [BIPM SI prefixes](https://www.bipm.org/en/measurement-units/si-prefixes).
- [NASA spectrum bands](https://imagine.gsfc.nasa.gov/science/toolbox/spectrum_chart.html) and [visible light](https://science.nasa.gov/ems/09_visiblelight/).
- [OpenStax mechanics](https://openstax.org/books/university-physics-volume-1/pages/index), [electric fields](https://openstax.org/books/university-physics-volume-2/pages/5-key-equations), and [electromagnetic waves](https://openstax.org/books/university-physics-volume-2/pages/16-2-plane-electromagnetic-waves).
- [MIT circular-motion notation](https://ocw.mit.edu/courses/8-01sc-classical-mechanics-fall-2016/pages/week-3-circular-motion/10-4-review-of-rotational-kinematics/).
- [Subject-specific references](DISCIPLINES.md#references) for bonding, atomic models, and quantum physics.
