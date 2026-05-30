# sampling-advisor
Research sampling method advisor tool
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sampling Method Advisor</title>
<style>
:root{
  --teal:#0F6E56;--teal-l:#E1F5EE;--teal-m:#5DCAA5;
  --navy:#1B3A5C;--amber:#B8960C;--amber-l:#FBF6E8;
  --rust:#8C3A1F;--rust-l:#FAF0EC;
  --g50:#F8F7F4;--g100:#EEECE6;--g200:#D8D5CB;
  --g400:#9A9590;--g600:#5F5E5A;--g800:#2E2C29;
  --white:#FFFFFE;
  --r:10px;--rl:14px;
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:system-ui,-apple-system,sans-serif;font-size:15px;color:var(--g800);background:var(--g50);padding:1.5rem;line-height:1.6}

.card{background:var(--white);border-radius:var(--rl);border:1px solid var(--g100);overflow:hidden;max-width:640px;margin:0 auto}
.card-head{
  background:var(--navy);padding:1.25rem 1.5rem;
  display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:8px;
}
.card-head-title{color:#fff;font-size:15px;font-weight:600}
.card-head-sub{color:rgba(255,255,255,0.5);font-size:12px;margin-top:2px}
.prog-wrap{display:flex;align-items:center;gap:8px}
.prog-track{width:60px;height:3px;background:rgba(255,255,255,0.2);border-radius:2px;overflow:hidden}
.prog-fill{height:100%;background:var(--teal-m);border-radius:2px;transition:width 0.3s}
.prog-txt{font-size:11px;color:rgba(255,255,255,0.4);white-space:nowrap}
.card-body{padding:1.5rem}

.q-num{font-size:11px;font-weight:600;letter-spacing:0.08em;color:var(--teal);text-transform:uppercase;margin-bottom:4px}
.q-text{font-size:16px;font-weight:600;color:var(--navy);line-height:1.4;margin-bottom:4px}
.q-hint{font-size:13px;color:var(--g400);line-height:1.55;margin-bottom:1.25rem}

.opts{display:grid;gap:8px;margin-bottom:1.5rem}
.opt{
  background:var(--white);border:1.5px solid var(--g100);border-radius:var(--r);
  padding:0.875rem 1rem;cursor:pointer;text-align:left;width:100%;
  display:flex;gap:10px;align-items:flex-start;transition:all 0.15s;
}
.opt:hover{border-color:var(--teal-m);background:var(--teal-l)}
.opt.sel{border-color:var(--teal);background:var(--teal-l)}
.opt-rb{
  width:16px;height:16px;border-radius:50%;border:2px solid var(--g200);
  flex-shrink:0;margin-top:2px;transition:all 0.15s;position:relative;
}
.opt.sel .opt-rb{border-color:var(--teal);background:var(--teal)}
.opt.sel .opt-rb::after{content:'';position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);width:5px;height:5px;border-radius:50%;background:#fff}
.opt-lbl{font-size:14px;font-weight:500;color:var(--g800);margin-bottom:2px}
.opt.sel .opt-lbl{color:var(--navy)}
.opt-sub{font-size:12px;color:var(--g400)}
.opt.sel .opt-sub{color:var(--teal)}

.nav{display:flex;justify-content:space-between;align-items:center}
.dots{display:flex;gap:4px}
.dot{width:5px;height:5px;border-radius:2.5px;background:var(--g100);transition:all 0.2s}
.dot.done{background:var(--teal-m)}
.dot.active{background:var(--teal);width:14px}
.btns{display:flex;gap:6px}
.btn{padding:7px 16px;border-radius:8px;border:1.5px solid var(--g200);background:var(--white);font-size:13px;font-weight:500;color:var(--g600);cursor:pointer;transition:all 0.15s}
.btn:hover{border-color:var(--g400);background:var(--g50)}
.btn-p{background:var(--teal);border-color:var(--teal);color:#fff}
.btn-p:hover{background:#085041;border-color:#085041}
.btn:disabled{opacity:0.35;cursor:not-allowed}

.badge{display:inline-flex;align-items:center;gap:5px;padding:4px 12px;border-radius:20px;font-size:11px;font-weight:600;letter-spacing:0.06em;text-transform:uppercase;margin-bottom:1rem}
.badge-rec{background:var(--teal-l);color:var(--teal)}
.badge-cau{background:var(--amber-l);color:var(--amber)}
.res-name{font-size:1.5rem;font-weight:700;color:var(--navy);line-height:1.2;margin-bottom:3px}
.res-also{font-size:12px;color:var(--g400);font-style:italic;margin-bottom:0.875rem}
.res-desc{font-size:14px;color:var(--g600);line-height:1.7;margin-bottom:1.25rem;padding-bottom:1.25rem;border-bottom:1px solid var(--g100)}
.pills{display:flex;flex-wrap:wrap;gap:5px;margin-bottom:1.25rem}
.pill{background:var(--g50);border:1px solid var(--g100);border-radius:20px;padding:3px 10px;font-size:11px;color:var(--g600)}
.rs{margin-bottom:1.25rem}
.rs-title{font-size:10px;font-weight:700;letter-spacing:0.1em;text-transform:uppercase;color:var(--g400);margin-bottom:6px}
.rs-block{background:var(--g50);border-radius:8px;padding:0.875rem 1rem;font-size:13px;color:var(--g600);line-height:1.65;border-left:3px solid var(--teal)}
.rs-list{display:grid;gap:6px}
.rs-item{display:flex;gap:8px;font-size:13px;color:var(--g600);line-height:1.55;align-items:flex-start}
.rs-dot{width:5px;height:5px;border-radius:50%;flex-shrink:0;margin-top:6px}
.rs-dot.g{background:var(--teal)}
.rs-dot.a{background:var(--amber)}
.cite-block{background:var(--navy);border-radius:8px;padding:0.875rem 1rem;font-size:12px;color:rgba(255,255,255,0.6);line-height:1.65;font-style:italic}
.alts{display:grid;gap:6px}
.alt{background:var(--white);border:1px solid var(--g100);border-radius:8px;padding:0.75rem 1rem}
.alt-name{font-size:13px;font-weight:600;color:var(--navy);margin-bottom:3px}
.alt-why{font-size:12px;color:var(--g400);line-height:1.5}
.res-actions{margin-top:1.5rem;padding-top:1.25rem;border-top:1px solid var(--g100);display:flex;gap:8px;flex-wrap:wrap}
.credit{text-align:center;font-size:11px;color:var(--g400);margin-top:1rem;letter-spacing:0.02em}
</style>
</head>
<body>

<div class="card" role="main">
  <div class="card-head">
    <div>
      <div class="card-head-title">Research Sampling Advisor</div>
      <div class="card-head-sub">Find the right sampling method for your study</div>
    </div>
    <div class="prog-wrap">
      <div class="prog-track"><div class="prog-fill" id="pf" style="width:0%"></div></div>
      <span class="prog-txt" id="pt">0 / 7</span>
    </div>
  </div>
  <div class="card-body" id="cb" aria-live="polite"></div>
</div>
<p class="credit">&copy; 2026. All rights reserved</p>

<script>
const Qs=[
  {id:"goal",q:"What is the primary goal of your research?",h:"Think about what kind of knowledge you are trying to produce.",
    os:[{l:"Understand experiences, meanings, or perspectives",s:"Depth over breadth — how and why",v:"qualitative"},{l:"Measure, count, or test relationships across a population",s:"How many, how much, whether X causes Y",v:"quantitative"},{l:"Both — explore and then test, or combine approaches",s:"Qualitative insight combined with quantitative evidence",v:"mixed"}]},
  {id:"population",q:"How well defined is the population you are studying?",h:"A population is the full group of people, objects, or events your research is about.",
    os:[{l:"Clearly defined — I can describe or list them",s:"e.g. all students in a programme, all nurses in a hospital",v:"defined"},{l:"Loosely defined — I know the type but not a complete list",s:"e.g. Caribbean artists who identify as evangelical practitioners",v:"loose"},{l:"Unclear or emergent — will become clearer as I go",s:"e.g. following a phenomenon; participants will emerge from it",v:"unclear"}]},
  {id:"access",q:"How easy is it to access your target participants?",h:"Be honest — access constraints are a legitimate factor in sampling decisions.",
    os:[{l:"Relatively easy — I can reach most of them",s:"Through institutions, organisations, or public channels",v:"easy"},{l:"Difficult — they are hard to find or contact",s:"Hidden, vulnerable, or dispersed populations",v:"hard"},{l:"Through gatekeepers or community networks only",s:"I need insiders to refer me to others",v:"gatekeeper"}]},
  {id:"purpose",q:"What do you want to do with the findings?",h:"This affects how rigorous your sampling process needs to be.",
    os:[{l:"Generalise to the wider population",s:"Make claims that apply beyond my specific sample",v:"generalise"},{l:"Understand deeply — rich insight over representativeness",s:"Develop theory, explore meaning, produce thick description",v:"depth"},{l:"Generate hypotheses or explore an under-researched area",s:"Lay groundwork for future research",v:"explore"}]},
  {id:"size",q:"How large does your sample need to be?",h:"Larger samples require different selection methods than small purposive ones.",
    os:[{l:"Small — fewer than 30 participants",s:"Typical for qualitative case studies, interviews, ethnography",v:"small"},{l:"Medium — 30 to 200 participants",s:"Common for mixed methods and some quantitative studies",v:"medium"},{l:"Large — more than 200 participants",s:"Surveys, statistical testing, epidemiological studies",v:"large"}]},
  {id:"sensitivity",q:"How sensitive is your research topic?",h:"Sensitive topics require extra care in how you approach and select participants.",
    os:[{l:"Not particularly sensitive",s:"Standard topic without significant emotional or social risk",v:"low"},{l:"Somewhat sensitive — requires care and trust",s:"Faith, identity, health, personal history, community dynamics",v:"medium"},{l:"Highly sensitive — vulnerability, trauma, or marginalisation",s:"Grief, abuse, illness, persecution, stigmatised identities",v:"high"}]},
  {id:"time",q:"What are your time and resource constraints?",h:"The best method you can actually execute is better than an ideal one you cannot.",
    os:[{l:"Tight — I need to recruit quickly",s:"A few weeks to a month for recruitment",v:"tight"},{l:"Moderate — a few months available",s:"Enough time to be deliberate but not unlimited",v:"moderate"},{l:"Generous — I have time to be thorough",s:"Six months or more; resources to reach a wide population",v:"generous"}]},
];

const SM={
  purposive:{n:"Purposive sampling",a:"Criterion-based or judgement sampling",b:"rec",
    d:"You deliberately select participants because they meet specific criteria relevant to your research question. Every person in your sample is there for a reason you can articulate.",
    w:"Best for qualitative and practice-led research where depth and relevance matter more than statistical representativeness.",
    e:"Selecting five Caribbean artists who identify as practising evangelicals and are currently producing visual work — because these characteristics are what your research is about.",
    s:["Produces highly relevant data directly linked to research questions","Allows theoretical rigour in participant selection","Appropriate for small qualitative samples","Widely respected in arts, humanities, and social science research"],
    l:["Cannot support claims about the wider population","Requires justification of every selection decision","Risk of researcher bias in who is considered appropriate"],
    c:"Patton, Michael Quinn. Qualitative Research and Evaluation Methods. 3rd ed., SAGE, 2002."},
  snowball:{n:"Snowball sampling",a:"Chain-referral or network sampling",b:"rec",
    d:"You recruit initial participants who then refer you to others in their network. The sample grows organically through social connections and community trust.",
    w:"Best when your population is hard to identify, dispersed, or only reachable through community insiders. Particularly well-suited to sensitive topics.",
    e:"Beginning with one evangelical Caribbean artist who refers you to others, gradually building a network of participants connected by shared identity and trust.",
    s:["Reaches hidden or hard-to-access populations","Builds trust through community referral","Appropriate for sensitive research where gatekeeping is protective"],
    l:["Sample biased toward social networks of initial participants","May miss isolated individuals within the community","Difficult to know when population is adequately covered"],
    c:"Biernacki, Patrick, and Dan Waldorf. 'Snowball Sampling.' Sociological Methods and Research, vol. 10, no. 2, 1981, pp. 141–163."},
  random:{n:"Simple random sampling",a:"Probability sampling",b:"rec",
    d:"Every member of your defined population has an equal chance of selection. Selection is made through a random mechanism, removing researcher bias entirely.",
    w:"Best when you have a clearly defined, accessible population, need generalisable findings, and are working with quantitative or mixed methods.",
    e:"Using a random number generator to select 200 participants from a complete register of all students in a programme.",
    s:["Supports statistical generalisation","Removes researcher selection bias entirely","Allows calculation of sampling error and confidence intervals"],
    l:["Requires a complete sampling frame","Impractical for populations that cannot be listed","May include participants with little relevance to specific questions"],
    c:"Creswell, John W. Research Design: Qualitative, Quantitative, and Mixed Methods Approaches. SAGE, 2014."},
  stratified:{n:"Stratified random sampling",a:"Proportional or disproportional stratified sampling",b:"rec",
    d:"You divide your population into subgroups (strata) and randomly sample from each, ensuring representation of key subgroups that might be missed by simple random sampling.",
    w:"Best when your population has distinct subgroups that are all important to your research and you need generalisable findings across those subgroups.",
    e:"Dividing Caribbean artists into subgroups by island or territory, then randomly sampling from each to ensure geographic representation.",
    s:["Ensures representation of key subgroups","More precise than simple random sampling","Allows meaningful comparison between subgroups"],
    l:["Requires knowledge of population subgroup structure","More complex to design and execute","Still requires a sampling frame for each stratum"],
    c:"Creswell, John W. Research Design: Qualitative, Quantitative, and Mixed Methods Approaches. SAGE, 2014."},
  convenience:{n:"Convenience sampling",a:"Availability or accidental sampling",b:"cau",
    d:"You recruit whoever is most accessible or willing to participate. No systematic selection criteria are applied beyond availability.",
    w:"Acceptable as a pragmatic starting point for exploratory or pilot research, or when resources are severely constrained. Must be justified transparently.",
    e:"Recruiting students in your own institution because they are available, then acknowledging this as a limitation of the study.",
    s:["Quick and inexpensive","Practical under severe constraints","Useful for pilot testing instruments"],
    l:["High risk of selection bias","Cannot support generalisation","Weakest form of sampling — requires explicit justification and acknowledgement"],
    c:"Etikan, Ilker, et al. 'Comparison of Convenience Sampling and Purposive Sampling.' American Journal of Theoretical and Applied Statistics, vol. 5, no. 1, 2016, pp. 1–4."},
  theoretical:{n:"Theoretical sampling",a:"Grounded theory sampling",b:"rec",
    d:"Sampling decisions are made iteratively as the research progresses, guided by emerging theory. You continue sampling until theoretical saturation.",
    w:"Best for grounded theory studies and exploratory qualitative research where theory is built from the data rather than tested against it.",
    e:"Beginning by interviewing evangelical artists, then deciding to also interview community leaders after emerging data suggests community response is central.",
    s:["Produces theory genuinely grounded in data","Sampling is responsive to what is being learned","Powerful for under-researched phenomena"],
    l:["Difficult to plan in advance — timelines uncertain","Requires significant methodological experience","Saturation is difficult to determine objectively"],
    c:"Glaser, Barney G., and Anselm L. Strauss. The Discovery of Grounded Theory. Aldine, 1967."},
};

const AR={
  random:"If your population is fully enumerable and you need statistical generalisability",
  stratified:"If distinct subgroups all need to be represented",
  purposive:"If you need specific participant characteristics tied to your questions",
  snowball:"If community network access would be more feasible or ethical",
  convenience:"As a pragmatic fallback — acknowledge limitations explicitly",
  theoretical:"If you are building theory iteratively from data",
};

const PL={
  goal:{qualitative:"Qualitative",quantitative:"Quantitative",mixed:"Mixed methods"},
  population:{defined:"Defined population",loose:"Loose population",unclear:"Emergent population"},
  access:{easy:"Easy access",hard:"Hard to access",gatekeeper:"Via gatekeepers"},
  purpose:{generalise:"Seeking generalisation",depth:"Seeking depth",explore:"Exploratory"},
  size:{small:"Small sample",medium:"Medium sample",large:"Large sample"},
  sensitivity:{low:"Low sensitivity",medium:"Moderate sensitivity",high:"High sensitivity"},
  time:{tight:"Tight timeline",moderate:"Moderate timeline",generous:"Generous timeline"},
};

function getRec(a){
  const{goal:g,population:p,access:ac,purpose:pu,size:sz,sensitivity:se,time:t}=a;
  if(g==="quantitative"&&sz==="large"&&p==="defined") return{p:"stratified",alts:["random"]};
  if(g==="quantitative"&&sz==="large") return{p:"random",alts:["stratified"]};
  if(g==="quantitative"&&sz==="medium") return{p:"stratified",alts:["random","purposive"]};
  if(ac==="gatekeeper"||(se==="high"&&ac==="hard")) return{p:"snowball",alts:["purposive"]};
  if(ac==="hard"&&se==="medium") return{p:"snowball",alts:["purposive","convenience"]};
  if(pu==="depth"&&g==="qualitative") return{p:"purposive",alts:["theoretical","snowball"]};
  if(pu==="explore"&&g==="qualitative") return{p:"theoretical",alts:["purposive","snowball"]};
  if(pu==="generalise"&&p==="defined") return{p:"stratified",alts:["random"]};
  if(pu==="generalise") return{p:"random",alts:["stratified","purposive"]};
  if(g==="mixed") return{p:"purposive",alts:["stratified","snowball"]};
  if(t==="tight"&&sz==="small") return{p:"convenience",alts:["purposive","snowball"]};
  if(sz==="small"&&g==="qualitative") return{p:"purposive",alts:["snowball","theoretical"]};
  return{p:"purposive",alts:["snowball","convenience"]};
}

let cur=0,ans={};

function up(){
  const pct=Math.round((cur/Qs.length)*100);
  document.getElementById("pf").style.width=pct+"%";
  document.getElementById("pt").textContent=cur+" / "+Qs.length;
}

function renderQ(){
  up();
  const q=Qs[cur],prev=ans[q.id]||null;
  let dots="";
  for(let i=0;i<Qs.length;i++) dots+=`<div class="dot${i<cur?" done":i===cur?" active":""}"></div>`;
  let opts=q.os.map(o=>`<button class="opt${prev===o.v?" sel":""}" onclick="pick('${o.v}',this)">
    <div class="opt-rb"></div>
    <div><div class="opt-lbl">${o.l}</div>${o.s?`<div class="opt-sub">${o.s}</div>`:""}</div>
  </button>`).join("");
  document.getElementById("cb").innerHTML=`
    <div class="q-num">Question ${cur+1} of ${Qs.length}</div>
    <div class="q-text">${q.q}</div>
    <p class="q-hint">${q.h}</p>
    <div class="opts">${opts}</div>
    <div class="nav">
      <div class="dots">${dots}</div>
      <div class="btns">
        ${cur>0?`<button class="btn" onclick="back()">&larr;</button>`:""}
        <button class="btn btn-p" id="nb" onclick="nxt()" ${!prev?"disabled":""}>${cur===Qs.length-1?"See result &rarr;":"Next &rarr;"}</button>
      </div>
    </div>`;
}

function pick(v,el){
  ans[Qs[cur].id]=v;
  document.querySelectorAll(".opt").forEach(b=>b.classList.remove("sel"));
  el.classList.add("sel");
  const nb=document.getElementById("nb");
  if(nb) nb.disabled=false;
}

function nxt(){
  if(!ans[Qs[cur].id]) return;
  cur++;
  cur>=Qs.length?renderRes():renderQ();
}

function back(){if(cur>0){cur--;renderQ();}}

function renderRes(){
  document.getElementById("pf").style.width="100%";
  document.getElementById("pt").textContent="Done";
  const rec=getRec(ans),m=SM[rec.p],alts=rec.alts.map(k=>({key:k,...SM[k]}));
  const pills=Object.entries(ans).map(([k,v])=>`<div class="pill">${PL[k]?PL[k][v]:v}</div>`).join("");
  const ss=m.s.map(x=>`<div class="rs-item"><div class="rs-dot g"></div><span>${x}</span></div>`).join("");
  const ls=m.l.map(x=>`<div class="rs-item"><div class="rs-dot a"></div><span>${x}</span></div>`).join("");
  const ah=alts.map(a=>`<div class="alt"><div class="alt-name">${a.n}</div><div class="alt-why">${AR[a.key]||""}</div></div>`).join("");
  document.getElementById("cb").innerHTML=`
    <div class="badge badge-${m.b}">${m.b==="rec"?"&#10003; Recommended":"&#9888; Use with caution"}</div>
    <div class="res-name">${m.n}</div>
    <div class="res-also">${m.a}</div>
    <p class="res-desc">${m.d}</p>
    <div class="pills">${pills}</div>
    <div class="rs"><div class="rs-title">When to use it</div><div class="rs-block">${m.w}</div></div>
    <div class="rs"><div class="rs-title">Example in context</div><div class="rs-block">${m.e}</div></div>
    <div class="rs"><div class="rs-title">Strengths</div><div class="rs-list">${ss}</div></div>
    <div class="rs"><div class="rs-title">Limitations to acknowledge</div><div class="rs-list">${ls}</div></div>
    <div class="rs"><div class="rs-title">Citation for your methodology chapter</div><div class="cite-block">${m.c}</div></div>
    ${alts.length?`<div class="rs"><div class="rs-title">Alternative approaches</div><div class="alts">${ah}</div></div>`:""}
    <div class="res-actions">
      <button class="btn" onclick="restart()">&#8635; Start over</button>
    </div>`;
}

function restart(){cur=0;ans={};renderQ();}
renderQ();
</script>
</body>
</html>
