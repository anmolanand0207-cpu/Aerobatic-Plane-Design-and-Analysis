# Aerobatic-Plane-Design-and-Analysis
# S1210 RC Wing: Airfoil Selection & Aerodynamic Analysis

This repo holds my part of a group project: the aerodynamic analysis of the wing for a small aerobatic RC aircraft. I did this in my second year, so it's a student project, not a production design. I've tried to be upfront about what was done, what wasn't, and where the numbers come from.

**Short version:** the design team gave me a 1.0 m × 0.20 m rectangular wing. I compared four low-Reynolds-number airfoils, picked the **S1210**, and then used hand calculations to understand what that choice means for a real, finite wing.

---

## What's in here

```
.
├── README.md
├── report/
│   └── S1210_Wing_Design_and_Manufacturing_Report.docx
└── figures/
    └── airfoil_polar_comparison.png   # XFLR5 comparison plot (Re = 100,000)
```

The report has the full write-up. This README is the quick tour.

---

## The wing (given to me, not designed by me)

| Parameter | Value |
|---|---|
| Span, b | 1.00 m |
| Chord, c | 0.20 m (constant) |
| Planform | Rectangular, unswept |
| Area, S | 0.20 m² |
| Aspect ratio, AR | 5.0 |
| Taper ratio | 1.0 |
| Thickness (12% section) | ~24 mm |

The planform came from the design team as a fixed constraint. My job was everything aerodynamic that follows from it.

---

## Part 1: Choosing the airfoil

I compared four candidates:

- **S1210**
- **S1223**
- **AG25**
- **NACA 2412**

All four were compared under identical conditions so the comparison is fair: **Re = 100,000, Mach 0, Ncrit = 9**. For each one I looked at five plots: Cl vs α, Cd vs α, Cl vs Cd, Cl/Cd vs α, and Cm vs α.

What I was looking for, roughly in this order:

1. **Lift** (Cl,max and stall behaviour). A lower stall speed matters on a small, light aircraft.
2. **Efficiency** (Cl/Cd). More lift for less drag at the angles we'd actually fly.
3. **Drag behaviour** across the useful α range, not just at one point.
4. **Pitching moment** (Cm). A very negative value means the tail has to work harder.

**Result:** S1210 gave strong lift and a high Cl/Cd across the useful range. From the plots I read roughly:

- Cl,max ≈ 1.8 to 2.0
- peak Cl/Cd ≈ mid-to-high 50s

These are **graph readings, not exported data**. I only had the plot, not the raw polar file, so treat them as approximate.

**Trade-off I'm aware of:** S1210 is a cambered section, so its Cm is negative (nose-down) and it isn't ideal for sustained inverted flight the way a symmetric airfoil would be. For an aerobatic aircraft that's a real consideration, and one I'd revisit with more mission data.

---

## Part 2: What does this mean for the actual wing?

An airfoil polar is 2-D (an infinite wing). A real wing with AR = 5 is not. So I used analytical relations to see what the finite wing does.

### Reynolds-number check
Re = Vc/ν with c = 0.20 m and ν ≈ 1.5 × 10⁻⁵ m²/s gives V ≈ **7.5 m/s (~27 km/h)** for Re = 100,000. That's a slow-model-aircraft regime, which is why this polar is relevant. If the real aircraft flies faster, the Reynolds number is higher and the analysis should be redone at that value.

### Lift
L = ½ρV²S·C_L

At 7.5 m/s: q ≈ 34.5 Pa, so qS ≈ 6.9 N. At C_L = 1.5 that's roughly 10.3 N (about 1.05 kgf); at C_L = 1.8, about 12.4 N. This is only a scale check, not a performance prediction.

### Stall speed
V_stall = √(2W / (ρ S C_L,max))

This should use a conservative **finite-wing** C_L,max, not the raw 2-D number from the plot. Aircraft mass wasn't fixed in the material I had, so I didn't compute a final stall speed. As an *illustration only*: a 1 kg aircraft with C_L,max = 1.4 would stall around 7.6 m/s.

### Induced drag
C_D,i = C_L² / (π e AR)

With AR = 5 and an assumed e = 0.8, the denominator is about 12.57, so **C_D,i ≈ 0.08 at C_L = 1**.

### Root bending moment
If lift were spread uniformly along the span, each half-wing carries W/2 acting b/4 from the root:

M_root ≈ Wb/8

A real lift distribution is closer to elliptical, which gives a slightly smaller moment, so this is a conservative first estimate. It also has to be multiplied by the load factor for manoeuvres.

### The main takeaway
The 2-D airfoil shows Cl/Cd in the 50s. But at C_L ≈ 1, induced drag (~0.08) is several times larger than profile drag (~0.01 to 0.02), so the wing's own L/D is more like **10**, not 55. That gap between the airfoil and the real wing was the most useful thing I learned from this project.

---

## Manufacturing

The wing was built by the team. The report's manufacturing section is written as a general process record (templates, ribs, spar alignment, jig assembly, surface finish, quality checks) because the exact materials and shop process weren't specified in the brief I worked from. I didn't do the structural design or the build myself.

---

## What this project does NOT show

- **No flight data.** The verification plan (static load test, control checks, first flight) is in the report, but test results aren't.
- **No 3-D analysis.** Everything finite-wing here is analytical. A VLM or CFD run would be the obvious next step.
- **No structural calculation beyond the simple root-moment estimate.** Spar sizing needs mass, load factor and material data that I didn't have.
- **No raw polar data.** Values are read from the plot.
- **Reynolds number is fixed at 100,000**, which may not match the real flight speed.

---

## If I were to continue this

1. Export the actual polar data (CSV) and re-run at the aircraft's real Reynolds number.
2. Do a 3-D finite-wing analysis (VLM in XFLR5, then CFD).
3. Compare against a symmetric section to quantify the inverted-flight trade-off.
4. Size the spar properly using real mass, load factor and material properties.
5. Static load test, then flight-test and compare with predictions.

---

## Tools and references

- XFLR5 / XFOIL for low-Re airfoil polars
- Abbott & von Doenhoff, *Theory of Wing Sections*
- Anderson, *Fundamentals of Aerodynamics*
- Drela, *XFOIL: An Analysis and Design System for Low-Reynolds-Number Airfoils*

---

## Credits

Group project at NIT Agartala. The wing geometry, structure and build were the team's work. The airfoil selection and aerodynamic analysis described above were mine.

If you spot a mistake or have a suggestion, feel free to open an issue. I'm still learning this stuff.
