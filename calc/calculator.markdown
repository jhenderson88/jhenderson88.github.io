---
# Calculator test
layout: page
title: "Calculators"
permalink: /calc/calculator
---

<button type="button" onclick="Clear()">
  Clear all</button>

### Calculate B(EL) from matrix element

Matrix element: <input id="ME" type="number">
Initial J: <input id="InitJ" type="number">
Final J: <input id="FinaJ" type="number">


<button type="button" onclick="CalcBEL()">
  Calculate</button>

B(EL;i &#8594; f): 
<input id="BELif" type="number"></p>  

B(EL;f &#8594; i):  
<input id="BELfi" type="number"></p>

<BR>
<BR>

### E2 Matrix element &#8596; spectroscopic quadrupole moment:

Quadrupole moment: <input id="Q" type="number">
Matrix element: <input id="E2" type="number">

State J: <input id="stateJ" type="number">

<button type="button" onclick="CalcMEQMom()">
  Calculate</button>

<script>
  function CalcBEL(){
    var ME = Number(document.getElementById("ME").value);
    var initJ = Number(document.getElementById("InitJ").value);
    var finaJ = Number(document.getElementById("FinaJ").value);
    var BELif = Math.pow(ME,2)/(2*initJ+1);
    var BELfi = Math.pow(ME,2)/(2*finaJ+1);
    var BELifstring = BELif.toFixed(5).toString();
    var BELfistring = BELfi.toFixed(5).toString();
    document.getElementById("BELif").innerHTML=BELifstring;
    document.getElementById("BELfi").innerHTML=BELfistring;
    document.getElementById("Test").innerHTML=finaJ;
  }
  function CalcMEQMom(){
    var ME = Number(document.getElementById("E2").value);
    var Q = Number(document.getElementById("Q").value);
    var stateJ = Number(document.getElementById("stateJ").value);
    var qMom = 0;
    var E2 = 0;
    var Jterm = (stateJ*(2*stateJ - 1))/((2*stateJ+1)*(2*stateJ+3)*(stateJ+1));
    var Jpi = Jterm * 16*Math.PI/5.
    if(Math.abs(ME) > 0){
      qMom = ME * Math.sqrt(Jpi);
      E2 = ME;
      document.getElementById("Q").value=qMom.toFixed(5);  
    }
    if(Math.abs(Q) > 0){
      E2 = Q / Math.sqrt(Jpi);
      qMoM = Number(Q);
      document.getElementById("E2").value=E2.toFixed(5);
    }
  }
  function Clear(){
    var x = document.querySelectorAll("input");
    var i;
    for (i = 0; i < x.length; i++) {
      x[i].value = "";
    }
  }
</script>
