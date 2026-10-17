## Update: 2026-10-17

Added Trigonometry with sine o/h, cosine a/h, and tangent sin/cos in the main formula table. Its separate MathJax identities panel has 8 grouped rows: sine/cosine/tangent sum and difference, double-angle, power reduction, half-angle, product-to-sum, and sum-to-product identities. Removed identity groups are archived in `REMOVED.md`. Domain restrictions and half-angle sign choices are stated. Uses the existing union filter and blank slate; the identity panel is not counted as a tool. No constants or calculators were added for trigonometry.

## Update: 2026-10-12

A factual review of all constants, formulas, units, and converters is recorded in `FACT_CHECK.md`. Numerical data matched live NIST CODATA. Applicability and wording corrections clarify impedance E/H, RMS average power, one-dimensional motion, fixed inertial axes, base-10 absorbance, and conventional exact values.

The application is `conversions.html` (older notes below refer to `index.html`). It is branded Science Tools and supports chemistry, spectroscopy, classical mechanics, electromagnetism, and atomic physics. Atomic physics adds Bohr radius, Rydberg constant, reduced Planck constant, fine-structure constant, and Bohr/transition formulas with single-electron and infinite-nuclear-mass assumptions. The Bohr row shows energy levels only; emission wavelength follows the transition row, now written E_gamma = E_Ry Z²(1/n_f² − 1/n_i²), with emission ordering n_i > n_f and the hydrogen-like Bohr assumptions stated. E_Ry is explicitly identified with the Rydberg energy constant and hcR∞. Spectroscopy’s Beer–Lambert and molar-photon-energy formulas and the consolidated standalone atomic rows are archived in `REMOVED.md`. Atomic energy/transition rows also share spectroscopy tags. Chemistry is selected initially. Multiple selected disciplines show the union of their tagged constants, tools, and formulas; no selection hides the reference content and shows a brief selection prompt. Show all checks every discipline and displays everything. Item tags are editorial relevance assignments. Filtering hides existing elements, retaining input and unit choices. Electromagnetism now shows only Coulomb’s law and electromagnetic wave speed, with vacuum permittivity, vacuum permeability, elementary charge, and speed of light. Unshared removed material is preserved in `REMOVED.md`. The wavelength/frequency light converter belongs to both spectroscopy and atomic physics. The precision note is visible with any selected discipline at the bottom, after SI prefixes and before the footer.

There are 16 constants and 18 compact formula table rows, displayed before the constants, rendered using MathJax TeX/SVG with inline mathematical variables, units, and applicability conditions. New constants: G (with uncertainty), standard gravity (exact conventional reference), Boltzmann constant, and molar gas constant. Chemistry shows only ideal gas law, molar gas constant, and Avogadro constant. Avogadro constant is tagged only for chemistry; the spectroscopy converter still uses it internally for per-mole outputs. Photon energy comes first in the formulas; constants begin with speed of light, Planck constant, elementary charge, electron mass, and proton mass. The amount-of-substance and thermal-translational-energy formulas were removed and archived in `REMOVED.md`. Other formerly shared chemistry items retain their other discipline tags. Boltzmann constant is removed from the table and preserved in `REMOVED.md`; the molar gas constant retains its defining numerical calculation. Classical mechanics is trimmed to spring force, kinetic energy, momentum p = mv = dK/dv (constant mass, one dimension), directly below kinetic energy, gravitational potential energy U = mgh, and universal gravitation. A separate circular-motion formula panel appears below the formula table, before constants, with MIT OCW 8.01SC lesson 10.4 vector notation (r-hat, theta-hat, k-hat; omega_z and alpha_z). It includes seven rows for position, angular velocity, velocity, angular acceleration, acceleration, and radial/tangential components. It is filtered to mechanics and excluded from tool-presence counting using `data-tool`. Its constants are G and Earth’s typical gravity, labeled g with no subscript; g uses the exact standard reference value 9.80665 m/s², with local variation noted. Values follow the local 2022 CODATA listing. The mechanics tool computes force, signed momentum, and kinetic energy from SI inputs, requiring positive mass and finite values. Standard gravity is explicitly distinguished from local gravity.

The shared formatter displays exponent 1 as exponent 0, retaining significant digits. Existing light converter validation and reserved error layout remain intact. Full inline script execution using a DOM mock checked discipline unions, show-all, mechanics values/invalid input, light error/recovery, and formatting. No real browser visual verification was available.

---

# Science Tools project notes

Updated: 2026-10-11.

## Purpose and setup

A static chemistry reference page with selectable units and energy/light converters. The user requested a well laid out HTML page, scientific notation, and SI defaults for the constants. Subsequent requests added a local source reference, light conversion, stable error layout, and photon/molar energy.

- `index.html`: complete application; HTML, CSS, and vanilla JavaScript are inline.
- `allascii.txt`: unmodified downloaded NIST complete 2022 CODATA listing (40,801 bytes). Keep this in the repository as the user's requested reference.
- `PROJECT.md`: this handoff document.

No framework, build step, or backend is needed. MathJax 4.0.0 loads from jsDelivr for TeX/SVG formula typesetting; calculators work offline, but polished math rendering requires connectivity. Open `index.html` directly in a browser. External reference links require connectivity. No hosted deployment was created. No commits have been made during this session; the application and reference file were untracked when these notes were written. Check current Git status rather than assuming this remains true.

## Current features

### Constants

Six rows: Avogadro constant, Planck constant, elementary charge, electron mass, proton mass, and speed of light. Each has its own unit selector. The reset button resets only these six selectors to SI, not the converters.

The `constants` array supplies SI values, significant-digit counts, unit factors, and optional standard uncertainties. Unit entries normally use `[label, multiplierFromSI]`; mass values in atomic mass units use independently tabulated values and uncertainties: `[label, null, value, uncertainty]`. Do not derive their uncertainties by simply dividing the kilogram uncertainty by an atomic mass constant; the tabulated quantities have different uncertainties.

Electron mass: 9.1093837139e-31 kg, standard uncertainty 2.8e-40 kg.
Proton mass: 1.67262192595e-27 kg, standard uncertainty 5.2e-37 kg.
The other four constants are exact SI definitions; some converted displays are rounded.

### SI prefix chart

A compact static chart sits at the bottom of the page, after the light converter and before the footer (moved there at the user’s request because it took up too much space above the converters). It lists all 24 SI prefixes, quetta (10^30) through quecto (10^-30), with names, case-sensitive symbols, and factors. Includes nm/THz examples and the unprefixed 10^0 factor. Source: https://www.bipm.org/en/measurement-units/si-prefixes (checked when added).

Uses a semantic description list (`.prefix-grid`) with six columns on wide screens, four at ≤ 900px, and three at ≤ 540px. It is reference-only; the converter unit selections are unchanged. Prefix presence/count, placement before the footer and after the light converter, and inline-script syntax were checked; visual browser QA remains outstanding.

### Standalone energy converter

J ↔ eV, default input 1 J. `convert()` handles empty/nonfinite values and overflow. Uses 1 eV = 1.602176634e-19 J. This is separate from the light converter.

### Light converter

`convertLight()` accepts positive decimal or scientific-notation text (e.g. `5.5e2`) and converts wavelength ↔ frequency in a vacuum. Enter only the numeric value; select the unit separately.

- Default: 550 nm, with nm and THz output selectors.
- Wavelength units: m, cm, mm, μm, nm, pm, fm, km.
- Frequency units: Hz, kHz, MHz, GHz, THz, PHz, EHz.
- Both quantities appear simultaneously, with additional nm and THz equivalents.
- Switching input kind converts a valid current value to preserve the physical quantity, using nm or THz for the new input.
- Changing the input unit reinterprets the current numeric input; changing an output unit only changes the displayed output.
- Energy output selector: J/photon, eV/photon, J/mol photons, eV/mol photons. Default is J/photon.
- Invalid, zero, negative, nonfinite, underflowing, or overflowing calculations produce an error. JavaScript floating-point limits apply; not every conceivable range is supported.

Formulas (exact defining constants):

- c = 299792458 m/s; f = c/λ, λ = c/f.
- h = 6.62607015e-34 J·s; E per photon = hf.
- N_A = 6.02214076e23 mol^-1; molar energy = N_A hf.
- Divide energy in J by 1.602176634e-19 to obtain eV, retaining the photon or mole basis.

`scientific()` formats main numerical outputs as a mantissa × 10 with a superscript exponent. Supplemental nm/THz values use compact ordinary numbers or JavaScript exponential notation. Main converted light outputs use 10 significant digits, which describes display precision, not input measurement precision.

### Spectrum and colors

`spectrum(nm)` classifies approximate wavelength bands:

| Band | Implemented wavelength interval |
| --- | --- |
| Gamma rays | < 0.01 nm |
| X-rays | ≥ 0.01 nm and < 10 nm |
| Ultraviolet | ≥ 10 nm and < 380 nm |
| Visible light | 380–700 nm inclusive |
| Infrared | > 700 nm and < 1 mm |
| Microwaves | ≥ 1 mm and < 10 cm |
| Radio waves | ≥ 10 cm |

Boundaries are conventions, not universal physical cutoffs. Microwaves also belong to the broader radio spectrum. The page explains this. Classification rounds nm to 12 significant digits first to avoid boundary errors from floating-point conversion (e.g. 700 nm becoming slightly greater than 700).

`visibleColor(nm)` uses a piecewise RGB approximation, edge intensity fading, and exponent 0.8. Names: violet < 450 nm, blue < 495, green < 570, yellow < 590, orange < 620, otherwise red within the visible range. This is an approximate screen swatch, not a colorimetric or perceptual model. No visible color is assigned outside 380–700 nm; a neutral striped swatch appears instead. The active band is highlighted in a seven-band strip.

## Layout and accessibility requirements

The visual style is cream/white with muted green, a serif heading, a reference table, and card sections. At widths ≤ 740px, light controls/results stack and the constants table scrolls horizontally. Three result cards show wavelength, frequency, and energy on desktop.

**Explicit user requirement: clearing or invalidating the light input must not collapse the light section or shift the page around.** `.light-display` reserves the result layout. On error, `#light-output` uses `visibility: hidden` (not `hidden` or `display: none`), retains its last content and space, is made `inert`, and has `aria-hidden="true"`. `#light-error` is absolutely positioned over this reserved space. On recovery, restore visibility, remove aria-hidden, clear inert, and hide the error. Preserve this behavior when adding results or changing validation. This preserves the last result's layout; it does not force all valid values to have identical height if text wraps differently.

Controls have labels, focus outlines, and live result/status announcements. The error input is linked to the error message with aria-describedby. Color names accompany swatches, so color is not the only signal.

## Sources

- NIST constants: https://physics.nist.gov/cuu/Constants/
- Downloaded listing: https://physics.nist.gov/cuu/Constants/Table/allascii.txt
- NASA approximate spectrum bands: https://imagine.gsfc.nasa.gov/science/toolbox/spectrum_chart.html
- NASA visible light (approximately 380–700 nm): https://science.nasa.gov/ems/09_visiblelight/

Sources were checked during implementation. NIST's 2022 CODATA values were the published values used here. The NASA band chart uses 400 nm for its optical lower bound; this page instead uses 380 nm based on NASA's visible-light explanation and explicitly describes bands as approximate.

## Verification and future checks

One-off Node.js `vm` checks executed the inline script with a lightweight mocked DOM. These checks were run in shell heredocs and are not saved as a test suite. They passed:

- Constant defaults, alternate units, mass values in u, and SI reset.
- J ↔ eV conversion and empty input.
- 550 nm ≈ 545.0771964 THz and input-kind round trips.
- Scientific input, selectable output units, all seven bands and their boundaries.
- Color generation and rejection of empty, invalid, negative, zero, and extreme inputs.
- Invalid-state visibility, retained result content, inert/aria-hidden behavior, and recovery.
- All four energy modes using E = hc/λ and N_A; frequency input agrees.

No visual browser QA has been performed. DOM mocks validate calculation and event logic, not rendering, actual focus behavior, or layout stability. If browser tooling becomes available, check desktop/mobile cards, long energy unit labels, error-state height before/after clearing input, keyboard navigation, screen-reader announcements, and print behavior.

Useful spot checks at 550 nm: frequency ≈ 545.0771964 THz; energy ≈ 3.61171974e-19 J/photon ≈ 2.25425815 eV/photon; molar energy ≈ 2.17502847e5 J/mol photons. Prefer computing exact expected values from the formulas when verifying.

Keep future work within the user's requested scope. There are no pending requested features as of this update. Read the actual source and current Git status before editing; this document is a snapshot, not a replacement for implementation.
