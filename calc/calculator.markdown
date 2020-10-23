---
# Calculator test
layout: page
title: "Calculator"
permalink: /calc/calculator
---

### Calculate B(EL) from matrix element

Matrix element:  <input id="ME" type="number">
Initial J: <input id="InitJ" type="number">
Final J: <input id="FinaJ" type="number">


<button type="button" onclick="CalcBEL()">
  Calculate</button>

B(EL;i &#8594; f): 
<p style="display:inline" id="BELif"></p>  

B(EL;f &#8594; i):  
<p style="display:inline" id="BELfi"></p>

<BR>
<BR>

### E2 Matrix element &#8596; spectroscopic quadrupole moment:

Quadrupole moment: <input id="Q" type="number">
Matrix element: <input id="E2" type="number">

State J: <input id="J" type="number">

<button type="button" onclick="CalcMEQMom()">
  Calculate</button>


<script>
  function CalcBEL(){
    var ME = document.getElementById("ME").value;
    var initJ = document.getElementById("InitJ").value;
    var finaJ = document.getElementById("FinaJ").value;
    var BELif = Math.pow(ME,2)/(2*initJ+1);
    var BELfi = Math.pow(ME,2)/(2*finaJ+1);
    var BELifstring = BELif.toFixed(5).toString();
    var BELfistring = BELfi.toFixed(5).toString();
    document.getElementById("BELif").innerHTML=BELifstring;
    document.getElementById("BELfi").innerHTML=BELfistring;
    document.getElementById("Test").innerHTML=finaJ;
  }
  function CalcMEQMom(){
    var ME = document.getElementById("E2").value;
    var Q = document.getElementById("Q").value;
    var J = document.getElementById("J").value;
    var qMom = 0;
    var E2 = 0;
    if(ME!=0){
      qMom = ME * Math.sqrt(((J * (2 * J -1))/((2*J+1)*(2*J+3)*(J+1))) * (16*Math.pi()/5.))
      E2 = ME
    }
    if(Q!=0){
      E2 = Q / Math.sqrt(((J * (2 * J -1))/((2*J+1)*(2*J+3)*(J+1))) * (16*Math.pi()/5.))
      qMoM = Q
    }
    document.getElementById("E2").value=E2.toFixed(5).toString();
    document.getElementById("Q").value=qMom.toFixed(5).toString();
  
  }
</script>
