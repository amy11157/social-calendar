<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>社群排程行事曆</title>
<style>
*{box-sizing:border-box;margin:0;padding:0;}
body{min-height:100vh;background:linear-gradient(135deg,#0f0c29 0%,#302b63 50%,#24243e 100%);font-family:'Noto Sans TC','PingFang TC','Microsoft JhengHei',sans-serif;color:#fff;padding:28px 16px;}
.wrap{max-width:1100px;margin:0 auto;}
h1{font-size:25px;font-weight:800;letter-spacing:-1px;background:linear-gradient(90deg,#fff,#c4b5fd);-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.subtitle{font-size:10px;letter-spacing:4px;color:#a78bfa;text-transform:uppercase;margin-bottom:5px;}
.header{display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:22px;flex-wrap:wrap;gap:14px;}
.stats{display:flex;align-items:center;gap:10px;flex-wrap:wrap;}
.stat-box{background:rgba(255,255,255,0.07);border-radius:11px;padding:7px 15px;text-align:center;border:1px solid rgba(255,255,255,0.09);}
.stat-num{font-size:19px;font-weight:800;}
.stat-label{font-size:9px;color:rgba(255,255,255,0.38);margin-top:1px;}
.sync{font-size:11px;min-width:130px;text-align:right;transition:color 0.3s;}
.members-row{display:flex;align-items:center;gap:8px;margin-bottom:16px;flex-wrap:wrap;}
.mlabel{font-size:10px;color:rgba(255,255,255,0.35);letter-spacing:1px;}
.mchip{display:flex;align-items:center;gap:5px;background:rgba(255,255,255,0.06);border-radius:8px;padding:4px 9px;border:1px solid rgba(255,255,255,0.09);font-size:11px;color:rgba(255,255,255,0.7);}
.av{border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:700;color:#fff;border:2px solid rgba(15,12,41,0.5);flex-shrink:0;}
.controls{display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;flex-wrap:wrap;gap:9px;}
.nav{display:flex;align-items:center;gap:9px;}
.navbtn{background:rgba(255,255,255,0.08);border:1px solid rgba(255,255,255,0.11);color:#fff;width:31px;height:31px;border-radius:7px;font-size:16px;cursor:pointer;font-family:inherit;}
.mlbl{font-size:16px;font-weight:700;min-width:108px;text-align:center;}
.filters{display:flex;gap:5px;flex-wrap:wrap;align-items:center;}
.chip{padding:6px 12px;border-radius:7px;font-size:11px;font-weight:600;cursor:pointer;color:#fff;border:1px solid rgba(255,255,255,0.11);background:rgba(255,255,255,0.07);font-family:inherit;}
.chip.aig{background:#E1306C;border:none;}
.chip.afb{background:#1877F2;border:none;}
.chip.aall{background:#7c3aed;border:none;}
.addchip{background:linear-gradient(90deg,#7c3aed,#a21caf);border:none;padding:6px 14px;}
select.chip{background:rgba(255,255,255,0.07);}
.calendar{background:rgba(255,255,255,0.04);border-radius:16px;overflow:hidden;border:1px solid rgba(255,255,255,0.07);margin-bottom:18px;}
.calhdr{display:grid;grid-template-columns:repeat(7,1fr);border-bottom:1px solid rgba(255,255,255,0.07);}
.dow{text-align:center;padding:10px 0;font-size:10px;font-weight:700;color:rgba(255,255,255,0.3);letter-spacing:2px;}
.dow.wk{color:#f472b6;}
.calgrid{display:grid;grid-template-columns:repeat(7,1fr);}
.cell{min-height:96px;padding:6px 5px;border-right:1px solid rgba(255,255,255,0.04);border-bottom:1px solid rgba(255,255,255,0.04);cursor:pointer;transition:background 0.12s;}
.cell:hover{background:rgba(167,139,250,0.07);}
.cell.sel{background:rgba(167,139,250,0.12);}
.cell.tod{background:rgba(167,139,250,0.06);}
.cell.emp{cursor:default;}
.dnum{width:22px;height:22px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:11px;color:rgba(255,255,255,0.6);margin-bottom:3px;}
.dnum.tn{background:#a78bfa;color:#fff;font-weight:800;}
.dnum.sn{color:#c4b5fd;}
.pill{border-radius:4px;padding:2px 4px;font-size:9px;font-weight:600;display:flex;align-items:center;gap:3px;cursor:pointer;overflow:hidden;white-space:nowrap;margin-bottom:2px;}
.pill.dr{border:1px dashed rgba(255,255,255,0.3);}
.ptitle{flex:1;overflow:hidden;text-overflow:ellipsis;}
.more{font-size:9px;color:rgba(255,255,255,0.3);padding-left:2px;}
.detail{background:rgba(255,255,255,0.04);border-radius:14px;padding:16px;border:1px solid rgba(167,139,250,0.2);}
.dtitle{font-size:11px;font-weight:700;color:#c4b5fd;margin-bottom:12px;letter-spacing:1px;}
.pcards{display:flex;flex-wrap:wrap;gap:10px;}
.pcard{background:rgba(255,255,255,0.05);border-radius:12px;padding:11px 13px;cursor:pointer;min-width:185px;max-width:235px;flex:1 0 185px;transition:transform 0.12s;}
.pcard:hover{transform:translateY(-2px);}
.ctop{display:flex;justify-content:space-between;margin-bottom:7px;}
.pbadge{border-radius:5px;padding:2px 7px;font-size:10px;font-weight:700;}
.sbadge{border-radius:5px;padding:2px 7px;font-size:10px;font-weight:700;}
.ctitle{font-weight:700;font-size:13px;margin-bottom:3px;}
.ccap{font-size:11px;color:rgba(255,255,255,0.38);margin-bottom:8px;}
.cfoot{display:flex;justify-content:space-between;align-items:center;}
.ctime{font-size:10px;color:#a78bfa;}
.avgrp{display:flex;}
.overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,0.72);backdrop-filter:blur(6px);align-items:center;justify-content:center;z-index:100;padding:16px;}
.overlay.show{display:flex;}
.modal{background:linear-gradient(135deg,#1e1b4b,#312e81);border:1px solid rgba(167,139,250,0.28);border-radius:20px;padding:24px;width:100%;max-width:450px;box-shadow:0 24px 80px rgba(0,0,0,0.65);max-height:90vh;overflow-y:auto;}
.mhdr{display:flex;justify-content:space-between;align-items:center;margin-bottom:16px;}
.mtitle{font-size:15px;font-weight:800;}
.xbtn{background:none;border:none;color:rgba(255,255,255,0.38);font-size:19px;cursor:pointer;}
.fcol{display:flex;flex-direction:column;gap:9px;}
.fgrid{display:grid;grid-template-columns:1fr 1fr;gap:7px;}
.inp{background:rgba(255,255,255,0.07);border:1px solid rgba(255,255,255,0.11);color:#fff;border-radius:9px;padding:8px 10px;font-size:12px;outline:none;width:100%;font-family:inherit;}
textarea.inp{resize:vertical;min-height:60px;}
.slbl{font-size:9px;color:rgba(255,255,255,0.3);margin-bottom:7px;letter-spacing:1px;text-transform:uppercase;}
.alist{display:flex;flex-wrap:wrap;gap:6px;}
.achip{display:flex;align-items:center;gap:6px;padding:5px 10px;border-radius:8px;cursor:pointer;transition:all 0.12s;}
.tagrow{display:flex;gap:6px;}
.tagbtn{background:rgba(167,139,250,0.16);border:1px solid rgba(255,255,255,0.11);color:#fff;border-radius:9px;padding:8px;font-size:12px;cursor:pointer;font-weight:800;width:36px;font-family:inherit;}
.tagslist{display:flex;flex-wrap:wrap;gap:5px;margin-top:6px;}
.titem{background:rgba(167,139,250,0.16);color:#c4b5fd;border-radius:6px;padding:2px 9px;font-size:10px;cursor:pointer;}
.savebtn{padding:11px;border-radius:9px;background:linear-gradient(90deg,#7c3aed,#a21caf);border:none;color:#fff;font-size:13px;font-weight:800;cursor:pointer;width:100%;font-family:inherit;}
.vbadges{display:flex;gap:6px;}
.vbadge{border-radius:7px;padding:3px 10px;font-size:11px;font-weight:700;}
.vtitle{font-size:18px;font-weight:800;margin-bottom:5px;}
.vcap{font-size:12px;color:rgba(255,255,255,0.48);margin-bottom:14px;line-height:1.7;}
.igrid{display:grid;grid-template-columns:1fr 1fr;gap:7px;margin-bottom:12px;}
.ibox{background:rgba(255,255,255,0.06);border-radius:8px;padding:8px 10px;}
.ilbl{font-size:9px;color:rgba(255,255,255,0.28);margin-bottom:3px;}
.ival{font-size:11px;font-weight:600;}
.asec{background:rgba(255,255,255,0.05);border-radius:10px;padding:10px 12px;margin-bottom:10px;}
.aslbl{font-size:9px;color:rgba(255,255,255,0.28);margin-bottom:8px;letter-spacing:1px;}
.acards{display:flex;flex-wrap:wrap;gap:7px;}
.acard{display:flex;align-items:center;gap:7px;border-radius:8px;padding:5px 10px;}
.vtags{display:flex;flex-wrap:wrap;gap:5px;margin-bottom:12px;}
.vtag{background:rgba(167,139,250,0.16);color:#c4b5fd;border-radius:6px;padding:2px 9px;font-size:10px;}
.sacts{display:flex;gap:5px;margin-bottom:9px;}
.sact{flex:1;padding:7px;border-radius:8px;border:none;font-size:11px;font-weight:700;cursor:pointer;font-family:inherit;}
.delbtn{width:100%;padding:9px;border-radius:8px;background:rgba(239,68,68,0.11);border:1px solid rgba(239,68,68,0.22);color:#fca5a5;font-size:12px;cursor:pointer;font-weight:600;font-family:inherit;}
.empty{color:rgba(255,255,255,0.22);font-size:12px;}
.tabs{display:flex;gap:3px;margin-bottom:18px;background:rgba(255,255,255,0.04);border-radius:11px;padding:3px;width:fit-content;}
.tab{padding:7px 16px;border-radius:8px;border:none;cursor:pointer;font-size:12px;font-weight:700;background:transparent;color:rgba(255,255,255,0.38);transition:all 0.13s;font-family:inherit;}
.tab.on{background:rgba(167,139,250,0.22);color:#c4b5fd;}
.mpanel{background:rgba(255,255,255,0.04);border-radius:16px;border:1px solid rgba(255,255,255,0.07);padding:22px;}
.mphdr{display:flex;justify-content:space-between;align-items:center;margin-bottom:18px;}
.mptitle{font-size:13px;font-weight:700;color:#c4b5fd;}
.mcards{display:flex;flex-wrap:wrap;gap:9px;}
.mcard{background:rgba(255,255,255,0.05);border-radius:12px;padding:13px 15px;display:flex;align-items:center;gap:11px;min-width:175px;}
.mname{font-weight:700;font-size:13px;}
.mrole{font-size:10px;color:rgba(255,255,255,0.38);margin-top:2px;}
.rmbtn{background:none;border:none;color:rgba(255,255,255,0.2);font-size:15px;cursor:pointer;padding:3px;}
.addbx{background:rgba(167,139,250,0.09);border:1px solid rgba(167,139,250,0.22);border-radius:11px;padding:14px;margin-bottom:14px;display:flex;gap:7px;flex-wrap:wrap;align-items:center;}
</style>
</head>
<body>
<div class="wrap">
  <div class="header">
    <div>
      <div class="subtitle">Social Media</div>
      <h1>內容排程行事曆</h1>
    </div>
    <div class="stats">
      <div class="sync" id="sync" style="color:transparent">　</div>
      <div class="stat-box"><div class="stat-num" id="s-total" style="color:#a78bfa">0</div><div class="stat-label">本月貼文</div></div>
      <div class="stat-box"><div class="stat-num" id="s-sch" style="color:#FCD34D">0</div><div class="stat-label">已排程</div></div>
      <div class="stat-box"><div class="stat-num" id="s-dr" style="color:#6EE7B7">0</div><div class="stat-label">草稿</div></div>
    </div>
  </div>

  <div class="tabs">
    <button class="tab on" onclick="setPanel('cal')">📅 行事曆</button>
    <button class="tab" id="tab-mem" onclick="setPanel('mem')">👥 成員管理</button>
  </div>

  <div id="panel-cal">
    <div class="members-row"><span class="mlabel">團隊成員</span><div id="mlist" style="display:flex;gap:8px;flex-wrap:wrap"></div></div>
    <div class="controls">
      <div class="nav">
        <button class="navbtn" onclick="pm()">&#8249;</button>
        <div class="mlbl" id="mlbl"></div>
        <button class="navbtn" onclick="nm()">&#8250;</button>
      </div>
      <div class="filters">
        <button class="chip aall" id="f-all" onclick="sf('ALL')">全部</button>
        <button class="chip" id="f-ig" onclick="sf('IG')">📸 IG</button>
        <button class="chip" id="f-fb" onclick="sf('FB')">👍 FB</button>
        <select class="chip" id="f-mem" onchange="sm(this.value)"><option value="ALL">👤 所有成員</option></select>
        <button class="chip addchip" onclick="openAdd()">＋ 新增貼文</button>
      </div>
    </div>
    <div class="calendar">
      <div class="calhdr">
        <div class="dow wk">日</div><div class="dow">一</div><div class="dow">二</div>
        <div class="dow">三</div><div class="dow">四</div><div class="dow">五</div>
        <div class="dow wk">六</div>
      </div>
      <div class="calgrid" id="calgrid"></div>
    </div>
    <div id="detpanel" class="detail" style="display:none">
      <div class="dtitle" id="dttl"></div>
      <div class="pcards" id="dcards"></div>
    </div>
  </div>

  <div id="panel-mem" style="display:none">
    <div class="mpanel">
      <div class="mphdr">
        <div class="mptitle" id="mptitle">團隊成員</div>
        <button class="chip addchip" onclick="toggleAddMem()">＋ 新增成員</button>
      </div>
      <div id="addmembox" class="addbx" style="display:none">
        <input class="inp" id="mn" placeholder="姓名 *" style="width:110px;flex:1 0 90px">
        <input class="inp" id="mr" placeholder="職稱（選填）" style="width:135px;flex:1 0 110px">
        <button class="chip" style="background:#7c3aed;border:none" onclick="addMem()">新增</button>
        <button class="chip" onclick="toggleAddMem()">取消</button>
      </div>
      <div class="mcards" id="mcards"></div>
    </div>
  </div>
</div>

<!-- Add Modal -->
<div class="overlay" id="addmodal" onclick="closeAdd(event)">
  <div class="modal" onclick="event.stopPropagation()">
    <div class="mhdr"><div class="mtitle">＋ 新增排程貼文</div><button class="xbtn" onclick="closeAdd()">✕</button></div>
    <div class="fcol">
      <input class="inp" id="ft" placeholder="貼文標題 *">
      <textarea class="inp" id="fc" placeholder="文案內容"></textarea>
      <div class="fgrid">
        <select class="inp" id="fp"><option value="IG">📸 Instagram</option><option value="FB">👍 Facebook</option></select>
        <select class="inp" id="fty"><option value="photo">🖼️ 貼文</option><option value="video">🎬 影片</option><option value="reel">✨ Reels</option><option value="story">⏱️ 限時動態</option></select>
        <input class="inp" type="date" id="fd">
        <input class="inp" type="time" id="ftm" value="12:00">
      </div>
      <select class="inp" id="fs"><option value="draft">草稿</option><option value="scheduled">已排程</option><option value="published">已發佈</option></select>
      <div><div class="slbl">指派負責人</div><div class="alist" id="falist"></div></div>
      <div>
        <div class="slbl">標籤</div>
        <div class="tagrow"><input class="inp" id="fti" placeholder="新增標籤" onkeydown="if(event.key==='Enter')at()"><button class="tagbtn" onclick="at()">#</button></div>
        <div class="tagslist" id="ftl"></div>
      </div>
      <button class="savebtn" onclick="submitPost()">儲存貼文</button>
    </div>
  </div>
</div>

<!-- View Modal -->
<div class="overlay" id="viewmodal" onclick="closeView(event)">
  <div class="modal" onclick="event.stopPropagation()">
    <div class="mhdr"><div class="vbadges" id="vb"></div><button class="xbtn" onclick="closeView()">✕</button></div>
    <div class="vtitle" id="vt"></div>
    <div class="vcap" id="vc"></div>
    <div class="igrid">
      <div class="ibox"><div class="ilbl">類型</div><div class="ival" id="vty"></div></div>
      <div class="ibox"><div class="ilbl">發佈時間</div><div class="ival" id="vtm"></div></div>
    </div>
    <div class="asec" id="vasec"><div class="aslbl">負責人</div><div class="acards" id="vac"></div></div>
    <div class="vtags" id="vtgs"></div>
    <div class="sacts" id="vsacts"></div>
    <button class="delbtn" onclick="delPost()">刪除此貼文</button>
  </div>
</div>

<script>
// ── 資料 ──
const PLAT={IG:{color:"#E1306C",icon:"📸"},FB:{color:"#1877F2",icon:"👍"}};
const TYPES={photo:{label:"貼文",icon:"🖼️"},video:{label:"影片",icon:"🎬"},reel:{label:"Reels",icon:"✨"},story:{label:"限時動態",icon:"⏱️"}};
const STAT={draft:{label:"草稿",color:"#9ca3af",bg:"#374151"},scheduled:{label:"已排程",color:"#FCD34D",bg:"#3d3010"},published:{label:"已發佈",color:"#6EE7B7",bg:"#0d3326"}};
const AVC=["#7c3aed","#db2777","#0891b2","#059669","#d97706","#dc2626","#0284c7"];
const MZH=["一月","二月","三月","四月","五月","六月","七月","八月","九月","十月","十一月","十二月"];

const STORAGE_KEY="social-cal-v3";
let D={year:new Date().getFullYear(),month:new Date().getMonth(),sel:null,fp:"ALL",fm:"ALL",panel:"cal",formTags:[],formAss:[],vpost:null};
let posts=[],members=[],addMemShown=false;

// ── 儲存 ──
async function save(){
  try{
    setSyncStatus("saving");
    await window.storage.set(STORAGE_KEY,JSON.stringify({posts,members}),true);
    setSyncStatus("saved");
  }catch(e){setSyncStatus("error");}
}
async function load(){
  try{
    const r=await window.storage.get(STORAGE_KEY,true);
    if(r&&r.value){const d=JSON.parse(r.value);if(d.posts)posts=d.posts;if(d.members)members=d.members;}
  }catch(e){}
  renderAll();
}

function setSyncStatus(s){
  const el=document.getElementById("sync");
  if(s==="saving"){el.style.color="#FCD34D";el.textContent="⏳ 同步中…";}
  else if(s==="saved"){el.style.color="#6EE7B7";el.textContent="☁ 已同步給所有人";setTimeout(()=>{el.style.color="transparent";el.textContent="　";},2500);}
  else if(s==="error"){el.style.color="#fca5a5";el.textContent="✗ 同步失敗";setTimeout(()=>{el.style.color="transparent";el.textContent="　";},3000);}
}

function esc(s){return String(s||"").replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;").replace(/"/g,"&quot;");}

// ── 面板切換 ──
function setPanel(p){
  D.panel=p;
  document.getElementById("panel-cal").style.display=p==="cal"?"block":"none";
  document.getElementById("panel-mem").style.display=p==="mem"?"block":"none";
  document.querySelectorAll(".tab").forEach((t,i)=>{t.className="tab"+(((i===0&&p==="cal")||(i===1&&p==="mem"))?" on":"");});
  if(p==="mem")renderMemPanel();
}

// ── 渲染 ──
function renderAll(){renderMemberRow();renderMonthLabel();renderCalendar();renderStats();renderMemberFilter();}
function av(m,sz){const n=m.fullName||m.name||"?";return'<div class="av" style="width:'+sz+'px;height:'+sz+'px;background:'+(m.color||"#7c3aed")+';font-size:'+(sz*0.42)+'px" title="'+esc(n)+'">'+esc(n[0])+"</div>";}
function avgrp(ids,sz){
  const f=(ids||[]).map(id=>members.find(m=>m.id===id)).filter(Boolean);
  return'<div class="avgrp">'+f.slice(0,3).map((m,i)=>'<div style="margin-left:'+(i===0?0:-(sz*0.3))+'px;z-index:'+(3-i)+'">'+av(m,sz)+"</div>").join("")+(f.length>3?'<div style="margin-left:'+(-(sz*0.3))+'px;width:'+sz+'px;height:'+sz+'px;border-radius:50%;background:#374151;display:flex;align-items:center;justify-content:center;font-size:'+(sz*0.33)+'px;color:#9ca3af">+'+(f.length-3)+"</div>":"")+"</div>";
}
function renderMemberRow(){document.getElementById("mlist").innerHTML=members.map(m=>'<div class="mchip">'+av(m,18)+"<span>"+esc(m.fullName||m.name)+"</span></div>").join("");}
function renderMemberFilter(){const s=document.getElementById("f-mem");s.innerHTML='<option value="ALL">👤 所有成員</option>'+members.map(m=>'<option value="'+m.id+'">'+esc(m.fullName||m.name)+"</option>").join("");}
function renderMonthLabel(){document.getElementById("mlbl").textContent=D.year+" 年 "+MZH[D.month];}
function renderStats(){
  const t=posts.filter(p=>{const[y,m]=(p.date||"").split("-").map(Number);return y===D.year&&m===D.month+1;});
  document.getElementById("s-total").textContent=t.length;
  document.getElementById("s-sch").textContent=t.filter(p=>p.status==="scheduled").length;
  document.getElementById("s-dr").textContent=t.filter(p=>p.status==="draft").length;
}
function gpd(day){
  const ds=D.year+"-"+String(D.month+1).padStart(2,"0")+"-"+String(day).padStart(2,"0");
  return posts.filter(p=>p.date===ds&&(D.fp==="ALL"||p.platform===D.fp)&&(D.fm==="ALL"||p.assignees&&p.assignees.includes(D.fm)));
}
function renderCalendar(){
  const g=document.getElementById("calgrid");
  const dim=new Date(D.year,D.month+1,0).getDate();
  const fd=new Date(D.year,D.month,1).getDay();
  const td=new Date();
  let h="";
  for(let i=0;i<fd;i++)h+='<div class="cell emp"></div>';
  for(let d=1;d<=dim;d++){
    const dp=gpd(d);
    const iT=td.getDate()===d&&td.getMonth()===D.month&&td.getFullYear()===D.year;
    const iS=D.sel===d;
    let pills=dp.slice(0,2).map(p=>{
      const c=(PLAT[p.platform]&&PLAT[p.platform].color||"#666")+(p.status==="draft"?"77":"");
      return'<div class="pill'+(p.status==="draft"?" dr":"")+'" style="background:'+c+'" onclick="event.stopPropagation();openView(\''+p.id+'\')"><span>'+(TYPES[p.type]&&TYPES[p.type].icon||"")+'</span><span class="ptitle">'+esc(p.title)+"</span>"+(p.assignees&&p.assignees.length?avgrp(p.assignees,12):"")+"</div>";
    }).join("");
    if(dp.length>2)pills+='<div class="more">+'+(dp.length-2)+" 則</div>";
    h+='<div class="cell'+(iT?" tod":"")+(iS?" sel":"")+'" onclick="selDay('+d+')"><div class="dnum'+(iT?" tn":"")+(iS&&!iT?" sn":"")+'">'+d+"</div>"+pills+"</div>";
  }
  g.innerHTML=h;
  renderDetail();
}
function renderDetail(){
  const p=document.getElementById("detpanel");
  if(!D.sel){p.style.display="none";return;}
  p.style.display="block";
  const ps=gpd(D.sel);
  document.getElementById("dttl").textContent=(D.month+1)+" 月 "+D.sel+" 日 · "+ps.length+" 則排程";
  const c=document.getElementById("dcards");
  if(!ps.length){c.innerHTML='<div class="empty">這天目前沒有排程內容</div>';return;}
  c.innerHTML=ps.map(post=>{
    const pl=PLAT[post.platform]||{color:"#666"};
    const st=STAT[post.status]||STAT.draft;
    return'<div class="pcard" style="border:1px solid '+pl.color+'44" onclick="openView(\''+post.id+'\')">'+'<div class="ctop"><div style="display:flex;gap:5px;align-items:center"><span class="pbadge" style="background:'+pl.color+'">'+post.platform+'</span><span style="font-size:11px">'+(TYPES[post.type]&&TYPES[post.type].icon||"")+'</span></div><span class="sbadge" style="background:'+st.bg+';color:'+st.color+'">'+st.label+'</span></div><div class="ctitle">'+esc(post.title)+'</div><div class="ccap">'+esc(post.caption||"")+'</div><div class="cfoot"><span class="ctime">🕐 '+(post.time||"—")+"</span>"+avgrp(post.assignees||[],20)+"</div></div>";
  }).join("");
}
function renderMemPanel(){
  document.getElementById("mptitle").textContent="團隊成員（"+members.length+" 人）";
  document.getElementById("mcards").innerHTML=members.map(m=>'<div class="mcard" style="border:1px solid '+m.color+'44">'+av(m,40)+'<div style="flex:1"><div class="mname">'+esc(m.fullName||m.name)+'</div><div class="mrole">'+(m.role||"—")+'</div></div><button class="rmbtn" onclick="rmMem(\''+m.id+'\')" title="移除">✕</button></div>').join("");
}

// ── 互動 ──
function selDay(d){D.sel=D.sel===d?null:d;renderCalendar();}
function pm(){if(D.month===0){D.month=11;D.year--;}else D.month--;D.sel=null;renderAll();}
function nm(){if(D.month===11){D.month=0;D.year++;}else D.month++;D.sel=null;renderAll();}
function sf(p){D.fp=p;["all","ig","fb"].forEach(x=>{const el=document.getElementById("f-"+x);if(el)el.className="chip";});if(p==="ALL")document.getElementById("f-all").className="chip aall";if(p==="IG")document.getElementById("f-ig").className="chip aig";if(p==="FB")document.getElementById("f-fb").className="chip afb";renderCalendar();}
function sm(v){D.fm=v;renderCalendar();}

// ── 新增成員 ──
function toggleAddMem(){addMemShown=!addMemShown;document.getElementById("addmembox").style.display=addMemShown?"flex":"none";}
function addMem(){
  const n=document.getElementById("mn").value.trim();
  if(!n)return;
  const r=document.getElementById("mr").value.trim();
  members.push({id:"m"+Date.now(),name:n,fullName:n,role:r,color:AVC[members.length%AVC.length]});
  document.getElementById("mn").value="";document.getElementById("mr").value="";
  toggleAddMem();save();renderMemberRow();renderMemberFilter();renderMemPanel();
}
function rmMem(id){members=members.filter(m=>m.id!==id);save();renderMemberRow();renderMemberFilter();renderMemPanel();}

// ── 新增貼文 ──
function openAdd(){
  D.formTags=[];D.formAss=[];
  ["ft","fc","fd"].forEach(id=>document.getElementById(id).value="");
  document.getElementById("fp").value="IG";document.getElementById("fty").value="photo";
  document.getElementById("ftm").value="12:00";document.getElementById("fs").value="draft";
  document.getElementById("fti").value="";renderFAss();renderFTags();
  document.getElementById("addmodal").classList.add("show");
}
function closeAdd(e){if(!e||e.target===document.getElementById("addmodal"))document.getElementById("addmodal").classList.remove("show");}
function renderFAss(){document.getElementById("falist").innerHTML=members.map(m=>{const n=m.fullName||m.name;const on=D.formAss.includes(m.id);return'<div class="achip" style="'+(on?"background:"+m.color+"33;border:1px solid "+m.color:"background:rgba(255,255,255,0.05);border:1px solid rgba(255,255,255,0.09)")+'" onclick="tgAss(\''+m.id+'\')">'+av(m,20)+'<span style="font-size:11px;font-weight:700;color:'+(on?"#fff":"rgba(255,255,255,0.65)")+'">'+esc(n)+"</span>"+(on?'<span style="font-size:10px;color:'+m.color+'">✓</span>':"")+"</div>";}).join("");}
function tgAss(id){if(D.formAss.includes(id))D.formAss=D.formAss.filter(x=>x!==id);else D.formAss.push(id);renderFAss();}
function at(){const v=document.getElementById("fti").value.trim();if(v&&!D.formTags.includes(v)){D.formTags.push(v);renderFTags();}document.getElementById("fti").value="";}
function rt(t){D.formTags=D.formTags.filter(x=>x!==t);renderFTags();}
function renderFTags(){document.getElementById("ftl").innerHTML=D.formTags.map(t=>'<span class="titem" onclick="rt(\''+esc(t)+'\')">#'+esc(t)+" ✕</span>").join("");}
function submitPost(){
  const title=document.getElementById("ft").value.trim();
  if(!title)return;
  posts.push({id:"p"+Date.now(),title,caption:document.getElementById("fc").value,platform:document.getElementById("fp").value,type:document.getElementById("fty").value,date:document.getElementById("fd").value,time:document.getElementById("ftm").value||"12:00",status:document.getElementById("fs").value,tags:[...D.formTags],assignees:[...D.formAss]});
  save();closeAdd();renderAll();
}

// ── 檢視貼文 ──
function openView(id){
  const post=posts.find(p=>p.id===id);if(!post)return;
  D.vpost=post;
  const pl=PLAT[post.platform]||{color:"#666",icon:"📌"};
  const st=STAT[post.status]||STAT.draft;
  document.getElementById("vb").innerHTML='<span class="vbadge" style="background:'+pl.color+'">'+pl.icon+" "+post.platform+'</span><span class="vbadge" style="background:'+st.bg+';color:'+st.color+'">'+st.label+"</span>";
  document.getElementById("vt").textContent=post.title;
  const vc=document.getElementById("vc");vc.textContent=post.caption||"";vc.style.display=post.caption?"block":"none";
  document.getElementById("vty").textContent=(TYPES[post.type]&&TYPES[post.type].icon||"")+" "+(TYPES[post.type]&&TYPES[post.type].label||post.type);
  document.getElementById("vtm").textContent=(post.date||"未設定")+" "+(post.time||"");
  const ass=(post.assignees||[]).map(id=>members.find(m=>m.id===id)).filter(Boolean);
  const as=document.getElementById("vasec");
  if(ass.length){as.style.display="block";document.getElementById("vac").innerHTML=ass.map(m=>'<div class="acard" style="background:'+m.color+'22;border:1px solid '+m.color+'55">'+av(m,22)+'<span style="font-size:11px;font-weight:700">'+esc(m.fullName||m.name)+"</span></div>").join("");}
  else as.style.display="none";
  document.getElementById("vtgs").innerHTML=(post.tags||[]).map(t=>'<span class="vtag">#'+esc(t)+"</span>").join("");
  document.getElementById("vsacts").innerHTML=Object.entries(STAT).filter(([k])=>k!==post.status).map(([k,v])=>'<button class="sact" style="background:'+v.bg+';color:'+v.color+'" onclick="chStat(\''+k+"')\">→ "+v.label+"</button>").join("");
  document.getElementById("viewmodal").classList.add("show");
}
function closeView(e){if(!e||e.target===document.getElementById("viewmodal"))document.getElementById("viewmodal").classList.remove("show");}
function delPost(){if(!confirm("確定要刪除這則貼文嗎？"))return;posts=posts.filter(p=>p.id!==D.vpost.id);save();closeView();renderAll();}
function chStat(s){posts=posts.map(p=>p.id===D.vpost.id?{...p,status:s}:p);save();closeView();renderAll();}

load();
</script>
</body>
</html>
