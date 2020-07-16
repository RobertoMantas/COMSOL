# COMSOL documentation
To open COMSOL, you have to download it from the webpage. We haven't got a subscription so you can download up to version 5.4. In case you dont have an account, you have to create it and use the license number 9074375. 
Once installed you need to add the license.dat file here included to a specific folder (there should be already one, but overwrite it with this one). For it to work, you have to be under LTUs network (VPN works) and "alvaros" computer must be turned on, because the license server is installed there.

Related to the work done to the analysis of the thermal contamination produced by the Radioisotope-Heating-Units (RHU) of ExoMars in case of non-nominal landing, I will expain the simulation procedure. 

The integrating domain is a solid cube that represents the Martian regolith, which will contain all the simulation domain with the following dimensions:

	•	1 x Block: Width: 20.0m, Depth: 20.0m and Height 5.0m
	
 The integration domain is sufficiently large to avoid any boundary condition interference within the region of interest (depth 1 m).

Inside the mentioned cube, there will be three clusters (cubes). These clusters can modify their position depth as desired and the distance between each other, depending on the measurement you are simulating. Without loss of generality, we have chosen the following cases:

	•	1 x Block: Width: 0.24m, depth: 0.36m and height 0.24m
	•	1 x Block: Width: 0.24m, depth: 0.36m and height 0.24m
	•	1 x Block: Width: 0.2m, depth: 0.3m and height 0.2m

Now, create several “monitoring” points around the clusters to be able to save the temperature evolution and plot it afterwards. For this, just create points close to the area you are interested in.

All the simulations of the Martian regolith have the following parameters in common:

	•	The soil is assumed to be dry and, because of the landing site, we do not consider ice-fill in the pores. Ice would have a significant impact on thermal conductivity and conductivity values as large as 2.5 W/(m·K)
	•	K Thermal conductivity: 0.1 W/(m·K) (Presley and Christensen 1997) 
	•	ρ Density: 1000 kg/m3 (Grott et al. 2007)
	•	Cp Heat capacity at constant pressure: 600 J/(Kg·K) (Ledlow et al. 1992)
	
These values are consistent with the recent studies of dry and ice-filled Mars regolith analogue (Experimental studies of dry and ice-filled Mars analogue regolith. Siegler, M., O. Aharonson, E. Carey, M. Choukroun, T. Hudson, N. Schorghofer, and S. Xu (2012), Measurements of thermal properties of icy Mars regolith analogs, J. Geophys. Res., 117, E03001, DOI:10.1029/2011JE003938.).  

For comparison, with an extreme case, we will finally do one other simulation with parameters that have been recently used for the thermal studies of the crust at Oxia Planum (Basaltic mantle properties. Regional heat flow and subsurface temperature patterns at Elysium Planitia and Oxia Planum areas, Mars. Isabel Egea-Gonzalez, Alberto Jiménez-Díaz, Laura M.Parro, Federico Mansilla James A.Holmes, Stephen R.Lewis, Manish R.Patel, JavierRuiz.) with a basaltic crust with a density of 2900 kg/m3 and thermal conductivity of 2W/(m·K).


The three heat-emitting clusters that emulate the RHUs have aluminum as a material with the following properties:

	•	'Heat capacity at a constant pressure of 900 J/(kg·K)
	•	Thermal conductivity of 238 W/(m·K) 
	•	Density ρ 2700 kg/m³

Next, we configure the heat transfer module. The options that we will adjust here will be the initial values for different solids, the boundary conditions of the block containing the regolith and the heat source for each cluster. Without loss of generality, the initial temperature value for the regolith is set to 235K. Independently of the chosen initial temperature, the system converges to a new periodic situation once the equation is integrated with the emitting power of the RHU and the surface sinusoidal temperature variation. The surface T is changing with a sinusoidal function to mimic the skin surface temperature response to solar heating. The upper and lower value of T is taken from TES remote observations. For the upper layer (the surface), the temperature will be changing with a sin function:

	T(t) =T_avg+∇T*sin⁡((2*π*(t-6))/24)

    Where:
	•	Tavg is the average temperature (235 K), which is semi-sum of Tmax and Tmin taken from TES data for Oxia Planum, (200 K-270 K) (I6).

	•	∇T is the semi-amplitude of the diurnal temperature variation (35 K) which is (Tmax-Tmin)/2 and Tmax and Tmin were taken from TES data for Oxia Planum, (200 K-270 K) (I6).

Next, we define the boundary condition and the heat source. In our case, our boundary condition will be an absorbing boundary condition at the sides and bottom (whereas the upper boundary condition is changing in time with periodic forcing which is defined as given above with T(t)).  The heat source for the cluster containing 6 RHUs is 51W, and the cluster containing 5 RHUs is 42.5W. 

Next, the integrating mesh is defined. To have good accuracy and allow for stability, we will choose an extremely fine mesh which is calibrated for general physics. The element size parameters are defined as:

	•	Maximum element size: 0.4 m
	•	Minimum element size: 0.004m
	•	Maximum element growth rate: 1.3
	•	Curvature factor: 0.2
	•	Resolution of narrow regions: 1

Finally, it is the moment to configure our solution to compute. The study is a time-dependent one. The specified integrating range is "range(0,0.5,120)" in hours, which indicates to start the simulation at 0 h and take a measurement every 30 minutes for a period of 120h. The tolerance selected is the "Physics controlled" one. 
