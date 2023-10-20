Two control files determine the code behaviour. A name list file ``ocnicepost.nml`` is used
to set the input file type (ice or ocean), the location of the ESMF weights, the dimensions
of the source and destination grids, the source variable to used to create an interpolation
mask and the required rotation variables and a flag to produce debugging output.

The list of variables to be remapped is expected in either ``ocean.csv`` or ``ice.csv``. Each
variable is specified by name, dimensionality, the required mapping method. For vectors, the
associated vector and it's grid is also listed.# ocnpost90

This program will remap MOM6 ocean or CICE6 ice output on the tripole grid to a set of
rectilinear grids using pre-computed ESMF weights to remap the chose fields to the
destination grid and write the results to a new netCDF file

The files containing the conservative and bilinear regridding weights are generated using
the see documentation https://ufs-community.github.io/UFS_UTILS/cpld_gridgen/index.html.
The weights files and this code utilize the MOM6 grid naming scheme, as follows:

MOM6 is on an Arakawa C grid. MOM6 refers to these locations as "Ct" for the centers and
"Cu", "Cv" and "Bu" for the left-right, north-south and corner points, respectively.

CICE6 is on an Arakawa B grid. CICE6 refers to these locations as TLAT,TLON for the centers
and ULAT,ULON for the corners. In this code, MOM6's nomenclature is followed, so that
CICE6's U-grid is referred to as "Bu".

MOM6 and CICE6 both output velocties on their native velocity points. For MOM6, that is
u-velocities on the Cu grid and v-velocites on the Cv grid. For CICE6, it is both u and
v-velocities on the Bu grid.

The rotation angle for both models are defined at center grid points. Therefore the
velocities need to be first remapped to the center points ("unstaggered") before rotation.
MOM6 and CICE6 also define opposite directions for the rotations. Finally, while the
grid points are identical between the two models, CICE6 calculates the rotation angle at
center grid points by averaging the four surrounding B grid points. MOM6 derives the rotation
angle at the center directly from the latitude and longitude of the center grid points. The
angles are therefore not identical between the two grids.

Two control files determine the code behaviour. A name list file ``ocnicepost.nml`` is used
to set the input file type (ice or ocean), the location of the ESMF weights, the dimensions
of the source and destination grids, the source variable to used to create an interpolation
mask and the required rotation variables and a flag to produce debugging output.

The list of variables to be remapped is expected in either ``ocean.csv`` or ``ice.csv``. Each
variable is specified by name, dimensionality, the required mapping method. For vectors, the
associated vector and it's grid is also listed.

A interpolation mask in either 2 or 3 dimensions is created by bilinearly mapping an array of 0s
and 1s (denoting valid source points) to the destination grid. This interpolation mask is used to
mask the remapped fields.
