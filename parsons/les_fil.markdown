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
    "                    data[student] = {}                \n" +
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
    "show_feedback": true
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
