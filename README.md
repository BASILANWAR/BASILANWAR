<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>GitHub Profile README Generator — CUSAT Instrumentation</title>
  <style>
    :root{--bg:#0f1724;--card:#0b1220;--accent:#06b6d4;--muted:#94a3b8}
    html,body{height:100%;margin:0;font-family:Inter,system-ui,Segoe UI,Roboto,Arial;background:linear-gradient(180deg,#071026 0%, #071A2B 100%);color:#e6eef6}
    .container{max-width:980px;margin:28px auto;padding:20px}
    h1{font-size:20px;margin:0 0 12px}
    .grid{display:grid;grid-template-columns:1fr 420px;gap:18px}
    .card{background:rgba(255,255,255,0.03);padding:14px;border-radius:10px;box-shadow:0 6px 18px rgba(2,6,23,0.6)}
    label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
    input[type=text], textarea, select{width:100%;padding:8px 10px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:inherit}
    textarea{min-height:82px}
    .row{display:flex;gap:10px}
    .btn{display:inline-block;padding:8px 12px;border-radius:8px;border:none;background:var(--accent);color:#042026;font-weight:600;cursor:pointer}
    .muted{font-size:13px;color:var(--muted)}
    .preview{white-space:pre-wrap;background:#021223;padding:12px;border-radius:8px;overflow:auto;max-height:620px}
    .small{font-size:12px}
    .chips{display:flex;flex-wrap:wrap;gap:8px}
    .chip{background:rgba(255,255,255,0.04);padding:6px 10px;border-radius:999px;font-size:13px}
    .footer{margin-top:14px;color:var(--muted);font-size:13px}
    .actions{display:flex;gap:8px;align-items:center}
    .hint{font-size:12px;color:var(--muted);margin-top:6px}
    .examples{margin-top:8px}
    .example-btn{background:transparent;border:1px solid rgba(255,255,255,0.05);padding:6px 8px;border-radius:6px;color:var(--muted);cursor:pointer}
  </style>
</head>
<body>
  <div class="container">
    <h1>GitHub Profile README Generator — <span class="muted">Instrumentation & Control — CUSAT</span></h1>
    <div class="grid">
      <div class="card">
        <form id="form">
          <label>Full name</label>
          <input id="name" type="text" placeholder="Basil Th..." value="BASIL" />

          <label>Display title / short tagline</label>
          <input id="title" type="text" placeholder="3rd year B.Tech — Instrumentation & Control" value="3rd year B.Tech — Instrumentation & Control (CUSAT)" />

          <label>University / Institute</label>
          <input id="university" type="text" value="Cochin University of Science and Technology (CUSAT)" />

          <label>Bio (short)</label>
          <textarea id="bio">I’m Basil, a driven 3rd-year B.Tech student in Instrumentation and Control Engineering at CUSAT. With hands-on experience in MATLAB, Simulink, Altium and practical lab projects, I enjoy solving engineering challenges and building automation solutions.</textarea>

          <label>Skills (comma-separated)</label>
          <input id="skills" type="text" value="MATLAB, Simulink, Altium, Python, C++, LaTeX, Signal Processing, PID control" />

          <label>Projects (use 
 between projects; format: Name — short description)</label>
          <textarea id="projects">PID Temperature Controller — Implemented a PID controller on Arduino for temperature regulation
Smart Sensor Node — LoRa-based remote sensor with power optimization
Data Logger — Multichannel DAQ logger using LabVIEW</textarea>

          <label>Contact / Socials (GitHub username required)</label>
          <div class="row">
            <input id="github" type="text" placeholder="github-username" value="" />
            <input id="linkedin" type="text" placeholder="linkedin-URL or handle" value="" />
          </div>
          <label>Email</label>
          <input id="email" type="text" placeholder="you@example.com" value="thekkethilbasil@gmail.com" />

          <label>Phone (optional)</label>
          <input id="phone" type="text" placeholder="+91 9037606139" value="+91 9037606139" />

          <label>Profile image URL (optional)</label>
          <input id="avatar" type="text" placeholder="https://...jpg" />

          <label>Extras</label>
          <select id="extras">
            <option value="none">None</option>
            <option value="github-stats">Add GitHub Readme Stats & Top Languages</option>
            <option value="visitors">Add visitor badge</option>
            <option value="all">Add stats + visitor badge</option>
          </select>

          <div style="margin-top:10px;display:flex;gap:8px;align-items:center">
            <button id="generate" class="btn" type="button">Generate README.md</button>
            <button id="previewBtn" class="example-btn" type="button">Toggle Preview</button>
            <button id="downloadBtn" class="example-btn" type="button">Download README.md</button>
            <button id="copyBtn" class="example-btn" type="button">Copy to clipboard</button>
          </div>

          <div class="hint">Tip: fill GitHub username to enable stats and profile links. Projects should be short lines; the generator formats them into markdown.</div>

        </form>

        <div class="footer">Built for Instrumentation & Control students — edit the generated README freely before pushing to your profile repo (<code>username/username</code>).</div>
      </div>

      <div class="card">
        <div style="display:flex;gap:10px;align-items:center;justify-content:space-between">
          <div>
            <div class="muted">Live Markdown Preview</div>
            <div class="small">What will be written into your README.md</div>
          </div>
          <div class="actions">
            <button id="presetInst" class="example-btn">Load Instrumentation Preset</button>
            <button id="clear" class="example-btn">Clear</button>
          </div>
        </div>

        <hr style="border:none;border-top:1px solid rgba(255,255,255,0.04);margin:10px 0" />
        <div id="preview" class="preview"></div>
      </div>
    </div>
  </div>

  <script>
    function escapeHtml(str){return String(str||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;')}

    function buildMarkdown(data){
      const lines = [];
      // Header
      if(data.avatar) lines.push(`<p align=\"center\">
  <img src=\"${data.avatar}\" width=\"150\" alt=\"profile\" />
</p>
`);
      lines.push(`# ${data.name}`);
      if(data.title) lines.push(`**${data.title}**  `);
      if(data.university) lines.push(`${data.university}  `);
      lines.push('
');

      if(data.bio) lines.push(`${data.bio}
`);

      // Skills
      if(data.skills && data.skills.length){
        const skills = data.skills.join(', ');
        lines.push(`### 🔧 Skills
${skills}
`);
      }

      // Projects
      if(data.projects && data.projects.length){
        lines.push('### 📁 Projects');
        data.projects.forEach(p=>{ lines.push(`- **${p.split('—')[0].trim()}** — ${p.split('—').slice(1).join('—').trim()}`) });
        lines.push('
');
      }

      // Contact
      const contacts = [];
      if(data.github) contacts.push(`[![GitHub](https://img.shields.io/badge/-GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/${data.github})`);
      if(data.linkedin) contacts.push(`[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](${data.linkedin})`);
      if(data.email) contacts.push(`[![Email](https://img.shields.io/badge/-Email-D14836?style=flat-square&logo=gmail&logoColor=white)](mailto:${data.email})`);
      if(data.phone) contacts.push(`[![Phone](https://img.shields.io/badge/-Phone-34A853?style=flat-square&logo=call&logoColor=white)](tel:${data.phone})`);
      if(contacts.length) lines.push(contacts.join(' ') + '
');

      // Extras
      if(data.extras === 'github-stats' || data.extras === 'all'){
        if(data.github){
          lines.push('---
');
          lines.push(`<p align=\"left\">
  <a href=\"https://github.com/${data.github}\">
    <img src=\"https://github-readme-stats.vercel.app/api?username=${data.github}&show_icons=true&theme=radical\" alt=\"${data.github} stats\"/>
  </a>
  <img src=\"https://github-readme-stats.vercel.app/api/top-langs/?username=${data.github}&layout=compact\" alt=\"Top languages\"/>
</p>
`);
        }
      }
      if(data.extras === 'visitors' || data.extras === 'all'){
        lines.push('
');
        lines.push(`<p align=\"left\">
  <img src=\"https://komarev.com/ghpvc/?username=${data.github || 'username'}&label=Profile%20views&color=0e75b6\" alt=\"views\" />
</p>
`);
      }

      // Footer / call to action
      lines.push('---
');
      lines.push('⭐️ If you like my work, feel free to follow me on GitHub.');

      return lines.join('
');
    }

    function readForm(){
      const get = id => document.getElementById(id).value.trim();
      return {
        name: get('name') || 'Your Name',
        title: get('title'),
        university: get('university'),
        bio: get('bio'),
        skills: (get('skills')||'').split(',').map(s=>s.trim()).filter(Boolean),
        projects: (get('projects')||'').split('
').map(s=>s.trim()).filter(Boolean),
        github: get('github'),
        linkedin: get('linkedin'),
        email: get('email'),
        phone: get('phone'),
        avatar: get('avatar'),
        extras: document.getElementById('extras').value
      }
    }

    function render(){
      const md = buildMarkdown(readForm());
      document.getElementById('preview').textContent = md;
      return md;
    }

    document.getElementById('generate').addEventListener('click', ()=>{
      const md = render();
      alert('README generated in the preview panel. Use Copy or Download to save it.');
    });

    document.getElementById('previewBtn').addEventListener('click', ()=>{
      const p = document.getElementById('preview');
      p.style.display = (p.style.display==='none') ? 'block' : 'none';
    });

    document.getElementById('copyBtn').addEventListener('click', async ()=>{
      const md = render();
      try{ await navigator.clipboard.writeText(md); alert('README copied to clipboard!') }catch(e){ alert('Copy failed — your browser may block clipboard access. Use Download instead.') }
    });

    document.getElementById('downloadBtn').addEventListener('click', ()=>{
      const md = render();
      const blob = new Blob([md], {type: 'text/markdown;charset=utf-8'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'README.md';
      document.body.appendChild(a);
      a.click();
      a.remove();
      URL.revokeObjectURL(url);
    });

    document.getElementById('presetInst').addEventListener('click', ()=>{
      document.getElementById('name').value = 'Basil';
      document.getElementById('title').value = '3rd year B.Tech — Instrumentation & Control';
      document.getElementById('university').value = 'Cochin University of Science and Technology (CUSAT)';
      document.getElementById('bio').value = 'Instrumentation & Control engineering student at CUSAT. Interested in automation, process control, embedded systems, and sensors. Experienced with MATLAB, Simulink, PLCs and data acquisition.';
      document.getElementById('skills').value = 'MATLAB, Simulink, PLC (Siemens/Allen-Bradley), PID control, Sensors, Signal Processing, Embedded C, Python, LabVIEW';
      document.getElementById('projects').value = 'PID Temperature Controller — Implemented a PID controller on Arduino for setpoint tracking
Smart Sensor Node — LoRaWAN sensor node for remote monitoring
Data Acquisition System — Multichannel DAQ with LabVIEW and CSV export';
      document.getElementById('github').value = '';
      document.getElementById('linkedin').value = '';
      document.getElementById('email').value = 'thekkethilbasil@gmail.com';
      document.getElementById('phone').value = '+91 9037606139';
      document.getElementById('avatar').value = '';
      document.getElementById('extras').value = 'github-stats';
      render();
    });

    document.getElementById('clear').addEventListener('click', ()=>{
      ['name','title','university','bio','skills','projects','github','linkedin','email','avatar','phone'].forEach(id=>document.getElementById(id).value='');
      document.getElementById('extras').value='none';
      render();
    });

    // initial render
    render();
  </script>
</body>
</html>


