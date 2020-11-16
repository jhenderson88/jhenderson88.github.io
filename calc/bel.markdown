---
# B(EL) calculator
layout: page
title: "B(EL) (mixed E2/M1) calculations"
---

Calculator for mixed E2/M1 transitions. Given:

1. Matrix elements: calculates B(E2) and B(M1), and lifetimes and mixing ratios
2. B(E2) and B(M1): calculates matrix elements, and lifetimes and mixing ratios
3. Lifetimes and mixing ratios: calculates B(E2) and B(M1), and matrix elements

The above is the priority order. For example, if given matrix elements, the calculator will always do option 1. If not given matrix elements but given B(E2) and B(M1), the calculator will do option 2. In order to calculate based on lifetimes, transition strengths and matrix elements must be set to zero.

If you select the mixing to be pure E2 or M1, &#948; and &#948;<sup>2</sup> values will be ignored.

Mass (A): <input id="A" type="number" value=0>

Lifetime: <input id="tau" type="number" value=100>
Half-life: <input id="thalf" type="number" value=0>   Time units: <select name="timeunits" id="timeunits">
<option value="1">ps</option>
<option value="1000">ns</option>
<option value="0.001">fs</option>
</select>

Transition energy (MeV): <input id="energy" type="number" value=1>

Mixing ratio &#948;: <input id="delta" type="number" value=0> &#948;<sup>2</sup>: <input id="delta2" type="number" value=0> Mixing: <select name="mixing" id="mixing">
<option value="0">Pure E2</option>
<option value="1">Pure M1</option>
<option value="2">Mixed</option>
</select>

Branching ratio: <input id="BR" type="number" value=1>

Higher E J: <input id="jInit" type="number" value=2> Lower E J: <input id="jFinal" type="number" value=0>

B(E2)&#8595;: <input id="B(EL)if" type=number value=0> B(E2)&#8593;: <input id="B(EL)fi" type=number value=0> Units: <select name="BELunits" id="BELunits">
<option value="0">e^2fm^4</option>
<option value="1">e^2b^2</option>
<option value="2">W.u.</option>
</select>

E2 matrix element: <input id="ME" type=number value=0> Units: <select name="E2MEunits" id="E2MEunits">
<option value="0">efm^2</option>
<option value="1">eb</option>
</select>

B(M1)&#8595;: <input id="B(ML)if" type=number value=0> B(M1)&#8593;: <input id="B(ML)fi" type=number value=0> Units: <select name="BMLunits" id="BMLunits">
<option value="0">&mu;^2_N</option>
</select>

M1 matrix element: <input id="ME_M1" type=number value=0> Units: <select name="M1MEunits" id="M1MEunits">
<option value="0">&mu;N</option>
</select>

<button type="button" onclick="Calculate()">
Calculate</button>

<button type="button" onclick="Reset()">
Reset</button>

<script>
	function Calculate(){
		var	A	= Number(document.getElementById("A").value);
		var	thalf 	= Number(document.getElementById("thalf").value);
		var	tau 	= Number(document.getElementById("tau").value);
		var	unit	= Number(document.getElementById("timeunits").value);
		var	BR	= Number(document.getElementById("BR").value);
		var	jInit	= Number(document.getElementById("jInit").value);
		var	jFinal	= Number(document.getElementById("jFinal").value);
		var	delta	= Number(document.getElementById("delta").value);
		var	mixv	= Number(document.getElementById("mixing").value);
		
		var 	delta2	= 0;
		if(delta>0){
			var	delta2	= Math.pow(delta,2);
		}
		else{
			delta2	= Number(document.getElementById("delta2").value);
		}
		var	BEL	= Number(document.getElementById("B(EL)if").value);
		if(BEL == 0){
			BEL	= Number(document.getElementById("B(EL)fi").value);
			BEL	*=	(2*jFinal+1)/(2*jInit+1);
		}
		var	BML	= Number(document.getElementById("B(ML)if").value);
		if(BML == 0){
			BML	= Number(document.getElementById("B(ML)fi").value);
			BML	*=	(2*jInit+1)/(2*jFinal+1);
		}
		var	ME	= Number(document.getElementById("ME").value);
		var	MEM1	= Number(document.getElementById("ME_M1").value);

		var	BE2u	= Number(document.getElementById("BELunits").value);
		var	BM1u	= Number(document.getElementById("BMLunits").value);

		var	ME2u	= Number(document.getElementById("E2MEunits").value);
		var	ME1u	= Number(document.getElementById("M1MEunits").value);
	
		var	eps	= Number(27491300000000);
		var	Energy	= Number(document.getElementById("energy").value);

		var	wuE2	= 0.05940 * Math.pow(A,4./3.);	// e2fm4

		var	L_M1	= 0.444444444;
		var	L_E2	= 0.013333333;
	
		var	E2Eterm	= Math.pow((Energy/197.3269718),5);	// E2 E term
		var	M1Eterm	= Math.pow((Energy/197.3269718),3);	// M1 E term
				
		var	E2_lam	= eps * L_E2 * E2Eterm;
		var	M1_lam	= eps * L_M1 * M1Eterm * Math.pow(0.105155,2);
		
		var	BELfi	= 0;
		var	BMLfi	= 0;

		if(Math.abs(ME) > 0 && Math.abs(MEM1)>0){

			if(ME2u == 1)	
				ME *= 100;	// convert to efm2

			BEL	= Math.pow(ME,2)/(2*jInit+1);	//e2fm4
			BELfi	= BEL*(2*jInit+1)/(2*jFinal+1);

			BML	= Math.pow(MEM1,2)/(2*jInit+1);
			BMLfi	= BML*(2*jInit+1)/(2*jFinal+1);	

			if(BE2u == 0){ // e2fm4
				document.getElementById("B(EL)if").value 	= BEL.toFixed(6);		
				document.getElementById("B(EL)fi").value	= BELfi.toFixed(6);
			}
			else if(BE2u == 1){ // e2b2
				document.getElementById("B(EL)if").value 	= (BEL/10000.).toFixed(6);		
				document.getElementById("B(EL)fi").value	= (BELfi/10000).toFixed(6);
			}
			else{ // Weisskopf units
				document.getElementById("B(EL)if").value 	= (BEL/wuE2).toFixed(6);		
				document.getElementById("B(EL)fi").value	= (BEL/wuE2).toFixed(6);
			}
			document.getElementById("B(ML)if").value 	= BML.toFixed(6);		
			document.getElementById("B(ML)fi").value	= BMLfi.toFixed(6);

		}

		if(BEL>0 && BML>0 && mixv == 2){

			if(BE2u == 1){ // e2b2
				BEL	= BEL * 10000;
			}
			else if(BE2u == 2){ // Weisskopf units 
				BEL	= BEL * wuE2;
			}

			var	lambdaE2	= E2_lam * BEL / 1000.;
			var	lambdaM1	= M1_lam * BML / 1000.;

			lambda	= lambdaE2 + lambdaM1;
	
			delta2	= lambda / lambdaM1 - 1;
			delta	= Math.sqrt(delta2);

			lambda	= lambda / BR;
		
			thalf	= 0.69314718056/lambda;
			tau	= 1/lambda;
		
			thalf	= thalf / unit;
			tau	= tau / unit;
	
			document.getElementById("tau").value	= tau.toFixed(6);
			document.getElementById("thalf").value	= thalf.toFixed(6);

			document.getElementById("delta").value	= delta.toFixed(6);
			document.getElementById("delta2").value	= delta2.toFixed(6);

			document.getElementById("B(EL)if").value = BEL;
			document.getElementById("B(EL)fi").value = BEL * (2*jInit+1)/(2*jFinal+1);

			document.getElementById("B(ML)if").value = BML;
			document.getElementById("B(ML)fi").value = BML * (2*jInit+1)/(2*jFinal+1);

			if(Math.abs(ME) == 0 || Math.abs(MEM1) == 0){
				ME	= Math.sqrt(BEL*(2*jInit+1));
				MEM1	= Math.sqrt(BML*(2*jInit+1));	

				if(ME2u == 1)	// eb
					ME	/= 100;		

				document.getElementById("ME").value		= ME.toFixed(6);
				document.getElementById("ME_M1").value		= MEM1.toFixed(6);

			}
		}
		else if(BEL > 0 && mixv == 0){ // Pure E2

			if(BE2u == 1){ // e2b2
				BEL	= BEL * 10000;
			}
			else if(BE2u == 2){ // Weisskopf units 
				BEL	= BEL * wuE2;
			}

			var	lambdaE2	= E2_lam * BEL / 1000.;

			lambda	= lambdaE2;

			lambda	= lambda / BR;
		
			thalf	= 0.69314718056/lambda;
			tau	= 1/lambda;
		
			thalf	= thalf / unit;
			tau	= tau / unit;
	
			document.getElementById("tau").value	= tau.toFixed(6);
			document.getElementById("thalf").value	= thalf.toFixed(6);

			document.getElementById("B(EL)if").value = BEL;
			document.getElementById("B(EL)fi").value = BEL * (2*jInit+1)/(2*jFinal+1);

			if(Math.abs(ME) == 0){
				ME	= Math.sqrt(BEL*(2*jInit+1));

				if(ME2u == 1)	// eb
					ME	/= 100;		

				document.getElementById("ME").value		= ME.toFixed(6);

			}

		}
		else if(BML > 0 && mixv == 1){ // Pure M1

			var	lambdaM1	= M1_lam * BML / 1000.;

			lambda	= lambdaM1;

			lambda	= lambda / BR;
		
			thalf	= 0.69314718056/lambda;
			tau	= 1/lambda;
		
			thalf	= thalf / unit;
			tau	= tau / unit;
	
			document.getElementById("tau").value	= tau.toFixed(6);
			document.getElementById("thalf").value	= thalf.toFixed(6);

			document.getElementById("B(ML)if").value = BML;
			document.getElementById("B(ML)fi").value = BML * (2*jInit+1)/(2*jFinal+1);

			if(Math.abs(MEM1) == 0){
				MEM1	= Math.sqrt(BML*(2*jInit+1));	

				document.getElementById("ME_M1").value		= MEM1.toFixed(6);

			}

		}
		else if(tau > 0 || thalf > 0){
	
			thalf	*= unit;
			tau	*= unit;
	
			var 	lambda 	= 0;
			if(tau > 0){
				lambda	= 1/tau;
			}
			else if(thalf > 0){
				lambda	= 0.69314718056/thalf;
			}
	
			lambda	*= BR;

			if(mixv == 2){ // Mixed
				var	lambdaE2	= lambda/(1+1/delta2);
				var	lambdaM1	= lambda/(1+delta2);
			
				BEL	= lambdaE2 * 1000. / E2_lam; 	
				BML	= lambdaM1 * 1000. / M1_lam;
				BELfi	= BEL*(2*jInit+1)/(2*jFinal+1);
				BMLfi	= BML*(2*jInit+1)/(2*jFinal+1);	
			
				ME	= Math.sqrt(BEL*(2*jInit+1));
				MEM1	= Math.sqrt(BML*(2*jInit+1));	
		

				if(BE2u == 0){ // e2fm4
					document.getElementById("B(EL)if").value 	= BEL.toFixed(6);		
					document.getElementById("B(EL)fi").value	= BELfi.toFixed(6);
				}
				else if(BE2u == 1){ // e2b2
					document.getElementById("B(EL)if").value 	= (BEL/10000.).toFixed(6);		
					document.getElementById("B(EL)fi").value	= (BELfi/10000).toFixed(6);
				}
				else{ // Weisskopf units
					document.getElementById("B(EL)if").value 	= (BEL/wuE2).toFixed(6);		
					document.getElementById("B(EL)fi").value	= (BEL/wuE2).toFixed(6);
				}
				document.getElementById("B(ML)if").value 	= BML.toFixed(6);		
				document.getElementById("B(ML)fi").value	= BMLfi.toFixed(6);

				if(ME2u == 1)
					ME	/= 100;

				document.getElementById("ME").value		= ME.toFixed(6);
				document.getElementById("ME_M1").value		= MEM1.toFixed(6);

				document.getElementById("delta2").value		= delta2;
			}
			else if(mixv == 0){ // Pure E2
				var	lambdaE2	= lambda;
			
				BEL	= lambdaE2 * 1000. / E2_lam; 	
				BELfi	= BEL*(2*jInit+1)/(2*jFinal+1);
			
				ME	= Math.sqrt(BEL*(2*jInit+1));
		

				if(BE2u == 0){ // e2fm4
					document.getElementById("B(EL)if").value 	= BEL.toFixed(6);		
					document.getElementById("B(EL)fi").value	= BELfi.toFixed(6);
				}
				else if(BE2u == 1){ // e2b2
					document.getElementById("B(EL)if").value 	= (BEL/10000.).toFixed(6);		
					document.getElementById("B(EL)fi").value	= (BELfi/10000).toFixed(6);
				}
				else{ // Weisskopf units
					document.getElementById("B(EL)if").value 	= (BEL/wuE2).toFixed(6);		
					document.getElementById("B(EL)fi").value	= (BEL/wuE2).toFixed(6);
				}

				if(ME2u == 1)
					ME	/= 100;

				document.getElementById("ME").value		= ME.toFixed(6);
			}
			else if(mixv == 1){ // Pure M1
				var	lambdaM1	= lambda;
			
				BML	= lambdaM1 * 1000. / M1_lam;
				BMLfi	= BML*(2*jInit+1)/(2*jFinal+1);	
			
				MEM1	= Math.sqrt(BML*(2*jInit+1));	

				document.getElementById("B(ML)if").value 	= BML.toFixed(6);		
				document.getElementById("B(ML)fi").value	= BMLfi.toFixed(6);
				document.getElementById("ME_M1").value		= MEM1.toFixed(6);
			}

		}

	}	
	
  	function Reset(){
    		var x = document.querySelectorAll("input");
    		var i;
    		for (i = 0; i < x.length; i++) {
      			x[i].value = 0;
    		}
		document.getElementById("A").value	= 100;
		document.getElementById("energy").value	= 1;
		document.getElementById("jInit").value	= 2;
		document.getElementById("jFinal").value	= 0;
		document.getElementById("BR").value	= 1;
		document.getElementById("mixing").value	= 0;
  	}
</script>
