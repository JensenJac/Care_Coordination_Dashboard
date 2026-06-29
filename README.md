<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Care Coordination Dashboard v2 — IMH</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:Arial,sans-serif;background:#F5F7FA;color:#1F2D3D;min-height:100vh}
/* ── role tab bar ── */
.tab-bar{display:flex;gap:0;border-bottom:2px solid #CCCCCC;background:#fff;padding:0 20px}
.tab{padding:13px 22px;font-size:13px;font-weight:600;cursor:pointer;border-bottom:3px solid transparent;margin-bottom:-2px;color:#888780;transition:all .15s}
.tab:hover{color:#1F2D3D}
.tab.active-cm{color:#1F4E79;border-bottom-color:#1F4E79}
.tab.active-cons{color:#0F6E56;border-bottom-color:#0F6E56}
.tab.active-admin{color:#7B3FA0;border-bottom-color:#7B3FA0}
/* ── layout ── */
.panel{display:none;padding:20px;max-width:860px;margin:0 auto}
.panel.active{display:block}
/* ── shared components ── */
.top-bar{display:flex;align-items:center;justify-content:space-between;margin-bottom:16px;flex-wrap:wrap;gap:8px}
.top-bar h1{font-size:19px;font-weight:600}
.subtitle{font-size:12px;color:#5A6675;margin-top:2px}
.badge{font-size:11px;padding:3px 10px;border-radius:12px;font-weight:700}
.badge-red{background:#FCEBEB;color:#A32D2D}
.badge-amber{background:#FAEEDA;color:#633806}
.badge-green{background:#E1F5EE;color:#0F6E56}
.badge-gray{background:#F1EFE8;color:#5F5E5A}
.badge-purple{background:#F0E8FA;color:#6A1FAA}
.section{background:#fff;border-radius:12px;padding:16px 18px;margin-bottom:14px;border:1px solid #E0E0E0;box-shadow:0 1px 3px rgba(0,0,0,.04)}
.sec-title{font-size:11px;font-weight:700;text-transform:uppercase;letter-spacing:.06em;color:#888780;margin-bottom:10px}
.sec-desc{font-size:12px;color:#5A6675;margin-bottom:12px;line-height:1.5}
/* ── inputs ── */
.input-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.input-block{display:flex;flex-direction:column;gap:4px}
.input-block label{font-size:12px;color:#5A6675;font-weight:500}
.input-block input,.input-block select{padding:8px 10px;border-radius:8px;border:1px solid #CCCCCC;background:#fff;color:#1F2D3D;font-size:13px;font-family:Arial,sans-serif;width:100%}
.input-block input:focus,.input-block select:focus{outline:none;border-color:#1F4E79}
/* ── buttons ── */
.run-btn{width:100%;padding:11px;border-radius:8px;border:none;font-size:13px;font-weight:700;cursor:pointer;margin-top:12px;font-family:Arial,sans-serif;letter-spacing:.02em;color:#fff}
.run-btn-cm{background:#1F4E79}.run-btn-cm:hover{background:#163A5A}
.run-btn-cons{background:#0F6E56}.run-btn-cons:hover{background:#085041}
.run-btn-admin{background:#7B3FA0}.run-btn-admin:hover{background:#5C2E7A}
.save-btn{width:100%;padding:9px;border-radius:8px;background:#F1EFE8;border:1px solid #CCCCCC;color:#1F2D3D;font-size:12px;cursor:pointer;margin-top:10px;font-family:Arial,sans-serif;font-weight:600}
.save-btn:hover{background:#E0DDD4}
/* ── alerts ── */
.alert-card{border-radius:8px;padding:11px 14px;margin-bottom:8px;border-left:4px solid}
.alert-red{background:#FCEBEB;border-color:#A32D2D}
.alert-amber{background:#FAEEDA;border-color:#EF9F27}
.alert-green{background:#E1F5EE;border-color:#0F6E56}
.alert-title{font-size:13px;font-weight:700;margin-bottom:3px;color:#1F2D3D}
.alert-body{font-size:12px;color:#5A6675;line-height:1.55}
.alert-action{font-size:11px;font-weight:700;margin-top:6px;padding:3px 9px;border-radius:6px;display:inline-block;background:#fff;border:1px solid #CCCCCC;color:#1F4E79}
.alert-action-cons{color:#0F6E56}
/* ── metric rows ── */
.metric-row{display:flex;justify-content:space-between;align-items:center;padding:8px 0;border-bottom:1px solid #F1EFE8;font-size:13px}
.metric-row:last-child{border-bottom:none}
.metric-label{color:#5A6675;font-size:12px}
.metric-val{font-weight:700;color:#1F2D3D}
.pill{font-size:10px;padding:2px 8px;border-radius:8px;font-weight:700}
.pill-red{background:#FCEBEB;color:#A32D2D}
.pill-amber{background:#FAEEDA;color:#633806}
.pill-green{background:#E1F5EE;color:#0F6E56}
.pill-gray{background:#F1EFE8;color:#5F5E5A}
/* ── capacity bar ── */
.cap-bar-bg{height:10px;border-radius:5px;background:#E0E0E0;overflow:hidden;flex:1}
.cap-bar-fill{height:100%;border-radius:5px;transition:width .4s}
/* ── action items ── */
.action-item{display:flex;gap:10px;align-items:flex-start;padding:8px 0;border-bottom:1px solid #F1EFE8;font-size:13px}
.action-item:last-child{border-bottom:none}
.action-num{font-weight:700;color:#A32D2D;min-width:18px}
/* ── documentation ── */
.doc-row{display:flex;gap:10px;align-items:center;padding:7px 0;border-bottom:1px solid #F1EFE8}
.doc-row:last-child{border-bottom:none}
.doc-label{font-size:12px;font-weight:600;min-width:140px;color:#5A6675}
.doc-input{flex:1;padding:6px 9px;border-radius:6px;border:1px solid #CCCCCC;background:#fff;color:#1F2D3D;font-size:12px;font-family:Arial,sans-serif}
/* ── admin stat cards ── */
.stat-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:4px}
.stat-card{border-radius:10px;padding:14px 16px;border:1px solid #E0E0E0;text-align:center}
.stat-num{font-size:28px;font-weight:700;margin-bottom:2px}
.stat-label{font-size:11px;color:#5A6675}
/* ── escalation table (consultant) ── */
.esc-row{display:flex;gap:10px;align-items:flex-start;padding:10px 0;border-bottom:1px solid #F1EFE8;font-size:13px}
.esc-row:last-child{border-bottom:none}
.esc-id{font-weight:700;min-width:70px;color:#1F4E79}
.esc-detail{flex:1;color:#5A6675;font-size:12px;line-height:1.4}
.esc-action{font-size:11px;font-weight:700;padding:3px 9px;border-radius:6px;background:#E1F5EE;color:#0F6E56;white-space:nowrap}
.empty{color:#B4B2A9;font-size:13px;font-style:italic;padding:6px 0}
.role-note{font-size:11px;padding:6px 10px;border-radius:6px;background:#EBF3FB;color:#185FA5;margin-bottom:12px;border-left:3px solid #4A86C8}
.guideline-section{background:#EBF3FB;border-color:#B5D4F4}
.guideline-text{font-size:12px;color:#185FA5;line-height:1.8}
</style>
</head>
<body>

<!-- ── ROLE TAB BAR ── -->
<div class="tab-bar">
  <div class="tab active-cm" id="tab-cm"    onclick="switchRole('cm')">👤 Case Manager</div>
  <div class="tab"           id="tab-cons"  onclick="switchRole('cons')">🩺 Consultant</div>
  <div class="tab"           id="tab-admin" onclick="switchRole('admin')">📊 Admin</div>
</div>

<!-- ══════════════════════════════════════════════════════════════
     PANEL 1 — CASE MANAGER
══════════════════════════════════════════════════════════════ -->
<div class="panel active" id="panel-cm">
  <div class="top-bar">
    <div><h1 style="color:#1F4E79">Case Manager Dashboard</h1>
    <div class="subtitle">Daily coordination check — early identification of high-risk patients</div></div>
    <span class="badge badge-gray" id="cm-badge">No data entered</span>
  </div>
  <div class="role-note">This dashboard surfaces coordination gaps and time-sensitive actions for the Case Manager only. Clinical decisions (risk reassessment, Mobile Crisis Team activation) are escalated to the Consultant — not actioned here.</div>

  <div class="section">
    <div class="sec-title">Patient coordination check</div>
    <div class="sec-desc">Enter details for one patient. The dashboard checks each coordination step against NICE NG53, SAMHSA, and MOH Singapore timeframes and surfaces what needs attention today.</div>
    <div class="input-grid">
      <div class="input-block"><label>Patient ID / initials</label><input id="cm-pat-id" placeholder="e.g. PT-042"/></div>
      <div class="input-block"><label>Days since ES discharge</label><input id="cm-days-dis" type="number" min="0" placeholder="e.g. 4"/></div>
      <div class="input-block"><label>Post-discharge contact attempted?</label>
        <select id="cm-contact"><option value="">— select —</option><option value="yes">Yes — documented in EHR</option><option value="no">No</option></select></div>
      <div class="input-block"><label>Follow-up appointment status</label>
        <select id="cm-appt"><option value="">— select —</option><option value="attended">Attended</option><option value="missed-out">Missed — outreach done</option><option value="missed-none">Missed — no outreach yet</option><option value="upcoming">Upcoming</option></select></div>
      <div class="input-block"><label>Caregiver contacted post-discharge?</label>
        <select id="cm-carer"><option value="">— select —</option><option value="yes">Yes — documented</option><option value="no">No</option><option value="na">No caregiver identified</option></select></div>
      <div class="input-block"><label>Escalation raised to consultant?</label>
        <select id="cm-esc"><option value="">— select —</option><option value="yes-ack">Yes — consultant acknowledged</option><option value="yes-pend">Yes — awaiting acknowledgement</option><option value="no-needed">Not needed</option><option value="no-unclear">Unclear whether needed</option></select></div>
    </div>
    <button class="run-btn run-btn-cm" onclick="runCM()">Run coordination check</button>
  </div>

  <div id="cm-output" style="display:none">
    <div class="section">
      <div class="sec-title">Priority alerts</div>
      <div style="font-size:11px;color:#888780;margin-bottom:10px;font-style:italic">Red = urgent action required today &nbsp;|&nbsp; Amber = review before end of shift &nbsp;|&nbsp; Green = no gap</div>
      <div id="cm-alerts"></div>
    </div>
    <div class="section">
      <div class="sec-title">Coordination metric summary</div>
      <div id="cm-metrics"></div>
    </div>
    <div class="section">
      <div class="sec-title">Recommended actions — today</div>
      <div class="sec-desc">Prioritised by urgency. Complete in order. Clinical decisions (e.g. Mobile Crisis Team, medication changes) must be escalated to the Consultant — do not action independently.</div>
      <div id="cm-actions"></div>
    </div>
    <div class="section">
      <div class="sec-title">Documentation prompt</div>
      <div class="sec-desc">Complete after each action. Transfer to EHR before end of shift — undocumented coordination is the same as coordination that did not happen.</div>
      <div class="doc-row"><div class="doc-label">Contact attempt</div><input class="doc-input" placeholder="Date, method, outcome (reached / voicemail / no answer)…"/></div>
      <div class="doc-row"><div class="doc-label">Appointment outcome</div><input class="doc-input" placeholder="Attended / missed / rescheduled to…"/></div>
      <div class="doc-row"><div class="doc-label">Caregiver contact</div><input class="doc-input" placeholder="Who contacted, what shared, crisis number given…"/></div>
      <div class="doc-row"><div class="doc-label">Escalation to consultant</div><input class="doc-input" placeholder="Raised to whom, date/time, reason for escalation…"/></div>
      <div class="doc-row"><div class="doc-label">Next planned action</div><input class="doc-input" placeholder="What happens next, by whom, and by when…"/></div>
      <button class="save-btn" onclick="alert('Note saved.\n\nRemember: transfer to your EHR before end of shift.')">Save documentation note</button>
    </div>
    <div class="section guideline-section">
      <div class="sec-title">Guideline references</div>
      <div class="guideline-text">
        NICE NG53 (2016, reviewed 2024) — follow-up within 72h, named coordinator before discharge: <a href="https://www.nice.org.uk/guidance/ng53" target="_blank" style="color:#185FA5">nice.org.uk/guidance/ng53</a><br>
        SAMHSA 2025 — warm hand-off, pre-agreed escalation triggers: <a href="https://library.samhsa.gov/product/national-behavioral-health-crisis-care-guidance/pep24-01-037" target="_blank" style="color:#185FA5">library.samhsa.gov</a><br>
        MOH Singapore 2023 — standardised triage, Mobile Crisis Team, early identification: <a href="https://www.moh.gov.sg" target="_blank" style="color:#185FA5">moh.gov.sg</a>
      </div>
    </div>
  </div>
</div>

<!-- ══════════════════════════════════════════════════════════════
     PANEL 2 — CONSULTANT
══════════════════════════════════════════════════════════════ -->
<div class="panel" id="panel-cons">
  <div class="top-bar">
    <div><h1 style="color:#0F6E56">Consultant Dashboard</h1>
    <div class="subtitle">Escalations, risk reassessments, and clinical decisions awaiting review</div></div>
    <span class="badge badge-gray" id="cons-badge">No data entered</span>
  </div>
  <div class="role-note" style="background:#E8F8F2;border-left-color:#0F6E56;color:#085041">This view shows only patients requiring clinical decision-making. Patient contact tasks, documentation prompts, and caseload management are handled in the Case Manager and Admin dashboards respectively.</div>

  <div class="section">
    <div class="sec-title">Escalation queue — enter pending cases</div>
    <div class="sec-desc">Enter details of patients escalated by case managers for clinical review. The dashboard surfaces what requires your decision today.</div>
    <div class="input-grid">
      <div class="input-block"><label>Patient ID / initials</label><input id="cons-pat-id" placeholder="e.g. PT-042"/></div>
      <div class="input-block"><label>Days since case manager escalated</label><input id="cons-days-esc" type="number" min="0" placeholder="e.g. 2"/></div>
      <div class="input-block"><label>Reason for escalation</label>
        <select id="cons-reason"><option value="">— select —</option>
          <option value="risk-elevated">Risk level elevated since discharge</option>
          <option value="missed-appt">Repeated missed appointments</option>
          <option value="unreachable">Patient unreachable — multiple attempts</option>
          <option value="crisis-concern">Case manager has crisis concern</option>
          <option value="reassessment">Scheduled risk reassessment due</option>
        </select></div>
      <div class="input-block"><label>Last documented risk level</label>
        <select id="cons-risk"><option value="">— select —</option>
          <option value="high">High</option><option value="medium">Medium</option><option value="low">Low</option><option value="unknown">Unknown / not recorded</option>
        </select></div>
      <div class="input-block"><label>Days since last clinical review</label><input id="cons-days-review" type="number" min="0" placeholder="e.g. 8"/></div>
      <div class="input-block"><label>Risk reassessment tool completed?</label>
        <select id="cons-tool"><option value="">— select —</option>
          <option value="yes">Yes — PHQ-9 / WHODAS 2.0 completed</option>
          <option value="no">No — not yet completed</option>
          <option value="partial">Partial — incomplete data</option>
        </select></div>
    </div>
    <button class="run-btn run-btn-cons" onclick="runCons()">Review escalation</button>
  </div>

  <div id="cons-output" style="display:none">
    <div class="section">
      <div class="sec-title">Clinical review alerts</div>
      <div id="cons-alerts"></div>
    </div>
    <div class="section">
      <div class="sec-title">Escalation summary</div>
      <div id="cons-metrics"></div>
    </div>
    <div class="section">
      <div class="sec-title">Clinical decisions required — today</div>
      <div class="sec-desc">These are decisions only the Consultant can make. Each item was escalated by the Case Manager and requires your review and documented response.</div>
      <div id="cons-actions"></div>
    </div>
    <div class="section">
      <div class="sec-title">Clinical review documentation</div>
      <div class="sec-desc">Document your clinical decisions below. Case Manager will be notified of outcomes to update coordination plan.</div>
      <div class="doc-row"><div class="doc-label">Risk reassessment outcome</div><input class="doc-input" placeholder="Current risk level, tool used, score…"/></div>
      <div class="doc-row"><div class="doc-label">Clinical decision made</div><input class="doc-input" placeholder="Medication review / Mobile Crisis Team / admission / continue monitoring…"/></div>
      <div class="doc-row"><div class="doc-label">Escalation acknowledged</div><input class="doc-input" placeholder="Date/time acknowledged, response sent to CM…"/></div>
      <div class="doc-row"><div class="doc-label">Next clinical review date</div><input class="doc-input" placeholder="Scheduled date and responsible clinician…"/></div>
      <button class="save-btn" onclick="alert('Clinical note saved.\n\nNotify Case Manager of your decision and transfer to EHR.')">Save clinical note</button>
    </div>
  </div>
</div>

<!-- ══════════════════════════════════════════════════════════════
     PANEL 3 — ADMIN
══════════════════════════════════════════════════════════════ -->
<div class="panel" id="panel-admin">
  <div class="top-bar">
    <div><h1 style="color:#7B3FA0">Admin Dashboard</h1>
    <div class="subtitle">Team caseload, workload trends, and capacity management</div></div>
    <span class="badge badge-gray" id="admin-badge">No data entered</span>
  </div>
  <div class="role-note" style="background:#F5EEFA;border-left-color:#7B3FA0;color:#5C2E7A">This view shows aggregated team metrics for resource planning. Individual patient details and clinical decisions are handled in the Case Manager and Consultant dashboards.</div>

  <div class="section">
    <div class="sec-title">Team workload inputs</div>
    <div class="sec-desc">Enter current team figures to generate capacity indicators and flag resource risks.</div>
    <div class="input-grid">
      <div class="input-block"><label>Number of active case managers</label><input id="adm-cms" type="number" min="0" placeholder="e.g. 6"/></div>
      <div class="input-block"><label>Total active patients (team)</label><input id="adm-total" type="number" min="0" placeholder="e.g. 180"/></div>
      <div class="input-block"><label>Total high-risk patients (team)</label><input id="adm-highrisk" type="number" min="0" placeholder="e.g. 48"/></div>
      <div class="input-block"><label>Overdue coordination actions (team)</label><input id="adm-overdue" type="number" min="0" placeholder="e.g. 12"/></div>
      <div class="input-block"><label>Escalations raised this week</label><input id="adm-esc" type="number" min="0" placeholder="e.g. 7"/></div>
      <div class="input-block"><label>Escalations acknowledged this week</label><input id="adm-esc-ack" type="number" min="0" placeholder="e.g. 5"/></div>
      <div class="input-block"><label>Missed appointments this week (team)</label><input id="adm-missed" type="number" min="0" placeholder="e.g. 9"/></div>
      <div class="input-block"><label>Post-discharge contacts overdue (team)</label><input id="adm-dis-overdue" type="number" min="0" placeholder="e.g. 4"/></div>
    </div>
    <button class="run-btn run-btn-admin" onclick="runAdmin()">Generate capacity report</button>
  </div>

  <div id="admin-output" style="display:none">
    <div class="section">
      <div class="sec-title">Team capacity at a glance</div>
      <div class="stat-grid" id="adm-stat-grid"></div>
    </div>
    <div class="section">
      <div class="sec-title">Capacity and workload alerts</div>
      <div id="adm-alerts"></div>
    </div>
    <div class="section">
      <div class="sec-title">Workload trend indicators</div>
      <div id="adm-metrics"></div>
    </div>
    <div class="section">
      <div class="sec-title">Recommended management actions</div>
      <div class="sec-desc">These are resource and capacity decisions for Admin and team leads — not patient-specific actions. Individual patient follow-up remains with Case Managers.</div>
      <div id="adm-actions"></div>
    </div>
  </div>
</div>

<script>
/* ── tab switching ── */
function switchRole(role){
  ['cm','cons','admin'].forEach(r=>{
    document.getElementById('panel-'+r).classList.remove('active');
    document.getElementById('tab-'+r).classList.remove('active-cm','active-cons','active-admin');
  });
  document.getElementById('panel-'+role).classList.add('active');
  document.getElementById('tab-'+role).classList.add('active-'+role);
}

/* ── helpers ── */
function pill(cls,txt){return `<span class="pill pill-${cls}">${txt}</span>`;}
function alertCard(lvl,title,body,action,actionCls=''){
  return `<div class="alert-card alert-${lvl}">
    <div class="alert-title">${title}</div>
    <div class="alert-body">${body}</div>
    ${action?`<span class="alert-action ${actionCls}">→ ${action}</span>`:''}
  </div>`;
}
function metricRow(label,val,pillCls,pillTxt){
  return `<div class="metric-row">
    <span class="metric-label">${label}</span>
    <span style="display:flex;align-items:center;gap:8px">
      <span class="metric-val">${val}</span>
      ${pill(pillCls,pillTxt)}
    </span>
  </div>`;
}
function actionItem(n,text){
  return `<div class="action-item"><span class="action-num">${n}.</span><span>${text}</span></div>`;
}
function empty(){return '<div class="empty">No items at this time.</div>';}

/* ══════════════════════════════════════════════════════════════
   CASE MANAGER LOGIC
══════════════════════════════════════════════════════════════ */
function runCM(){
  const id      = document.getElementById('cm-pat-id').value.trim()||'Patient';
  const days    = parseInt(document.getElementById('cm-days-dis').value)||0;
  const contact = document.getElementById('cm-contact').value;
  const appt    = document.getElementById('cm-appt').value;
  const carer   = document.getElementById('cm-carer').value;
  const esc     = document.getElementById('cm-esc').value;

  let urgency='green'; const alerts=[],actions=[],metrics=[];
  const flag=l=>{if(l==='red')urgency='red';else if(l==='amber'&&urgency!=='red')urgency='amber';};

  /* post-discharge contact */
  if(contact==='no'&&days>0){
    const crit=days>=3; flag(crit?'red':'amber');
    alerts.push(alertCard(crit?'red':'amber',
      crit?`No post-discharge contact — ${days} days elapsed`:`Contact window closing — ${days} day${days!==1?'s':''} since discharge`,
      crit?'NICE NG53 requires contact within 72 hours of discharge. This window has passed with no documented attempt. Patient is in an unmonitored gap.':'The 72-hour post-discharge contact window closes soon.',
      'Attempt contact now and document outcome in EHR before end of shift.'));
    actions.push(`Contact ${id} immediately — no post-discharge outreach after ${days} day${days!==1?'s':''}.`);
  } else if(contact==='yes'){
    alerts.push(alertCard('green','Post-discharge contact documented','Outreach recorded. Confirm risk status, medication, and caregiver support were assessed during contact.',''));
  }

  /* appointment */
  if(appt==='missed-none'){
    flag('red');
    alerts.push(alertCard('red','Missed appointment — no outreach documented',
      'A missed appointment with no same-day outreach is a silent failure. SAMHSA requires this to trigger an escalation pathway. If outreach fails after 2 attempts within 48 hours, escalate to Consultant — do not decide independently on clinical risk.',
      `Begin outreach for ${id} now. Document each attempt with timestamp. After 2 failed attempts, escalate to Consultant for review.`));
    actions.push(`Missed appointment for ${id}: attempt outreach now, document timestamps, escalate to Consultant if unreachable after 2 attempts.`);
  } else if(appt==='missed-out'){
    flag('amber');
    alerts.push(alertCard('amber','Missed appointment — outreach in progress','Outreach attempted. Document outcome and confirm next appointment date.',
      'Record outreach outcome and set next appointment.'));
  } else if(appt==='attended'){
    alerts.push(alertCard('green','Follow-up appointment attended','Patient attended. Confirm standardised risk tool was completed (PHQ-9 / WHODAS 2.0) and care plan reviewed.',''));
  }

  /* caregiver */
  if(carer==='no'&&days>=1){
    const crit=days>=3; flag(crit?'red':'amber');
    alerts.push(alertCard(crit?'red':'amber','Caregiver not yet contacted post-discharge',
      'NICE NG53 requires carers to be formally informed. Without this, the patient\'s home safety net is inactive.',
      `Contact caregiver for ${id} today. Brief on warning signs and provide crisis contact number.`));
    actions.push(`Caregiver contact for ${id}: brief on warning signs, confirm they have the crisis line, document.`);
  }

  /* escalation */
  if(esc==='yes-pend'){
    flag('red');
    alerts.push(alertCard('red','Escalation to Consultant — awaiting acknowledgement',
      'An unacknowledged escalation means the clinical handover is incomplete. SAMHSA requires documented acknowledgement from the receiving role.',
      'Follow up with Consultant today. If no response within 4 hours, re-escalate to supervisor and document.'));
    actions.push('Chase Consultant acknowledgement of escalation — document timestamp of follow-up.');
  } else if(esc==='no-unclear'){
    flag('amber');
    alerts.push(alertCard('amber','Escalation need unclear',
      'Review patient status against pre-agreed risk thresholds. If threshold is met, escalate to Consultant — do not defer the decision.',
      'Review risk thresholds and determine whether to escalate to Consultant before end of shift.'));
  }

  /* metrics */
  metrics.push(metricRow('Days since discharge', days>0?days+'d':'—', days>=3&&contact==='no'?'red':days>0?'green':'gray', days>=3&&contact==='no'?'Flag':'OK'));
  metrics.push(metricRow('Post-discharge contact', contact==='yes'?'Documented':contact==='no'?'Not made':'—', contact==='no'?'red':contact==='yes'?'green':'gray', contact==='no'?'Flag':contact==='yes'?'OK':'—'));
  metrics.push(metricRow('Follow-up appointment', appt==='attended'?'Attended':appt==='missed-none'?'Missed — no response':appt==='missed-out'?'Missed — outreach done':appt==='upcoming'?'Upcoming':'—',
    appt==='missed-none'?'red':appt==='missed-out'?'amber':appt==='attended'?'green':'gray',
    appt==='missed-none'?'Flag':appt==='missed-out'?'Watch':appt==='attended'?'OK':'—'));
  metrics.push(metricRow('Caregiver contacted', carer==='yes'?'Yes':carer==='no'?'No':carer==='na'?'N/A':'—', carer==='no'?'amber':carer==='yes'?'green':'gray', carer==='no'?'Watch':carer==='yes'?'OK':'—'));
  metrics.push(metricRow('Escalation to consultant', esc==='yes-ack'?'Acknowledged':esc==='yes-pend'?'Pending':esc==='no-needed'?'Not needed':esc==='no-unclear'?'Unclear':'—',
    esc==='yes-pend'?'red':esc==='no-unclear'?'amber':esc==='yes-ack'?'green':'gray',
    esc==='yes-pend'?'Flag':esc==='no-unclear'?'Watch':esc==='yes-ack'?'OK':'—'));

  document.getElementById('cm-alerts').innerHTML   = alerts.length?alerts.join(''):empty();
  document.getElementById('cm-metrics').innerHTML  = metrics.join('');
  document.getElementById('cm-actions').innerHTML  = actions.length?actions.map((a,i)=>actionItem(i+1,a)).join(''):empty();
  const b=document.getElementById('cm-badge');
  b.textContent=urgency==='red'?'Urgent gaps detected':urgency==='amber'?'Review required':'All checks clear';
  b.className='badge '+(urgency==='red'?'badge-red':urgency==='amber'?'badge-amber':'badge-green');
  document.getElementById('cm-output').style.display='block';
  document.getElementById('cm-output').scrollIntoView({behavior:'smooth',block:'start'});
}

/* ══════════════════════════════════════════════════════════════
   CONSULTANT LOGIC
══════════════════════════════════════════════════════════════ */
function runCons(){
  const id       = document.getElementById('cons-pat-id').value.trim()||'Patient';
  const daysEsc  = parseInt(document.getElementById('cons-days-esc').value)||0;
  const reason   = document.getElementById('cons-reason').value;
  const risk     = document.getElementById('cons-risk').value;
  const daysRev  = parseInt(document.getElementById('cons-days-review').value)||0;
  const tool     = document.getElementById('cons-tool').value;

  let urgency='green'; const alerts=[],actions=[],metrics=[];
  const flag=l=>{if(l==='red')urgency='red';else if(l==='amber'&&urgency!=='red')urgency='amber';};

  /* escalation overdue */
  if(daysEsc>=1){
    const crit=daysEsc>=1; flag(crit?'red':'amber');
    alerts.push(alertCard('red',`Escalation awaiting review — ${daysEsc} day${daysEsc!==1?'s':''} pending`,
      'Case Manager has escalated this patient for clinical review. An unacknowledged escalation leaves the coordination loop open and the patient without a clinical decision.',
      `Review escalation for ${id} today and send acknowledgement to Case Manager.`,'alert-action-cons'));
    actions.push(`Acknowledge and review escalation for ${id} — Case Manager is waiting on your clinical decision.`);
  }

  /* reassessment tool */
  if(tool==='no'||tool==='partial'){
    flag('red');
    alerts.push(alertCard('red','Standardised risk reassessment not completed',
      tool==='no'?'No PHQ-9 or WHODAS 2.0 has been completed since escalation. A clinical decision cannot be safely made without current, structured risk data.':'Risk reassessment data is incomplete. Ensure full standardised tool is completed before deciding on intervention level.',
      `Complete or request completion of PHQ-9 / WHODAS 2.0 for ${id} before making a clinical decision.`,'alert-action-cons'));
    actions.push(`Request or complete standardised risk assessment (PHQ-9 / WHODAS 2.0) for ${id} before clinical review.`);
  } else if(tool==='yes'){
    alerts.push(alertCard('green','Standardised risk tool completed','PHQ-9 / WHODAS 2.0 data available. Use this as the basis for your clinical decision.',''));
  }

  /* risk level */
  if(risk==='high'){
    flag('red');
    alerts.push(alertCard('red',`Last recorded risk level: HIGH — ${id}`,
      'Patient was last recorded as high-risk. Clinical review and documented decision on intervention level is required today. Consider: Mobile Crisis Team activation, medication review, or admission assessment.',
      'Make and document a clinical decision today. Notify Case Manager of outcome and next clinical review date.','alert-action-cons'));
    actions.push(`High-risk patient ${id}: make a clinical decision today (Mobile Crisis Team / medication review / admission assessment) and notify Case Manager.`);
  } else if(risk==='unknown'){
    flag('red');
    alerts.push(alertCard('red','Risk level not recorded — assessment required',
      'No documented risk level exists for this patient. A clinical assessment is required before any coordination decision can be made.',
      `Conduct and document a risk assessment for ${id} today.`,'alert-action-cons'));
    actions.push(`No recorded risk level for ${id}: conduct formal risk assessment and document before end of shift.`);
  } else if(risk==='medium'){
    flag('amber');
    alerts.push(alertCard('amber',`Last recorded risk level: MEDIUM — ${id}`,
      'Patient is at medium risk. Review whether current care plan is adequate given the reason for escalation.',
      'Review care plan and escalation reason. Document whether intervention level needs to change.','alert-action-cons'));
  }

  /* clinical review overdue */
  if(daysRev>7){
    flag('red');
    alerts.push(alertCard('red',`Clinical review overdue — ${daysRev} days since last review`,
      'Over 7 days without a documented clinical review for an escalated patient. Risk status may have changed significantly.',
      `Conduct clinical review for ${id} today and document outcome.`,'alert-action-cons'));
    actions.push(`Clinical review overdue for ${id}: conduct and document review today, set next review date.`);
  }

  /* reason-specific alerts */
  const reasonLabels={'risk-elevated':'Reason: risk level elevated since discharge','missed-appt':'Reason: repeated missed appointments','unreachable':'Reason: patient unreachable after multiple attempts','crisis-concern':'Reason: Case Manager has crisis concern','reassessment':'Reason: scheduled risk reassessment due'};
  if(reason){
    alerts.push(alertCard('amber',reasonLabels[reason]||'',
      reason==='crisis-concern'?'Case Manager has raised a specific crisis concern — this requires your immediate clinical attention, not just acknowledgement.':
      reason==='unreachable'?'Patient has not responded to multiple Case Manager contact attempts. Consider whether Mobile Crisis Team outreach or emergency escalation is warranted.':
      'Review the Case Manager\'s documented reason for escalation before making your clinical decision.',
      '',''));
  }

  metrics.push(metricRow('Days since escalation raised', daysEsc>0?daysEsc+'d':'—', daysEsc>=1?'red':'green', daysEsc>=1?'Flag':'OK'));
  metrics.push(metricRow('Last recorded risk level', risk||'—', risk==='high'||risk==='unknown'?'red':risk==='medium'?'amber':risk==='low'?'green':'gray', risk==='high'||risk==='unknown'?'Flag':risk==='medium'?'Watch':risk==='low'?'OK':'—'));
  metrics.push(metricRow('Risk tool completed', tool==='yes'?'Yes':tool==='no'?'No':tool==='partial'?'Partial':'—', tool==='no'||tool==='partial'?'red':tool==='yes'?'green':'gray', tool==='no'?'Flag':tool==='partial'?'Watch':tool==='yes'?'OK':'—'));
  metrics.push(metricRow('Days since last clinical review', daysRev>0?daysRev+'d':'—', daysRev>7?'red':daysRev>=5?'amber':daysRev>0?'green':'gray', daysRev>7?'Flag':daysRev>=5?'Watch':daysRev>0?'OK':'—'));

  document.getElementById('cons-alerts').innerHTML  = alerts.length?alerts.join(''):empty();
  document.getElementById('cons-metrics').innerHTML = metrics.join('');
  document.getElementById('cons-actions').innerHTML = actions.length?actions.map((a,i)=>actionItem(i+1,a)).join(''):empty();
  const b=document.getElementById('cons-badge');
  b.textContent=urgency==='red'?'Review required today':urgency==='amber'?'Action needed':'No urgent items';
  b.className='badge '+(urgency==='red'?'badge-red':urgency==='amber'?'badge-amber':'badge-green');
  document.getElementById('cons-output').style.display='block';
  document.getElementById('cons-output').scrollIntoView({behavior:'smooth',block:'start'});
}

/* ══════════════════════════════════════════════════════════════
   ADMIN LOGIC
══════════════════════════════════════════════════════════════ */
function runAdmin(){
  const cms        = parseInt(document.getElementById('adm-cms').value)||0;
  const total      = parseInt(document.getElementById('adm-total').value)||0;
  const highrisk   = parseInt(document.getElementById('adm-highrisk').value)||0;
  const overdue    = parseInt(document.getElementById('adm-overdue').value)||0;
  const esc        = parseInt(document.getElementById('adm-esc').value)||0;
  const escAck     = parseInt(document.getElementById('adm-esc-ack').value)||0;
  const missed     = parseInt(document.getElementById('adm-missed').value)||0;
  const disOverdue = parseInt(document.getElementById('adm-dis-overdue').value)||0;

  let urgency='green'; const alerts=[],actions=[],metrics=[];
  const flag=l=>{if(l==='red')urgency='red';else if(l==='amber'&&urgency!=='red')urgency='amber';};

  const avgCaseload = cms>0?Math.round(total/cms):0;
  const hrRatio     = total>0?Math.round((highrisk/total)*100):0;
  const escUnackPct = esc>0?Math.round(((esc-escAck)/esc)*100):0;

  /* average caseload */
  if(avgCaseload>40){
    flag('red');
    alerts.push(alertCard('red',`Average caseload critical — ${avgCaseload} patients per CM`,
      'Average caseload exceeds 40 patients per Case Manager. Time-sensitive coordination steps (72-hour follow-up, missed appointment outreach) are at high risk of being missed across the team.',
      'Convene team lead meeting today to review caseload distribution and consider reallocation or additional resource.'));
    actions.push('Average caseload is critically high — convene team lead meeting and review caseload distribution today.');
  } else if(avgCaseload>30){
    flag('amber');
    alerts.push(alertCard('amber',`Average caseload elevated — ${avgCaseload} patients per CM`,
      'Caseload is approaching a level where capacity to complete all time-sensitive coordination steps becomes strained.',
      'Monitor closely. Consider whether any cases can be redistributed before caseload increases further.'));
  }

  /* high-risk ratio */
  if(hrRatio>=40){
    flag('red');
    alerts.push(alertCard('red',`Team high-risk ratio critical — ${hrRatio}% of caseload`,
      'Over 40% of the team\'s active patients are flagged as high-risk. This significantly increases the volume of time-sensitive coordination tasks per Case Manager.',
      'Review whether high-risk designations are current and accurate. Consider escalating resource needs to management.'));
    actions.push(`High-risk ratio is ${hrRatio}% — flag to management and review whether resource increase is needed.`);
  } else if(hrRatio>=30){
    flag('amber');
    alerts.push(alertCard('amber',`High-risk ratio elevated — ${hrRatio}%`,
      'High-risk patient proportion is approaching the 30% threshold. Monitor for signs that CMs are unable to complete all priority coordination tasks.',
      'Review overdue coordination data and flag to team leads if trend continues.'));
  }

  /* overdue coordination */
  if(overdue>0){
    const crit=overdue>=10; flag(crit?'red':'amber');
    alerts.push(alertCard(crit?'red':'amber',`Overdue coordination actions — ${overdue} team-wide`,
      crit?`${overdue} coordination actions are overdue across the team. At this volume, some high-risk patients are likely in unmonitored gaps right now.`:`${overdue} coordination actions are overdue. Review which Case Managers are affected and whether workload rebalancing is needed.`,
      'Identify which CMs have the highest overdue volumes and prioritise support or reallocation today.'));
    actions.push(`${overdue} overdue coordination actions team-wide: identify affected CMs and review workload distribution.`);
  }

  /* unacknowledged escalations */
  const unack=esc-escAck;
  if(unack>0){
    flag(unack>=3?'red':'amber');
    alerts.push(alertCard(unack>=3?'red':'amber',`Unacknowledged escalations — ${unack} pending`,
      `${unack} escalation${unack!==1?'s':''} raised by Case Managers have not been acknowledged by Consultants. Open escalation loops mean patients are waiting for clinical decisions.`,
      'Alert Consultant team to pending escalations. Escalation acknowledgement rate this week: '+(esc>0?Math.round((escAck/esc)*100):0)+'%.'));
    actions.push(`${unack} unacknowledged escalation${unack!==1?'s':''}: alert Consultant team and track acknowledgement rate.`);
  }

  /* missed appointments trend */
  if(missed>=10){
    flag('amber');
    alerts.push(alertCard('amber',`High missed appointment volume — ${missed} this week`,
      'A high volume of missed appointments may indicate systemic barriers (transport, communication, patient disengagement) rather than individual case failures. This is a service-level signal.',
      'Review missed appointment data by CM and patient profile. Consider systemic barrier assessment.'));
    actions.push(`${missed} missed appointments this week: review for systemic patterns and consider barrier assessment.`);
  }

  /* post-discharge overdue */
  if(disOverdue>0){
    flag('red');
    alerts.push(alertCard('red',`Post-discharge contacts overdue — ${disOverdue} patients`,
      `${disOverdue} patient${disOverdue!==1?'s':''} discharged from ES have no documented contact attempt within 72 hours. These patients are in unmonitored gaps right now — this is the highest-risk coordination failure the team can experience.`,
      'Alert affected Case Managers immediately. Escalate to team lead if CM capacity is the barrier.'));
    actions.push(`${disOverdue} post-discharge contacts overdue: alert affected CMs immediately and escalate to team lead if needed.`);
  }

  /* stat cards */
  document.getElementById('adm-stat-grid').innerHTML = [
    {num:avgCaseload||'—',label:'Avg patients per CM',color:avgCaseload>40?'#FCEBEB':avgCaseload>30?'#FAEEDA':'#E1F5EE',tc:avgCaseload>40?'#A32D2D':avgCaseload>30?'#633806':'#0F6E56'},
    {num:hrRatio>0?hrRatio+'%':'—',label:'Team high-risk ratio',color:hrRatio>=40?'#FCEBEB':hrRatio>=30?'#FAEEDA':'#E1F5EE',tc:hrRatio>=40?'#A32D2D':hrRatio>=30?'#633806':'#0F6E56'},
    {num:overdue||'0',label:'Overdue coordination actions',color:overdue>=10?'#FCEBEB':overdue>0?'#FAEEDA':'#E1F5EE',tc:overdue>=10?'#A32D2D':overdue>0?'#633806':'#0F6E56'},
    {num:unack||'0',label:'Unacknowledged escalations',color:unack>=3?'#FCEBEB':unack>0?'#FAEEDA':'#E1F5EE',tc:unack>=3?'#A32D2D':unack>0?'#633806':'#0F6E56'},
    {num:disOverdue||'0',label:'Post-discharge contacts overdue',color:disOverdue>0?'#FCEBEB':'#E1F5EE',tc:disOverdue>0?'#A32D2D':'#0F6E56'},
    {num:missed||'0',label:'Missed appointments this week',color:missed>=10?'#FAEEDA':'#F1EFE8',tc:missed>=10?'#633806':'#5F5E5A'},
  ].map(s=>`<div class="stat-card" style="background:${s.color};border-color:${s.color}"><div class="stat-num" style="color:${s.tc}">${s.num}</div><div class="stat-label">${s.label}</div></div>`).join('');

  metrics.push(metricRow('Total active patients', total>0?total:'—', 'gray','—'));
  metrics.push(metricRow('Active case managers', cms>0?cms:'—', 'gray','—'));
  metrics.push(metricRow('Average caseload per CM', avgCaseload>0?avgCaseload:'—', avgCaseload>40?'red':avgCaseload>30?'amber':'green', avgCaseload>40?'Flag':avgCaseload>30?'Watch':'OK'));
  metrics.push(metricRow('High-risk ratio (team)', hrRatio>0?hrRatio+'%':'—', hrRatio>=40?'red':hrRatio>=30?'amber':'green', hrRatio>=40?'Flag':hrRatio>=30?'Watch':'OK'));
  metrics.push(metricRow('Escalation acknowledgement rate', esc>0?Math.round((escAck/esc)*100)+'%':'—', esc>0&&escAck<esc?'amber':esc>0?'green':'gray', esc>0&&escAck<esc?'Watch':'OK'));
  metrics.push(metricRow('Post-discharge contacts overdue', disOverdue>0?disOverdue:'0', disOverdue>0?'red':'green', disOverdue>0?'Flag':'OK'));

  document.getElementById('adm-alerts').innerHTML  = alerts.length?alerts.join(''):empty();
  document.getElementById('adm-metrics').innerHTML = metrics.join('');
  document.getElementById('adm-actions').innerHTML = actions.length?actions.map((a,i)=>actionItem(i+1,a)).join(''):empty();
  const b=document.getElementById('admin-badge');
  b.textContent=urgency==='red'?'Capacity risk detected':urgency==='amber'?'Trends to monitor':'Team within capacity';
  b.className='badge '+(urgency==='red'?'badge-red':urgency==='amber'?'badge-amber':'badge-green');
  document.getElementById('admin-output').style.display='block';
  document.getElementById('admin-output').scrollIntoView({behavior:'smooth',block:'start'});
}
</script>
</body>
</html>
