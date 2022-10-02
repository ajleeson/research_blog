## Structured Plan to Add WWTPs and Tiny Rivers to LiveOcean

In this blog post I share the details of how I plan to incorporate Ecology's tiny river and point source (how does TRAPS sound?) data into LiveOcean. This is really a map of where and how I anticipate adding new scripts to the code base.

The new scripts will borrow code described in my prior blog posts, [WWTP placement algorithm](https://ajleeson.github.io/research_blog/2022/07/25/WWTP-PointSource-Algorithm.html) and [combined river and WWTP forcing](https://ajleeson.github.io/research_blog/2022/08/01/river-and-wwtp-forcing.html).

<p style="color:red">I've color-coded open questions and anticipated sticking points in red.</p>

---

## Overall Structure

There are three sets of files that need to be included. The first is [SSM_source_info.xlsx](https://github.com/ajleeson/LO_user/files/9465489/SSM_source_info.xlsx), the excel file I created previously which lists the lat/lon coordinates of all rivers and point sources in the Salish Sea Model. The second and third are Ecology's river and point source timeseries data, which can be downloaded from [Ecology's website](https://fortress.wa.gov/ecy/ezshare/EAP/SalishSea/SalishSeaModelBoundingScenarios.html). These data are provided in directories, with one file corresponding to a timeseries for one river or marine point source. I propose that all of these files be added to LO_data.

All of the scripting to add these source data to LiveOcean will be handled within the river forcing folder. New functions could easily be added to rivfun.py, but I could also create a set of helper functions in a new python file for separation. Either way, TRAPS handling can all be within a forcing/[riv-forcing] folder. The new TRAPS functions will be called within make_forcing_main.py. This way, I am hoping that LiveOcean users will not need to change their workflow to enable TRAPS handling. I plan to add five logical switches within make_forcing_main.py that enable or disable:
1.  the addition of tiny rivers
2.  the addition of marine point sources
3.  using Ecology's biogeochem to force tiny rivers
4.  using Ecology's biogeochem to force marine point sources
5.  using Ecology's biogeochem to force the larger rivers already present in LiveOcean

If tiny rivers and marine point sources (1 and 2) are enabled, but biogeochem forcing is disabled (3 and 4), then these rivers and point sources will only discharge freshwater at the flowrate and temperature provided in Ecology's dataset (no nutrients).

---

## TRAPS Script

I've broken the script down into two parts. The first part describes the helper function that will calculate the nearest coastal grid cell to a WWTP or river mouth. The second part describes ROMS forcing file generation.

### Calculating Nearest Coastal Grid Cell

The new script will contain a function that identifies the nearest coastal grid cell to all TRAPS, and generates a .csv in LO_output with the row/col indices of the LiveOcean grid cell corresponding to the new river mouth or point source. Note that two separate .csv files will be created: one for the tiny rivers and one for the point sources.

**traps_placement(source_type, gridname)**

Inputs
- source_type, a str of either 'riv' or 'wwtp' to specify which source to place
- gridname, a str of the gridname

Outputs
- no outputs, but creates .csv file in LO_output/pgrid/[gridname] with the row/col indices of the river mouth or marine point source

*Pseudo-Code:*

```python

if the final csv files already exist in LO_output:
    return # function ends without doing anything

else: # actually run the function

    if source_type == 'wwtp':
        output_fn = roms_wwtp_info.csv
        inflow_type = 'Point Source'
    elif source_type == 'riv':
        output_fn = roms_triv_info.csv
        inflow_type = 'River'
    else:
        throw error

    grid_data = read grid data from LO_data
    latlon_df = read lat/lon coordinates from LO_data (SSM_source_info.xlsx)

    rowcol_df = initialize dataframe to save results

    for source in latlon_df with 'Inflow_Type' == inflow_type:

        if inflow_type == 'River':
            if source in LiveOcean already:
                continue # don't add rivers that already exist
            elif source contains '1' in its name: # some large river mouths have two lat/lon coords
                search for corresponding source containing '2' in its name
                lat = average of both lats
                lon = average of both lons
            elif source contains '2' in its name:
                pass # already accounted in the above if case
            else:
                read lat & lon coordinates
            find nearest coastal grid cell # function call to the code I wrote this summer
            save coordinates to rowcol_df
            find idir, isign, uv, & offsets?? # Need to borrow some code from carve_river here...
            add river idir, isign, etc. info to rowcol_df
        
        else: # wwtps are easier
            read lat & lon coordinates
            find nearest coastal grid cell # function call to the code I wrote this summer
            save coordinates to rowcol_df

    write rowcol_df to LO_output/pgrid/[gridname]/output_fn
    return

```

Note that if the user wants to make any changes to the rivers or wwtps being used, the user must first delete the prexisting .csv file in LO_output so this function will generate a new one.

The reason I have decided against overwriting the csv files each time the forcing script is run is because I do not anticipate the historical nutrient data to change often. Also, I would expect the river/wwtp placement algorithm to take some time to run, so it's probably better to save the data and re-run the algorithm only when necessary.

<p style="color:red">When this script places river mouths, it will check whether the river already exists within LiveOcean (I don't want to add a river mouth as a point source if LiveOcean already has river tracks). I will need to be careful for times in which the same river may be named differently within LiveOcean and within Ecology's dataset.</p>

<p style="color:red">I need to also somehow confirm that my UTM to lat/lon conversion is correct. I used the python package pyproj. When I look up some of the resulting lat/lon coordinates in Google maps, I'm finding river mouths out in the middle of Puget Sound. Either the pyproj conversion has error, or the SSM river mouth coordinates are offset from the coast, or a combination of both? These strange coordinates are an issue because (1), it could lead to inaccurate placement of rivers and wwtps, and (2), rivers introduce horizontal momentum to the system. Horizontal momentum sources use LuvSrc, which requires that the grid cell be a coastal cell and not a cell out in the ocean. Thus, I need to confirm that the conversion to lat/lon is correct and I need to (potentially) force river mouths to be placed at coastal grid cells.</p>

<p style="color:red">It might also be difficult to calculate the direction and sign of rivers. In carve_rivers, these values depend on the river_track. In my case, I will probably need to write new code that determines river directions based on which surrounding cell are water (unmasked) and which are land (masked). Again, this will require that the river mouths are placed on a coastal grid cell.</p>

### Generating ROMS Forcing

All of the below code will be implemented within make_forcing_main.py. Make_forcing_main.py will create one single river.nc forcing file containing ROMS forcing for pre-existing LiveOcean rivers, new tiny rivers, and new WWTPs. However, these three categories of sources will be handled individually in make_forcing_main.py, and will be user-controlled via logical switches.

At the top of make_forcing_main.py, I will include three user-defined logical switches (as described earlier). The first two switches allow the user to include or not include tiny rivers or WWTPs in LiveOcean. If either of these switches are True, then traps_placement will be run.

The next three logical switches allow the user to decide whether or not to include Ecology's biogeochemistry data in LiveOcean. Biogeochemistry for tiny rivers and WWTPs can only be enabled if the tiny rivers and WWTPs themselves are enabled. Turning on biogeochemistry for LiveOcean's pre-existing rivers will overwrite the current biogeochem forcing in rivfun with Ecology's timeseries data instead.

In each of these cases, make_forcing_main.py will search in LO_data for Ecology's timeseries data, and convert it into a .nc file for ROMS.

*Pseudocode:*

```python
# logical switches
include_trivs = T/F 
include_wwtps = T/F
triv_biog = T/F
wwtp_biog = T/F
LOriv_biog = T/F

if include_trivs:
    traps_placement('riv',gridname) # run placement algorithm
if include_wwtps:
    traps_placement('wwtp',gridname) # run placement algorithm

# LIVEOCEAN PRE-EXISTING RIVERS
initialize LiveOcean pre-existing river dataset
if not LOriv_biog:
    let river forcing do what it is already doing
else: # add Ecology biogeochem to pre-existing LiveOcean rivers
    for every river in LiveOcean:
        if river not in Ecologys data:
            let LiveOcean do what it was already doing
        else:
            read daily biogeochemistry values # from Ecology's data
            add biogeochem to dataset

# TINY RIVERS
initialize tiny rivers dataset
if include_trivs:
    for every river in tiny rivers .csv file:
        define river shape # distribute transport equally between layers
        define river position # based on info in .csv from traps_placement
        define river direction # based on info in .csv from traps_placement
        read daily flowrate and temperature # from Ecology's data
        add flowrate/temp to dataset
        if triv_biog:
            read daily biogeochemistry # from Ecology's data
        else:
            set all biogeochem to zero
        add biogeochem to dataset

# MARINE POINT SOURCES
initialize wwtp dataset
if include_wwtps:
    for every wwtp in point source .csv file:
        define river shape # What layer does the wwtp discharge to?
        define river position # based on info in .csv from traps_placement
        define river direction # Always 2 for LwSrc (vertical sources)
        read monthly flowrate and temperature # from Ecology's data
        assume constant value for every day in the month
        add flowrate/temp to dataset
        if wwtp_biog:
            read monthly biogeochemistry # from Ecology's data
            assume constant value for every day in the month
        else:
            set all biogeochem to zero
        add biogeochem to dataset

Merge datasets of LiveOcean pre-existing rivers, tiny rivers, and wwtps

Write final dataset to .nc file 

```

For pre-existing LiveOcean rivers, I'm planning to let the code base handle these rivers as it already has been. If the user wants to add Ecology's biogeochemisty to these rivers, then *only* the biogeochemistry will be added. I don't have plans to change anything else about the pre-existing rivers at the moment.

I will need to be careful about the timing of data for rivers and point sources. Ecology's dataset provides river data on a daily basis, but provides point source data on a monthly basis. To handle this, I will assume that the point source properties are constant every day within the month.

I also note that defining the river_direction and river_Xposition/river_Eposition is trivial for point sources (vertical sources), but a bit more complicated for rivers (horizontal sources). I won't get into the details here, though, because that portion of the code is already written. Just want to have a reminder for myself that rivers and WWTPs need to be handled differently.

<p style="color:red">
Something that will be tricky is defining river_Vshape for the marine point sources. Most of the WWTPs discharge from the bottom of the water column. However, there are a few that discharge somewhere in the middle. I'll need to add some exceptions so that these sources discharge to the middle of the water column. I should also learn more about how river_Vshape works for LwSrc to ensure that I am using it correctly.</p>

<p style="color:red">The Salish Sea Model lists which sigma layer the WWTPs discharge from, but that does not easily correspond to a discharge depth, or something that I can input to LiveOcean easily. I may need to reach out to Ben because I think he has some information about WWTP discharge depth.</p>

<p style="color:red">At the end of all of this code implementation, it also seems like a good idea to have some verification that the resulting .nc file is what I expect it to be. I don't really know what to check for or compare the .nc file to yet. Maybe I can plot timeseries of a few rivers or WWTPs using the resulting .nc file, and compare them to timeseries using data directly from Ecology's .csv file.</p>

---

## Comments about Adaptability

If new nutrient data becomes available in the future, we can replace the datasets in LO_data with the updated information. I will need to check how the new data is formatted, and maybe alter the specific lines of code that finds flowrate and biogeochem values. But overall structure of the code base will not need to change (I think...).

<p style="color:red">However, a restructure will be required if we want to use the TRAPS scripts for forecasting. The current setup will be only compatible with hindcasts using pre-existing datasets.</p>