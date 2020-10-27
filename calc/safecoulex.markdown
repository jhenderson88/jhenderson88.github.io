---
layout: page
title: "Safe Coulomb excitation"
---

Calculates the maximum energy that can be used to satisfy safe Coulomb excitation based on a user provided minimum separation. The "Cline condition" if selected uses the canonical 5 fm separation, regardless of user input. The scattering angle (in the center of mass frame) can also be specified, although this should be used with caution as nuclear effects can "focus" particles to forward angles, bringing unsafe events into the safe regime.

Target A: <input id="targetA" type="number"> Target Z: <input id="targetZ" type="number">

Projectile A: <input id="projA" type="number"> Projectile Z: <input id="projZ" type="number">

Condition: <select name="condition" id="condition">
<option value=0>Cline</option>
<option value=1>Minimum separation</option>
</select>

Separation (fm): <input id="separation" type="number">

Angle (center of mass, degrees): <input id="angle" type="number" value="180">

Safe CoulEx limit (MeV): <var id="safelim"></var>

<button type="button" onclick="Calculate()">
Calculate </button>

<button type="button" onclick="Clear()">
Clear </button>

<script>
	
	function Calculate(){

		var	tA	= Number(document.getElementById("targetA").value);
		var	tZ	= Number(document.getElementById("targetZ").value);
		var	pA	= Number(document.getElementById("projA").value);
		var	pZ	= Number(document.getElementById("projZ").value);
		
		var	sep	= 0;
		var	con	= Number(document.getElementById("condition").value);
		if(con == 0)
			sep	= 5;
		else
			sep	= Number(document.getElementById("separation").value);

		var	angle	= Number(document.getElementById("angle").value);
		angle	= angle * Math.PI/180.;

		var	closest	= sep + 1.25*(Math.pow(tA,1./3.)+Math.pow(pA,1./3.));

		var	lim	= 0.72 * (pZ*tZ/closest)*((tA+pA)/tA)*(1+(1/Math.sin(angle/2.)));

		document.getElementById("safelim").innerHTML	= lim.toFixed(2).toString();
		document.getElementById("separation").value	= sep;

	}

	function Clear(){
    		var x = document.querySelectorAll("input");
    		var i;
    		for (i = 0; i < x.length; i++) {
      			x[i].value = "";
    		}
		document.getElementById("angle").value = 180;
	}

</script>
