# Parsons Practice

<style>
.container {
  display: flex;
  flex-direction: row;
  gap: 20px;
  flex-wrap: wrap;
}

.sidebar {
  flex: 1 1 250px;
  border-right: 1px solid #ccc;
  padding-right: 10px;
}

.content {
  flex: 2 1 500px;
}

iframe {
  width: 100%;
  height: 500px;
  border: none;
}

/* Mobile responsiveness */
@media (max-width: 768px) {
  .container {
    flex-direction: column;
  }
  .sidebar {
    border-right: none;
    border-bottom: 1px solid #ccc;
    padding-bottom: 10px;
  }
}
</style>

<div class="container">
  <div class="sidebar">
    ### Problem List
    <ul>
      <li>#Les fil og lag dictionary</a></li>
      <li><a href="#"igende, Eksamen 22 morgen, oppg 16</a></li>
    </ul>
  </div>
  <div class="content">
    ### Problem View
    <iframe id="problemFrame" src=""></iframe>
  </div>
</div>

<script>
function loadProblem(url) {
  document.getElementById('problemFrame').src = url;
}
</script>
