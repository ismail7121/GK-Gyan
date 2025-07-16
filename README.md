<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>GK-Gyan – 10,000 सवाल</title>
  <style>
    *{box-sizing:border-box} body{font-family:system-ui,Arial;margin:0;background:#f4f6fa}
    header{background:#007bff;color:#fff;text-align:center;padding:1rem}
    #search{width:100%;max-width:480px;padding:.6rem;border-radius:6px;border:1px solid #ccc;margin:1rem auto;display:block}
    .qbox{background:#fff;margin:.6rem auto;max-width:680px;padding:1rem;border-radius:8px;box-shadow:0 1px 4px rgba(0,0,0,.1)}
    .answer{color:#007bff;margin-top:.3rem}
    nav{display:flex;justify-content:center;gap:.5rem;padding:1rem 0}
    button.pg{padding:.4rem .7rem;border:none;border-radius:4px;background:#007bff;color:#fff;cursor:pointer}
    button.pg[disabled]{opacity:.4;cursor:not-allowed}
    footer{background-color:#007bff;color:#fff;text-align:center;padding:.8rem;margin-top:2rem;font-size:.9rem}
  </style>
</head>
<body>

  <header><h1>📘 GK-Gyan</h1><p>10,000 सामान्य ज्ञान प्रश्न-उत्तर</p></header>

  <input id="search" placeholder="सवाल खोजें…">

  <div id="list"></div>

  <nav>
    <button id="prev" class="pg">« पिछला</button>
    <span id="pageInfo"></span>
    <button id="next" class="pg">अगला »</button>
  </nav>

  <footer>© 2025 GK-Gyan | Ismail Shaikh</footer>

<script>
const PER_PAGE = 100;            
let all=[], page=1, filtered=[];

async function load(){
  const res = await fetch('questions.json');        
  all = await res.json();
  filtered = all;
  render();
}
function render(){
  const start=(page-1)*PER_PAGE, end=start+PER_PAGE;
  const curr = filtered.slice(start,end);
  document.getElementById('list').innerHTML =
    curr.map((q,i)=>`<div class="qbox"><strong>Q${start+i+1}:</strong> ${q.q}<div class="answer">👉 ${q.a}</div></div>`).join('');
  document.getElementById('pageInfo').textContent = `Page ${page} / ${Math.ceil(filtered.length/PER_PAGE)}`;
  document.getElementById('prev').disabled = page===1;
  document.getElementById('next').disabled = end >= filtered.length;
}
document.getElementById('search').oninput=e=>{
  const key=e.target.value.toLowerCase().trim();
  filtered = key ? all.filter(o=>o.q.toLowerCase().includes(key)) : all;
  page=1; render();
};
document.getElementById('prev').onclick=()=>{ if(page>1){page--;render();}};
document.getElementById('next').onclick=()=>{ if(page*PER_PAGE<filtered.length){page++;render();}};
load();
</script>
</body>
</html>
