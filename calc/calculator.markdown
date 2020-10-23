---
# Calculator test
layout: page
title: "Calculator"
permalink: /calc/calculator
---

Calculate B(EL) from matrix element

Matrix element:  <input id="ME" type="number">
Initial J: <input id="InitJ" type="number">
Final J: <input id="FinaJ" type="number">


<button type="button" onclick="CalcBEL()">
  Calculate</button>

B(EL;i &#8594; f): 
<p style="display:inline" id="BELif"></p>

B(EL;f &#8594; i): 
<p style="display:inline" id="BELfi"></p>

Test: <p style="display:inline" id="Test"></p>

<script>
  function CalcBEL(){
    var ME = document.getElementById("ME").value;
    var initJ = document.getElementById("InitJ").value;
    var finaJ = document.getElementById("FinaJ").value;
    var BELif = Math.pow(ME,2)/(2*initJ+1);
    var BELfi = Math.pow(ME,2)/(2*finaJ+1);
    var BELifstring = BELif.toString();
    var BELfistring = BELfi.toString();
    document.getElementById("BELif").innerHTML=BELifstring;
    document.getElementById("BELfi").innerHTML=BELfistring;
    document.getElementById("Test").innerHTML=finaJ;
  }
</script>
