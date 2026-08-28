# Graph Report - Uncertain-Transit-Access  (2026-08-27)

## Corpus Check
- cluster-only mode — file stats not available

## Summary
- 410 nodes · 624 edges · 52 communities (49 shown, 3 thin omitted)
- Extraction: 100% EXTRACTED · 0% INFERRED · 0% AMBIGUOUS · INFERRED: 1 edges (avg confidence: 0.85)
- Token cost: 0 input · 0 output

## Graph Freshness
- Built from commit: `e5273c0f`
- Run `git rev-parse HEAD` and compare to check if the graph is stale.
- Run `graphify update .` after code changes (no API cost).

## Community Hubs (Navigation)
- Community 0
- Community 1
- Community 2
- Community 3
- Community 4
- Community 5
- Community 6
- Community 7
- Community 8
- Community 9
- Community 10
- Community 11
- Community 12
- Community 13
- Community 14
- Community 15
- Community 17

## God Nodes (most connected - your core abstractions)
1. `compilerOptions` - 16 edges
2. `TractPanel()` - 13 edges
3. `main()` - 13 edges
4. `fmtPct2()` - 11 edges
5. `download_file()` - 8 edges
6. `download_gtfs()` - 8 edges
7. `main()` - 8 edges
8. `adjacency_from_queen()` - 8 edges
9. `write_sampling_meta()` - 8 edges
10. `computeKDE()` - 8 edges

## Surprising Connections (you probably didn't know these)
- `main()` --calls--> `load_merged_config()`  [EXTRACTED]
  scripts/extract_osm_pbf.py → src/utils/config.py
- `main()` --calls--> `find_osmium_executable()`  [EXTRACTED]
  scripts/extract_osm_pbf.py → src/utils/paths.py
- `TransitMap()` --calls--> `fmtPct2()`  [EXTRACTED]
  app/src/components/Map/TransitMap.tsx → app/src/lib/formatters.ts
- `InterventionDiff()` --calls--> `fmtPct2()`  [EXTRACTED]
  app/src/components/TractPanel/InterventionDiff.tsx → app/src/lib/formatters.ts
- `GeoDataState` --references--> `Metadata`  [EXTRACTED]
  app/src/hooks/useGeoData.ts → app/src/types/index.ts

## Import Cycles
- None detected.

## Communities (52 total, 3 thin omitted)

### Community 0 - "Community 0"
Cohesion: 0.07
Nodes (33): Dashboard, HomeClient(), Bin, binColor(), ExceedanceHistogram(), Props, Dashboard(), zfill11() (+25 more)

### Community 1 - "Community 1"
Cohesion: 0.10
Nodes (34): DensityConfoundScatter(), M, Props, Q_COLORS, Badge(), Tone, tones, NarrativeCard() (+26 more)

### Community 2 - "Community 2"
Cohesion: 0.06
Nodes (35): dependencies, d3-array, d3-scale, d3-scale-chromatic, @deck.gl/core, @deck.gl/layers, @deck.gl/react, framer-motion (+27 more)

### Community 3 - "Community 3"
Cohesion: 0.06
Nodes (33): devDependencies, autoprefixer, eslint, eslint-config-next, postcss, tailwindcss, @types/d3-array, @types/d3-scale (+25 more)

### Community 4 - "Community 4"
Cohesion: 0.11
Nodes (31): _configure_osmnx(), download_census(), download_file(), download_gtfs(), download_lodes(), download_osm(), download_osm_pbf_for_r5(), ensure_mobility_feeds_csv() (+23 more)

### Community 5 - "Community 5"
Cohesion: 0.11
Nodes (28): main(), artifact_run_id(), deep_merge(), legacy_stems_for_semantic(), load_merged_config(), netcdf_file_is_readable(), pipeline_run_id(), Path (+20 more)

### Community 6 - "Community 6"
Cohesion: 0.07
Nodes (26): compilerOptions, allowJs, esModuleInterop, incremental, isolatedModules, jsx, lib, module (+18 more)

### Community 7 - "Community 7"
Cohesion: 0.12
Nodes (23): Model, adjacency_from_queen(), _connect_islands_to_nearest(), _id_order_to_geoids_and_geoms(), GeoDataFrame, ndarray, spatial.py — spatial weight matrix utilities for BayesTransitEquity Provides:…, Return a dict of summary statistics for the adjacency matrix W. (+15 more)

### Community 8 - "Community 8"
Cohesion: 0.20
Nodes (24): Any, InferenceData, convergence_assessment(), _count_divergences(), diagnostic_var_names(), _find_latest_idata(), _load_config(), main() (+16 more)

### Community 9 - "Community 9"
Cohesion: 0.16
Nodes (19): InterventionDiff(), M, Props, M, NeighborComparisonPlot(), NeighborCurveSpec, Props, M (+11 more)

### Community 10 - "Community 10"
Cohesion: 0.17
Nodes (16): desertProbForTooltip(), OSM_STYLE, pickMetricValue(), Props, TransitMap(), CROSSED_LINE, DELTA_STOPS, DESERT_STOPS (+8 more)

### Community 11 - "Community 11"
Cohesion: 0.23
Nodes (11): collapse_feed_info_dataframe(), ensure_single_feed_info_in_gtfs_zip(), DataFrame, Path, r5py_cache_dir(), GTFS helpers for Conveyal R5 / r5py (Java) compatibility., Same directory as r5py.util.Config().CACHE_DIR (without importing r5py — avoids…, r5py WorkingCopy copies each GTFS basename into CACHE_DIR only if missing; it… (+3 more)

### Community 12 - "Community 12"
Cohesion: 0.25
Nodes (6): find_repo_root(), _neighbors_geopandas_touches(), GeoDataFrame, Path, export_frontend.py ================== Generates the static data files consumed…, Queen-style adjacency without libpysal (shared boundary / vertex).

### Community 13 - "Community 13"
Cohesion: 0.40
Nodes (3): inter, metadata, mono

## Knowledge Gaps
- **98 isolated node(s):** `Bin`, `Props`, `Props`, `GeoFC`, `AppState` (+93 more)
  These have ≤1 connection - possible missing edges or undocumented components.
- **3 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `dependencies` connect `Community 2` to `Community 3`?**
  _High betweenness centrality (0.020) - this node is a cross-community bridge._
- **What connects `Bin`, `Props`, `Props` to the rest of the system?**
  _98 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `Community 0` be split into smaller, more focused modules?**
  _Cohesion score 0.07092198581560284 - nodes in this community are weakly interconnected._
- **Should `Community 1` be split into smaller, more focused modules?**
  _Cohesion score 0.10188261351052048 - nodes in this community are weakly interconnected._
- **Should `Community 2` be split into smaller, more focused modules?**
  _Cohesion score 0.05714285714285714 - nodes in this community are weakly interconnected._
- **Should `Community 3` be split into smaller, more focused modules?**
  _Cohesion score 0.058823529411764705 - nodes in this community are weakly interconnected._
- **Should `Community 4` be split into smaller, more focused modules?**
  _Cohesion score 0.11491935483870967 - nodes in this community are weakly interconnected._