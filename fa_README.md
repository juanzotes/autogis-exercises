# Rural Pharmacy Accessibility in Guadalajara Province (Spain)
## AutoGIS Final Assignment — Juan Zotes, UCM

**Status:** [ ] Submitted &nbsp;&nbsp; [x] I'm still working on my final assignment.

---

## Topic

How accessible are pharmacies for residents of rural municipalities in Guadalajara province?  
This workflow measures **estimated driving time** from each municipality centroid to the nearest pharmacy using the OpenStreetMap road network, and compares results across **Goerlich territorial typologies** and **comarcas**.

Guadalajara is one of Spain's most affected provinces by rural depopulation, with over 200 municipalities under 100 inhabitants. The Serranía Celtibérica — a cross-regional area of extreme depopulation — covers much of its eastern half.

---

## Research questions

1. Which comarcas in Guadalajara have the worst pharmacy accessibility?
2. How does travel time to the nearest pharmacy vary across Goerlich typologies (Rural Remoto → Urbano)?
3. Is there a relationship between municipal population size and pharmacy access time?

---

## Structure of this repository

```
├── final_assignment.ipynb        ← Main analysis notebook
├── README.md                     ← This file
├── outputs/
│   ├── guadalajara_pharmacy_accessibility.csv
│   ├── summary_by_comarca.csv
│   ├── summary_by_goerlich.csv
│   ├── map_travel_time_pharmacy.png
│   ├── map_goerlich_typology.png
│   ├── bar_comarca_travel_time.png
│   ├── boxplot_goerlich_travel_time.png
│   ├── scatter_pop_vs_time.png
│   └── map_interactive_pharmacy_accessibility.html
```

---

## Input data

| Dataset | Source | Notes |
|---|---|---|
| Municipal boundaries + comarca/province hierarchy | IGN / Goerlich et al. (2016), processed by UCM RURIMESCAPE project | Local GeoPackage, not included in repo |
| Population by municipality 2023 + Goerlich typology | INE Padrón Municipal de Habitantes, processed CSV | Local file, not included in repo |
| Road network (drive) | OpenStreetMap via OSMnx | Downloaded at runtime |
| Pharmacy locations | OpenStreetMap `amenity=pharmacy` via OSMnx | Downloaded at runtime |

**Note:** The GeoPackage and padrón CSV are not included due to file size. See paths in notebook cell 0.  
A sample of the output CSV is available in `outputs/`.

---

## Analysis steps

1. **Data loading** — Filter municipal GeoPackage to Guadalajara (`Prov_Code == '19'`); join population and Goerlich typology from padrón CSV
2. **OSM download** — Download drivable road network and pharmacy POIs for the province boundary
3. **Network preparation** — Add edge speeds and travel times (`ox.add_edge_speeds`, `ox.add_edge_travel_times`)
4. **Shortest path** — For each municipality centroid (snapped to nearest network node), compute Dijkstra shortest path to every pharmacy node; return minimum travel time
5. **Summary statistics** — Aggregate by comarca and Goerlich typology (mean, max, P75 travel time; population weighted)
6. **Visualization** — Choropleth map, Goerlich typology map, bar chart (comarcas), boxplot (typologies), scatter (population vs time), interactive Folium map

---

## Results

*To be filled after running the notebook.*

---

## Requirements

```bash
conda activate rural-migration   # or your environment
pip install osmnx branca
```

Key packages: `geopandas`, `osmnx`, `networkx`, `folium`, `pandas`, `matplotlib`, `branca`

---

## References

- Goerlich, F.J. et al. (2016). *Cambios en la estructura y localización de la población*. IVIE / Fundación BBVA.
- OSMnx documentation: https://osmnx.readthedocs.io
- INE Padrón Municipal: https://www.ine.es

---

## Use of AI

Claude (Anthropic, claude-sonnet-4-6) was used to help structure the notebook, write boilerplate code, and define the `nearest_pharmacy_time()` function skeleton.  
All analytical decisions (study area, metrics, typology conventions, color palette), parameter choices, and interpretations are the author's own.  
AI-generated code sections are marked with `# AI-assisted` inline comments in the notebook.
