---
layout: default
title: Longes increasing, Eksamen 2022 morgen, oppg 16
---
Vi ønsker nå en funksjon longest_incr(f, x_values) som skal finne det lengste intervallet hvor en funksjon f er strengt stigende innenfor serien av x-verdier i arrayet x_values. Som et eksempel, anta at funksjonen som gis inn for parameter f er np.sin(x) som vist i plottet under, og at x_values er np.arange(-6.0, 6.0, 0.01). Funksjonen skal da finne intervallet fra x = -1.57 til x = 1.57 siden dette er det lengste strekket hvor funksjonen er stigende her, fra bunnpunktet til toppunktet vist med blå piler. Altså,

longest_incr(np.sin(x), np.arange(-6.0,6.0,0.1)) skal returnere (-1.57, 1.57).

Hvis det ikke fins noen strekning hvor f er stigende i det gitte intervallet, f.eks. hvis vi har samme funksjon men x-values er np.arange(2.0, 4.0, 0.1), der funksjonen bare synker, skal longest_incr() returnere et tuppel med to like tall, f.eks. (2.0, 2.0), siden programmet som skal bruke funksjonen vår, vil kunne skjønne at med to like tall i retur, fins det egentlig ikke noe intervall med stigning.


<div id="sortableTrash" class="sortable-code"></div> 
<div id="sortable" class="sortable-code"></div> 
<div style="clear:both;"></div> 
<p> 
    <input id="feedbackLink" value="Get Feedback" type="button" /> 
    <input id="newInstanceLink" value="Reset Problem" type="button" /> 
</p> 
<script type="text/javascript"> 
(function(){
  var initial = "def longest_incr(f,x_values):\n" +
    "    start = long_start = end = long_end = x_values[0]\n" +
    "    for i in range(1, len(x_values)):\n" +
    "        if f(x_values[i]) &gt; f(x_values[i-1]):\n" +
    "            end = x_values[i]\n" +
    "            if end - start &gt; long_end - long_start:\n" +
    "                long_start, long_end = start, end\n" +
    "        else:\n" +
    "            start = end = x_values[i]\n" +
    "    return long_start, long_end\n" +
    "data[student] = [] #distractor";
  var parsonsPuzzle = new ParsonsWidget({
    "sortableId": "sortable",
    "max_wrong_lines": 10,
    "grader": ParsonsWidget._graders.LineBasedGrader,
    "exec_limit": 2500,
    "can_indent": true,
    "x_indent": 50,
    "lang": "en",
    "show_feedback": true,
    "trashId": "sortableTrash"
  });
  parsonsPuzzle.init(initial);
  parsonsPuzzle.shuffleLines();
  $("#newInstanceLink").click(function(event){ 
      event.preventDefault(); 
      parsonsPuzzle.shuffleLines(); 
  }); 
  $("#feedbackLink").click(function(event){ 
      event.preventDefault(); 
      parsonsPuzzle.getFeedback(); 
  }); 
})(); 
</script>
