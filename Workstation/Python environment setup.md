I use Anaconda's Python 3.6 distribution to setup my environment:

1. Download anaconda from https://www.anaconda.com/download/

2. Once installed, use conda to add packages. Run:

    ```sh
    conda install obspy pysw4 pyfftw numpy scipy numba matplotlib basemap \
        basemap-data-hires pygrib proj4 pyproj argcomplete geographiclib \
        sphinx jupyter_contrib_nbextensions
    ```

    In fact, this installs a whole lot more beacause some of the above packages
    have dependencies that get installed as well.

3. Run ``conda update --all`` to make sure everything is up to date.

4. Finally , run ``conda clean --all`` to get rid of unused packages and
unneeded cache. This can clear up several GB.



