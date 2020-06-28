master
======

*Changes*

- obspy.clients.filesystem
  * add get_waveforms_bulk() method to SDS client (see #2616, #2626)


maintenance_1.2.x
=================

- obspy.core
  * Fix wrong values in Stats object after deepcopy or pickle of Stats object
    for edge cases (see #2601)
- obspy.clients.fdsn
  * EIDA routing client: fix an issue that leaded to a request of *all* EIDA
    data when requesting an invalid, out-of-epochs time window for a valid
    station (see #2611)
  * update RASPISHAKE URL mapping to use https
- obspy.clients.neic
  * Make client socket blocking (see #2617)
- obspy.io.nordic
  * Fixed a bug raising an exception when reading a nordic file with a non
    positive-definite covariance matrix (see #2593)


1.2.1
=====
https://doi.org/10.5281/zenodo.3706479


*Changes*

- fix an installation issue with pip and setuptools version 46 (see #2578)
- fix response plots when providing `axes=...` with a numpy array of Axes
  instances (see #2579)


1.2.0
=====
https://doi.org/10.5281/zenodo.3674646

Work on this release was in parts and among others supported by the following
institutions/companies and grants (in alphabetical order):

- Earthquake Commision of New Zealand (EQC), grant 18/753
- École et Observatoire des Sciences de la Terre - Université de Strasbourg
- ETH Zürich
- Friedrich-Schiller-Universität Jena
- Geoscience Australia
- Incorporated Research Institutions for Seismology (IRIS), NSF (SAGE) award
  :: EAR-1851048
- Institut de Physique du Globe de Strasbourg
- Institut de Physique du Globe de Paris
- Institutions for Seismology (IRIS) through the PASSCAL Instrument Center at
  New Mexico Tech. The facilities of the IRIS Consortium are supported by the
  National Science Foundation under Cooperative Agreement EAR-1261681 and the
  DOE National Nuclear Security Administration.
- Istituto Nazionale di Geofisica e Vulcanologia, Osservatorio Etneo (Italy),
  Allegato B2 DPC-INGV 2012-2021 Task 10
- Ludwig-Maximilians-Universität München
- National Institute for Occupational Safety and Health
- Royal Netherlands Meteorological Institute (KNMI)
- School of Geography, Environment and Earth Sciences, Victoria University of
  Wellington
- The European Union’s Horizon 2020 research and innovation programme under
  the ChEESE project, grant agreement No. 823844
- The Royal Observatory of Belgium
- U.S. Geological Survey

*Changes*

- obspy.core
  * Inventory objects have been adapted to StationXML 1.1 for details on
    changes see #2510 and
    https://github.com/FDSN/StationXML/blob/master/Changes.md
  * Fixed import of custom plugins (see #2423)
  * Fixed "==" comparison for Stream and Trace which was very slow in case of
    traces with >1e6 samples (see #2377)
  * Added almost_equal method for Stream and Trace classes (see #2286).
  * Casting FDSN identifiers to strings upon setting in the stats dictionary
    (see #1997).
  * UTCDateTime objects will now always evaluate equal if their string
    representations are equal (see #2049).
  * UTCDateTime objects now issue depreciation warnings when setting any
    attributes outside of init, or comparing UTCDateTime objects with
    different precisions (see #2077).
  * UTCDateTime objects can now accept hour, minute, second, and microsecond
    values greater than their normal limits by setting the strict keyword
    argument to False (see #2232).
  * Fixed UTCDateTime(..., julday=366) for non-leap years. This was returning
    January 1st of the next year in case of non-leap years being used. Now it
    properly raises an out-of-bounds ValueError (see #2369)
  * When reading StationXML/SC3ML, make sure to properly read empty string
    fields as empty strings instead of "None" (see #2519 and #2527)
  * Better ISO8601 detection for UTCDateTime objects and UTCDateTime(...,
    iso8601=False) now completely disables ISO8601 handling (see #2447)
  * Added replace method to UTCDateTime class (see #2077).
  * Added remove method to Inventory class (see #2088).
  * Added id property to WaveformStreamID (see #2131).
  * Added __str__ and _repr_pretty_ method for Comment class (see #2115)
  * Added __eq__ to QuantityError so empty instances equal None (see #2185).
  * Reworked the event scoped resource identifiers for the event classes
    hopefully fixing all edge-cases (see #2091).
  * Added a hook to allow users to customize finding objects for
    resource_ids which are not found via the normal means (see #2279).
  * Calling Stream.write(...) on an empty stream will now raise an
    ObsPyException consistently across all I/O plugins (see #2201)
  * Stream.get_gaps() will now properly report gaps within Traces that
    have masked arrays (i.e. Traces that have been merged without a fill
    value, see #2299 and #2300).
  * Added copy method to Inventory class (see #2322).
  * The Response.recalculate_overall_sensitivity() method now accepts integers
    (see #2338, #2343).
  * Added wildcard and url support to read_inventory (see #2326).
  * Modified stream.get_gaps() to deal with overlaps correctly (see #1403)
  * Added option "label_epoch_dates" to Inventory/Network.plot_response() to
    optionally add channel epoch start/end dates to legend labels (see #2309)
  * Deprecated the convert_id_to_quakeml_uri, regenerate_uuid, and
    get_quakeml_uri methods of the ResourceIdentifier class (see #2303).
  * Added get_quakeml_uri_str and get_quakeml_id methods to the
    ResourceIdentifier class (see #2303).
  * New method to create response objects directly from poles and zeros (see
    #1962).
  * Added Stream.stack method (see #2440).
  * Added a component field to the Stats object which allows to get and set
    the last character of the SEED channel (see #2484).
  * Fixed a bug in Stream.plot(type='section', reftime=..., ...) that caused
    wrong relative start times of traces relative to given reftime (see #2493)
  * Fixed a Windows-specific path case issue in a helper function that returns
    a list of untracked files in the git repository (see #2296)
  * Fix a bug that was causing an exception being raised in `Response.plot()`
    because a float was being passed down to numpy.linspace as length of array
    (see #2533)
  * Geographic select of inventory/network/station (see #2515)
  * Select traces in Stream based on an Inventory (see #2531)
- obspy.clients.fdsn
  * Add new `_discover_services` boolean flag to the Client, which allows the
    Client to skip the initial services query at instantiation.  This can
    reduce the load on service providers, but skips checks against unsupported
    query parameters.
  * Adding more location codes to the default priority list in the mass
    downloader (see #2155, #2159).
  * The mass downloader now raises a warning if all channels from a station
    have been deselected due to the default location priorities setting. This
    is a pure usability improvement as it has been confusing users
    (see #2159).
  * Make sure that streams fetched via FDSN are properly trimmed to user
    requested times if data center serves additional data around the start/end
    (see #1887, #2298)
  * Fix a problem that could spam subprocesses that were not closed in routed
    clients (see #2342 and #2344)
  * Make it possible to use signed EIDA tokens and also skip token validation
    completely (see #2297)
  * Adding a mapping for RASPISHAKE
- obspy.clients.filesystem.tsindex
  * Add new Client & Indexer modules based on IRIS time series index (see
    #2206)
- obspy.clients.iris
  * Results of distaz method are now returned as native floats (see #2499)
- obspy.clients.neic
  * Properly use specified timeout value (see #2450)
- obspy.clients.seedlink
  * Add method "get_info()" to fetch information on what
    networks/stations/locations/channels are served by the seedlink server
    (see #2405)
  * "get_waveforms()" can now be used with '*' and '?' wildcards in any part
    of requested SEED ID, i.e. network, station, location and channel (see
    #2405)
- obspy.clients.seishub
  * Properly handle fetching poles and zeros in presence of multiple metadata
    files for a given station (see #2411)
- obspy.geodetics
  * New utility function `inside_geobounds()` to check whether an object is
    inside a geographic bound (see #2515)
- obspy.imaging
  * obspy-scan can now be used with wildcarded SEED IDs when specifying what
    to plot after scanning data (see #2227)
  * Fix a problem in Scanner when loading npz on Python3 that was written on
    Python2 (see #2413)
  * Fix an issue that could make small landmasses not get plotted in basemap
    plots (see #2471, #2477)
  * Fixed a bug in Stream.plot(type='section', reftime=..., ...) that caused
    wrong relative start times of traces relative to given reftime (see #2493)
- obspy.io
  * Added read support for receiver gather format v. 1.6 (see #2070)
  * Added read support for FOCMEC 'out' and 'lst' files (see #2156)
  * Added read support for HypoDD 'pha' files (see #2378)
- obspy.io.arclink
  * Accommodate change in SeisComP3 publicID delimiter from '#' to '/' in
    ArclinkXML (see #2552)
- obspy.io.dmx
  * Add read support for INGV's DMX format (see #2452)
- obspy.io.gcf
  * Fixes Python 3.8 compatibility of GCF reader. (see #2505)
- obspy.io.mseed
  * Fix a bug resulting in an infinite loop when trying to read a FullSEED
    file without any data records (see #2534 and #2535)
  * Add ability to write int64 data to mseed if it can safely be downcast
    to int32 data, otherwise raises ObsPyMSEEDError. (see #2356)
  * The recordanalyzer can now detect calibration blockettes 300, 310,
    and 320 (see #2370).
  * Can now write zero sampling-rate traces. (see #2488, 2509)
- obspy.io.nordic
  * Add ability to read and write focal mechanisms and moment tensor
    information. (see #1924)
  * Add explicit warnings regarding unsupported sections of Nordic files.
  * Fix mapping of magnitude-types between MS to S and Ms to s.
  * Output preferred origin when writing to Nordic format instead of using
    the first origin (see #2195)
  * Include high-accuracy phase-pick reading and writing - high-accuracy is
    now the default phase-writing format, a boolean flag `high_accuracy`
    has been added to turn this off. (see #2351 and #2348)
  * Allow long-phase names (both reading and writing) - longer than 4 char.
    (see #2351)
  * Include AIN as takeoff-angle when reading and writing nordic files
    (see #2404).
  * Add error ellipses and read high-accuracy hypocenter lines (see #2451)
  * Fix the incorrect handling of events missing pick evaluation information
    (see #2520)
- obspy.io.reftek
  * Implement reading reftek encodings '16' and '32' (uncompressed data,
    16/32bit integers, see #2058 and #2059)
- obspy.io.rg16
  * Implement module to read waveforms and headers from fcnt format (see #2265).
  * Fix reading when start+endtime are inside one data packet (see #2485).
- obspy.io.sac
  * Fix bug writing inventory with SOH channels to SACPZ (see #2200).
  * Blank-pad character header variables, as opposed to NUL-padding them, to
    comply with SAC behavior (see #2543).
- obspy.io.segy
  * Raise nicer error messages when header packing fails (see #2194, #2196).
  * Show nice error message when trying to write a trace with too many samples
    in it (see #2358, #1393)
- obspy.io.seg2
  * Handle data format code 3 trace data (#2022, #2385).
  * Improve parsing of free-form entries (#2385).
  * Fix non-native endian data loading (#2385).
- obspy.io.seiscomp
  * Adding support for SC3ML 0.10 (see #2024).
  * Update xsl to allow conversion of amplitude picks not associated with
    origins (see #2273).
  * Very large performance improvement reading large sc3ml inventory files by
    pre-indexing sensors, dataloggers and responses and reducing lxml calls
    (see #2296).
  * When reading StationXML/SC3ML, make sure to properly read empty string
    fields as empty strings instead of "None" (see #2519 and #2527)
- obspy.io.sh
  * Add read support for SeismicHandler EVT event files (see #2109)
- obspy.io.shapefile
  * Add possibility to add custom database columns when writing catalog or
    inventory objects to shapefile (see #2012 and #2305)
- obspy.io.stationxml
  * When reading StationXML/SC3ML, make sure to properly read empty string
    fields as empty strings instead of "None" (see #2519 and #2527)
  * Inventory objects have been adapted to StationXML 1.1 for details on
    changes see #2510 and
    https://github.com/FDSN/StationXML/blob/master/Changes.md
- obspy.io.quakeml
  * Allow writing invalid ids but raise a warning
    (see #2104, #2090, #2093, #1872).
  * Skip invalid enumeration values during reading but raise a warning.
    (see #2106, #2098, #2095)
  * Catalogs with empty event description objects can be round-tripped (see
    #2339, #2340).
  * Correctly handle QuakeML native namespaces (if they are not matching the
    document root's namespace) as custom namespaces and parse them into
    `.extra` (see #2466)
- obspy.io.xseed
  * Ability to parse SEED files with extra newlines between blockettes
    (see #2383)
- obspy.signal.cross_correlation
  * Add new `correlate_template()` function with 'full' normalization option,
    required for correlations in template-matching
    (see #2035 and #2042).
  * 'domain' parameter in correlate function is deprecated in favour of new
    'method' parameter to be consistent with recent SciPy versions
    (see #2042).
  * Add `correlate_stream_template()` and `correlation_detector()`
    functions to detect events based on template matching (see #2315)
- obspy.signal.PPSD
  * Changed numpy-based serialization as to not require pickling (see #2424).
  * Fixed exact trace cutting for PSD segments (see #2040).
  * Timestamp representations internally and in npz I/O were changed to use
    integer nanosecond POSIX timestamps to avoid any potential floating point
    inaccuracies and since this is also what UTCDateTime is based on nowadays
    (see #2045).
  * Fixed the check for new PSD slices whether they should be added or whether
    they would add unwanted duplicated data (see #2229).
  * Fix `period_lim` option when `xaxis_frequency=True` (see #2246).
  * Added `allow_pickle` parameter to `PPSD.add_npz` and `PPSD.load_npz` and
    set its default to `False` (see #2457).
  * Added representation of earthquake models from Clinton & Heaton (2002) in
    'plot()' method using option 'show_earthquakes' (see #2455).
- obspy.signal.polarization
  * Fix an issue with covariance matrix in vidale algorithm and make adaptive
    windowing opt-out (see #2565)
  * Fix an issue in selecting Z/N/E traces from given stream (see #2365)
- obspy.signal.trigger
  * Fix a bug in AR picker (see #2157)
  * Option to return Baer-Kradolfer characteristic function from pk_mbaer
    function added (see #2341)
- obspy.taup
  * Fix cycling through colors in ray path plots (see #2470, #2478)
  * Fix a floating point issue on IBM machines (see #2559, #2560)

1.1.1
=====
https://doi.org/10.5281/zenodo.1040770

- General
  * Tests pass with numpy 1.14 (see #2044).
  * Map plots now also work with matplotlib >= 2.2 (see #2089).
- obspy.core
  * UTCDateTime now raises a meaningful exceptions when passing invalid or
    out-of-bounds 'julday' during initialization (see #1988)
  * Fix pickling of traces with a sampling rate of 0 (see #1990)
  * read_inventory() used with non-existing file path (e.g. typo in filename)
    now shows a proper "No such file or directory" error message (see #2062)
  * Fix Trace.times(type='matplotlib') being slow (see #2112)
  * read_events() and read_inventory() now trial most common plugins first
    (QuakeML/StationXML, ...) in case of automatic file format detection (i.e.
    when file type was not explicitly specified, see #2113)
  * Event instances with Origin instances that have do not have defined
    latitude/longitude attributes will no longer raise a TypeError when
    creating a string representation (see #2119 and #2127).
  * Fix Stream.get_gaps() when a trace is completely overlapping another trace
    (see #2218).
  * Fix Exception when comparing ComparingObjects (see #2220).
  * Fix UTCDateTime.strftime() when year is <1900 on Python 2 (see #2167)
  * Inventory objects are more convenient to create now. Network, station, and
    channel codes can now be optional. Additionally the source parameter of
    inventories must no longer be specified at init time (see #2307, #2314).
- obspy.clients.arclink
  * Raise a warning at import time that the ArcLink protocol will be
    deprecated soon (see #1987).
- obspy.clients.fdsn
  * Mass downloader: Priority lists are now correctly overwritten if `channel`
    and/or `location` are set (see #1810, #2031, #2047).
  * A few fixes and stability improvements for the mass downloader (see
    #2081).
  * Fixed routing startup error when running under certain locales (see #2147)
  * Update the IPGP mapping (see #2268).
  * Adding a mapping for the KNMI (see #2270) services.
- obspy.clients.nrl
  * Set input units of overall sensitivity to input units of first stage in
    NRL.get_response() (see #2248)
- obspy.geodetics
  * Fix the vincenty inverse calculation for equatorial lines (see #2282).
- obspy.imaging
  * Normalize moment tensors prior to plotting in the mopad wrapper to
    stabilize the algorithm (see #2114, #2125).
  * fix some map plotting issues with cartopy and local projection (see #2193,
    #2204)
- obspy.io.ascii
  * Fixes an issue with the time representation (see #2165, #2179).
- obspy.io.cnv
  * Bugfix when phase_mapping is passed as argument when writing a Catalog
    object to CNV (see #2001)
- obspy.io.css
  * Fix automatic filetype detection (see #2160 and #2162)
- obspy.io.gcf
  * Fix reading stream ID for station/channel code in header (see #2289,
    #2311)
  * Fix bitmask in getting compression code (see #2290, #2310)
- obspy.io.mseed
  * Ability to read files that have embedded chunks of non SEED data. (see
    #1981, #2057).
  * Fix util.get_start_and_end_time returning sample rate = 0 when sample rate
    = 1 (see #2069)
  * Avoid showing invalid warnings when guessing endian during parsing
    timestamps (see #1988)
  * util.get_record_information() now works correctly for negative sampling
    rate factors and multipliers (see #2030, #2191).
- obspy.io.nordic
  * Bug-fix for amplitudes without magnitude_hint (see #2021)
  * Bug-fix for wavefiles with full path stripping (see #2021)
  * Bug-fix for longitudes between -100 and -180 (see #2197)
- obspy.io.reftek
  * Fix problems reading some Reftek 130 files, presumably due to floating
    point accuracy issues in comparing timestamps. Internal representation of
    time stamps is changed to integer nanosecond POSIX timestamp (see #2036,
    #2038, #2105)
  * Fix a bug that prevents reading files that have no data in first channel
    (see #2101)
- obspy.io.sac
  * Allow passing on the byteorder flag from the top-level `obspy.read()`
    function (see #2285, #2292).
- obspy.io.seiscomp
  * Fix inventory read when maxClockDrift is unset in SC3ML (see #1993)
  * Fix the reading of FIR coefficients when multiple whitespaces in SC3ML
    (see #2259)
  * Fix the reading of the poles and zeros when multiple whitespaces in SC3ML
    (see #2260).
  * Fix reading files with zero sampling rates (see #2294 and #2293)
  * Fix divide by zero error when parsing sc3ml files of zero sampling rage
    (see #2294).
- obspy.io.stationxml
  * Allow writing of dates before 1900 also on Python 2 (see #2013, #2015).
  * Write the UTC time zone specifier to all times (see #2015).
  * Units of first response stage as well as unit response stages are now
    determined with some heuristics (see #2250, #2318).
- obspy.io.xseed
  * Third condition to split blockettes when reading RESP files. Now more
    forgiving for slightly different files (see #2170, #2189)
- obspy.signal
  * Allow singular COUNT units in evalresp (see #2003, #2011).
  * Fix an evalresp issue in case of an analog PAZ stage zero denominator (see
    #2171 and #2190)
  * PPSD: for safety reasons, raise an ObsPyException if trying to read a PPSD
    npz file that was written with a newer version of the npz representation
    than is used by current ObsPy version (see #2051)
  * The ar_pick() trigger function now raises an error if the three data
    arrays don't have the same length (see #1801, #2148).
  * fix a precision issue in AR picker in case of low amplitude input (see
    #2252 and #2253)
- obspy.taup
  * Fallback to linear slowness interpolation for very small and shallow
    layers (see #2126, #2129).
  * Fix bug preventing constant-velocity models with discontinuities at every
    layer boundary from being built (see #2264).
  * More robust resize method so TauPy now works properly on Python 3.7 (see
    #2280, #2319).

1.1.0
=====
https://doi.org/10.5281/zenodo.165135

- General
  * Read support for Guralp Compressed Format (GCF) waveform data,
    obspy.io.gcf (see #1449)
  * Read support for Reftek 130 (rt130) waveform data,
    obspy.io.reftek (see #1433)
  * Add Nordic format (s-file) read/write (see #1517)
  * Read and write support for events in the SCARDEC catalogue format
    (see #1391).
  * Read support for IASPEI ISF ISM 1.0 Bulletin event data,
    (see #1946)
  * Write support for AH (Ad Hoc version 1) format (see #1754)
  * Client to access the Nominal Response Library (NRL) (see #1185).
  * `obspy.read_inventory()` can now read dataless SEED and RESP files
    (see #1185).
  * change version number scheme for scenarios when no official version number
    can be determined (see #1889 and #1916)
  * Support for the IRIS Federator and EIDAWS FDSNWS web routing services
    (see #1779 and #1919).
- obspy.core
  * UTCDateTime is now based on nanoseconds (long) instead of a unix
    timestamp in microseconds (float) - resulting in higher precision and
    support for years 1-9999 (see #1325)
  * Ensure that Trace.data is always C-contiguous in memory (see #1732)
  * Event/ResourceIdentifier is now object aware, meaning even if two
    objects share a resource_id the distinct objects will be returned with
    the get_referred_object method provided both are still in scope. If one
    of the objects gets garbage collected, however, a warning will be issued
    and the behavior will be the same as before (see #1644).
  * Better error message when attempting to write invalid QuakeML resource
    ids (see #1699).
  * Stream/Trace.write() can now autodetect file format from file extension
    (see #1321).
  * New convenience property `.matplotlib_date` for `UTCDateTime` objects to
    get matplotlib datetime float representation (which can be used in
    time-based matplotlib axes, e.g. by Stream.plot(); see #1339).
  * Trace.times() has new options `type` and `reftime` to support fetching an
    array of sampletimes in various different timing varieties ("relative":
    the old default, float relative to trace starttime or `reftime` in
    seconds; "utcdatetime": absolute times as UTCDateTime objects;
    "timestamp": array of float POSIX timestamps, compare
    `UTCDateTime.timestamp`; "matplotlib": array of float matplotlib dates,
    useful for plotting on matplotlib time axes; see #1307)
  * A trace's stats.network/station/location/channel can now also be set in
    one line using a SEED ID string (e.g. `trace.id = "GR.FUR..HHZ"`,
    see #1439).
  * Instrument correction for response list stages originating from inventory
    objects (see #1514).
  * `Stream.rotate(...)` can now also be used to rotate unaligned channels to
    Z-N-E, given an Inventory (see #1310)
  * Non finite floats (NaN, inf, -inf) can now no longer be set for all
    event objects (see #1597).
  * Instrument responses can now also be calculated for a given list of
    frequencies (see #1598).
  * Order of extra tags for event type classes serialized to QuakeML can now
    be controlled by using an OrderedDict (see #1617)
  * Bode plots can now optionally plot the phase in degrees (see #1763).
  * `Stream.select()` now also works on the component level if channels only
    have one letter (see #1847).
  * Now strips all invalid characters from the temporary filenames used for
    downloading data using the `read_X()` methods (see #1958).
- obspy.clients.earthworm
  * Much faster trace unpacking (see #1762).
- obspy.clients.fdsn
  * empty SEED codes (e.g. ``network=''``) will now be properly sent to the
    server as options and not omitted, which led to wildcard matching (for
    details see #1578)
  * The mass downloader now has `exclude_networks` and `exclude_stations`
    arguments to not download certain pieces of data. (see #1305)
  * The mass downloader can now download stations that are part of a given
    inventory object.
  * The mass downloader now also works with restricted data. (See #1350)
  * No data (HTTP 204) responses now raise `FDSNNoDataException` rather than
    the more general `FDSNException`.
  * Fixing cross implementation of bulk waveform and station requests (see
    #1685).
  * Adding mappings for the TEXNET (see #1852) and the ICGC (see #1902)
    services.
  * Support for the non-standard EIDA token authentication (see #1928).
- obspy.imaging
  * The functionality behind the `obspy-scan` command line script has been
    refactored into a `Scanner` class so that it can be reused in custom
    workflows. (see #1444)
  * Almost always return the figure handle for the plots to make it work with
    the jupyter notebooks (see #1615).
- obspy.imaging.cm
  * new colormap: viridis_white. This is a modification of viridis that
    goes to white instead of yellow but remains perceptually uniform. It
    is especially useful for printing when an image should merge with the
    white background.
- obspy.imaging.waveform
  * Support for filling the wiggles when plotting sections (horizontal and
    vertical, see #1445).
- obspy.io.arclink
   * Read support for Arclink Inventory XML (see #1539)
   * default for `route` parameter in metadata requests is changed to `False`
    (see #1756)
- obspy.io.ascii
   * Custom formatting of sample values when writing SLIST and TSPAIR.
- obspy.io.datamark:
   * Renamed without deprectation to obspy.io.win to match its original name.
    Datamark is a datalogger, saving the WIN format.
- obspy.io.gse2
   * Read support for GSE2.0 bulletin (see #1528)
- obspy.io.nlloc
   * Also parse author information and COMMENT line (see #1484)
   * Fix reading hypocenter files created by NonLinLoc versions of the 6.0.x
    beta branch (see #1760 and #1783)
   * Fix reading NonLinLoc files that have 24:00 as hours or 60 seconds (see
    #2222, #2324)
- obspy.io.quakeml
   * Read and write support for nested custom tags (see #1463)
   * Fix some minor bugs that could lead to empty stub elements, e.g. like
    empty MomentTensor when reading and later writing again a QuakeML file
    with a FocalMechanism but no MomentTensor, potentially resulting in
    QuakeML files that breach the QuakeML schema (see #1896)
- obspy.io.seiscomp
   * Read and write support for SC3ML event (see #1638 and #1848)
   * Fix bug where files with arbitrary publicIDs and files with missing
    depth, latitude, longitude, or elevation tags could not be read
    (see #1817)
- obspy.io.stationtxt
   * Write support for stationtxt format (see #1466)
- obspy.io.stationxml
   * Read and write support for custom tags (see #1024)
   * No longer add the (unused) time zone field to StationXML datetimes to
    follow the example of big data centers. (see #1572)
   * Level of detail can be specified during inventory write (see #1830)
    using the level keyword (one of: network, station, channel, response).
   * Skip empty and incomplete channels during reading (see #1839, #1840).
- obspy.io.segy
   * Fixing an issue when comparing two still packed SEG-Y trace headers
    (see #1735).
   * Iterative reading of large SEG-Y and SU files with
    `obspy.io.segy.segy.iread_segy` and `obspy.io.segy.segy.iread_su`.
    (see #1400).
   * Write correct revision number (see #1737).
   * Textual headers will now always contain the file revision number and the
    end header mark if nothing else exists at these positions (see #1738).
   * The SEG-Y format detection now also checks the format version number
    (see #1781).
   * Enable reading SEG-Y files that have day of year 0 in trace header
    (see #1722).
   * Write textual file headers also if given as a text string
    (see #1811, #1813).
- obspy.io.css
  * Read support for NNSA KB Core format waveform data. (see #1332)
- obspy.io.mseed
  * New generic get_flags() utility function able to retrieve statistics
    about all fixed header flags and the timing quality. This makes the
    get_timing_and_data_quality() function obsolete which is thus
    deprecated and will be removed with the next release. The get_flags()
    function is also much faster. (see #1141)
  * Always hook up the libmseed logging to its Python counterpart to avoid
    some rare segfaults. (see #1658)
  * Update to libmseed v2.19.5 (see #1703, #1780, #1939).
  * Correctly read MiniSEED files with a data offset of 48 bytes (see #1540).
  * InternalMSEEDReadingError now called InternalMSEEDError and
    InternalMSEEDReadingWarning now called InternalMSEEDWarning as both
    can now also be raised in non-reading contexts (see #1658).
  * Should no-longer segfault with arbitrarily truncated files (see #1728).
  * Will now raise an exception when attempting to directly read mini-SEED
    files larger than 2048 MiB (#1746).
  * `.stats.mseed` attributes are no longer per-file but per-trace where
    applicable (see #1782).
  * `get_record_information()` - Don't fail if the word order is invalid.
- obspy.io.nlloc
  * Set preferred origin of event (see #1570)
- obspy.io.nordic
  * Add Nordic format (s-file) read/write (see #1517)
- obspy.io.win
  * see obspy.io.datamark.
- obspy.io.xseed
  * Added azimuth and dip to the get_coordinates() function. (see #1315)
  * Fixing some issues with the get_resp() output on Python 3 (see #1748).
  * Can now also parse RESP files (see #1185).
  * Can transform responses in the Parser object to ObsPy Inventory objects
    (see #1185).
- obspy.scripts
  * obspy-scan command line script now also plots and prints overlaps
    alongside gaps (see #1366)
  * obspy-plot now has option to disable min/max plot (see #1583)
- obspy.signal
  * fixed a bug in calibration.rel_calib_stack (resulting amplitude response
    had wrong scaling if using non-default "overlap_fraction", see #1821)
  * fixed a bug in coincidence_trigger() with event templates. when a template
    with mismatching SEED ID was encountered all following (potentially valid)
    templates were skipped as well (see #1850)
  * New obspy.signal.quality_control module to compute quality metrics from
    MiniSEED files. (see #1141)
  * New correlate function for calculating the cross-correlation function
    (new implementation based on Scipy).
    To calculate the shift of the maximum of the cross correlation use
    xcorr_max. The old xcorr function is deprecated but currently still
    exists (see #1585).
  * New obspy.signal.regression module to compute linear regressions, with or
    without weights, with or without allowing for an intercept. (see #1716,
    #1747)
  * add new plotting capabilities to PPSD (temporal variations per frequency
    and spectrogram-like plot) and also make underlying processed PSDs
    available via `PPSD.psd_values` property (see #1327)
  * Fixed bug in `rotate2zne()` for non-orthogonal configurations
    (see #1913, #1927).
  * Fixed build warnings in evalresp, partially backported from evalresp
    4.0.6 (see #1939).
- obspy.taup
  * Add obspy.taup.taup_geo.calc_dist_azi, a function to return the distance,
    azimuth and backazimuth for a source - receiver pair. (see #1538)
  * Fixing calculations through very small regional models. (see #1761)
  * Updated ray path plot method, added travel time plot method, and wrapper
    functions for both ray path and travel time plotting. (see #1501, #1877)

1.0.3
=====
https://doi.org/10.5281/zenodo.165134

- obspy.core
  * properly pass through kwargs specified for Trace.plot() down to the
    low-level plotting routines (e.g. events were not shown properly in
    dayplot of a trace, see #1566)
  * properly pass through kwargs from Stream.detrend() to Trace.detrend()
    (see #1607)
  * Correctly splitting masked arrays in Trace objects for a couple of corner
    cases (see #1650, #1653).
- obspy.core.event.source
  * Fix `farfield` if input `points` is a 2D array. (see #1499, #1553)
- obspy.clients.earthworm
  * Better end of stream detection. (see #1605)
  * More efficient unpacking of server response. (see #1680)
- obspy.clients.neic
  * Better end of stream detection. (see #1563)
- obspy.clients.seedlink
  * Better end of stream detection. (see #1605)
- obspy.clients.seishub
  * Fix wrong kwargs `first_pick` and `last_pick` in
    `Client.event.get_list()`. (see #1661)
- obspy.io.mseed
  * ObsPy can now also read (Mini)SEED files with noise records. (see #1495)
  * ObsPy can now read records with a data-offset of zero. (see #1509, #1525)
  * ObsPy can now read MiniSEED files with micro-second wrap arounds.
    (see #1531)
  * ObsPy can now read MiniSEED files with no blockette 1000. (see #1544)
  * ObsPy now always writes Blockette 100 if sampling rate accuracy is
    otherwise lost. (see #1550)
  * obspy.io.mseed.util.set_flags_in_fixed_header() now works with Python 3
    and also for files with Blockette 100 (see #1648).
- obspy.io.quakeml
  * write StationMagnitude.residual even when it is zero (see #1625)
  * read & write Event.region
- obspy.io.sac
  * `SACTrace.lpspol` and `lcalda` are `True` and `False` by default, when
     created via `SACTrace.from_obspy_trace` with a `Trace` that has no SAC
     inheritance. (see #1507)
  * Reference time not written to SAC file when made from scratch
    (see #1575)
  * Reinforce ASCII encoding in reading non-ASCII SAC files regardless of
     default encoding setting. (see #1768)
  * Implementing encoding flag for reading/writing SAC files.
  * Fix passing through "byteorder" kwarg from obspy.read() (see #2292)
- obspy.io.sh
  * Fix writing of long headers for Python 3 (see #1526)
  * Whitespace in header fields is not ignored anymore (see #1552)
- obspy.io.stationxml
  * Datetime fields are written with microseconds to StationXML if
    microseconds are present. (see #1511)
- obspy.io.zmap
  * Use first origin/magnitude when writing to zmap if no origin/magnitude is
    set as preferred. (see #1569)
  * Parse origin time seconds as a float to avoid losing accuracy (see #1573)
- obspy.signal
  * PPSD: fix warning message on Python 3 that gets shown when waveforms and
    metadata mismatch (see #1506)
- obspy.taup
  * Allow for more than 10 phases with identical names (can happen for certain
    custom models, see #1593).

1.0.2
=====
https://doi.org/10.5281/zenodo.49636

- obspy.core
  * Added workaround for numpy issue where many FFTs of various lengths fill
    a cache that never gets cleared, effectively creating a memory leak
    (see #1424).
  * Trace.filter and Stream.filter don't work on masked arrays anymore because
    it produced unpredictable results due to the un-initialized data-chunk.
    The uninitialized masked gap is now also initialized to np.nan in case
    of floating point data which and a consistent fill value in case of
    integer data. (see #1363)
- obspy.clients.fdsn
  * Fixing issue with location codes potentially resulting in unwanted data
    to be requested. (see #1422)
  * Included low-gain seismometers in default channel filters in
    mass-downloader, also included non-oriented channels by default
    (see #1373).
- obspy.db
  * Fixed a bug in obspy-indexer command line script (see #1369,
    command line script was not working, probably since 0.10.0)
- obspy.imaging
  * Fixed a bug that leads to pressure/tension color blending when plotting
    semi-transparent DC beachball patches (i.e. with "alpha" not equal to 1,
    see #1464)
  * Fixed a bug when plotting non-DC beachball patches without fill colors
    (i.e. with "nofill=True", see #1464)
  * Fix arbitrary units in waveform section plot's offset axis, making it
    possible to add customizations to the plot afterwards (see #1382, see
    #1383)
- obspy.io.ascii
  * Fixed a bug that lead to wrong header information in output files when
    writing non-integer sampling rate data to SLIST or TSPAIR formats
    (see #1447)
- obspy.io.cmtsolution
  * Make sure newer CMTSOLUTION files can also be read (see #1479).
- obspy.io.gse2
  * Fixed a bug that could lead to network code not present in GSE2 output
    (see #1448)
- obspy.io.mseed
  * Fixed a bug in obspy-mseed-recordanalyzer (see #1386)
- obspy.io.nlloc:
  * Use geographic coordinates from the NonLinLoc Hypocenter-Phase file if
    no custom coordinate converter is provided. (see #1390)
  * Fix reading NonLinLoc Hypocenter-Phase files with more than one
    hypocenter in it. (see #1480)
  * Fix reading NonLinLoc Hypocenter-Phase files with unicode characters in
    them. (see #1483)
- obspy.io.quakeml
  * Fixed issue with improperly raised warnings when the same file is read
    twice. (#1376)
  * Fix writing empty network/station/channel codes in WaveformStreamID
    objects to QuakeML. (see #1483)
- obspy.io.sac
  * Try to set SAC distances (dist, az, baz, gcarc) on read, if "lcalda" is
    true.  If "dist" header is found, distances aren't calculated.
  * SACTrace class returns header values as native Python types instead of
    NumPy types.
  * SACTrace.iqual is no longer accepts enumerated string values, but
    arbitrary integer values. (see #1472)
  * SACTrace.read now replaces non-ASCII and null-termination characters in
    string headers with whitespace unless the "debug_strings=True" flag is
    used. (see #1432)
- obspy.io.stationxml
  * Always set the number attribute for poles and zeros. (see #1481)
- obspy.signal
  * PPSD.plot(): fix plotting of percentiles, mode and mean and setting
    period limits when using "xaxis_frequency=True" (see #1406, #1416)
  * Work around a bug in SciPy that results in wrong results for bandpass
    filter when using Nyquist frequency (or higher) as high corner of the
    passband (see #1451)
- obspy.taup
  * Fixing path for Pn. (see #1392)

1.0.1
=====
https://doi.org/10.5281/zenodo.48254

- General
  * Some methods might have unnecessarily upcasted float32 arrays to float64.
    Now methods for which it makes sense and which don't lose accuracy don't
    upcast float32 arrays. Integers are still upcasted. Trace.resample() will
    also no longer return the original dtype which might have resulted in a
    large loss of accuracy but it now always returns float64 arrays.
    (see #1302)
- obspy.core
  * Trace.normalize() does no longer divide by zero in case an all-zeros
    data trace is being used. (see #1343)
  * Inventory.select() and consorts now behave as expected even with empty
    child elements. (see #1126, #1348)
  * Code formatting is no longer checked for clean release versions. Thus
    updates to the linters no longer break the tests for releases.
    (see #1312)
  * remove_response(..., pre_file=None, plot=True) works again. (see #1320)
- obspy.clients.arclink
  * Restored ArcLink encryption support. (see #1352, #1347)
- obspy.clients.fdsn
  * Local URLs are now recognized as valid URLs. (see #1309)
  * Some bug fixes for the mass downloader. (see #1293, #1304)
  * The NOA node has been added to the list of known nodes.
    (see 2347a25714bc3e16068031f4b6138fafd627d34e)
- obspy.io.sac
  * More automatic merging of SAC and ObsPy headers. The new `obspy.io.sac`
    modules thus behaves more like the old one and more in line with
    expectations of users. (see #1285)
  * No more out of bounds errors when assigning coordinates. (see #1300)
  * The evdp header can be set again. (see #1345)
  * Correctly propagating sampling rate changes to the SAC headers.
    (see #1317)
  * Always set nvhdr, leven, lovrok, iftype to ensure valid SAC files.
    (see #1204)
- obspy.io.xseed
  * The Parser.get_paz() method now works with multiple blockette 53s.
    (see #1281)
- obspy.taup
  * Fixed wrong azimuth direction for paths > 180 degrees distance (see #1289)
  * Azimuth is appended to arrivals as well (see #1289)
  * Fixed issue with taup cache function on Python 2.7. (see #1308)

1.0.0
=====
https://doi.org/10.5281/zenodo.46151

- General
  * Requirements have been increased to reflect latest distributions:
    * Removed support for Python 2.6.
    * Added support for Python 3.5.
    * matplotlib >= 1.1.0 is now required.
    * numpy >= 1.6.1 is now required
    * scipy >= 0.9.0 is now required
  * Reorganized the submodule structure. We provide a deprecation path so the
    old imports will continue to work for one ObsPy version.
  * Consistent naming scheme across the code base. This results in some
    functions having different names. Most things that worked with ObsPy 0.10
    will continue to work with this version, but starting with the next
    version they will fail. Pay attention to the deprecation warnings.
  * Support for additional waveform data formats:
    - Read support for the ASCII format for waveforms from the K-NET and
      KiK-net strong-motion seismograph networks.
  * Support for additional event data formats:
    - CMTSOLUTION files used by many waveform solvers.
    - ESRI shapefile write support, useful in GIS applications (see #1066)
    - Google Earth KML output.
  * Support for additional station data format:
    - The FDSN web service station text format can now be read.
    - Read support for the NIED's moment tensor TEXT format (see #1125)
    - Google Earth KML output.
    - Read support for SeisComP3 inventory files.
- obspy.core
  * New method for generating sliding windows from Stream/Trace windows.
    (see #860)
  * Stream/Trace.slice() now has the optional `nearest_sample` argument from
    Stream/Trace.trim().
  * Trace.remove_response() now has `plot` option to show/output a plot of all
    individual steps of instrument response removal in frequency domain
    (see #1116).
  * New method Stream/Trace.remove_sensitivity() to remove instrument
    sensitivity
  * Fix incorrect parsing of some non-ISO8601 date/time strings. (see #1215)
  * Added plotting method to Event (customizable subplots from a selection
    of map, beachball and farfield radiation plots, see #1192)
- obspy.clients.fdsn
  * Replace FDSN webservice shortcut `NERIES` with `EMSC` and deprecate the
    `NERIES` shortcut, will be removed in a future release (see #1146).
  * Now requests gzipped data for the XML files. Much smaller files!
  * The station service can now also be used to download files in the text
    format. This has limited information but is much faster.
  * New mass downloader to assist in downloading data across a large number
    of FDSN web services.
  * Catch invalid URLs when initialising Client and avoid confusing error
    messages (see #1162)
- obspy.clients.filesystem.sds
  * New client to read data from local SDS directory structure (see #1135).
  * Command line script `obspy-sds-report` to generate html page with
    information on latency, data availability percentage and number of gaps
    for a local SDS archive (see #1202)
- obspy.clients.neries
  * Removed the dedicated client. Data can still be accessed by using the FDSN
    client.
- obspy.clients.syngine
  * New client for the IRIS Syngine service to retrieve custom synthetic
    seismograms.
- obspy.imaging
  * Experimental support for Cartopy when plotting maps. Use the `method`
    argument to functions that plot maps to select between Basemap or Cartopy.
  * New default colormap for all plots. A backport of the new viridis colormap
    from matplotlib is available for those using older matplotlib releases.
  * Added plotting routines for farfield radiation patterns of moment tensors
- obspy.io.kml
  * New module for Google KML output of Inventory and Catalog objects
    (e.g. for use in Google Earth)
- obspy.io.mseed
  * Upgrade to libmseed 2.16
- obspy.io.seiscomp.sc3ml
  * New module reading SeisComP3 inventory files to ObsPy inventory objects
    (see #1182).
- obspy.io.shapefile
  * New module for ESRI shapefile write support (see #1066)
- obspy.io.stationtxt
  * New module reading the FDSN station files.
- obspy.signal
  * Switch to second-order sections for filters; backported from SciPy 0.16.0
    (see #1028)
  * New Lanczos interpolation/resampling (see #1101)
  * Higher order detrending methods (see #1173)
  * PPSD (see #931, #1108, #1130, #1187):
    - Algorithm for PSD computation was improved, especially affecting results
      at long periods (for detailed discussion see #931 and #1108).
    - Keywords `paz` and `parser` were removed in favor of new keyword
      `metadata`. PPSD now accepts `metadata` in a much wider range
      of formats:
      * Inventory objects (e.g. from StationXML or from FDSN webservice)
      * obspy.io.xseed Parser objects (e.g. from dataless SEED file)
      * filename of a RESP file
      * dictionary with poles and zeros information (like in prior versions)
      Most old codes should still work, issuing a deprecation warning, but
      old code that specifies *both* `paz` and `parser` keywords will raise
      an exception.
    - Whenever possible (i.e. when using for `metadata` an Inventory,
      a Parser or a RESP file), response calculation now takes into account
      the full response (all stages) as opposed to only using the poles and
      zeros response stage (as was done in previous versions when using a
      Parser object). When using a poles and zeros dictionary response
      calculation is unchanged (as no information on other stages is
      available, of course).
    - PPSD now stores the psd for each time segment that gets processed,
      instead of only storing the stacked histogram. That way, differing
      custom stacks with various selection criteria (e.g. time of day, by
      weekday, etc.) can now be made from the same processed data
      (see #1130).
    - New save/load mechanism using numpy .npz binary format that circumvents
      some problems with the old pickle mechanism:
      `PPSD.save_npz()` and `PPSD.load_npz()` (and `PPSD.add_npz()` to add
      data from additional npz files)
    - Change default colormap to new obspy default sequential colormap
      (matplotlibs new viridis colormap). The old PQLX colormap is provided by
      `obspy.imaging.cm.pqlx` and can be used with
      `PPSD.plot(..., cmap=...)`.
    - new option `PPSD.plot(..., cumulative=True)` for a cumulative plot of
      the histogram, i.e. a non-exceedence percentage visualization, similar
      to the `percentile` option.
    - x axis in `PPSD.plot()` can be switched to frequency in Hz with
      `PPSD.plot(..., xaxis_frequency=True)` (see #1130)
    - changes to special handling of rotational: now handled by kwarg
      `special_handling="ringlaser"` (kwarg `is_rotational_data` is
      deprecated, see #916)
    - special handling option for hydrophone data (no differentiation, see
      #916)
    - bin width on frequency axis can now be controlled using
      `PPSD(..., frequency_bin_width_octaves=...)` (in fractions of octaves,
      default is the old fixed setting of 1/8 octaves as in PQLX)
- obspy.taup
  * Added support for nd file format for input velocity models. Allows for
    named discontinuities at arbitrary depths allowing for less Earth like
    models (see #1147).
  * Added three methods (`get_travel_times_geo()`, `get_pierce_points_geo()`
    and `get_ray_paths_geo()`) to `TauPyModel` to handle station and
    event location data as latitude and longitude, instead of the source to
    station distance in degrees. In addition `get_ray_paths_geo()` and
    `get_pierce_points_geo()` decorate the returned pierce points and ray
    paths with the latitude and longitude of each point. Some functionality
    needs the `geographiclib` module to be installed. (See #1164.)
  * ObsPy now ships with a bunch of new velocity models in addition to the
    existing ones: `prem`, `sp6`, `1066a,b`, `herrin` (See #1196).
  * Add support for buried receivers (see #1103.)
  * Port more accurate calculation of ray parameter from Java. The effect is
    stronger for longer phases, but also corrects issues with shorter body and
    surface waves (see #986.)
  * Fix incorrect branch splitting which also caused issues for extremely
    shallow phases (see #1057.)
  * Proper cache for model splits resulting in much faster calculations if
    the source depth is repeatedly the same (see #1248).


0.10.3
======
https://doi.org/10.5281/zenodo.46150

- obspy.core
  * Fix reading of multiple catalog files using globs (see #1065).
  * Fixed a bug when using
    `Trace.remove_response(..., water_level=None)`.
    With that setting that is supposed to not use any water level
    stabilization in the inversion of the instrument response
    spectrum actually the instrument response was never inverted and
    thus instead of a deconvolution a convolution was performed
    (see #1104).
  * Fixing floating point precision/rounding issue with UTCDateTime when
    initializing with floating point seconds, i.e. with microseconds,
    that could lead to microseconds being off by 1 microsecond
    (see #1096)
  * Correct gap/overlap time returned by Stream.get_gaps() and printed
    by Stream.print_gaps() which was incorrect by one time the sampling
    interval (see #1151)
  * Stream.get_gaps(): return overlaps specified in units of samples
    as negative integers (see #1151)
- obspy.fdsn
  * More detailed error messages on failing requests (see #1079)
  * Follows redirects for POST requests (see #1143)
- obspy.imaging
  * fix some bugs in `obspy-scan` (see #1138)
- obspy.mseed
  * Blockette 100 is now only written for Traces that need it. Previously
    it was written or not for all Traces, depending on whether the last
    Trace needed it or not. (see #1069)
  * Fixed a bug that prevented microsecond accuracy for times before 1970
    (see #1102).
  * Updated to libmseed 2.17.
- obspy.signal
  * Bug fixed within rotate.rotate2zne(). Additionally it can now also
    perform the inverse rotation (see #1061).
  * Bug fixed in triggering. When using option `max_len_delete` and a trigger
    occurred right before the end of data, one trigger was potentially lost
    (see #1145 for details).
- obspy.station
  * Plotting responses across multiple channels is more robust now in
    presence of some strange channels (e.g. with zero sampling rate,
    happens e.g. for state of health channels, see #1115)
  * ObsPy no longer assumes that the StationXML namespace is the default
    namespace (see #1060).
  * Checking if a file is a StationXML file is less rigorous (and much
    faster) now (not checking strict validity against xsd schema but
    only looking for a FDSNStationXML root element, see #1114).
    This means that `read_inventory()` without explicitly specified
    format will correctly detect more files as StationXML that have very
    slight breaches of the schema but still can be interpreted as
    StationXML.
  * fix saving `xcorrPickCorrection()` results to an image file (see #1154)
- obspy.taup
  * Calculating arrival times for surface waves now works (see #1055)
  * Calculating arrivals for underside reflections now works (see #1089)
- obspy.y
  * correct misspelled name of a Y specific header field (see #1127)
- obspy.zmap
  * Add support for time values with sub-second precision (see #1093)


0.10.2
======
https://doi.org/10.5281/zenodo.17641

- obspy.core
  * Fix catalog plot with events that have no origin depth or
    origin time (see #1021)
- obspy.datamark
  * Fix datawide=3 and datawide=0.5 block reading (see #1016)
- obspy.earthworm
  * Fix Python3 compatibility problem
- obspy.imaging
  * Fix flipped maps due to bug in Basemap
  * Fix handling of velocity reduction in section plots with degree offsets
    (see #1029)
  * Allow section plots over existing figures by not modifying existing lines
  * Don't prematurely close waveform figure if returning its handle
- obspy.seedlink
  * Fix Python3 compatibility for seedlink.easyseedlink
  * Basic seedlink client: Properly timeout requests with valid station
    selection but no data available in selected time window (see #1045)
- obspy.signal
  * Fix return data types/values of polarization routines (see #1026)
- obspy.station
  * Fix URL to FDSN StationXML schema in StationXML output (see #1023)


0.10.1
======
https://doi.org/10.5281/zenodo.16248

- minor changes for correct distribution of official release
  tar/zipball (see #993, #994)
- one minor encoding-related bugfix in mopad script (see #992)

0.10.0
======
https://doi.org/10.5281/zenodo.16200

- Highlights
  * Python3 support
  * anaconda support
  * New formats: AH, CNV, Kinemetrics EVT, NDK, NLLOC, PDAS, ZMAP
  * ObsPy licensed under LGPL v3.0 now as a whole.
- General
  * Support for Python 3.3 and 3.4 in addition to 2.6 and 2.7
  * ObsPy licensed under LGPL v3.0 now as a whole.
  * More generic processing history for most Stream and Trace methods.
  * Now requires NumPy >= 1.4.0
  * Now requires SciPy >= 0.7.2
  * Tested compatibility with most major Linux distributions still
    receiving updates.
  * The next major obspy release (0.11) will drop support for:
    * Python < 2.7
    * matplotlib < 1.1
    * numpy < 1.6
    * scipy < 0.10
- obspy.ah
  * New submodule for reading the AH (Ad Hoc) waveform format
- obspy.arclink:
  * add support for Poles and Zeros type "B" (Analog, Hz), see #899
- obspy.core
  * Preview waveform plot improved: interactive updating of ticks and
    ticklabels, correct ticklabels for sub-minute zoom level (#657)
  * fixed a problem with UTCDateTime with timestamps of far future
    dates (larger than 2038, often seen in StationXML end dates,
    see #805)
  * Support for basic custom namespace tags in QuakeML I/O (see #454)
  * `interpolate()` method for Stream/Trace objects.
  * Dictionary values added to an AttribDict will now be converted to an
    AttribDict.
  * Removed custom OrderedDict backport for Python 2.6. Now relies on the one
    provided by the future package.
  * Renamed 'type' argument to 'method' in the Trace.differentiate() method.
  * Renamed 'type' argument to 'method' in the Trace.integrate() method.
    Additionally, several broken alternate methods have been removed.
  * new plugins for NonLinLoc formats for readEvents() and
    Catalog/Event.write() (see obspy.nlloc and #900)
  * The wrap_long_string utility function is deprecated. Users may use the
    textwrap module which provides similar functionality.
  * new plugin for CNV event format (used by VELEST) for
    Catalog/Event.write() (see obspy.cnv and #905)
  * better customizable control during merging traces with sub-sample shift
    of sampling points (see #980)
- obspy.cnv
  * new plugin to write CNV event files (used by VELEST) from
    Catalog/Event objects. (see #905)
- obspy.css
  * Support for little-endian binary and ASCII files (see #881).
  * Support exporting Inventory objects to CSS relations.
- obspy.fdsn
  * WADL files are cached per Python process.
  * Bulk station downloading using POST requests.
  * Support for FDSNWS 1.1, e.g. the `matchtimeseries` parameter for the
    station service.
- obspy.imaging
  * Maintain beach ball aspect ratio through optional axes argument (see
    #734)
  * Refactored Catalog.plot() into helper routine
    obspy.imaging.maps.plot_basemap() (see #753).
  * The projections of Catalog.plot() have been modified slightly to provide
    equal-area projections:
    * The `"cyl"` projection is now named `"global"`. It is now the Mollweide
      projection.
    * The '"local"` projection now uses the Albers Equal Area projection.
- obspy.kinemetrics
  * New submodule for reading the Kinemetrics EVT waveform format
- obspy.mseed
  * Support for reading and writing all encodings supported by libmseed.
  * proper error reporting while reading
  * `details=True` when reading will now write to
    `Trace.stats.mseed.blkt1001.timing_quality` instead of
    `Trace.stats.mseed.timing_quality`.
  * The timing quality will now also be written to a file if it is set.
  * Non-existing values when reading with `details=True` will now be set to
    `False` instead of `-1`.
  * New utility function `obspy.mseed.util.set_flags_in_fixed_header()`
    giving the ability to overwrite flags in the fixed header of existing
    MiniSEED files.
  * The sequence number of the first record of each Trace can now be
    specified when writing MiniSEED files.
  * `obspy-mseed-recordanalyzer`:
      - Bugfix: when specifying an out-of-bounds record number, information
        about the last record in the file was displayed (see #957). Now a
        proper error message is shown and the command line script exits
        with non-zero exit code.
      - Faster reading of a single record header
      - Added option "-a" to print information of all records
  * upgrade to libmseed 2.15
- obspy.ndk
  * New submodule able to read NDK files from the Global CMT project.
- obspy.neries
  * The whole module is deprecated and will be removed with the next major
    release. To access EMSC event data please use the obspy.fdsn client
    (use `Client(base_url='NERIES', ...)`), for access to ORFEUS waveform
    data please use the obspy.fdsn client (use
    `Client(base_url='ORFEUS', ...)`) and for travel times please use
    obspy.taup.
- obspy.nlloc
  * new plugins to write NonLinLoc Phase observations files from
    Catalog/Event objects and to read NonLinLoc Hypocenter-Phase file into
    Catalog/Event objects. (see #900)
- obspy.pdas
  * read support for PDAS waveform files
- obspy.sac
  * New `byteorder` option for writing sac files to disk.
  * Can now read/write from/to file-like objects like io.BytesIO and open
    files.
- obspy.seedlink
  * bugfix: INFO responses from the IRIS ringserver are now parsed
    correctly (see #807)
  * New submodule `easyseedlink` providing an easier way to create
    SeedLink clients
  * New `Client` class providing a basic seedlink client for individual
    requests of finite time windows (i.e. non-continuous programs)
  * Fix memory leak in `SLClient` (MiniSEED record leak in packet parser,
    see #918)
- obspy.seisan
  * bugfix the actual data were misaligned by one
- obspy.seishub
  * use specified timeout in all requests to server (see #786)
  * Helper method `Client.event.getEvents()` to fetch a `Catalog` object
    from a seishub server of version 1.4.0 or higher.
- obspy.signal
  * Increased performance of PPSD plotting.
  * Interpolating methods. Wrappers around routines from scipy and a custom
    `weighted average slopes` method from Wiggins 1976.
  * PPSD has new methods to extract mean and mode of the histogram by
    frequency (see #804)
  * PPSD: water level in instrument correction can now be specified by user
    on PPSD initialization
  * New polarization analysis methods: flinn, vidale, pm
- obspy.station
  * add plotting methods (response/bode, location maps) to
    Inventory/Station/Channel/Response objects (see #750)
  * add get_coordinates method to inventory and network objects (see #740)
  * read/write support for DataAvailability tags in StationXML files.
  * write support for SACPZ ASCII representation of channel responses.
- obspy.taup
  * Replaced Fortran implementation with much more powerful Python port of
    Java TauP. This enabled us to drop all Fortran code, which simplifies
    releases and builds tremendously.
- obspy.xseed
  * add support for Poles and Zeros type "B" (Analog, Hz), see #899
- obspy.zmap
  * New module which adds ZMAP read/write support
- scripts
  * All scripts now require argparse instead of optparse.
  * All scripts now accept -V or --version to print version information.
  * obspy-dataless2xseed: -v and --version options are renamed to -x and
    --xml-version to not conflict with above option.
  * obspy-indexer: Options have been modified or amended slightly:
    * --data is a new alias to -d.
    * --db-uri is a new alias to -u.
    * --log is a new alias to -l.
    * --poll-interval is a new alias to -i.
    * --recent is a new alias to -r.
    * -a is a new alias to --all-files.
    * -f is a new alias to --force-reindex.
    * -H is a new alias to --host.
    * -p is a new alias to --port.
    * --check_duplicates is renamed to --check-duplicates.
    * --drop_database is renamed to --drop-database.
    * --mapping_file is renamed to --mapping-file.
    * --run_once is renamed to --run-once.
  * obspy-mopad: Options have been modified or amended slightly:
    * convert subcommand:
     * No changes.
    * decompose subcommand:
     * --input_system is renamed to --input-system.
     * --output_system is renamed to --output-system.
    * gmt subcommand:
     * --show_1fp is renamed to --show-1fp.
     * --show_isotropic_part is renamed to --show-isotropic-part.
    * plot subcommand:
     * --basis_vectors is renamed to --basis-vectors.
     * --full_sphere is renamed to --full-sphere.
     * --input_system is renamed to --input-system.
     * --lines_only is renamed to --lines-only.
     * --output_file is renamed to --output-file.
     * --pa_system is renamed to --pa-system.
     * --pressure_colour is renamed to --pressure-colour.
     * --show1fp is renamed to --show-1fp.
     * --show_isotropic_part is renamed to --show-isotropic-part.
     * --tension_colour is renamed to --tension-colour.
  * obspy-plot: --format option is accepted as an alias of -f.
  * obspy-print: Options have been modified or amended slightly:
    * --format is a new alias of -f.
    * --nomerge is renamed to --no-merge.
  * obspy-runtests: -a option is accepted as an alias of --all.
  * obspy-scan: Options have been modified or amended slightly:
    * --endtime is renamed to --end-time.
    * --event-times is renamed to --event-time. --event-time may be specified
      multiple times.
    * --ids is renamed to --id. --id may be specified multiple times.
    * --nox is renamed to --no-x.
    * --nogaps is renamed to --no-gaps.
    * --starttime is renamed to --start-time.


0.9.2
=====
https://doi.org/10.5281/zenodo.10648

- general
  * fix installation on CygWin (see #755)
- obspy.core
  * bugfix: Input/Output to/from QuakeML was missing Amplitude
    elements (see #763)
  * fixing very slow response removal for some magic bad values of npts
    (see #715)
  * extend remove_response for polynomial responses
    (thanks to Sebastien/bonaime, see #566)
- obspy.datamark
  * bugfix: channel code now correctly read (4 hex char)
  * bugfix: channels can have different sampling rate
  * improvement: datawide 0.5 (4 bits) encoding now supported
  * century of data can now be specified
- obspy.fdsn
  * time out errors get raised properly now. timeout can be specified at
    Client initialization now. (see #717)
  * for advanced users: endpoints of any particular service can now be
    specified explicitly (see #754)
  * new known FDSN providers: 'ORFEUS', 'GFZ', 'NERIES'
  * more robust WADL parser
  * the `attach_response=True` argument now uses a faster approach to
    download the station data
- obspy.imaging
  * Fixing waveform plotting.
- obspy.sac
  * SAC files with two digit year header field are now interpreted as
    "19xx", same as done by SAC (see #779)
- obspy.seedlink
  * bugfix: different instances of a SeedLink connection had a shared
    state (see #561)
  * multiple smaller bugfixes (see #777)
  * trailing null characters are now stripped from INFO responses (see #778)
- obspy.seg2
  * numbers are now also recognized as months
  * now filters non-printable chars from the header enabling it to read some
    more files
- obspy.signal
  * the TF misfits now correctly use logarithmic axes instead of scaling an
    image
- obspy.station
  * some bugfixes in the obspy.station object classes (see #710)
  * more robust writing of StationXML in case of missing elements
- obspy.taup
  * bugfix: avoid a bug that caused multiple calls to taup to result in
    spurious unexpected results (see #728)


0.9.0
=====

- general
  * Added mock testing library.
- obspy.arclink
  * user keyword is now required during client initialization
- obspy.core
  * Stream/Trace.attach_response(): convenience method to attach response to
    traces from inventories.
  * new method Stream/Trace.remove_response() to remove instrument response
    from Response object attached to trace(s), e.g. after parsing a
    StationXML file. Similar to Stream/Trace.simulate(seedresp=...) for
    using a Parser object (from dataless or xseed) or RESP file, but less
    cluttered parameters and without the simulating a different instrument
    part.
  * Updated event classes to QuakeML 1.2 final.
  * Moved obspy.core.event.validate() to obspy.core.quakeml.validate()
  * The writeQuakeML() function, also accessible through
    Catalog.write(..., format="quakeml"), now has an optional keyword
    argument 'validate'. If True, the resulting QuakeML file will be
    validated against the QuakeML schema before being written. An
    AssertionError will be raised in case the validation fails.
  * validation of QuakeML against official schema working now
  * renamed obspy.core.util.types into obspy.core.util.obspy_types (#595)
  * new parameter replace for Enums which allows definition of replaceable
    keywords (fixes #531)
  * Trace.split() will return a stream object containing traces with unmasked
    arrays
  * trim(pad=True, fill_value=xxx) will return a NumPy ndarray as stated in
    the API documentation (#540)
  * read() supports now tar und zip archives and variants (tar.gz, tar.bz2)
  * new options for Stream/Trace.taper() to control the length of the
    tapering for all windowing functions and perform one-sided tapering
  * Many Stream and Trace methods are now chainable, e.g. st.taper().plot()
  * when using Stream/Trace.simulate(seedresp={...})) parameter "date" can
    now be omitted, start time of each trace is used for response lookup then
  * when using Stream/Trace.simulate(seedresp={...})) for parameter
    "filename" instead of the path to a local file now also can be provided
    either a file-like object with RESP information or an obspy.xseed.Parser
    object (e.g. created reading a dataless SEED file).
  * fix Stream.select() when using values like "" or 0, e.g.
    Stream.select(location="") or when filtering by component with a channel
    code less than 3 characters long (now these traces will be omitted from
    the result when filtering by component).
  * fix a bug when merging valid data into a masked trace (see #638)
  * event.ResourceIdentifier objects are now initialized with a QuakeML
    conform string by default, i.e. if no custom prefix is provided during
    initialization.
  * event.ResourceIdentifier.resource_id attribute was renamed to
    event.ResourceIdentifier.id
  * event.ResourceIdentifier now was has a method regenerate_uuid() that
    allows the random hash part to be regenerated for resource identifiers
    with no fixed id string (can be useful to generate a new hash if the
    referred object changes).
  * added a new test that asserts that the whole codebase is valid according
    to the flake8 tool.
  * inverse filtering of catalogs.
  * bugfix: Trace.simulate() now passes the SEED network, station, location,
    and channel identifiers to evalresp.
  * added command line script "obspy-print" to print information on local
    waveform files
  * check if ndim == 1 when setting Trace.data and raise if necessary,
    see #695
  * change waveform_id parameter in obspy.core.event.FocalMechanism to list
    of WaveformStreamID as specified in QuakeML docs (#633)
- obspy.css
  * new module for CSS (Center for Seismic Studies) format
  * currently read support for waveform data
- obspy.db
  * obspy-indexer script uses from now on hash symbols (#) instead
    of pipe (|) for features because pipe has a special meaning on
    most operating systems
- obspy.fdsn
  * new client module to access servers based on the FDSN web service
    definition (https://www.fdsn.org/webservices/)
- obspy.gse2
  * read/write STA2 header line which is officially mandatory but in
    pratice often not used
- obspy.imaging
  * more options to customize day plots
  * dayplot now plots matching picks (station, network, location) if a list
    of event objects is provided using the `events` kwarg.
  * obspy-scan: new option --print-gaps
  * added plotting of record sections
  * automatic merging can be disabled for obspy-plot
- obspy.pde
  * new module for reading NEIC PDE bulletin files into an obspy catalog
    object. Only the "mchedr" format (file format revision of February 24,
    2004) is supported.
- obspy.realtime
  * two new processing plugins (offset, kurtosis)
- obspy.seg2
  * adding read support for SEG2 data format code 1 and 2
    (signed 16bit/32bit integer)
- obspy.segy
  * fix a bug in plotting (see #689)
  * Removed the SEG Y benchmark plots. Now part of obspy/apps.
- obspy.signal
  * adding cross correlation single-station similarity checking with
    master event templates to coincidence trigger
  * new function for rotating arbitrarily oriented components to vertical,
    north, and east.
  * add PPSD support for segments of arbitrary length
  * default bin width of PPSD is changed to 1dB. This is the value used by
    McNamara and Buland 2004.
  * fix a bug when using evalresp with RESP files with very short epochs.
    see #631.
  * for seisSim(seedresp={...})) for parameter "filename" instead of the
    path to a local file now also can be provided either a file-like
    object with RESP information or an obspy.xseed.Parser object
    (e.g. created reading a dataless SEED file).
  * seisSim(seedresp={...}): the seedresp dictionary now requires network,
    station, location, and channel keys.
  * removed deprecated psd module - use spectral_estimation module instead
  * removed deprecated sonic function - use array_processing function instead
  * corrected function signature of c_sac_taper
- obspy.station
  * adding support for FDSN StationXML
- obspy.mseed
  * new kwarg arguments for reading mseed files: header_byteorder and
    verbose
  * libmseed v2.12
- obspy.neic
  * new module to access data from CWB QueryServer run at the National
    Earthquake Information Center (NEIC) in Golden, CO USA.
- obspy.y
  * adding read support for Nanometrics Y file format
- scripts
  * obspy-plot: new option "-o" to output plot to file instead of opening
    a window


0.8.4
=====
- bugfixes to make ObsPy work with the latest Python 2.x and NumPy releases
- critical bugfixes for the waveform plotting and the xml wrapper
- bugfix so that copy.deepcopy() works with the obspy.core.stream.Stream class
- fixing some imports


0.8.3
=====
- circumventing an issue in the current libmseed release that can lead to
  some float values being read in wrongly


0.8.2
=====
- fixing a bug in plotting methods of Trace and Stream
- stream/trace.plot(type="dayplot") can display event information now


0.8.1
=====
- fixing a bug parsing QuakeML from a StringIO object using xml and
  autodetection


0.8.0
=====
- version numbering: one single, common version number for ObsPy now.
  Use "import obspy; print obspy.__version__"
- discontinuing Python 2.5 support
- most important classes/functions can be imported like "from obspy import
  ...", currently: read, Trace, Stream, UTCDateTime and readEvents
- obspy.arclink:
  * refactored attributes in getPAZ to stick better with the SEED standard
- obspy.core
  * fixing preview generation for sampling rates containing floats
  * fixing deprecated_keywords decorator for the case of removed keywords
  * fixing SLIST and TSPAIR reading/writing of empty traces or traces
    containing only one or two data points
  * adding taper() method to Trace/Stream using cosTaper of ObsPy and also
    all scipy windowing functions
  * adding cutout() method to Stream
  * removed all deprecated UTCDateTime methods
  * adding a class and script to determine flinn-engdahl regions for given
    longitude and latitude
  * adding rotate() method to Stream wrapping rotate functions in
    obspy.signal
- obspy.imaging
  * obspy-scan: adding options to control start/endtime and channels, adding
    options to not plot gaps and reducing file size for plots considerably.
- obspy.iris
  * many services have been discontinued on the server side. Use obspy.fdsn
    instead for discontinued services.
  * still existing services now are distinguished by a major version of the
    particular service (like obspy.fdsn).
- obspy.mseed
  * Bugfix writing traces containing one or two samples only
  * writeMSEED emits an UserWarning while writing an empty trace
- obspy.sac
  * fixing SAC and SACXY reading/writing of empty traces or traces containing
    only one or two data points
  * new debug_headers flag for reading SAC files in order to extract all
    header variables (issue #390)
- obspy.segy
  * unpack SEGYTrace.data on-the-fly patch contributed Nathaniel Miller
  * fixing a bug related to negative values in trace header
- obspy.seishub
  * adding kwarg to control number of retries for failing requests
  * adding obspy.xseed as dependency (in setup.py and debian/control)
  * changing obspy.client.station.getPAZ() call syntax to use seed_id
    (args/kwargs)
  * adding local caching of requests for PAZ and coordinates to avoid
    repeated requests to server
- obspy.sh
  * file extension 'QBN' not added twice anymore if data_directory was set
  * fixing SH_ASC and Q reading/writing of empty traces or traces containing
    only one or two data points
- obspy.signal
  * module psd has been refactored to spectral_estimation
  * adding function for cross correlation pick correction
  * removing pitsa-compatibility in response function calculation
    (no complex conjugate)
  * preventing a possible duplicated overall sensitivity removal in seisSim
    when using the option seedresp
  * adding optimized C-code for classic STALTA. Runs approximately, 1000x
    faster than pure python code. It has now the same order of speed as the
    recursive STALTA
  * new CAPON method for array_analysis / array_processing
  * sonic was renamed to array_processing, sonic is now deprecated
- obspy.xseed
  * fixed a bug with Dataless to XSEED conversion using split_stations=True
  * fixed a bug affecting getPAZ() and getCoordinates() when selecting
    specific channels from complex dataless files
    (see: https://github.com/obspy/obspy/issues/412)
  * added getInventory() method to the Parser object. Returns a dictionary
    about the contents of the Parser object. This is also integrated in the
    string representation and makes it more informative.
  * allow parsing of oversized VariableString from SEED, but warn and cut
    string to max_length at writing SEED (#701)
- obspy.mseed
  * adding experimental details option, which extracts timing quality and
    info on the calibration


0.7.1
=====
- obspy.arclink
  * proper DeprecationWarning for deprecated keywords for
    Client.getWaveform()
- obspy.core
  * fixing negative azimuths returned by gps2DistAzimuth [#375]


0.7.0
=====
- obspy.arclink
  * requesting time spans (using 'starttime' and 'endtime' keywords) are
    deprecated in Client.getPAZ() and Client.getMetadata() - use 'time'
    instead
  * output format has changed for Client.getPAZ(..., time=dt)
  * 'getCoordinates' and 'getPAZ' keywords are deprecated in
    Client.getWaveform() - use 'metadata' instead
  * Client.getWaveform(..., metadata=True) will return both keywords as well
    as PAZ - inventory request is done only once per request -> huge
    performance improvement compared to previous implementation
  * traces requested via Client.getWaveform(..., metadata=True) covering
    multiple instrumentations will be split and the correct PAZ are appended
- obspy.core
  * new Catalog/Event classes
  * read/write support for QuakeML files
  * new resample method for Trace and Stream object
  * Trace.__mod__ (splits Trace into Stream containing traces with num
    samples)
  * Trace.__div__ (splits Trace into Stream containing num traces)
  * implementation of __mul__ method for Trace and Stream objects
  * new formatSeedLink method for UTCDateTime object
  * new split method for transforming streams containing masked arrays into
    contiguous traces
  * new util.xmlwrapper module for uniform API for Python's default xml and
    lxml
  * new obspy.core.util.types.Enum class
  * refactored obspy.core.util.ordereddict into obspy.core.util.types
  * refactored kilometer2degrees and locations2degrees from obspy.taup into
    obspy.core.util.geodetics
  * adding 'equal_scale' option to plot() method
  * removing __hash__ fixture for Stream and Trace
  * stream.select works now case insensitive
  * support for initialization of UTCDateTime from numpy.string_ types
  * new dtype parameter on read method allows converting data into given
    dtype
  * AttribDict may now be initialized with (key, value) kwarg pairs, e.g.
    AttribDict(a=1, b=2).
  * changed many setter/getter in UTCDateTime to private methods, e.g.
    _getDate
  * added UTCDateTime.DEFAULT_PRECISION
  * import of an unsupported waveform will result into a TypeError [#338]
  * added compatibility methods for AttribDict and UTCDateTime
  * retaining trace order in stream while merging
  * deprecated_keywords decorator may warn and ignore keywords by setting the
    keyword mapping to None
- obspy.db
  * added client for a database created by obspy.db
  * adapting to changes in obspy.core.util.base version 0.6.0 and above
- obspy.gse2
  * bugfix for buffer overflow in test_readDos
  * bugfix checksum calculation of GSE2/GSE1
- obspy.imaging
  * Trace.label/Stream.label can be used to overwrite default labels
  * better support for huge/tiny y-ticks and plots containing multiple traces
  * adding 'equal_scale' option to plot() method
  * Limited localization support and the time axis(es) can be swapped.
  * traces with same id but different processing steps will not be merged
    anymore using the plot() method
  * accept a list of two values for width of beachballs (using Ellipse patch)
- obspy.iris
  * added low-level interface for IRIS timeseries WS
  * added low-level interface for IRIS traveltime WS
  * new Client.getEvents method able to return a ObsPy catalog object
- obspy.mseed
  * changing license to LGPL (same as libmseed)
  * libmseed 2.7 (fixes sampling rates above 32,767 Hz)
  * adding read/write support for very large and very small sampling rates
    using blockette 100 in MiniSEED
  * new obspy-mseed-recordanalyzer script for analyzing SEED files via
    console
  * new obspy.mseed.util.shiftTimeOfFile() function for shifting
    the time of all records without interfering with the rest of the file.
- obspy.neries
  * new format 'catalog' for getEvents, getEventDetail and getLatestEvents
    methods - deprecating old format defaults
- obspy.sac
  * bugfix for SAC files containing null terminated strings
- obspy.seg2
  * bugfix in parsing starttime from seg2 header
- obspy.signal
  * adding toolbox to calculate Time-Frequency Misfits
  * fixed bug in calculation of time derivatives
  * fixing a misleading entry point for trigger, adding a missing one
  * adding coincidence triggering routine
- obspy.taup
  * deprecated kilometer2degrees and locations2degrees - one can find those
    methods on obspy.core.util now
- obspy.xseed
  * fixed a bug with exactly one pole or one zero in response information


0.1.0
=====
- obspy.datamark
  * read support
- obspy.realtime
  * initial release