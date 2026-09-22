.. _rst_History:

History
==============

mizuRoute was first developed in the early 2010s, when catchment- and
vector-based representations were emerging as an important alternative to
regular grid discretizations for large-domain hydrologic modeling. At the time,
most large-domain routing models were designed for gridded river networks
because they coupled naturally with gridded land surface models. These
approaches required the routing network to be regenerated whenever the
computational grid changed, and the resulting river networks did not faithfully
represent channel geometry, drainage areas, or river lengths, particularly at
coarser resolutions. The goal of mizuRoute was to provide a routing model that
could operate directly on vector river networks while remaining compatible with
the gridded workflows used by existing land surface models.

The origins of **mizuRoute** trace back to the river routing component of the
TopNet hydrologic model (:ref:`Bandaragoda et al., 2002 <Bandaragoda2004>`,
:ref:`Clark et al., 2008 <Clark2008>`), which was developed at the National
Institute of Water and Atmospheric Research (NIWA) in New Zealand (now part of
Earth Sciences New Zealand). The routing component implemented the
kinematic wave-tracking algorithm developed by Derek Goring
(:ref:`Goring, 1994 <Goring1994>`). As part of the TopNet modernization
documented by :ref:`Clark et al. (2008) <Clark2008>`, the original kinematic
wave-tracking implementation was rewritten in Fortran 95 to improve modularity
and use derived data types to organize river-network and model-state
information. This modernized implementation provided the software foundation
for the subsequent development of **mizuRoute**.

The initial development of **mizuRoute** involved extracting the modernized
kinematic wave-tracking (KWT) routing component from TopNet and further refactoring it into a
standalone routing model (:ref:`Mizukami et al., 2016 <Mizukami2016>`). The
principal objective was to generalize the wave-tracking algorithm from a
component embedded within a catchment model into a routing system capable of
operating over large, vector-based river networks. A second objective was to
provide an implementation of the impulse response function (IRF) routing scheme
of :ref:`Lohmann et al. (1996) <Lohmann1996>`, which was widely used in
continental-domain hydrologic prediction studies, including CMIP3- and
CMIP5-driven streamflow projection studies in the United States. Supporting
both the KWT and IRF routing schemes allowed mizuRoute to to apply the same routing methods used in existing prediction
workflows based on the Lohmann IRF approach, while also providing the
wave-tracking algorithm inherited from TopNet. The first application of
mizuRoute used the
U.S. Geological Survey Geospatial Fabric for the contiguous United States,
demonstrating the scalability of the approach to continental domains
(:ref:`Mizukami et al., 2016 <Mizukami2016>`).

As preparations began for the first **mizuRoute** publication, the routing
model required a distinct name. During discussions within our research group,
Andy Wood suggested **mizuRoute**, observing that the surname of the lead
developer, Naoki Mizukami, naturally divides into two parts, with the first
part, *Mizu* (水 in kanji), being the Japanese word for "water." The suggestion
was immediately adopted by the group. An additional advantage of the name is
that it is concise, memorable, and not an acronym, avoiding the long and often
difficult-to-remember names that are common among hydrologic models.

Since its initial release, mizuRoute has undergone several major rounds of
development that have substantially expanded its computational capabilities,
physical realism, and applicability. To support applications over increasingly
large, high-resolution river networks, Martyn Clark redesigned the
river-network connectivity algorithms, removing a key computational bottleneck
and substantially improving scalability. These improvements enabled mizuRoute
to be applied efficiently over continental and global domains.

A second major milestone was the development of a hierarchical river-network
decomposition algorithm for hybrid parallel computing (:ref:`Mizukami et al., 
2021 <Mizukami2021>`). Unlike structured atmospheric and ocean grids, river
networks form complex branching trees that cannot be partitioned using
conventional domain decomposition methods. The new algorithm decomposed river
networks into hydrologically independent tributary domains and nested
subdomains, enabling efficient hybrid MPI/OpenMP parallelization. This
innovation allowed mizuRoute to scale to global vector river networks, making it practical for large
ensemble simulations and coupling with Earth system models.

Subsequent development expanded the scope of mizuRoute beyond river routing to
include the explicit simulation of lakes and reservoirs within vector river
networks. Between 2020 and 2022, Shervan Gharari (University of Saskatchewan)
developed mizuRoute-Lake (:ref:`Gharari et al., 2024 <Gharari2024>`), which 
provided a flexible framework to represent both natural lakes and
managed reservoirs. Rather than relying on a single lake parameterization,
mizuRoute-Lake allows different water balance models (including the Döll, HYPE,
and Hanasaki formulations) to be selected for individual water bodies within the
same simulation. Building on this work, Inne Vanderkelen (Vrije Universiteit
Brussel) collaborated with Gharari to investigate the influence of natural
lakes and managed reservoirs on continental and global hydrology, demonstrating
the importance of representing inland surface waters for large-domain
hydrologic and Earth system simulations (:ref:`Vanderkelen et al., 2022 <Vanderkelen2022>`).

More recently, mizuRoute has become an integral component of reproducible,
modular hydrologic modeling workflows. mizuRoute provides the river-routing
component in standardized workflows developed for SUMMA and large-domain
hydrologic prediction (:ref:`Knoben et al., 2022 <Knoben2022>`,
:ref:`Farahani et al., 2025 <Farahani2025>`, :ref:`Tang et al., 2025
<Tang2025>`). These workflows have enabled reproducible applications spanning
local catchments to continental and global river networks.

**References**

.. _Bandaragoda2004:

Bandaragoda, C., Tarboton, D.G. and Woods, R. (2004). Application of TOPNET in
the distributed model intercomparison project. Journal of Hydrology, 298(1-4),
pp.178-201.
https://doi.org/10.1016/j.jhydrol.2004.03.038

.. _Clark2008:

Clark, M.P., Rupp, D.E., Woods, R.A., Zheng, X., Ibbitt, R.P., Slater, A.G., Schmidt, J. and Uddstrom, M.J. (2008).
Hydrological data assimilation with the ensemble Kalman filter: Use of streamflow observations to update states in a distributed hydrological model.
Advances in water resources, 31(10), pp.1309-1324.
https://doi.org/10.1016/j.advwatres.2008.06.005

.. _Farahani2025:

Farahani, M.A., Wood, A.W., Tang, G. and Mizukami, N. (2025). Calibrating
a large-domain land/hydrology process model in the age of AI: the SUMMA CAMELS
emulator experiments. Hydrology and Earth System Sciences, 29(18),
pp.4515-4537.
https://doi.org/10.5194/hess-29-4515-2025

.. _Gharari2024:

Gharari, S., Vanderkelen, I., Tefs, A., Mizukami, N., Kluzek, E., Stadnyk, T.,
Lawrence, D. and Clark, M.P. (2024). A flexible framework for simulating the
water balance of lakes and reservoirs from local to global scales:
mizuRoute‐Lake. Water Resources Research, 60(5), p.e2022WR032400.
https://doi.org/10.1029/2022WR032400

.. _Goring1994:

Goring, D. G. (1994)
Kinematic shocks and monoclinal waves in the Waimakariri, a steep, braided, gravel-bed river,
Proceedings of the International Symposium on waves: Physical and numerical modelling, University of British Columbia, Vancouver, Canada, 336–345.

.. _Knoben2022:

Knoben, W.J.M., Clark, M.P., Bales, J., Bennett, A., Gharari, S., Marsh, C.B.,
Nijssen, B., Pietroniro, A., Spiteri, R.J., Tang, G., Tarboton, D.G., and Wood,
A.W. (2022).
Community workflows to advance reproducibility in hydrologic modeling:
Separating model‐agnostic and model‐specific configuration steps in
applications of large‐domain hydrologic models. Water Resources Research,
58(11), p.e2021WR031753.
https://doi.org/10.1029/2021WR031753

.. _Lohmann1996:

Lohmann, D., Nolte-Holube, R. and Raschke, E (1996)
A large-scale horizontal routing model to be coupled to land surface parametrization schemes,
Tellus A, 48: 708-721
https://doi.org/10.1034/j.1600-0870.1996.t01-3-00009.x

.. _Mizukami2016:

Mizukami, N., Clark, M.P., Sampson, K., Nijssen, B., Mao, Y., McMillan, H.,
Viger, R.J., Markstrom, S.L., Hay, L.E., Woods, R. Arnold, J.R., and Brekke,
L.D. (2016). mizuRoute version 1: A river network routing tool for continental domain
water resources applications. Geoscientific Model Development, 9(6),
pp.2223-2238.
https://doi.org/doi:10.5194/gmd-9-2223-2016

.. _Mizukami2021:

Mizukami, N., Clark, M.P., Gharari, S., Kluzek, E., Pan, M., Lin, P., Beck,
H.E. and Yamazaki, D. (2021). A vector‐based river routing model for Earth
system models: Parallelization and global applications. Journal of Advances in
Modeling Earth Systems, 13(6), p.e2020MS002434.
https://doi.org/10.1029/2020MS002434

.. _Tang2025:

Tang, G., Clark, M.P., Knoben, W.J., Liu, H., Gharari, S., Arnal, L., Wood,
A.W., Newman, A.J., Freer, J. and Papalexiou, S.M. (2025). Uncertainty hotspots
in global hydrologic modeling: the impact of precipitation and temperature
forcings. Bulletin of the American Meteorological Society, 106(1),
pp.E146-E166.
https://doi.org/10.1175/BAMS-D-24-0007.1 

.. _Vanderkelen2022:

Vanderkelen, I., Gharari, S., Mizukami, N., Clark, M.P., Lawrence, D.M.,
Swenson, S., Pokhrel, Y., Hanasaki, N., Van Griensven, A. and Thiery, W., 2022.
Evaluating a reservoir parametrization in the vector-based global routing model
mizuRoute (v2. 0.1) for Earth system model coupling. Geoscientific Model
Development, 15(10), pp.4163-4192.
https://doi.org/10.5194/gmd-15-4163-2022

Last updated on July 25th, 2026
