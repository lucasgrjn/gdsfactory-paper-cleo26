# GDSFactory: An Open-Source Python Library for Chip Design Automation

**Authors:** Joaquin Matres<sup>1,*</sup>, Simon Bilodeau<sup>2,3</sup>, Niko Savola<sup>2,4</sup>, Ali Hammoud<sup>5</sup>, Helge Gehring<sup>6</sup> and Troy Tamas <sup>1</sup>

**Affiliations:**
1. GDSFactory, 650 Castro St Ste 120 PMB 98035, Mountain View, CA 94041, USA
2. X Development LLC, 100 Mayfield Ave, Mountain View, CA 94043, USA
3. Department of Electrical and Computer Engineering, Princeton University, Princeton, NJ 08544, USA
4. Department of Applied Physics, Aalto University, PO Box 13500, FIN-00076 Aalto, Finland
5. Department of Electrical Engineering and Computer Science, University of Michigan, Ann Arbor, MI 48109, USA
6. Google LLC, 1600 Amphitheatre Parkway, Mountain View, CA 94043, USA

**Corresponding author:** *jmatres@GDSFactory.com

## Abstract

We present GDSFactory, an open-source Python library for integrated circuit design automation supporting photonics, analog, quantum, and MEMS applications with unified design, simulation, verification, and validation workflows.

## 1. Introduction

Hardware iterations typically require months and millions of dollars, while software iterations cost hundreds of dollars and take hours. GDSFactory bridges this gap by providing a comprehensive Python API for chip development, including layout design, verification, and validation [1].

Unlike constrained logic-driven electronic design flows [2], integrated photonics requires freeform parametric geometries. GDSFactory addresses this with scriptable parametric cells (PCells), hierarchical assembly, and routing---all in Python's extensive scientific ecosystem.

## 2. Design Capabilities

GDSFactory implements cells as Python functions returning a `Component` class with polygons, port metadata, and convenience methods. Using a C++-based gdstk backend [3], users define PCells with a functional programming approach:

```python
import GDSFactory as gf

@gf.cell
def mzi_with_bend(radius=10):
    c = gf.Component()
    mzi = c.add_ref(gf.components.mzi())
    bend = c.add_ref(
        gf.components.bend_euler(radius=radius))
    bend.connect('o1', mzi['o2'])
    return c
```

The `@gf.cell` decorator handles caching, eliminating redundant regeneration. Port metadata enables automatic routing via `get_route` and `get_bundle` functions that connect components following `CrossSection` specifications (Fig. 1).

![Figure 1](figures/GDSFactory_components/mzi_with_bend.pdf)
![Figure 1b](figures/GDSFactory_components/nxn.pdf)

**Figure 1:** (a) MZI with Euler bend showing port connections. (b) Routed n×n components using S-bends.

## 3. Simulation and Verification

GDSFactory's gplugins repository interfaces with simulators by reusing layout abstractions. Device-level exports include: FDTD solvers (MEEP [4], Tidy3D, Lumerical), mode solvers (Femwell, MPB), EME (MEOW), and TCAD (DEVSIM, Sentaurus). Components can be meshed via GMSH for cross-sectional or 3D analysis (Fig. 2).

![Figure 2](figures/gmsh/dessin.eps)

**Figure 2:** GDSFactory meshing: (a) heater layout, (b) cross-sectional mesh, (c) 3D mesh.

Circuit-level simulation uses netlist extraction with SAX for S-parameter analysis [5] and VLSIR for Spice formats. Design rule checking integrates with KLayout through an extensible API.

## 4. Process Design Kits

Open-source PDKs include GlobalFoundries 180nm, SkyWater 130nm [6], VTT 3 μm SOI, and SiEPIC. Commercial PDKs under NDA include AIM, AMF, TowerSemi, IMEC, and HHI. The generic PDK follows standard layer conventions [7] for cross-foundry compatibility.

## 5. Conclusion

GDSFactory provides a unified Python-driven workflow for chip design, verification, and validation. Its integration with simulators, open-source PDKs, and extensible architecture accelerates hardware development across photonics, quantum, and analog applications. The library is freely available at https://github.com/GDSFactory/GDSFactory.

## References

1. J. Matres *et al.*, "GDSFactory," GitHub (2024), https://github.com/GDSFactory/GDSFactory.

2. W. Bogaerts *et al.*, "Silicon photonics circuit design: methods, tools and challenges," Laser Photon. Rev. **12**, 1700237 (2018).

3. L. H. Gabrielli, "gdstk," GitHub (2023), https://github.com/heitzmann/gdstk.

4. A. F. Oskooi *et al.*, "MEEP: A flexible free-software package for electromagnetic simulations by the FDTD method," Comput. Phys. Commun. **181**, 687--702 (2010).

5. F. Laporte, "SAX," GitHub (2023), https://github.com/flaport/sax.

6. SkyWater Technology Foundry and Google, "SkyWater Open Source PDK," GitHub (2023), https://github.com/google/skywater-pdk.

7. L. Chrostowski and M. Hochberg, *Silicon Photonics Design* (Cambridge, 2015).
