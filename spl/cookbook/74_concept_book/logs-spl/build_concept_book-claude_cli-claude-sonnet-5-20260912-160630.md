# SPL Run: build_concept_book

- **Adapter:** claude_cli
- **Model:** claude-sonnet-5
- **Tokens:** 1758 in / 3124 out
- **Latency:** 65837ms
- **Timestamp:** 2026-09-12 16:06:30

## Output

```output


---

## Concentration Gradient

**Definition.** Let $\Omega \subset \mathbb{R}^3$ be a spatial region and let $C(\mathbf{x}, t)$ denote the concentration (in $\text{mol}/\text{L}$ or $\text{mol}/\text{m}^3$) of a solute at position $\mathbf{x} = (x, y, z)$ and time $t$. A **concentration gradient** exists whenever $C$ is not spatially uniform, i.e., $\nabla C \neq \mathbf{0}$ for some $\mathbf{x} \in \Omega$. The gradient vector

$$
\nabla C = \left( \frac{\partial C}{\partial x}, \frac{\partial C}{\partial y}, \frac{\partial C}{\partial z} \right)
$$

points in the direction of steepest increase in concentration, and its magnitude $|\nabla C|$ quantifies how sharply concentration changes over distance.

**Worked example.** Consider a one-dimensional system where concentration decreases linearly from $C_1 = 5\ \text{mmol/L}$ at $x = 0$ to $C_2 = 1\ \text{mmol/L}$ at $x = L = 2\ \text{mm}$. Then

$$
C(x) = C_1 + \frac{C_2 - C_1}{L}x, \qquad \frac{dC}{dx} = \frac{C_2 - C_1}{L} = \frac{1 - 5}{2} = -2\ \text{mmol/(L·mm)}.
$$

The negative sign indicates concentration decreases in the direction of increasing $x$; the magnitude $2\ \text{mmol/(L·mm)}$ measures the steepness of the gradient.

**Key theorem (Fick's First Law).** A concentration gradient generates a diffusive flux $J$ proportional to its magnitude:

$$
J = -D \frac{dC}{dx},
$$

where $D$ is the diffusion coefficient. The negative sign expresses that net movement occurs from high to low concentration, down the gradient, until equilibrium ($\nabla C = 0$) is reached. This relation underlies transport phenomena in cell membranes, tissues, and engineered systems.

**Lab cell (SymPy).**
```python
import sympy as sp

x, C1, C2, L, D = sp.symbols('x C1 C2 L D', positive=True)
C = C1 + (C2 - C1) / L * x
dCdx = sp.diff(C, x)
J = -D * dCdx

print("dC/dx =", dCdx)
print("Flux J =", sp.simplify(J))
```

---

## Electrochemical Gradient

**Definition.** For an ion species $X$ with charge $z$ (in units of elementary charge), the electrochemical gradient across a membrane is the total driving force on that ion, combining its chemical (concentration) gradient and the electrical potential difference across the membrane. Quantitatively, the electrochemical potential of the ion on side $i$ is

$$
\mu_i = \mu^\circ + RT\ln[X]_i + zF V_i,
$$

where $R$ is the gas constant, $T$ absolute temperature, $F$ Faraday's constant, and $V_i$ the local electric potential. The net driving force per mole moving from side 1 to side 2 is $\Delta\mu = \mu_2 - \mu_1$, and the ion moves spontaneously toward decreasing $\mu$. Setting $\Delta\mu = 0$ defines the ion's equilibrium potential, given by the **Nernst equation**:

$$
E_X = \frac{RT}{zF}\ln\frac{[X]_{out}}{[X]_{in}}.
$$

**Worked example.** Consider $\mathrm{K}^+$ ($z=+1$) with $[K^+]_{out} = 5\ \text{mM}$, $[K^+]_{in} = 140\ \text{mM}$, at $T = 310\ \text{K}$. Then

$$
E_K = \frac{(8.314)(310)}{(1)(96485)}\ln\frac{5}{140} \approx -0.0267 \times \ln(0.0357) \approx -0.0267 \times (-3.33) \approx -0.089\ \text{V} = -89\ \text{mV}.
$$

If the measured membrane potential $V_m$ differs from $E_K$, potassium is not at equilibrium: for $V_m > E_K$, K$^+$ flows outward; for $V_m < E_K$, inward.

**Key theorem.** *(Equilibrium condition)* An ion $X$ is at electrochemical equilibrium across a membrane if and only if $V_m = E_X$ as given by the Nernst equation; otherwise a net flux occurs in the direction that reduces $|\Delta\mu|$, with flux magnitude approximately proportional to $(V_m - E_X)$ under constant-field (Goldman–Hodgkin–Katz) assumptions.

**Lab cell (SymPy).**
```python
import sympy as sp

R, T, F, z = sp.symbols('R T F z', positive=True)
Xout, Xin = sp.symbols('X_out X_in', positive=True)

E_X = (R*T)/(z*F) * sp.log(Xout/Xin)
E_K_numeric = E_X.subs({R: 8.314, T: 310, F: 96485, z: 1,
                          Xout: 5, Xin: 140})
print(sp.nsimplify(E_K_numeric.evalf(), rational=False))
```

---

## Active Transport

**Definition.** Active transport is the movement of a solute across a plasma membrane against its concentration or electrochemical gradient, driven by the direct or indirect expenditure of metabolic energy, typically supplied by ATP hydrolysis. This contrasts with passive transport, in which net flux always proceeds down a gradient and requires no cellular energy input. Active transport is classified as **primary** (ATP hydrolysis directly powers the conformational change of a transport protein) or **secondary** (the potential energy stored in an ion gradient, itself established by primary active transport, drives movement of a second solute).

**Worked example.** The Na⁺/K⁺-ATPase pump exports 3 Na⁺ ions and imports 2 K⁺ ions per ATP hydrolyzed, against both ions' gradients. Consider a cell where $[\text{Na}^+]_{\text{in}} = 15\ \text{mM}$, $[\text{Na}^+]_{\text{out}} = 145\ \text{mM}$, membrane potential $V_m = -70\ \text{mV}$, temperature $T = 310\ \text{K}$. The Nernst equilibrium potential for Na⁺ is
$$E_{\text{Na}} = \frac{RT}{zF}\ln\frac{[\text{Na}^+]_{\text{out}}}{[\text{Na}^+]_{\text{in}}} \approx +61\ \text{mV}.$$
Since $V_m \ll E_{\text{Na}}$, Na⁺ is far from equilibrium and would passively flood inward; the pump must continuously expend ATP to maintain this gradient, confirming that transport is active rather than diffusive.

**Key theorem (thermodynamic criterion).** A transport process is active if and only if the free energy change per mole of solute transported,
$$\Delta G = RT\ln\frac{C_{\text{in}}}{C_{\text{out}}} + zF\Delta V,$$
is positive in the direction of net flux; energy input equal to at least $\Delta G$ must be supplied to drive the process, satisfying the second law of thermodynamics.

**Lab cell (SymPy).**
```python
import sympy as sp

R, T, z, F, Cin, Cout, Vm = sp.symbols('R T z F C_in C_out V_m', positive=True)
dG = R*T*sp.log(Cin/Cout) + z*F*Vm
dG_numeric = dG.subs({R: 8.314, T: 310, z: 1, F: 96485,
                      Cin: 0.015, Cout: 0.145, Vm: -0.070})
print(sp.N(dG_numeric, 4))  # J/mol required to pump Na+ out against gradient
```

---

## Primary Active Transport

**Definition.** Primary active transport is a mechanism by which a transmembrane protein moves solutes against their electrochemical gradient using energy released directly from the hydrolysis of adenosine triphosphate (ATP). Because the transported species is an ion, the process generates a net separation of charge across the membrane, contributing to the membrane potential $V_m$. The canonical example is the $\mathrm{Na}^+/\mathrm{K}^+$-ATPase, which for each catalytic cycle exports $3\,\mathrm{Na}^+$ and imports $2\,\mathrm{K}^+$, consuming one ATP molecule.

**Worked example.** Suppose the pump operates at a turnover rate of $r = 100$ cycles per second per pump, and a cell membrane contains $N = 10^6$ pump proteins. The net charge translocated outward per second is
$$
I = N \cdot r \cdot (3 - 2) \cdot e,
$$
where $e = 1.602 \times 10^{-19}\,\mathrm{C}$ is the elementary charge. Substituting values:
$$
I = (10^6)(100)(1)(1.602\times10^{-19}\,\mathrm{C}) = 1.602\times10^{-11}\,\mathrm{A}.
$$
This small outward current is one contributor to the resting membrane potential, alongside passive ion channel fluxes described by the Goldman–Hodgkin–Katz equation.

**Key theorem (thermodynamic coupling).** For transport to proceed against a gradient, the free energy released by ATP hydrolysis, $\Delta G_{\mathrm{ATP}} \approx -30.5\,\mathrm{kJ/mol}$, must exceed the electrochemical work required:
$$
\Delta G_{\mathrm{transport}} = RT \ln\!\left(\frac{[S]_{\text{out}}}{[S]_{\text{in}}}\right) + zF\Delta V_m \le -\Delta G_{\mathrm{ATP}}.
$$
This inequality, a direct consequence of the second law applied to coupled reactions, sets an upper bound on the concentration gradient a single ATP-driven cycle can sustain.

**Lab cell (SymPy).**
```python
import sympy as sp

R, T, z, F, dVm, S_out, S_in = sp.symbols('R T z F dVm S_out S_in', positive=True)
dG_transport = R*T*sp.log(S_out/S_in) + z*F*dVm
dG_ATP = -30500  # J/mol

max_ratio = sp.solve(sp.Eq(dG_transport, -dG_ATP), S_out/S_in)
print(max_ratio)
```

---

## Sodium Potassium Pump

**Definition.** The sodium–potassium pump ($\text{Na}^+/\text{K}^+$-ATPase) is an integral membrane protein that carries out primary active transport, using the chemical energy of ATP hydrolysis to move ions against their electrochemical gradients. Each catalytic cycle exports $3\,\text{Na}^+$ ions from the cytoplasm to the extracellular space and imports $2\,\text{K}^+$ ions in the opposite direction, consuming one molecule of ATP:
$$
\text{ATP} + \text{H}_2\text{O} + 3\,\text{Na}^+_{\text{in}} + 2\,\text{K}^+_{\text{out}} \longrightarrow \text{ADP} + \text{P}_i + 3\,\text{Na}^+_{\text{out}} + 2\,\text{K}^+_{\text{in}}
$$
Because the net transfer of charge per cycle is $+1$ (three positive charges leave, two enter), the pump is *electrogenic*: it directly contributes a small outward current that hyperpolarizes the membrane, in addition to establishing the concentration gradients themselves.

**Worked example.** Consider a neuron cycling the pump at a rate of $r = 3\times 10^{6}$ cycles per second across its surface. The net outward current generated directly by the pump's electrogenicity is
$$
I_{\text{pump}} = r \cdot e = (3\times10^{6}\,\text{s}^{-1})(1.6\times10^{-19}\,\text{C}) \approx 4.8\times10^{-13}\,\text{A} = 0.48\ \text{pA}.
$$
This is small compared to typical action-potential currents but is persistent and sets the baseline resting potential together with passive ion channels via the Goldman–Hodgkin–Katz equation.

**Key theorem (stoichiometric conservation).** For $n$ complete catalytic cycles, exactly $n$ ATP molecules are hydrolyzed, $3n$ $\text{Na}^+$ ions are exported, and $2n$ $\text{K}^+$ ions are imported; the ratio $3:2:1$ (Na$^+$:K$^+$:ATP) is fixed by the pump's conformational mechanism (the E1–E2 cycle) and does not vary with substrate concentration, only the rate $r$ does.

**Lab cell (SymPy).**
```python
import sympy as sp

r, e = sp.symbols('r e', positive=True)
I_pump = r * e
print(I_pump.subs({r: 3e6, e: 1.6e-19}))  # net pump current in amperes
```

---

## Payoff

The sodium-potassium pump ($\text{Na}^+/\text{K}^+$-ATPase) is the molecular device that converts chemical energy stored in ATP into an electrochemical gradient — the currency that powers nearly all active transport and electrical signaling in animal cells. Formally, the pump catalyzes the vectorial, stoichiometrically fixed exchange

$$
3\,\text{Na}^+_{\text{in}} + 2\,\text{K}^+_{\text{out}} + \text{ATP} + \text{H}_2\text{O} \;\longrightarrow\; 3\,\text{Na}^+_{\text{out}} + 2\,\text{K}^+_{\text{in}} + \text{ADP} + \text{P}_i,
$$

moving both ions against their concentration gradients. It is the natural endpoint of this text because it integrates every prerequisite concept: membrane structure (the lipid bilayer as a barrier), the thermodynamics of diffusion (Fick's law and the Nernst potential), and the enzymology of coupled reactions (free-energy transduction via phosphorylation). Nothing downstream can be understood without it.

**Worked example.** Using the Nernst equation $E_{\text{ion}} = \frac{RT}{zF}\ln\frac{[\text{ion}]_{\text{out}}}{[\text{ion}]_{\text{in}}}$, a typical mammalian neuron with $[\text{Na}^+]_{\text{in}} = 15\ \text{mM}$, $[\text{Na}^+]_{\text{out}} = 145\ \text{mM}$ at $T = 310\ \text{K}$ gives $E_{\text{Na}} \approx +61\ \text{mV}$, while $[\text{K}^+]_{\text{in}} = 140\ \text{mM}$, $[\text{K}^+]_{\text{out}} = 5\ \text{mM}$ gives $E_{\text{K}} \approx -89\ \text{mV}$. These are precisely the gradients the pump maintains against passive leak.

**Key theorem.** At steady state, pump flux must balance passive leak flux for each ion; the resting membrane potential is the Goldman–Hodgkin–Katz weighted average of $E_{\text{Na}}$, $E_{\text{K}}$, and $E_{\text{Cl}}$, with the pump's electrogenicity (net $+1$ charge extruded per cycle) contributing a small hyperpolarizing offset.

```python
from sympy import symbols, log, ln, exp
R, T, z, F = symbols('R T z F', positive=True)
c_out, c_in = symbols('c_out c_in', positive=True)
E_ion = (R*T)/(z*F) * ln(c_out/c_in)
E_ion.subs({R: 8.314, T: 310, z: 1, F: 96485, c_out: 145, c_in: 15}).evalf()
```

From here, the pump's gradient is the launchpad for action potentials, secondary active transport, osmoregulation, and cellular signaling — an invitation to trace how any one of these systems draws directly on the ionic asymmetry established here.
```
