# hello-world
A beginner
Some stata commands

*insheet using "F:\Econometrics Class\ch 15\CO2.csv"
use "F:\Econometrics Class\ch 15\CO2.dta"
sort country1 year1
tsset country1 year1

regress  co2 urban pop gdp ei
xtreg co2 urban pop gdp ei, fe /*Fixed effects model; F test for fixed effects*/
	estimates store fixed
xtreg co2 urban pop gdp ei, re /*Random effects model*/
	estimates store random
	hausman fixed random /*FEM V.S REM*/
xttest0 /******xttest0 for LM test for random effects**********/

/******* LR test for heteroscedasticity **************/
xtgls co2 urban pop gdp ei, i(country1) t(year1) igls panels(heteroskedastic)
	estimates store hetero
 xtgls co2 urban pop gdp ei, i(country1) t(year1)
	estimates store nohetero
local df=e(N_g)-1
lrtest hetero nohetero , df(`df')

*****wooldridge test for first order autocorrelation; Pesaran (2004) test for cross-sectional dependence****
* tsset country1 year1, year1ly
* findit xtserial
* net sj 3-2 st0039         
* net install st0039        
xtserial co2 urban pop gdp ei

xtcd co2 urban pop gdp ei

*****FGLS Estimation*********
iis country1
tis year1
xtgls co2 urban pop gdp ei, i(country1) t(year1) panels(correlated)corr(psar1)


/**********PCSE ESTIMATION ******************/
xtpcse co2 urban pop gdp ei, corr(psar1)
