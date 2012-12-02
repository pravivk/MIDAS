MIDAS
=====

Modular Isosurface Data Analysis Software is built to
function in multipe environments. In offline-mode, 
the user interacts with saved history/restart files. At runtime,
the software is part of the on-line initialization procedure for
tracers and interface positions in GOLD.  


<- MIDAS Snapshot 11/29/2012 ->

                .state [(t),(s),y,x]              <--- Top level state
                   .supergrid[y,x]                   <---  Top level grid (FMS supergrid)
                      .grid[y,x]                        <---  Derived from supergrid (or externally)
                                                        <---  fms mom4,gold grids and supergrids
                                                        <---  can be imported.
                        .interfaces[(t),s,y,x]          <---  Interfaces reside on a static grid but 
                                                        <---  vary in time at each interface                  
                        .variables                      <---  generic [(t),(s),y,x] fields    


                Read a restart or history file and use to initialize a state.
                Any number of generic tracers lying on a simple 
                grid with chosen stagger which may or may not include metrics.
                
                Example:
                
                from midas import *
                grid=generic_rectgrid('http://data.nodc.noaa.gov/thredds/dodsC/woa/WOA09/NetCDFdata/temperature_annual_1deg.nc',var='t_an',cyclic=True)
                State=state(grid=grid,path='http://data.nodc.noaa.gov/thredds/dodsC/woa/WOA09/NetCDFdata/temperature_annual_1deg.nc',fields=['t_an'])
                print State.variables                


    
Information can be readily exchanged between the .state container and the user. For example, attribute detection
is a frequent point of failure for MIDAS routines.  The user can fix a known attribute problem, e.g.

                print state.var_dict['t_an']['Ztype']
                state.var_dict['t_an']['Ztype']='Fixed'
                
                
MIDAS state variables have several useful methods , e.g. volume integrals are
available for data on geopotential coordinate grids, s=s(z):

                from midas import *
                grid=generic_rectgrid('http://data.nodc.noaa.gov/thredds/dodsC/woa/WOA09/NetCDFdata/temperature_annual_1deg.nc',var='t_an',cyclic=True)
                State=state(grid=grid,path='http://data.nodc.noaa.gov/thredds/dodsC/woa/WOA09/NetCDFdata/temperature_annual_1deg.nc',fields=['t_an'])
                State.volume_integral('t_an','XY',normalize=True)
                print State.variables
                print State.t_an_xyav
                State.volume_integral('t_an','XYZ',normalize=True)
                print State.t_an_xyzav
                




