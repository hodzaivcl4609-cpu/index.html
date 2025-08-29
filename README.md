# Create the v2 full demo HTML implementing 11 subjects and the hạnh kiểm review workflow.
html_v2 = r"""<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Class Monitor Demo v2</title>
<style>
  :root { --bg:#0b1020; --card:#121a33; --muted:#9fb3ff; --text:#eef2ff; --accent:#5b7cff; --bad:#ff6666; --good:#66d97a; }
  *{box-sizing:border-box} body{margin:0;font-family:system-ui,-apple-system,Segoe UI,Roboto,Inter,Ubuntu,Arial,sans-serif;background:linear-gradient(180deg,#050814,#0b1020);color:var(--text);}
  .wrap{max-width:1000px;margin:24px auto;padding:16px}
  .card{background:linear-gradient(180deg,#0f1731 0%, #0a1230 100%);border:1px solid #1a2553;border-radius:14px;box-shadow:0 10px 30px rgba(0,0,0,.35);padding:14px;margin:12px 0}
  h1{font-size:28px;margin:8px 0 16px}
  h2{font-size:20px;margin:8px 0 12px}
  h3{font-size:16px;margin:8px 0}
  .muted{color:var(--muted);font-size:13px}
  input,select,textarea{width:100%;padding:10px 12px;border-radius:10px;border:1px solid #27346a;background:#0d1531;color:var(--text);outline:none}
  textarea{min-height:72px;resize:vertical}
  .row{display:flex;gap:10px;flex-wrap:wrap}
  .col{flex:1 1 220px}
  .btn{background:linear-gradient(180deg,#5b7cff,#3252e3);color:white;border:none;border-radius:10px;padding:9px 12px;cursor:pointer;font-weight:600}
  .btn:hover{filter:brightness(1.05)}
  .btn.bad{background:linear-gradient(180deg,#ff7b7b,#d83d3d)}
  .btn.good{background:linear-gradient(180deg,#66d97a,#2fa34a)}
  .btn.ghost{background:#141e40;border:1px solid #2a3d8f}
  .flex{display:flex;align-items:center;gap:8px}
  .right{margin-left:auto}
  .pill{display:inline-block;padding:2px 10px;border-radius:999px;font-size:12px;border:1px solid #2a3d8f;background:#0c1533}
  .list{list-style:none;padding:0;margin:0}
  .list li{padding:10px 8px;border-bottom:1px dashed #223071}
  .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(240px,1fr));gap:12px}
  .hidden{display:none !important}
  .kbd{border:1px solid #35469c;border-bottom-width:3px;background:#101a3b;border-radius:8px;padding:2px 6px;font-size:12px}
  .footer{margin-top:16px;font-size:12px;color:#aab3e6;opacity:.95}
  .tag{font-size:11px;padding:2px 8px;border-radius:999px;background:#132255;border:1px solid #2d3fa0;margin-left:6px}
  .table{width:100%;border-collapse:collapse}
  .table th,.table td{border-bottom:1px dashed #2b397e;padding:8px;text-align:left}
  .center{text-align:center}
</style>
</head>
<body>
<div class="wrap">
  <h1>📚 Class Monitor – Demo v2</h1>
  <div class="muted">Mô phỏng đầy đủ: 3 mục, 11 môn, GOOD/BAD, xếp hạng, và hạnh kiểm có duyệt bởi 11 GV.</div>

  <!-- Gate -->
  <div id="gate" class="card">
    <h2>🔒 Vào lớp (mật khẩu lớp)</h2>
    <div class="row">
      <div class="col"><input id="classPass" type="password" placeholder="Nhập mật khẩu lớp... (mặc định: lop10a1)"></div>
      <div style="flex:0 0 160px"><button class="btn" id="enterClass">Vào lớp</button></div>
    </div>
    <div class="muted" style="margin-top:6px">GVCN có thể đổi mật khẩu lớp trong mục GVCN.</div>
  </div>

  <!-- Portal chooser -->
  <div id="portalChooser" class="card hidden">
    <h2>📂 Chọn mục</h2>
    <div class="grid">
      <div class="card">
        <h3>🧑‍🎓 Học sinh</h3>
        <div class="muted">Tố giác nặc danh, bỏ phiếu xác minh, xem xếp hạng khi GVCN bật.</div>
        <div class="row" style="margin-top:8px">
          <input id="passStudent" type="password" placeholder="Mật khẩu HS (mặc định: student123)">
          <button class="btn" id="enterStudent">Vào</button>
        </div>
      </div>
      <div class="card">
        <h3>👩‍🏫 GV bộ môn</h3>
        <div class="muted">Mỗi giáo viên chọn môn của mình, chấm GOOD/BAD, duyệt hạnh kiểm chờ.</div>
        <div class="row" style="margin-top:8px">
          <input id="passTeacher" type="password" placeholder="Mật khẩu GV bộ môn (mặc định: teacher123)">
          <button class="btn" id="enterTeacher">Vào</button>
        </div>
      </div>
      <div class="card">
        <h3>📘 GVCN</h3>
        <div class="muted">Quản lý HS, nhận tố giác, đánh giá hạnh kiểm và gửi duyệt tới 11 GV.</div>
        <div class="row" style="margin-top:8px">
          <input id="passHomeroom" type="password" placeholder="Mật khẩu GVCN (mặc định: gvcn123)">
          <button class="btn" id="enterHomeroom">Vào</button>
        </div>
      </div>
    </div>
  </div>

  <!-- Student Portal -->
  <div id="portalStudent" class="hidden">
    <div class="card">
      <div class="flex"><h2>🧑‍🎓 Mục Học sinh</h2><button class="btn ghost right" onclick="logoutPortal()">Đổi mục</button></div>
      <div class="row">
        <div class="col">
          <h3>📣 Tố giác nặc danh</h3>
          <input id="reportTarget" placeholder="Tên học sinh vi phạm">
          <textarea id="reportText" placeholder="Mô tả hành vi vi phạm"></textarea>
          <button class="btn" onclick="submitReport()">Gửi tố giác</button>
          <div class="muted" style="margin-top:6px">≥2 tố giác trùng → tự động +1 BAD và thông báo GVCN.</div>
        </div>
        <div class="col">
          <h3>🗳️ Bỏ phiếu xác minh</h3>
          <div id="voteBox" class="card hidden">
            <div id="voteText" style="margin-bottom:8px"></div>
            <div class="row">
              <button class="btn good" onclick="castVote('yes')">Đúng</button>
              <button class="btn bad" onclick="castVote('no')">Sai</button>
            </div>
          </div>
          <div id="noVote" class="muted">Hiện chưa có phiên bỏ phiếu nào mở.</div>
        </div>
      </div>
    </div>
    <div class="card">
      <h3>🏆 Bảng xếp hạng tuần</h3>
      <div id="rankingHidden" class="muted">GVCN chưa bật công khai bảng xếp hạng.</div>
      <div id="rankingVisible" class="hidden">
        <table class="table" id="rankTable"></table>
      </div>
    </div>
  </div>

  <!-- Teacher Portal (shared) -->
  <div id="portalTeacher" class="hidden">
    <div class="card">
      <div class="flex"><h2>👩‍🏫 Mục GV bộ môn</h2><button class="btn ghost right" onclick="logoutPortal()">Đổi mục</button></div>
      <div class="row">
        <div class="col">
          <h3>🔎 Chọn môn của bạn</h3>
          <select id="teacherSubject"></select>
          <div id="teacherPending" style="margin-top:10px"></div>
          <h3 style="margin-top:12px">🎯 Đánh giá học sinh</h3>
          <div id="studentsList"></div>
        </div>
        <div class="col">
          <h3>📊 Xếp hạng theo môn (tuần)</h3>
          <table class="table" id="subjectRank"></table>
          <div class="muted">3 GOOD sẽ xóa 1 BAD (nếu có).</div>
        </div>
      </div>
    </div>
  </div>

  <!-- Homeroom Portal -->
  <div id="portalHomeroom" class="hidden">
    <div class="card">
      <div class="flex"><h2>📘 Mục GVCN</h2><button class="btn ghost right" onclick="logoutPortal()">Đổi mục</button></div>
      <div class="grid">
        <div class="card">
          <h3>👥 Quản lý học sinh</h3>
          <div class="row">
            <input id="newStudent" placeholder="Tên học sinh mới...">
            <button class="btn" onclick="addStudent()">Thêm</button>
          </div>
          <ul id="studentList" class="list"></ul>
        </div>
        <div class="card">
          <h3>🔑 Mật khẩu</h3>
          <div class="muted">Đổi mật khẩu cho lớp và 3 mục</div>
          <div class="row" style="margin-top:8px">
            <div class="col"><input id="pwClass" placeholder="Mật khẩu lớp"></div>
            <div class="col"><input id="pwStudent" placeholder="Mật khẩu HS"></div>
          </div>
          <div class="row" style="margin-top:8px">
            <div class="col"><input id="pwTeacher" placeholder="Mật khẩu GV bộ môn"></div>
            <div class="col"><input id="pwHomeroom" placeholder="Mật khẩu GVCN"></div>
          </div>
          <button class="btn" style="margin-top:8px" onclick="savePasswords()">Lưu mật khẩu</button>
        </div>
        <div class="card">
          <h3>📣 Tố giác nặc danh (đến ngay GVCN)</h3>
          <ul id="reportsList" class="list"></ul>
        </div>
        <div class="card">
          <h3>🗳️ Đánh giá & duyệt hạnh kiểm</h3>
          <div class="muted">Chọn học sinh → chọn mức hạnh kiểm → gửi duyệt đến 11 GV.</div>
          <div class="row" style="margin-top:8px">
            <select id="hkStudentSel"></select>
            <select id="hkLevelSel">
              <option value="Tốt">Tốt</option>
              <option value="Khá">Khá</option>
              <option value="Trung bình">Trung bình</option>
              <option value="Kém">Kém</option>
            </select>
            <button class="btn" onclick="submitHanhKiem()">Gửi đánh giá</button>
          </div>
          <div style="margin-top:8px" class="muted">Danh sách đánh giá chờ duyệt:</div>
          <ul id="pendingHK" class="list"></ul>
          <div style="margin-top:8px" class="muted">Cơ chế: nếu **ít nhất 1 GV** Không đồng ý → hạ 1 mức và kết thúc; nếu **11 GV** đồng ý → giữ.</div>
        </div>
        <div class="card">
          <h3>📊 Bảng xếp hạng tuần (GVCN xem)</h3>
          <table class="table" id="rankTableGvcn"></table>
        </div>
      </div>
    </div>
  </div>

  <div class="footer">Demo v2 • Dữ liệu lưu cục bộ (localStorage)</div>
</div>

<script>
// ---------- State + subjects ----------
const LS_KEY = "classMonitorDemo_v2";
const SUBJECTS = ["Toán","Văn","Sử","Địa","Công nghệ","Tiếng Anh","Giáo dục kinh tế & pháp luật","Giáo dục thể chất","Giáo dục địa phương","Tin học","Giáo dục quốc phòng và an ninh"];
function baseState(){
  return {
    passwords:{ class:"lop10a1", student:"student123", teacher:"teacher123", homeroom:"gvcn123" },
    students:[
      {name:"Nguyen Van A", good:1, bad:0, subjects:{}},
      {name:"Tran Thi B", good:0, bad:2, subjects:{}},
      {name:"Le Van C", good:2, bad:1, subjects:{}}
    ],
    reports:[],
    notifications:[],
    publicRanking:false,
    vote:{ active:false, reportIndex:null, yes:0, no:0 },
    // hạnh kiểm pending list: array of {id, studentIndex, proposedLevel, approvals: {subject: null/true/false}, resolved:false, finalLevel:null}
    hanhkiem: []
  };
}
function loadState(){ return Object.assign(baseState(), JSON.parse(localStorage.getItem(LS_KEY) || "{}")); }
function saveState(){ localStorage.setItem(LS_KEY, JSON.stringify(state)); }
let state = loadState();

function $(id){ return document.getElementById(id); }
function show(id, on=true){ $(id).classList.toggle("hidden", !on); }

// ---------- Gate & portal ----------
$("enterClass").onclick = () => {
  const val = $("classPass").value.trim();
  if(val === state.passwords.class){ show("gate", false); show("portalChooser", true); }
  else alert("Sai mật khẩu lớp!");
};
$("enterStudent").onclick = () => {
  const val = $("passStudent").value.trim();
  if(val === state.passwords.student){ enterPortal("student"); }
  else alert("Sai mật khẩu học sinh!");
};
$("enterTeacher").onclick = () => {
  const val = $("passTeacher").value.trim();
  if(val === state.passwords.teacher){ enterPortal("teacher"); }
  else alert("Sai mật khẩu GV bộ môn!");
};
$("enterHomeroom").onclick = () => {
  const val = $("passHomeroom").value.trim();
  if(val === state.passwords.homeroom){ enterPortal("homeroom"); }
  else alert("Sai mật khẩu GVCN!");
};
function enterPortal(role){
  show("portalChooser", false);
  show("portalStudent", role==="student");
  show("portalTeacher", role==="teacher");
  show("portalHomeroom", role==="homeroom");
  if(role==="student"){ refreshVoteBox(); renderRankingStudent(); }
  if(role==="teacher"){ initTeacher(); }
  if(role==="homeroom"){ renderHomeroom(); }
}
function logoutPortal(){
  show("portalStudent", false);
  show("portalTeacher", false);
  show("portalHomeroom", false);
  show("portalChooser", true);
}

// ---------- Reports ----------
function submitReport(){
  const target = $("reportTarget").value.trim();
  const text = $("reportText").value.trim();
  if(!target || !text){ alert("Nhập đầy đủ người và hành vi."); return; }
  state.reports.push({target,text});
  const same = state.reports.filter(r=> r.target===target && r.text===text).length;
  if(same>=2){
    const stu = state.students.find(s=>s.name===target);
    if(stu){ stu.bad +=1; }
    state.notifications.push(`Tự động cộng 1 BAD cho ${target}`);
    alert(`❗ ${target} nhận 1 BAD (≥2 tố giác trùng). GVCN được thông báo.`);
  } else {
    state.notifications.push(`Nhận tố giác về ${target}`);
  }
  saveState();
  $("reportTarget").value=""; $("reportText").value="";
  if(!$("portalHomeroom").classList.contains("hidden")) renderHomeroom();
}

// ---------- Voting ----------
function refreshVoteBox(){
  if(state.vote.active && state.vote.reportIndex!=null){
    const rep = state.reports[state.vote.reportIndex];
    if(rep){ $("voteText").innerText = `Xác minh: ${rep.target} – ${rep.text}`; show("voteBox", true); show("noVote", false); return; }
  }
  show("voteBox", false); show("noVote", true);
}
function castVote(v){
  if(!state.vote.active){ alert("Không có phiên bỏ phiếu."); return; }
  if(v==="yes") state.vote.yes++; else state.vote.no++;
  saveState();
  alert("Đã ghi nhận lá phiếu.");
}

// ---------- Teacher portal (select subject, view pending HK, approve/disapprove) ----------
function initTeacher(){
  const sel = $("teacherSubject"); sel.innerHTML="";
  SUBJECTS.forEach(s=>{ const o=document.createElement("option"); o.value=s; o.textContent=s; sel.appendChild(o); });
  sel.onchange = renderTeacherPending;
  sel.value = SUBJECTS[0];
  renderTeacherPending();
  renderTeacherStudents();
  renderSubjectRanking();
}
function renderTeacherPending(){
  const box = $("teacherPending"); box.innerHTML = "<h4>📥 Hạnh kiểm chờ duyệt (dành cho môn bạn)</h4>";
  const subj = $("teacherSubject").value;
  const pend = state.hanhkiem.filter(h=>!h.resolved);
  if(pend.length===0){ box.innerHTML += "<div class='muted'>Không có hạnh kiểm chờ.</div>"; return; }
  pend.forEach(h=>{
    const li = document.createElement("div"); li.className="card";
    const stud = state.students[h.studentIndex];
    const approvals = Object.keys(h.approvals).map(k=>`${k}: ${h.approvals[k]===null?'?':h.approvals[k]?'Đồng ý':'Không'}`).join(" | ");
    li.innerHTML = `<div><b>${stud.name}</b> → đề xuất: ${h.proposedLevel} <div class='muted'>${approvals}</div></div>`;
    const agree = document.createElement("button"); agree.className="btn good"; agree.textContent="Đồng ý"; agree.onclick = ()=>teacherVote(h.id, subj, true);
    const disagree = document.createElement("button"); disagree.className="btn bad"; disagree.textContent="Không đồng ý"; disagree.onclick = ()=>teacherVote(h.id, subj, false);
    const actions = document.createElement("div"); actions.className="row"; actions.appendChild(agree); actions.appendChild(disagree);
    li.appendChild(actions);
    box.appendChild(li);
  });
}
function teacherVote(hid, subj, val){
  const h = state.hanhkiem.find(x=>x.id===hid);
  if(!h || h.resolved){ alert("Không tìm thấy hoặc đã xử lý."); return; }
  if(h.approvals[subj]!==null){ alert("Bạn đã phản hồi cho môn này."); return; }
  h.approvals[subj] = val;
  saveState();
  // if any disagree -> finalize downgrade immediately
  if(val===false){
    finalizeHanhKiemReject(h);
  } else {
    // check if all approved
    const all = Object.values(h.approvals).every(v=>v===true);
    if(all) finalizeHanhKiemAccept(h);
  }
  renderTeacherPending();
  renderHomeroom();
  renderTeacherStudents();
}
function finalizeHanhKiemAccept(h){
  h.resolved = true; h.finalLevel = h.proposedLevel;
  state.notifications.push(`Hạnh kiểm cho ${state.students[h.studentIndex].name} được duyệt giữ: ${h.finalLevel}`);
  saveState();
}
function finalizeHanhKiemReject(h){
  // downgrade one level and finalize
  const order = ["Kém","Trung bình","Khá","Tốt"]; // ascending
  const idx = order.indexOf(h.proposedLevel);
  const newIdx = Math.max(0, idx-1);
  h.resolved = true; h.finalLevel = order[newIdx];
  // write final hạnh kiểm somewhere (we store in student.hk)
  const stu = state.students[h.studentIndex];
  stu.hk = h.finalLevel;
  state.notifications.push(`Hạnh kiểm cho ${stu.name} bị hạ: ${h.proposedLevel} → ${h.finalLevel}`);
  saveState();
  alert(`Hạnh kiểm bị hạ: ${stu.name} → ${h.finalLevel}`);
}

// ---------- Teacher: evaluate GOOD/BAD per student ----------
function renderTeacherStudents(){
  const list = $("studentsList"); list.innerHTML = "";
  state.students.forEach((s,idx)=>{
    const row = document.createElement("div"); row.className="card";
    row.innerHTML = `
      <div class="flex">
        <div><b>${s.name}</b> <span class="tag">GOOD: ${s.good}</span> <span class="tag">BAD: ${s.bad}</span> ${s.hk?'<span class=\"tag\">HK:'+s.hk+'</span>':''}</div>
        <div class="right flex">
          <button class="btn good" data-i="${idx}" data-type="good">GOOD</button>
          <button class="btn bad" data-i="${idx}" data-type="bad">BAD</button>
        </div>
      </div>
    `;
    list.appendChild(row);
  });
  list.querySelectorAll("button").forEach(btn=>{
    btn.onclick = ()=>{
      const i = +btn.dataset.i, type = btn.dataset.type;
      const subj = $("teacherSubject").value;
      const stu = state.students[i];
      if(type==="good"){
        stu.good += 1;
        if(stu.good>=3 && stu.bad>0){ stu.good -=3; stu.bad -=1; }
        stu.subjects[subj] = stu.subjects[subj] || {good:0,bad:0};
        stu.subjects[subj].good +=1;
        if(stu.subjects[subj].good>=3 && stu.subjects[subj].bad>0){ stu.subjects[subj].good-=3; stu.subjects[subj].bad-=1; }
      } else {
        stu.bad +=1;
        stu.subjects[subj] = stu.subjects[subj] || {good:0,bad:0};
        stu.subjects[subj].bad +=1;
      }
      state.notifications.push(`GV ${subj} đánh giá ${stu.name}: ${type.toUpperCase()}`);
      saveState();
      renderTeacherStudents();
      renderSubjectRanking();
      renderHomeroom();
    };
  });
  renderSubjectRanking();
}
function renderSubjectRanking(){
  const subj = $("teacherSubject").value;
  const rows=[["Học sinh","GOOD","BAD","Hạng"]];
  state.students.forEach(s=>{
    const ps = s.subjects[subj]||{good:0,bad:0};
    rows.push([s.name, ps.good, ps.bad, rankOf(ps.good,ps.bad)]);
  });
  renderTable($("subjectRank"), rows);
}

// ---------- Homeroom functions ----------
function renderHomeroom(){
  // students list
  const ul = $("studentList"); ul.innerHTML="";
  state.students.forEach((s,i)=>{
    const li = document.createElement("li");
    li.innerHTML = `<div class="flex"><span><b>${s.name}</b> <span class="tag">GOOD:${s.good}</span> <span class="tag">BAD:${s.bad}</span> ${s.hk?'<span class=\"tag\">HK:'+s.hk+'</span>':''}</span><button class="btn ghost right" data-i="${i}">Xóa</button></div>`;
    ul.appendChild(li);
  });
  ul.querySelectorAll("button").forEach(btn=>{ btn.onclick = ()=>{ const i=+btn.dataset.i; state.students.splice(i,1); saveState(); renderHomeroom(); }; });

  // passwords
  $("pwClass").value = state.passwords.class;
  $("pwStudent").value = state.passwords.student;
  $("pwTeacher").value = state.passwords.teacher;
  $("pwHomeroom").value = state.passwords.homeroom;

  // reports list
  const rl = $("reportsList"); rl.innerHTML="";
  state.reports.forEach((r,i)=>{ const li=document.createElement("li"); li.textContent = `${i+1}. ${r.target} – ${r.text}`; rl.appendChild(li); });

  // pending hanhkiem list
  const ph = $("pendingHK"); ph.innerHTML = "";
  state.hanhkiem.forEach(h=>{
    const stu = state.students[h.studentIndex];
    const li = document.createElement("li");
    const approvals = Object.entries(h.approvals).map(([k,v])=>`${k}:${v===null?'?':v?'Đ':'K'}`).join(" | ");
    li.innerHTML = `<div><b>${stu.name}</b> đề xuất: ${h.proposedLevel} → Trạng thái: ${h.resolved?'Đã xử lý':'Chờ' } <div class="muted">${approvals}</div></div>`;
    ph.appendChild(li);
  });

  // vote select
  const vs = $("hkStudentSel"); vs.innerHTML = "";
  state.students.forEach((s,i)=>{ const o=document.createElement("option"); o.value=i; o.textContent=`${s.name}`; vs.appendChild(o); });

  // ranking table
  renderRankingGvcn();
}
function addStudent(){ const name=$("newStudent").value.trim(); if(!name) return; state.students.push({name,good:0,bad:0,subjects:{}}); $("newStudent").value=""; saveState(); renderHomeroom(); }
function savePasswords(){ state.passwords.class=$("pwClass").value.trim()||state.passwords.class; state.passwords.student=$("pwStudent").value.trim()||state.passwords.student; state.passwords.teacher=$("pwTeacher").value.trim()||state.passwords.teacher; state.passwords.homeroom=$("pwHomeroom").value.trim()||state.passwords.homeroom; saveState(); alert("Đã lưu mật khẩu."); }

function submitHanhKiem(){
  const idx = +$("hkStudentSel").value;
  const level = $("hkLevelSel").value;
  if(isNaN(idx)){ alert("Chọn học sinh."); return; }
  // create pending hanhkiem item
  const id = Date.now() + Math.floor(Math.random()*1000);
  const approvals = {}; SUBJECTS.forEach(s=> approvals[s]=null);
  const item = {id, studentIndex:idx, proposedLevel:level, approvals, resolved:false, finalLevel:null};
  state.hanhkiem.push(item);
  saveState();
  alert("Đã gửi đánh giá hạnh kiểm tới 11 GV để duyệt.");
  renderHomeroom();
}

// ---------- finalize and ranking ----------
function rankOf(good,bad){ const score = good-bad; if(score>=3) return "A"; if(score>=1) return "B"; if(score===0) return "C"; if(score===-1) return "D"; if(score===-2) return "E"; return "F"; }
function renderTable(el, rows){ el.innerHTML=""; rows.forEach((r,ri)=>{ const tr=document.createElement("tr"); r.forEach((c,ci)=>{ const td=document.createElement(ri===0?"th":"td"); td.textContent=c; tr.appendChild(td); }); el.appendChild(tr); }); }
function renderRankingGvcn(){ const rows=[["Học sinh","GOOD","BAD","Hạng","Hạnh kiểm"]]; state.students.forEach(s=> rows.push([s.name,s.good,s.bad,rankOf(s.good,s.bad), s.hk||"Chưa"])); renderTable($("rankTableGvcn"), rows); renderRankingStudent(); }
function renderRankingStudent(){ if(!state.publicRanking){ show("rankingHidden", true); show("rankingVisible", false); return; } show("rankingHidden", false); show("rankingVisible", true); const rows=[["Học sinh","GOOD","BAD","Hạng","Hạnh kiểm"]]; state.students.forEach(s=> rows.push([s.name,s.good,s.bad,rankOf(s.good,s.bad), s.hk||"Chưa"])); renderTable($("rankTable"), rows); }

// ---------- Utilities ----------
function renderSubjectRanking(){ const subj = $("teacherSubject").value; const rows=[["Học sinh","GOOD","BAD","Hạng"]]; state.students.forEach(s=>{ const ps = s.subjects[subj]||{good:0,bad:0}; rows.push([s.name,ps.good,ps.bad,rankOf(ps.good,ps.bad)]); }); renderTable($("subjectRank"), rows); }

// Init UI state
renderRankingStudent();

// Expose some functions to window for buttons
window.logoutPortal = logoutPortal;
window.submitReport = submitReport;
window.castVote = castVote;
window.initTeacher = initTeacher;
window.renderTeacherStudents = renderTeacherStudents;
window.addStudent = addStudent;
window.savePasswords = savePasswords;
window.submitHanhKiem = submitHanhKiem;

</script>
</body>
</html>
"""

# Save to file
path = "/mnt/data/class_monitor_demo_v2.html"
with open(path, "w", encoding="utf-8") as f:
    f.write(html_v2)
path
