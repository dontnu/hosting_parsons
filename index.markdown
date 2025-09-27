---
layout: default
title: Parsons Practice
---

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
  min-width: 260px;
}

.content {
  flex: 2 1 500px;
  min-width: 320px;
}

iframe {
  width: 100%;
  height: 500px;
  border: none;
}

/* Tree styles */
.tree {
  font-family: system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, "Apple Color Emoji", "Segoe UI Emoji";
  font-size: 14px;
  line-height: 1.4;
  padding-left: 0;
}

.tree ul {
  list-style: none;
  padding-left: 1rem;
  margin: 0.2rem 0;
}

.tree li {
  margin: 0.1rem 0;
}

.tree a {
  color: #0969da;
  text-decoration: none;
}

.tree a:hover {
  text-decoration: underline;
}

.tree details > summary {
  cursor: pointer;
  list-style: none;
  position: relative;
}

.tree details > summary::-webkit-details-marker {
  display: none;
}

.tree summary::before {
  content: "▸";
  display: inline-block;
  width: 1rem;
  color: #57606a;
}

.tree details[open] > summary::before {
  content: "▾";
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
    <h3>Problem List</h3>
    <div id="tree" class="tree" role="tree">
      <noscript>
        Please enable JavaScript to view the problem list.
      </noscript>
    </div>
  </div>
  <div class="content">
    <h3>Problem View</h3>
    <iframe id="problemFrame" name="problemFrame" src=""></iframe>
  </div>
</div>

<script>
// Collect all pages under the "parsons" folder from Jekyll site data.
{% assign parsons_pages = site.pages | where_exp: "p", "p.path contains 'parsons/'" | sort: "path" %}

// Build a simple array we can use in JS.
var parsonsPages = [
{% for p in parsons_pages %}
  {
    path: "{{ p.path | escape }}", // e.g., "parsons/example1.md" or "parsons/subdir/problem.md"
    url: "{{ p.url | relative_url }}", // Jekyll will include baseurl here
    title: "{{ p.title | default: p.name | split: '.' | first | replace: '-', ' ' | replace: '_', ' ' | capitalize }}"
  }{% unless forloop.last %},{% endunless %}
{% endfor %}
];

// Build a nested tree structure from the paths.
function buildTree(pages) {
  const root = {};

  pages.forEach(page => {
    const parts = page.path.split('/'); // ["parsons", "subdir", "file.md"]
    let node = root;
    for (let i = 0; i < parts.length; i++) {
      const part = parts[i];
      const isFile = i === parts.length - 1;
      if (isFile) {
        if (!node._files) node._files = [];
        node._files.push({
          name: page.title || part.replace(/\.[^/.]+$/, ''),
          url: page.url,
          filename: part
        });
      } else {
        node[part] = node[part] || {};
        node = node[part];
      }
    }
  });

  return root;
}

// Render the tree to DOM using <details>/<summary> for folders and <a> for files.
function renderNode(node, label, isRoot = false) {
  const ul = document.createElement('ul');

  // Directories (keys that are not "_files")
  const dirNames = Object.keys(node).filter(k => k !== '_files').sort((a, b) => a.localeCompare(b));
  dirNames.forEach(dirName => {
    const li = document.createElement('li');
    const details = document.createElement('details');
    if (isRoot) details.open = true; // Open the root "parsons" by default
    const summary = document.createElement('summary');
    summary.textContent = dirName;
    details.appendChild(summary);

    const childNode = renderNode(node[dirName], dirName, false);
    details.appendChild(childNode);
    li.appendChild(details);
    ul.appendChild(li);
  });

  // Files
  const files = (node._files || []).slice().sort((a, b) => a.name.localeCompare(b.name));
  files.forEach(file => {
    const li = document.createElement('li');
    const a = document.createElement('a');
    a.textContent = file.name;
    a.href = file.url;
    // Load into the iframe by targeting its name
    a.target = 'problemFrame';
    li.appendChild(a);
    ul.appendChild(li);
  });

  return ul;
}

function init() {
  const treeContainer = document.getElementById('tree');

  if (!Array.isArray(parsonsPages) || parsonsPages.length === 0) {
    treeContainer.textContent = 'No problems found in the "parsons" folder.';
    return;
  }

  const tree = buildTree(parsonsPages);

  // If "parsons" is the top-level folder, show its children under a single root node
  // Otherwise, just render whatever the top-level keys are.
  const topKeys = Object.keys(tree).filter(k => k !== '_files');
  if (topKeys.length === 1 && topKeys[0] === 'parsons') {
    const rootDetails = document.createElement('details');
    rootDetails.open = true;
    const rootSummary = document.createElement('summary');
    rootSummary.textContent = 'parsons';
    rootDetails.appendChild(rootSummary);
    rootDetails.appendChild(renderNode(tree['parsons'], 'parsons', true));
    treeContainer.appendChild(rootDetails);
  } else {
    // Fallback: render everything at top-level
    const root = document.createElement('div');
    root.appendChild(renderNode(tree, 'root', true));
    treeContainer.appendChild(root);
  }

  // Optionally load the first problem by default
  const firstLink = treeContainer.querySelector('a[target="problemFrame"]');
  if (firstLink) {
    document.getElementById('problemFrame').src = firstLink.href;
  }
}

document.addEventListener('DOMContentLoaded', init);
</script>
