
- Data format: NetCDF
- NetCDF is an interface for array oriented data access and a library that provides an implementation of the interface
- `ncdisp` command: get information about NetCDF file
- `ncread` command: read data from NetCDF file
Example:
1. Displaying column information:
```Octave
ncdisp("/MATLAB Drive/adaptor.mars.internal-1702029592.401762-29764-12-d9b88cdf-3135-40c5-8094-43f8e64e1078.nc");
```
>Output:
>![[Pasted image 20231211145838.png]]

2. Storing columns into variables:
```octave
u100 = ncread("/MATLAB Drive/adaptor.mars.internal-1702029592.401762-29764-12-d9b88cdf-3135-40c5-8094-43f8e64e1078.nc", 'u100');
v100 = ncread("/MATLAB Drive/adaptor.mars.internal-1702029592.401762-29764-12-d9b88cdf-3135-40c5-8094-43f8e64e1078.nc", 'v100');
time = ncread("/MATLAB Drive/adaptor.mars.internal-1702029592.401762-29764-12-d9b88cdf-3135-40c5-8094-43f8e64e1078.nc", 'time');
longitude = ncread("/MATLAB Drive/adaptor.mars.internal-1702029592.401762-29764-12-d9b88cdf-3135-40c5-8094-43f8e64e1078.nc", 'longitude');
latitude = ncread("/MATLAB Drive/adaptor.mars.internal-1702029592.401762-29764-12-d9b88cdf-3135-40c5-8094-43f8e64e1078.nc", 'latitude');
```
___


Use a `.shp` file to overlay to mask the regions you don't want to include in the figure

