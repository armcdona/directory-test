---
layout: default
title: Home
---

# {{ site.title }}

Type in any field to filter. Searches are case-insensitive and match partial words.

<div id="directory-app">
  <div class="filters">
    <label>
      Name
      <input id="q-name" type="text" placeholder="e.g., Ada or Lovelace" />
    </label>
    <label>
      Institution
      <input id="q-inst" type="text" placeholder="e.g., MIT" />
    </label>
    <label>
      Research area
      <input id="q-area" type="text" placeholder="e.g., machine learning" />
    </label>
    <button id="clear-btn" type="button">Clear</button>
  </div>

  <div id="counts" aria-live="polite"></div>

  <table id="results" class="dir-table">
    <thead>
      <tr>
        <th>Name</th>
        <th>Email</th>
        <th>Institution</th>
        <th>Research area</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>
</div>

<style>
  #directory-app { max-width: 1100px; }
  .filters { display: grid; grid-template-columns: repeat(auto-fit,minmax(220px,1fr)); gap: .75rem; margin: 1rem 0; }
  .filters label { display: grid; gap: .25rem; font-weight: 600; }
  .filters input { padding: .5rem; font: inherit; }
  #clear-btn { padding: .55rem .9rem; margin-top: auto; align-self: end; }
  .dir-table { width: 100%; border-collapse: collapse; }
  .dir-table th, .dir-table td { padding: .6rem .5rem; border-bottom: 1px solid #e5e5e5; vertical-align: top; }
  .dir-table tbody tr:hover { background: #fafafa; }
  .muted { color: #666; }
</style>

<script>
  // Load the array directly from _data/people.json
  const PEOPLE = {{ site.data.people | jsonify }};

  const state = { name: '', inst: '', area: '' };

  const $ = sel => document.querySelector(sel);
  const tbody = document.querySelector('#results tbody');
  const counts = $('#counts');

  function normalize(s){ return (s || '').toString().normalize('NFKD').toLowerCase(); }

  function matches(person){
    const n = normalize(state.name);
    const i = normalize(state.inst);
    const a = normalize(state.area);

    const nameHay = normalize(`${person.first_name} ${person.last_name}`);
    const instHay = normalize(person.institution);
    const areaHay = normalize(person.area);

    return (!n || nameHay.includes(n)) &&
           (!i || instHay.includes(i)) &&
           (!a || areaHay.includes(a));
  }

  function render(){
    const rows = PEOPLE.filter(matches);
    tbody.innerHTML = rows.map(r => `
      <tr>
        <td><strong>${r.first_name} ${r.last_name}</strong></td>
        <td>${r.email ? `<a href="mailto:${r.email}">${r.email}</a>` : '<span class="muted">—</span>'}</td>
        <td>${r.institution || '<span class="muted">—</span>'}</td>
        <td>${r.area || '<span class="muted">—</span>'}</td>
      </tr>
    `).join('');

    const total = PEOPLE.length;
    const shown = rows.length;
    counts.textContent = `${shown} of ${total} ${total === 1 ? 'entry' : 'entries'} shown`;
  }

  function bind(){
    $('#q-name').addEventListener('input', e => { state.name = e.target.value; render(); });
    $('#q-inst').addEventListener('input', e => { state.inst = e.target.value; render(); });
    $('#q-area').addEventListener('input', e => { state.area = e.target.value; render(); });
    $('#clear-btn').addEventListener('click', () => {
      state.name = state.inst = state.area = '';
      $('#q-name').value = $('#q-inst').value = $('#q-area').value = '';
      render();
    });
  }

  bind();
  render();
</script>
