---
# Calculator test
layout: page
title: "Energy velocity calculator"
---

Simple calculator to relate masses, energies and velocities. Given:

1. Mass and energy: calculates &#946; and velocity 
2. Velocity and/or &#946; and mass: calculates energy 
3. Velocity and/or &#946; and mass: calculates mass

Mass: <input id="Mass" type="number" value=0> <select name="massunits" id="massunits">
<option value="0">MeV/c^2</option>
<option value="1">u</option>
</select>

Energy <input id="Energy" type="number" value=0> <select name="energyunits" id="energyunits">
<option value="1">MeV</option>
<option value="0.001">keV</option>     
<option value="1000">GeV</option>
<option value="-1">MeV/u</option>
</select>

Beta: <input id="Beta" type="number" value=0> Velocity: <input id="velo" type="number" value=0> <select name="velounits" id="velounits">
<option value="1">cm/ns</option>
<option value="0.001">cm/ps</option>
</select>

<button type="button" onclick="Calculate()">
Calculate</button> 

<button type="button" onclick="Clear()">
Clear</button>

<p id="Error"></p>

<script>

	function Calculate(){

		var	mass	= Number(document.getElementById("Mass").value);
		var	mass_u	= Number(document.getElementById("massunits").value);
		if(mass_u == 1)
			mass *= 931.494102;

		var	energy	= Number(document.getElementById("Energy").value);
		var	e_u	= Number(document.getElementById("energyunits").value);
		if(e_u == -1){
			energy	*= mass/931.494102;
		}
		else{
			energy	*= e_u;
		}

		var	beta	= Number(document.getElementById("Beta").value);
		var	velo	= Number(document.getElementById("velo").value);
		var	v_u	= Number(document.getElementById("velounits").value);
		velo 	*= v_u;

		if(beta == 0 && velo == 0){	// No velocity information
			var	Tmc 	= energy/mass;
			var	gamma	= Tmc + 1;
			beta	= Math.sqrt(1-(1/Math.pow(gamma,2)));
			velo	= beta * 29.9792;
			document.getElementById("Beta").value	= beta;
			document.getElementById("velo").value	= velo * v_u;
		}
		else if(energy == 0){	// Calculate energy from velocity and mass
			if(beta == 0)
				beta	= velo / 29.9792;
			else
				velo	= beta * 29.9792;
			var 	gamma	= Math.sqrt(1/(1-Math.pow(beta,2)));
			var	Tmc	= gamma	- 1;
			energy	= Tmc * mass;
			if(energyunits == -1)
				energy = Tmc;
			else
				energy /= e_u;
			document.getElementById("Beta").value	= beta;
			document.getElementById("velo").value	= velo * v_u;
			document.getElementById("Energy").value	= energy;
			if(beta>1)
				document.getElementById("Error").innerHTML = "Error: in this household we do not go faster than the speed of light";
		}
		else{	// Calculate mass from velocity and energy
			if(beta == 0)
				beta	= velo / 29.9792;
			else
				velo	= beta * 29.9792;
			var	gamma	= Math.sqrt(1/(1-Math.pow(beta,2)));
			var	Tmc	= gamma - 1;
			var	mass	= energy / Tmc;
			if(mass_u == 1)
				mass /= 931.494102;
			document.getElementById("Beta").value	= beta;
			document.getElementById("velo").value	= velo * v_u;
			document.getElementById("Mass").value	= mass;
			if(beta>1)
				document.getElementById("Error").innerHTML = "Error: in this household we do not go faster than the speed of light";
		}

	}
	function Clear(){
    		var x = document.querySelectorAll("input");
    		var i;
    		for (i = 0; i < x.length; i++) {
      			x[i].value = "";
    		}
		document.getElementById("Error").innerHTML = "";
	}

</script>
