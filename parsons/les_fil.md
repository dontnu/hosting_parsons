---
layout: default
title: Les karakterer fra fil
---
Du er gitt en datafil (som tekst) hvor hver linje har studentnavn, emne og karakter, skilt med semikolon.
Første linje er overskrift.

Eksempel:
```
student_name;course_name:grade
Alice;Mathematics:A
Bob;Physics:B
Alice;Physics:A
Charlie;Mathematics:C
Bob;Mathematics:B
```

Skriv funksjonen ```read_grades(filename)``` som leser filen og returnerer en ordbok, hvor
- nøkkelen er studentens navn,
- verdiene er en ordbok som kobler emne til karakter

For eksempel-data over skal følgende returneres:
```python
{'Alice': {'Mathematics': 'A', 'Physics': 'A'}, 'Bob': {'Physics': 'B', 'Mathematics': 'B'}, 'Charlie': {'Mathematics': 'C'}}
```

<div id="sortableTrash" class="sortable-code"></div> 
<div id="sortable" class="sortable-code"></div> 
<div style="clear:both;"></div> 
<p> 
    <input id="feedbackLink" value="Get Feedback" type="button" /> 
    <input id="newInstanceLink" value="Reset Problem" type="button" /> 
</p> 
<script type="text/javascript"> 
(function(){
  var initial = "def read_grades(filename):\n" +
    "    data = {}\n" +
    "    try:\n" +
    "        with open(filename, &#039;r&#039;) as file:\n" +
    "            for line in file:\n" +
    "                line = line.strip()\n" +
    "                if not line or &quot;;&quot; not in line or &quot;:&quot; not in line:\n" +
    "                    continue\n" +
    "                student, rest = line.split(&quot;;&quot;)\n" +
    "                course, grade = rest.split(&quot;:&quot;)              \n" +
    "                if student not in data:\n" +
    "                    data[student] = $$toggle::1::2::$$                \n" +
    "                data[student][course] = grade\n" +
    "    except FileNotFoundError:\n" +
    "        print(f&quot;Error: The file {filename} was not found.&quot;)\n" +
    "    return data";
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
