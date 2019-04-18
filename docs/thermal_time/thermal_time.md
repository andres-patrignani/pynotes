
# Thermal Time

Thermal time is a proxy for biological time. Thermal time ihas been used to describe multiple biological processes such as:

* Seed germination of pasture seeds: Moot, D.J., Scott, W.R., Roy, A.M. and Nicholls, A.C., 2000. Base temperature and thermal time requirements for germination and emergence of temperate pasture species. New Zealand Journal of Agricultural Research, 43(1), pp.15-25.

* Crop development: Ritchie, J.T. and NeSmith, D.S., 1991. Temperature and crop development. Modeling plant and soil systems, (modelingplantan), pp.5-29.

* Thermal biology of insects: Dewar, R.C. and Watt, A.D., 1992. Predicted changes in the synchrony of larval emergence and budburst under climatic warming. Oecologia, 89(4), pp.557-559.


```python
def thermaltime (TMAX,TMIN,Tlimits,method):
    """Calculates cumulative growing degree days.

    Keyword arguments:
    TMAX -- vector of air or soil maximum temperature in degrees Celsius.
    TMIN -- vector of air or soil minimum temperature in degrees Celsius.
    Tlimits = It can be a scalar or a two-element vector. If scalar the
            value represents the base temperature at which a given 
            crop has low or negligible growth (biomass accumulation) 
            and development (meristematic differentiation). 
            If Tlimits is a vector, the first element represents Tbase, and the 
            second element is the upper temperature limit for crop 
            growth and development. These values are crop-specific.
    method = There two methods according to McMaster and Wilhelm, 1997.
           1. The comparison to Tbase and Tupper is after computing 
              average temperature.

           2. The comparison to Tbase and Tupper is prior computing 
               average temperature.

    NOTE: The function accepts NaNs and no substitutions or estimations
          are carried out for those days.

    Outputs:
           TT = daily thermal time. Units are C-d (or Cd).
           TTcum = cumulative thermal time.

    Andres Patrignani - 19 Mar 2017.

    Code based on manuscript by McMaster, G.S. and W.W. Wilhelm. 1997. Agric.
    and Forest meteorology. 87:291-300.
    """

    Tbase = Tlimits[0]
    Tupper = Tlimits[1]

    # Compare to Tbase after computing TAVG.
    if method == 1:
        TAVG = np.nanmean([TMAX,TMIN],axis=0)
        TAVG = np.nanmax(TAVG,Tbase)
        if len(Tlimits) == 2: # If upper temperature threshold was entered.
            TAVG = np.min(TAVG,Tupper)

    # Compare to Tbase before computing TAVG.    
    elif method == 2: 
        TMAX = np.max(TMAX,Tbase)
        TMIN = np.max(TMIN,Tbase)
        if len(Tlimits) == 2: # If upper temperature threshold was entered.
            TMAX = np.min(TMAX,Tupper)
            TMIN = np.min(TMIN,Tupper)

        TAVG = np.nanmean([TMAX,TMIN],0)    


    TT = TAVG - Tbase # General thermal time equation.
    TT = np.nan_to_num(TT)
    TTcum = np.cumsum(TT) # Cumulative thermal time.

    return TT,TTcum
```
