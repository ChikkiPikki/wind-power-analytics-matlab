1. Resultant wind speed
$$v_{\text{resultant}}=\sqrt{ u_{100}^2+v_{100}^2 }$$
MATLAB:
```Octave
v_resultant = sqrt(u100.^2+v100.^2);
```

2. Wind direction
$$v_{\text{direction}}=\tan^{-1}(v_{100}, u_{100})$$



Problem: $\tan^{-1}$ will give the angle, but we don't know the quadrant. For that, we use the function `atan2d` in MATLAB to get angle in terms of $2\pi$ rather than $\pi$.

`atan2d`: $\tan^{-1}$, $2\pi$, degree output

MATLAB:
```Octave
v_dir = atan2d(v100, u100);
```
___
## Time series data

1. Data extraction
```octave
Guj = v_resultant(31, 3, :); %% 4th row, 31st column
V_Guj = Guj(:);

plot(V_Guj)
```
Automatically the last column would be taken when semicolon is taken (line 2)
___
## Important graphs

1. Time v/s velocity
2. Wind rose
![[Pasted image 20231211161101.png|400]]
```octave
u100 = ncread("/MATLAB Drive/adaptor.mars.internal-1702029592.401762-29764-12-d9b88cdf-3135-40c5-8094-43f8e64e1078.nc", 'u100');
v100 = ncread("/MATLAB Drive/adaptor.mars.internal-1702029592.401762-29764-12-d9b88cdf-3135-40c5-8094-43f8e64e1078.nc", 'v100');
time = ncread("/MATLAB Drive/adaptor.mars.internal-1702029592.401762-29764-12-d9b88cdf-3135-40c5-8094-43f8e64e1078.nc", 'time');
longitude = ncread("/MATLAB Drive/adaptor.mars.internal-1702029592.401762-29764-12-d9b88cdf-3135-40c5-8094-43f8e64e1078.nc", 'longitude');
latitude = ncread("/MATLAB Drive/adaptor.mars.internal-1702029592.401762-29764-12-d9b88cdf-3135-40c5-8094-43f8e64e1078.nc", 'latitude');

v_resultant = sqrt(u100.^2+v100.^2);
v_dir = atan2d(v100, u100);
Guj = v_resultant(31, 4, :); %% 4th row, 31st column
V_Guj = Guj(:);
D_Guj = v_dir(31,4,:);
dvd_Guj = D_Guj(:);
plot(V_Guj)

wind_rose(dvd_Guj, V_Guj)
```
3. Wind power density
$$
\mathrm{WPD}=\frac{1}{2}\rho v^3
$$
	$v$: resultant velocity
	$\rho$: density of air in $\text{kg/m}^3$ (assume $1.225$)
	Power generation in $1 \text{ sq m area}$ ^f04cb4 ^53e079 ^66ed5f
4. Weibul distribution #incomplete
	 Depends on scale and shape, which can be extracted using the function:
```octave
paraMat = wblfit(V_Guj);
```
![[Pasted image 20231211161336.png]]
5. Capacity factor: the ratio of power generated to the rated power. Tells how efficient our turbine is
$$
	\text{CF} = \frac{\text{PW}}{\text{PWR}}
$$
___
### Syntax for power generation
```octave
for k = 1:132
	U = v_Guj;
	if (U(k)<4)
		Pw(k)=0;
	elseif (U(k)>14)
		Pw(k) = Pwr;
	else
		Pw(k) = 
```
___
# Footnotes
1. Code for wind-rose
```octave
function wind_rose(wind_direction,wind_speed)
%WIND_ROSE Plot a wind rose
%   this plots a wind rose
figure
pax = polaraxes;
polarhistogram(deg2rad(wind_direction(wind_speed<25)),deg2rad(0:10:360),'displayname','20 - 25 m/s')
hold on
polarhistogram(deg2rad(wind_direction(wind_speed<20)),deg2rad(0:10:360),'FaceColor','red','displayname','15 - 20 m/s')
polarhistogram(deg2rad(wind_direction(wind_speed<15)),deg2rad(0:10:360),'FaceColor','yellow','displayname','10 - 15 m/s')
polarhistogram(deg2rad(wind_direction(wind_speed<10)),deg2rad(0:10:360),'FaceColor','green','displayname','5 - 10 m/s')
polarhistogram(deg2rad(wind_direction(wind_speed<5)),deg2rad(0:10:360),'FaceColor','blue','displayname','0 - 5 m/s')
pax.ThetaDir = 'clockwise';
pax.ThetaZeroLocation = 'top';
legend('Show')
title('Wind Rose')

end
```
