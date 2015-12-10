# GWsky
The interactive script GWsky (v2) defines a sequence of Fields of View (FoV) centers from a fixed position over the sky. North/South/East/West directions are allowed. The results are displayed in Aladin Sky Atlas (http://aladin.u-strasbg.fr/) using the |SAMPIntegratedClient| class. The airmass at the FoV center and the integrated probability (%)
are provided during the FoV sequence.
    
    The FoV sequence is generated along the coordinate grid; no roll angle is provided in this  release.
The FoV centers were evenly space assuming that the shortest angular distance between two points on the celestial sphere is measured along a great circle that passes through both of them:

                            cosθ=sinδ1sinδ2+cosδ1cosδ2cos(α1−α2), 
where (α1,δ1) and (α2,δ2) are the right ascension and declination of the two points on the sky (expressed in degrees, then converted to radians).

The Fig. below shows a FoV sequence that covers a skymap region in which 50% of probability is contains (red dots).

![gwsky_fov](https://cloud.githubusercontent.com/assets/11920251/11462523/da085a7a-9715-11e5-959a-5a89076b1e1e.jpg)

**Running it**

    from idle:  execfile('GWsky.py')
    
    from terminal: python GWsky.py


**Input Parameters:**


    sky_map : str 
    LVC probability skymap in healpix format

    percentage : float
    probability percentage to determine the area (in square degrees) confined in it

    FOV_base   : float
    FOV_height : float
    size of Field-of-View Instrument in degrees

    time_input : str
    the time in the format "2012-7-12 23:00:00"

    lat_input : float
    Geodetic coordinates of the Observatory: latitude (deg)

    lon_input : float
    Geodetic coordinates of the Observatory: longitude (deg)

    height_input : float
    Geodetic coordinates of the Observatory: altitude (m)

    ra : float
    right ascention of FoV center (deg)

    dec : float
    declination of FoV center (deg)


**Return:**

    area_probability : float
    the area (in square degrees) confined in a given probability percentage

    percentage_poly : float
    the integrated probability in a specific FOV (%)

    airmass : float
    the airmass of the FOV center

    contour_ipix.out: list
    the table that contained the pixels confined in a given probability percentage

    ra_max : float
    right ascention of the highest probability pixel

    dec_max : float
    declination of the highest probability pixel

    instrument_FOV.vot : VOTABLE
    Instrument Footprint Editor from http://aladin.u-strasbg.fr/footprint_editor/

    N/S/E/W/R/Q : str
    N/S/E/W: a set of command line to add contiguous FOVs in North/South/East/West  (N/S/E/W) directions;
    R: to insert a new FOV center RA[deg], DEC[deg] to begin a new sequence in N/S/E/W directions;
    Q: quit
    
**SUMMARY OF DEPENDENCIES**

    from astropy.vo.samp import SAMPIntegratedClient
    import urlparse
    import os.path
    from configobj import ConfigObj
    ----> install "sudo apt-get install python-configobj"
    from astropy.io.votable import parse
    from math import sin, cos, acos, degrees, radians
    import healpy
    import numpy
    import time                                                                   
    import sys 

**Script functions**

    import send_Aladin_image as sAi
    import send_Aladin_script as sAs
    import print_area_prob as pap
    import table_ipix_percentage as tip
    import highest_probability_pixel as hpp
    import instrument_FOV as iF
    import airmass as airmass
    from gw_core import *
    import progress_bar as pb
