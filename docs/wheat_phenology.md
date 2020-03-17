# Wheat phenology model

Implementation of the Wang-Engel model (Wang and Engel, 1998) for simulating the phenological development of winter wheat based on air temperature records and variety-specific cardinal temperatures. In this exercise we will implement an improved version of the Wang-Engel model as detailed in Streck et al. (2003).

To better understand the model it is recommended some level of familiarity with wheat phenological stages and the Zadoks decimal code (Zadoks et al., 1974).



```python
def wheatstages(Tmax,Tmin,photoperiod,par):
    
    tavg = np.mean([Tmax,Tmin],axis=0)
    limVg1 = 0.45; # Limit first vegetative stage 0.4=first hollow stem
    limVg2 = 1;   # Limit anthesis

    # Define Lamda functions
    alphafn = lambda TMIN,TOPT,TMAX: log(2)/log((TMAX-TMIN)/(TOPT-TMIN))
    betafun = lambda TMIN,TOPT,TMAX,T,ALPHA: max((2*(T-TMIN)**ALPHA * (TOPT-TMIN)**ALPHA - (T-TMIN)**(2*ALPHA))/(TOPT-TMIN)**(2*ALPHA),0)
    hopp = 17; # Optimal photoperiod. Major, 1980. PHOTOPERIOD RESPONSE CHARACTERISTICS CONTROLLING FLOWERING OF NINE CROP SPECIES 
    Pc = hopp - 4/par['omega']
    Pfun = lambda Pp: max(1-exp(-par['chi']*par['omega']*(Pp-Pc)),0)
    Vfun = lambda VD: (VD**5) / ((par['VDfull']/2)**5 + VD**5)

    # Define weighing functions
    alphaVg1 = alphafn(par['TminVg1'],par['ToptVg1'],par['TmaxVg1'])
    alphaVg2 = alphafn(par['TminVg2'],par['ToptVg2'],par['TmaxVg2'])
    alphaVn = alphafn(par['TminVn'],par['ToptVn'],par['TmaxVn'])
    alphaRp = alphafn(par['TminRp'],par['ToptRp'],par['TmaxRp'])

    
    # Initialize arrays
    N = len(Tmax)
    r = np.full(N,np.nan)
    Tstress = np.full(N,np.nan)
    VD = np.full(N,np.nan)
    stage = np.full(N,np.nan)
    TT = np.full(N,np.nan)
    TTcum = np.full(N,np.nan)
    
    # Values at time time t=0
    r[0] = 0
    Tstress[0] = 0
    VD[0] = 0
    stage[0] = 0
    TT[0] = thermaltime(Tmax[0],Tmin[0], [par['TminEm'],par['TmaxEm']])
    TTcum[0] = TT[0]
    
    for i in range(1,N):

        if TTcum[i-1] < par['TTemerge']:
            r[i] = 0.0
            Tlimits = [par['TminEm'],par['TmaxEm']]
            
        elif stage[i-1] < limVg1:

            # Temperature
            if np.logical_or(tavg[i] < par['TminVg1'], tavg[i] > par['TmaxVg1']):
                fT = 0.0
            else:
                fT = betafun(par['TminVg1'],par['ToptVg1'],par['TmaxVg1'],tavg[i],alphaVg1)

            # Photoperiod
            fP = Pfun(photoperiod[i])

            # Vernalization
            if np.logical_or(tavg[i] < par['TminVn'], tavg[i] > par['TmaxVn']):
                VD[i] = 0.0 # Strong control over the initial stages.
            else:
                VD[i] = betafun(par['TminVn'],par['ToptVn'],par['TmaxVn'],tavg[i],alphaVn)

            fV = Vfun(np.nansum(VD[0:i]))

            r[i] = max(par['RmaxVg1'] * fT * fP * fV, 0.001) # Calculate development rate when vernalization was triggered

            # Select cardinal temperatures for current stage
            Tlimits = [par['TminVg1'],par['TmaxVg1']]
            
            # Temperature stress index
            Tstress[i] = 1.0 - fT

        elif np.logical_and(stage[i-1] >= limVg1, stage[i-1] <= limVg2):

            # Temperature
            if np.logical_or(tavg[i] < par['TminVg2'], tavg[i] > par['TmaxVg2']):
                fT = 0.0
            else:
                fT = betafun(par['TminVg2'],par['ToptVg2'],par['TmaxVg2'],tavg[i],alphaVg2)

            # Photoperiod
            fP = Pfun(photoperiod[i])

            # Calculate development rate
            r[i] = par['RmaxVg2'] * fT * fP

            # Select cardinal temperatures for current stage
            Tlimits = [par['TminVg2'],par['TmaxVg2']]
            
            # Temperature stress index
            Tstress[i] = 1.0 - fT

        else:
            # Temperature
            if np.logical_or(tavg[i] < par['TminRp'], tavg[i] > par['TmaxRp']):
                fT = 0.0 
            else:
                fT = betafun(par['TminRp'],par['ToptRp'],par['TmaxRp'],tavg[i],alphaRp)

            # Calculate development rate
            r[i] = par['RmaxRp'] * fT

            # Select cardinal temperatures for current stage
            Tlimits = [par['TminRp'],par['TmaxRp']]
            
            # Temperature stress index
            Tstress[i] = 1.0 - fT
        
        TT[i] = thermaltime(Tmax[i],Tmin[i],Tlimits)
        TTcum[i] = TTcum[i-1] + TT[i]
        stage[i] = min(sum(r[0:i]),2)
        
    return stage,TT,TTcum,Tstress
```

## References

Streck, N.A., Weiss, A., Xue, Q. and Baenziger, P.S., 2003. Improving predictions of developmental stages in winter wheat: a modified Wang and Engel model. Agricultural and Forest Meteorology, 115(3-4), pp.139-150.

Wang, E. and Engel, T., 1998. Simulation of phenological development of wheat crops. Agricultural systems, 58(1), pp.1-24.

Zadoks, J.C., Chang, T.T. and Konzak, C.F., 1974. A decimal code for the growth stages of cereals. Weed research, 14(6), pp.415-421.
