# Availability

Estimating geographic space available to animals based on telemetry data.

## Installation:
```
library(devtools)
install_github("AustralianAntarcticDataCentre/availability")
```

## Example usage:
Using the vector-AR method (see [1]):
```
library(availability)
realtrack=... ## load observed track, 2-column matrix of longitude and latitude. The track points should be equally sampled in time
arf=surrogateARModel(realtrack) ## fit AR model to track
st=surrogateAR(arf,realtrack) ## simulate new track
```

Or to fit using the crawl-based track simulator:
```
library(availability)
fit=crwMLE(...) ## fit crawl to your raw data
pr=fit$pred ## extract predicted (fitted) track
pr=pr[pr$locType=="p",] ## keep only regularly-interpolated locations
## construct the corresponding transition and covariance matrices of the state space model for time increment `dt`
model=surrogateCrawlModel(fit,dt)
## and finally simulate the track
stcrw=surrogateCrawl(model,as.matrix(pr[,c("mu.x","mu.y","nu.x","nu.y")]),pr$date)
```
## References
[1] Raymond B et al. (2014) Important marine habitat off East Antarctica revealed by two decades of multi-species predator tracking. /Ecography/ *38*:121–129. [doi:10.1111/ecog.01021](http://dx.doi.org/10.1111/ecog.01021)
