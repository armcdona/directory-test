---
{% endraw %}{% for p in site.data.people %}{% raw %}
{
first_name: {% endraw %}{{ p["First Name"] | jsonify }}{% raw %},
last_name: {% endraw %}{{ p["Last Name"] | jsonify }}{% raw %},
email: {% endraw %}{{ p["email address"] | jsonify }}{% raw %},
institution: {% endraw %}{{ p["institution"] | jsonify }}{% raw %},
area: {% endraw %}{{ p["research area"] | jsonify }}{% raw %}
}{% endraw %}{% unless forloop.last %}{% raw %},{% endraw %}{% endunless %}{% endfor %}{% raw %}
];


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
{% endraw %}
