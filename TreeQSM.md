Below is an English summary of the paper (methods described in detail; references listed as given). Source: Kumazaki, *Application of 3D tree modeling using point cloud data by terrestrial laser scanner* (Landscape Research 84(5), 2021). 

---

**One-paragraph summary**
Kumazaki (2021) proposes and validates a pipeline to build accurate, reproducible 3D tree models for Japanese-garden trees from terrestrial laser scanner (TLS) point clouds by combining (1) data-driven removal of leaf points using TLS ancillary attributes (pulse shape deviation and calibrated reflectance), (2) segmentation into single-tree point clouds, (3) noise cleaning, and (4) quantitative structural modeling (QSM) via the open-source TreeQSM. The method makes TLS-QSM usable even when trees have foliage (including evergreen species) and yields high geometric fidelity (mean/SD errors at the centimeter scale) and useful biological/metric outputs (volume, biomass proxies, heights, branch lengths). 

---

# Methods — detailed (what they did, parameters and software)

**Field acquisition (page 1–2).**

* Scanner: RIEGL VZ-400i. Settings: pulse repetition rate (PRR) 1.2 MHz, effective measurement rate 500,000 points/s, maximum range set to 250 m, angular resolution 0.04° (vertical & horizontal). Instrument measurement accuracy guaranteed within ~5 mm.
* Survey: 59 scan positions; total on-site scanning ~14.0 hours (scanning per setup 35–40 s). Study site: Kiyosumi Garden (Taito/Koto, Tokyo), target area near Taisho Memorial Hall. Processing runs reported on a PC with 3.20 GHz CPU (8 cores /16 threads) and 256 GB RAM. 

**Overall workflow and times (Table 1, page 1).**

1. Data acquisition — 14.0 h.
2. Data merging — 0.5 h.
3. Deviation threshold separation (leaf vs trunk/branch) — 1.5 h.
4. Reflectance threshold separation — 1.0 h.
5. Single-tree segmentation (Computree) — 3.0 h.
6. Noise processing for remaining leaf points: Statistical Outlier Removal (SOR) + Moving Least Squares (MLS) smoothing — 1.5 h.
7. TreeQSM processing (QSM construction) — 5.0 h. 

**Leaf / trunk-branch separation (pages 2–3).**

* Inputs used for classification: *pulse shape deviation* (a measure of echo waveform complexity / deviation) and *calibrated reflectance* (amplitude corrected for range, expressed in dB).
* Observations: trunk/branch points tend to have low deviation (clustered ~0–6), whereas leaf points have higher deviation. Reflectance histograms for multiple species showed a separation (bifurcation) between leaf and woody points around –7 dB to –5 dB.
* Chosen thresholds (empirical): deviation ≤ **5** selects higher-quality echo pulses (retaining trunk/branch); reflectance ≥ **–5.5 dB** used to preferentially extract trunk/branch points across studied garden tree species. Thresholds were derived by manually extracting sample leaf and woody point subsets then inspecting deviation and reflectance distributions. Figures and histograms illustrating these distributions are provided (see Figures on pages 2–3). 

**Segmentation and noise cleaning (page 3).**

* Single-tree segmentation: Computree (open source) was used to label/segment the scene point cloud into individual trees before QSM input.
* Remaining leaf/noise removal: Statistical Outlier Removal (SOR) and Moving Least Squares (MLS) smoothing were applied to remove leaf point residues and smooth the branch surface prior to modeling. 

**QSM construction (pages 2–3).**

* QSM engine: TreeQSM (InverseTampere/TreeQSM; open source). TreeQSM approximates tree structure as a network of connected cylindrical segments, iteratively estimating segment radii and directions to reconstruct trunk and branch geometry. The cleaned single-tree point cloud is the input. Outputs include per-segment radii/lengths, whole-tree and component volumes, heights, and other derived geometric metrics. 

**Accuracy assessment (page 3–4).**

* Strategy: extract QSM model node coordinates and for each model point find nearest neighbor(s) in the original tree point cloud using k-nearest neighbors (KNN), with k set to **5**. Distances between model points (queries) and nearest point cloud points quantify geometric reproducibility.
* Test specimen: a large camphor tree (Cinnamomum camphora / クスノキ), tree height 17.66 m. 

---

# Results (main quantitative outcomes)

* After thresholding and cleaning, TreeQSM constructed 3D tree models for the study area (individual trees and the whole area shown in Figures on pages 3–4). 
* **Accuracy (camphor tree):**

  * Whole tree: mean distance ≈ **0.018 m**, standard deviation **0.016 m**.
  * Branch portion: mean distance ≈ **0.012 m**, standard deviation **0.007 m**.
    (These values are reported from the KNN comparison with k=5; illustrated in Figure-7 and text on page 4.) 
* **Comparison to another method (AdTree):** prior AdTree validation reported branch mean distance ~0.1 m and std ~0.08 m for similar comparisons; the TLS-QSM pipeline here achieved substantially smaller errors (centimeter vs decimeter level) in this test, indicating strong reproducibility of tree shape. 

---

# Conclusions and applications (page 4)

* The combined use of pulse shape deviation and calibrated reflectance enables effective removal of leaf points so TLS-QSM can be applied even when foliage is present (including evergreen species), broadening the applicability of QSM approaches in garden/landscape contexts.
* The pipeline produces high-fidelity 3D tree models (cylinder-based QSMs) with centimeter-level agreement to the measured TLS point clouds; the models also supply volumetric and structural metrics useful for biomass estimation, maintenance records, and visualization for restoration or heritage preservation.
* The authors highlight practical applications in Japanese garden preservation (recording and reproducing important tree shapes), and potential integration with BIM/CIM workflows for improved landscape/plant management productivity. 

---

# Key figures / locations to inspect in the paper

* Table 1 (page 1): overall workflow and per-step processing times. 
* Figures 2–3 (page 2): histograms for deviation and reflectance distributions (example: Cinnamomum/Camphor). These show the empirical bifurcation used to set thresholds. 
* Figure 4 (page 3): processing result examples (before/after reflectance & deviation filtering). 
* Figures 5–6 (page 3): example TreeQSM single-tree models and whole-area model. 
* Figure 7 (page 4): accuracy histograms / distance distributions from KNN comparison (numerical accuracy values summarized above). 

---

# References (as listed in the paper — author/editor formatting preserved roughly as in the article)

1. ICOMOS-IFLA International Committee for Historic Gardens (1981): *Florence Charter on Historic Gardens*, 1–3. 
2. 林レイ子（1954）：庭園の芸術性について：造園雑誌 19, 1–3. 
3. 東京都建設局公園緑地部 社団法人日本造園学会（1989）：東京都における文化財庭園の保存・復元・管理等に関する調査報告書，21–22. 
4. Bienert, A., Georgi, L., Kunz, M., Maas, H., Oheimb, V. G. (2018): *Comparison and combination of mobile and terrestrial laser scanning for natural forest inventories.* Forest, 9, 395. 
5. Tanago, J. G., Lau, A., Bartholomeus, H., Herold, M., Avitabile, V., Raumonen, P., Martius, C., Goodman, R. C., Disney, M., Manuri, S., Burt, A., Calders, K. (2018): *Estimation of above-ground biomass of large tropical trees with terrestrial LiDAR.* MEE, 9(2), 223–234. 
6. Markku, Å., Raumonen, P., Kaasalainen, M., Casella, E. (2015): *Analysis of geometric primitives in quantitative structure models of tree stem.* Remote Sensing, 7(4), 4581–4603. 
7. 本田友里香・浅輪貴史・梅干野晁・押尾晴樹（2011）：地上型近赤外レーザースキャナによる樹木の三次元形態情報の取得と分析：第 21 回 日本赤外線学会研究発表会（ポスター発表）. 
8. RIEGL Laser Measurement Systems GmbH (2017): *LAS Extrabytes Implementation in RIEGL Software*, 4–7. 
9. 斎藤和人・増田宏・平岡裕一郎・松下通也・高橋誠（2016）：三次元大規模点群に基づく樹木の形状再構成 第 3 報：精密工学会秋季大会学術講演会講演論文集, 489–490. 
10. Tampere University: *InverseTampere TreeQSM* (GitHub). (referenced). 
11. Raumonen, P. (2017): *TreeQSM Quantitative Structure Models of Single Trees from Laser Scanner Data*. 
12. Computree group: *Computree Forest inventory with Lidar* (website). 
13. Khawaldeh, S., Aleef, T. A., Pervaiz, U., Minh, V. H., Hagos, Y. B. (2017): *Complete End-to-End Low Cost Solution to a 3D Scanning System with Integrated Turntable.* IJCSIT 9, 39–55. 
14. 和田俊和（2009）：最近傍探索の理論とアルゴリズム：情報処理学会研究報告 13, 1–12. 
15. Du, S., Lindenbergh, R., Ledoux, H., Stoter, J., Nan, L. (2019): *AdTree: Accurate, Detailed, and Automatic Modelling of Laser-Scanned Trees.* Remote Sensing, 11(18), 2074. 
16. Fan, G., Nan, L., Chen, F., Dong, Y., Wang, Z., Li, H., Chen, D. (2020): *A New Quantitative Approach to Tree Attributes Estimation Based on LiDAR Point Clouds.* Remote Sensing, 12(11), 1779. 
17. Ministry of Land, Infrastructure, Transport and Tourism (Japan): *i-Construction* committee report (website). 
18. 髙橋暁（2016）：建築ストック活用に向けた建築物の情報マネジメント 技術に関する研究と課題：BRI-H28 テキスト，21–28. 

---

If you want, I can now:

* Produce a 1-page English executive summary (suitable for inclusion in a report).
* Extract the figures (pages) as higher-resolution images and annotate the threshold histograms and QSM vs point-cloud distance histograms.
* Reformat the Methods section into a protocol checklist you could follow in the field.

Which one should I do next?

# Procedure — Estimating Tree Structure from TLS Point Clouds

*(Direct, step-by-step workflow based on Kumazaki 2021; methods and literature cited where relevant.)*

---

## 0) Summary of goal

Produce reproducible 3D tree structure models (Quantitative Structure Models, QSMs) from terrestrial laser scanner (TLS) point clouds that accurately represent trunk and branch geometry (radii, lengths, topology) and provide volumetric/structural metrics suitable for biomass proxies, garden preservation, or visualization.

**Primary references:** Kumazaki (2021) — *Application of 3D tree modeling using point cloud data by terrestrial laser scanner*; TreeQSM (Raumonen et al. / InverseTampere); Computree. See Kumazaki for empirical thresholds and validation.

---

## 1) Field acquisition (collect the point cloud)

1. Choose TLS hardware and settings. Example from Kumazaki: **RIEGL VZ-400i**; PRR ≈ **1.2 MHz**, effective rate ≈ **500k pts/s**, max range 250 m, angular resolution **0.04°**. Guarantee instrument accuracy (millimeter order).
2. Plan scan positions so each tree is scanned from multiple viewpoints to avoid occlusion. Kumazaki used **~59** scan positions for a garden area.
3. Record instrument metadata needed for later processing: range, incidence angle, pulse waveform attributes, and calibrated reflectance (intensity corrected for range). Keep raw export that preserves waveform/extra bytes (pulse shape/deviation) if available.
4. Save scans in standard formats (e.g., *.las / *.laz / vendor format that preserves extra attributes). Keep a scan log.

**Output:** Multi-scan TLS dataset with waveform/pulse attributes and calibrated reflectance.

---

## 2) Scan registration & merge

1. Register all scans into a single coordinate system (target-based or cloud-to-cloud). Use robust registration (ICP with coarse initial alignment).
2. Merge scans into a single point cloud; retain per-point attributes: calibrated reflectance (amplitude corrected for range), and pulse/waveform deviation metrics (if provided by scanner export).
3. Estimate and record overall point density and noise characteristics.

**Output:** Merged, georegistered point cloud with intensity & pulse attribute fields.

---

## 3) Preliminary cleaning (global)

1. Remove obvious outliers and scanner artifacts (floating points beyond plausible ranges).
2. Optionally ground-classify and remove ground points if they are not needed.
3. Compute and save per-tree bounding box candidates (for later segmentation).

**Tools:** CloudCompare, PDAL, LAStools, or vendor software.

---

## 4) Leaf vs woody (trunk/branch) separation — primary innovation

**Principle:** use TLS ancillary attributes — *pulse shape deviation* and *calibrated reflectance* — to discriminate leaves (high deviation / different reflectance) from woody surfaces (low deviation, higher reflectance).

1. Extract sample subsets of points manually labeled as leaf and as woody (a few hundred points each from representative species). Inspect their distributions for:

   * **pulse shape deviation** (or equivalent waveform complexity metric)
   * **calibrated reflectance** (in dB or intensity normalized for range)
2. Following Kumazaki, set empirical thresholds. Recommended starting values from the paper:

   * **deviation ≤ 5** — retains trunk/branch echoes.
   * **reflectance ≥ −5.5 dB** — favors woody points.
     Use both checks in combination (logical AND) to select woody candidates.
3. Apply thresholds across the merged cloud to create a *woody candidate* subset and a *leaf/residual* subset.
4. Visual-check results and iterate thresholds per species/site if necessary (inspect histograms like Kumazaki’s Figures 2–3).

**Notes:** Thresholds are empirical—recalibrate on new sensors/species. If pulse deviation is unavailable, rely on calibrated reflectance plus geometric filters (but accuracy will drop).

**Output:** Two labeled point sets: woody_candidate.pcd and leaf_candidate.pcd.

---

## 5) Single-tree segmentation

1. Segment the woody candidate cloud into individual tree objects. Tools used by Kumazaki: **Computree**; alternatives: marker-based region growing, graph-cut clustering, or manual segmentation for crowded sites.
2. Verify segmentation boundaries and correct any merged/split trees manually where necessary.

**Output:** A folder of single-tree point clouds (tree_001.pcd, tree_002.pcd, ...).

---

## 6) Noise cleaning and surface smoothing (per-tree)

1. For each single-tree cloud:

   * Apply **Statistical Outlier Removal (SOR)** to remove sparse noise (typical SOR: k = 16..32 neighbors, stddev multiplier 1.0–2.0). Kumazaki applied SOR but did not fix exact k; use common robust defaults and adjust if over/under-filtering.
   * Apply **Moving Least Squares (MLS)** smoothing to reduce residual leaf-scatter and produce smoother branch surfaces. Choose MLS search radius relative to point spacing (e.g., 1.5–3 × mean spacing).
2. Output a cleaned cloud suitable for geometric reconstruction.

**Warning:** Over-smoothing will blunt small branches and bias radii; keep parameters conservative.

**Output:** cleaned_tree_XXX.pcd

---

## 7) QSM construction (per-tree)

1. Use **TreeQSM** (Raumonen / InverseTampere) to build quantitative structure models from each cleaned per-tree cloud. Fundamental steps inside TreeQSM:

   * Fit cylindrical segments iteratively to approximate trunk and branches.
   * Estimate segment radii, axes, and connectivities; output tree graph (nodes, cylinders).
2. Typical TreeQSM tuning parameters to consider:

   * voxel size or downsample resolution (use finer resolution for thin branches)
   * initial cylinder radius guesses and branching angle tolerances
   * min segment length (set relative to point spacing; avoid smaller than sampling support)
3. Run TreeQSM and save outputs: cylinder list (radius, length, centerline), topology graph, total volume.

**Output:** QSM files (e.g., .json/.xml of cylinder network), per-segment metrics.

---

## 8) Accuracy assessment & validation

1. For geometric fidelity compare QSM model points to original point cloud:

   * Sample node points along QSM centerlines (or use cylinder surface samples).
   * For each QSM sample point, compute distance to nearest neighbor(s) in the original (merged) tree point cloud using **k-NN** (Kumazaki used **k=5**).
   * Compute statistics: mean distance, standard deviation, median, percentiles. Kumazaki reported mean ≈ **0.018 m** and SD ≈ **0.016 m** for a 17.66 m camphor tree.
2. Validate derived metrics (volume, height, branch-length distributions) against any available ground truth (manual measurements, destructive sampling where possible, or other validated methods like AdTree results for comparison).
3. Produce diagnostic plots: distance histograms, per-branch error maps.

**Acceptance criteria:** Aim for centimeter-scale mean errors for mature TLS surveys in similar conditions; if errors are decimeter-scale, inspect segmentation, leaf removal, and QSM tuning.

---

## 9) Post-processing outputs & uses

1. Compute whole-tree metrics:

   * Total woody volume (sum of cylinder volumes)
   * Height, DBH (if modeled), branch length/diameter distributions
   * Derived biomass proxies (apply species-specific wood density if available).
2. Store outputs in interoperable formats for visualization / BIM: OBJ/PLY for mesh approximations, CSV/JSON for metrics, and a provenance file describing parameters used (thresholds, SOR/MLS settings, QSM parameters).
3. Archive key intermediate datasets (merged cloud, woody candidate cloud, cleaned per-tree clouds, QSMs).

---

## 10) Recommended parameter checklist (start values; tune per site)

* Scanner: settings similar to Kumazaki (PRR 1.2 MHz; angular res 0.04°).
* Leaf/woody thresholds: **pulse deviation ≤ 5** AND **reflectance ≥ −5.5 dB** (recalibrate).
* SOR: neighbors = **16–32**, stddev multiplier **1.0–2.0** (adjust).
* MLS: search radius = **1.5–3 × mean point spacing**.
* QSM: voxel/downsample resolution ≈ point spacing; min segment length ≈ 2–3×point spacing.
* k-NN for fidelity: **k = 5**.

---

## 11) Pitfalls, limitations, and mitigation

* **No waveform/pulse data available:** reflectance-only separation is weaker; expect worse woody/leaf discrimination. Mitigate with multi-season scans (leaf-off) if feasible.
* **Dense foliage / evergreen species:** thresholds may fail for some species; tune thresholds per species and consider additional geometric filters (slope, normal consistency). Kumazaki showed their method works even with evergreen species when thresholds are tuned.
* **Occlusion / missing small branches:** TLS cannot capture all fine twigs; QSM will miss below-sampling branches and so underestimate total fine woody volume — document detection limits.
* **Over-smoothing destroys small branches:** choose conservative MLS/SOR settings.
* **Segmentation errors in dense stands:** manual correction or improved segmentation algorithms required.

---

## 12) Reproducibility & reporting (must include in any result)

For each processed tree / dataset include:

* Scanner model + raw settings and export options.
* Exact thresholds and parameters used: pulse deviation threshold, reflectance threshold, SOR/MLS parameters, TreeQSM parameters.
* Number of scan positions, total points before/after each major step, processing time per step.
* Validation statistics (mean & SD distances, sample histograms).
  Kumazaki’s paper provides this level of reporting and is the model to follow.

---

## 13) Useful software / libraries

* **TreeQSM** (InverseTampere) — QSM reconstruction.
* **Computree** — single-tree segmentation.
* **CloudCompare, PDAL, LAStools** — point cloud I/O, SOR, general ops.
* **KD-tree / FLANN / sklearn** — k-NN distance computations for validation.
* Scripting: Python with pyntcloud/pdal bindings, or custom pipelines.

---

## 14) Key literature to cite (from Kumazaki)

* Kumazaki (2021). *Application of 3D tree modeling using point cloud data by terrestrial laser scanner* (Landscape Research 84(5)).
* Raumonen, P.; InverseTampere: TreeQSM publications and GitHub.
* Du et al. (2019). *AdTree: Accurate, Detailed, and Automatic Modelling of Laser-Scanned Trees.* Remote Sensing.
* Bienert et al. (2018); Tanago et al. (2018) — TLS/biomass methods referenced by Kumazaki.

---

If you want, I will:

* convert this into a one-page protocol checklist with exact command examples for CloudCompare / PDAL / TreeQSM, or
* produce a YAML/JSON pipeline manifest that documents each file input/output and parameters so you can run it reproducibly.

Which of those should I produce next?
