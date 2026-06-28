[care_coordination_dashboard.html](https://github.com/user-attachments/files/29432815/care_coordination_dashboard.html)
# Care_Coordination_Dashboard<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:var(--font-sans,Arial,sans-serif);background:transparent;padding:16px;color:var(--color-text-primary)}
h2{font-size:16px;font-weight:500;margin-bottom:12px}
.top-bar{display:flex;align-items:center;justify-content:space-between;margin-bottom:16px;flex-wrap:wrap;gap:8px}
.top-bar h1{font-size:18px;font-weight:500}
.badge{font-size:11px;padding:3px 10px;border-radius:12px;font-weight:500}
.badge-red{background:var(--color-background-danger);color:var(--color-text-danger)}
.badge-amber{background:#FAEEDA;color:#633806}
.badge-green{background:var(--color-background-success);color:var(--color-text-success)}
.badge-gray{background:var(--color-background-secondary);color:var(--color-text-secondary)}
.section{background:var(--color-background-secondary);border-radius:12px;padding:14px 16px;margin-bottom:12px;border:1px solid var(--color-border-tertiary)}
.sec-title{font-size:12px;font-weight:500;text-transform:uppercase;letter-spacing:.05em;color:var(--color-text-secondary);margin-bottom:10px}
.input-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.input-block{display:flex;flex-direction:column;gap:4px}
.input-block label{font-size:12px;color:var(--color-text-secondary)}
.input-block input,.input-block select{padding:7px 10px;border-radius:8px;border:1px solid var(--color-border-primary);background:var(--color-background-primary);color:var(--color-text-primary);font-size:13px;font-family:inherit;width:100%}
.run-btn{width:100%;padding:10px;border-radius:8px;background:#1F4E79;color:#fff;border:none;font-size:13px;font-weight:500;cursor:pointer;margin-top:10px;font-family:inherit}
.run-btn:hover{background:#163A5A}
.alert-card{border-radius:8px;padding:10px 12px;margin-bottom:8px;border-left:3px solid}
.alert-red{background:var(--color-background-danger);border-color:var(--color-text-danger)}
.alert-amber{background:#FAEEDA;border-color:#EF9F27}
.alert-green{background:var(--color-background-success);border-color:var(--color-text-success)}
.alert-title{font-size:13px;font-weight:500;margin-bottom:2px}
.alert-body{font-size:12px;color:var(--color-text-secondary);line-height:1.5}
.alert-action{font-size:11px;font-weight:500;margin-top:5px;padding:3px 8px;border-radius:6px;display:inline-block;background:var(--color-background-primary);border:1px solid var(--color-border-secondary)}
.metric-row{display:flex;justify-content:space-between;align-items:center;padding:7px 0;border-bottom:1px solid var(--color-border-tertiary);font-size:13px}
.metric-row:last-child{border-bottom:none}
.metric-label{color:var(--color-text-secondary);font-size:12px}
.metric-val{font-weight:500}
.pill{font-size:10px;padding:2px 7px;border-radius:8px;font-weight:500}
.pill-red{background:var(--color-background-danger);color:var(--color-text-danger)}
.pill-amber{background:#FAEEDA;color:#633806}
.pill-green{background:var(--color-background-success);color:var(--color-text-success)}
.patient-row{display:flex;align-items:center;gap:8px;padding:8px 0;border-bottom:1px solid var(--color-border-tertiary);font-size:13px}
.patient-row:last-child{border-bottom:none}
.pat-name{font-weight:500;min-width:80px}
.pat-tags{display:flex;gap:4px;flex-wrap:wrap;flex:1}
.pat-action{font-size:11px;color:var(--color-text-secondary);text-align:right;min-width:110px}
.doc-row{display:flex;gap:8px;align-items:flex-start;padding:8px 0;border-bottom:1px solid var(--color-border-tertiary)}
.doc-row:last-child{border-bottom:none}
.doc-label{font-size:12px;font-weight:500;min-width:120px;color:var(--color-text-secondary)}
.doc-input{flex:1;padding:5px 8px;border-radius:6px;border:1px solid var(--color-border-primary);background:var(--color-background-primary);color:var(--color-text-primary);font-size:12px;font-family:inherit}
.save-btn{width:100%;padding:8px;border-radius:8px;background:var(--color-background-secondary);border:1px solid var(--color-border-primary);color:var(--color-text-primary);font-size:12px;cursor:pointer;margin-top:8px;font-family:inherit}
.save-btn:hover{background:var(--color-background-tertiary)}
.empty{color:var(--color-text-tertiary);font-size:13px;font-style:italic;padding:8px 0}
#output{display:none}
.capacity-bar-bg{height:8px;border-radius:4px;background:var(--color-border-tertiary);overflow:hidden;flex:1}
.capacity-bar-fill{height:100%;border-radius:4px;transition:width .4s}
</style>

<div class="top-bar">
  <h1>Care coordination dashboard</h1>
  <span class="badge badge-gray" id="cm-badge">No data entered</span>
</div>

<div class="section">
  <div class="sec-title">Patient intake — enter case details</div>
  <div class="input-grid">
    <div class="input-block">
      <label>Patient ID / initials</label>
      <input id="pat-id" placeholder="e.g. PT-042" />
    </div>
    <div class="input-block">
      <label>Days since ES discharge</label>
      <input id="days-discharge" type="number" min="0" placeholder="e.g. 4" />
    </div>
    <div class="input-block">
      <label>Contact attempted post-discharge?</label>
      <select id="contact-made">
        <option value="">— select —</option>
        <option value="yes">Yes — documented</option>
        <option value="no">No</option>
      </select>
    </div>
    <div class="input-block">
      <label>Follow-up appointment status</label>
      <select id="appt-status">
        <option value="">— select —</option>
        <option value="attended">Attended</option>
        <option value="missed-responded">Missed — outreach done</option>
        <option value="missed-no-response">Missed — no outreach yet</option>
        <option value="upcoming">Upcoming</option>
      </select>
    </div>
    <div class="input-block">
      <label>Caregiver contacted post-discharge?</label>
      <select id="carer-contact">
        <option value="">— select —</option>
        <option value="yes">Yes — documented</option>
        <option value="no">No</option>
        <option value="na">No caregiver identified</option>
      </select>
    </div>
    <div class="input-block">
      <label>Days since consultant last updated</label>
      <input id="days-consultant" type="number" min="0" placeholder="e.g. 6" />
    </div>
    <div class="input-block">
      <label>Escalation raised this week?</label>
      <select id="escalation">
        <option value="">— select —</option>
        <option value="yes-ack">Yes — acknowledged</option>
        <option value="yes-pending">Yes — awaiting acknowledgement</option>
        <option value="no-needed">No — not needed</option>
        <option value="no-unknown">No — unclear if needed</option>
      </select>
    </div>
    <div class="input-block">
      <label>Active caseload size</label>
      <input id="caseload" type="number" min="0" placeholder="e.g. 34" />
    </div>
  </div>
  <div class="input-block" style="margin-top:10px">
    <label>High-risk patients in caseload</label>
    <input id="high-risk-count" type="number" min="0" placeholder="e.g. 11" />
  </div>
  <button class="run-btn" onclick="runDashboard()">Run coordination check</button>
</div>

<div id="output">
  <div class="section">
    <div class="sec-title">Priority alerts</div>
    <div id="alerts-container"><div class="empty">No alerts generated yet.</div></div>
  </div>

  <div class="section">
    <div class="sec-title">Metric summary</div>
    <div id="metrics-container"></div>
  </div>

  <div class="section">
    <div class="sec-title">Caseload capacity indicator</div>
    <div style="display:flex;align-items:center;gap:10px;font-size:13px">
      <span style="min-width:110px;color:var(--color-text-secondary)">High-risk ratio</span>
      <div class="capacity-bar-bg"><div class="capacity-bar-fill" id="cap-bar" style="width:0%"></div></div>
      <span id="cap-pct" style="min-width:40px;font-weight:500;text-align:right"></span>
    </div>
    <div id="cap-note" style="font-size:12px;color:var(--color-text-secondary);margin-top:6px"></div>
  </div>

  <div class="section">
    <div class="sec-title">Recommended actions — today</div>
    <div id="actions-container"></div>
  </div>

  <div class="section">
    <div class="sec-title">Documentation prompt</div>
    <div class="doc-row"><div class="doc-label">Contact attempt</div><input class="doc-input" placeholder="Date, method, outcome…" /></div>
    <div class="doc-row"><div class="doc-label">Appointment outcome</div><input class="doc-input" placeholder="Attended / missed / rescheduled…" /></div>
    <div class="doc-row"><div class="doc-label">Caregiver contact</div><input class="doc-input" placeholder="Who was contacted, what was shared…" /></div>
    <div class="doc-row"><div class="doc-label">Escalation note</div><input class="doc-input" placeholder="Raised to whom, on what date, outcome…" /></div>
    <div class="doc-row"><div class="doc-label">Consultant update</div><input class="doc-input" placeholder="Date, content, response…" /></div>
    <div class="doc-row"><div class="doc-label">Next action planned</div><input class="doc-input" placeholder="What happens next and by when…" /></div>
    <button class="save-btn" onclick="alert('Documentation saved — remember to transfer this to your EHR.')">Save documentation note</button>
  </div>
</div>

<script>
function runDashboard(){
  const patId        = document.getElementById('pat-id').value || 'Patient';
  const daysDis      = parseInt(document.getElementById('days-discharge').value) || 0;
  const contactMade  = document.getElementById('contact-made').value;
  const apptStatus   = document.getElementById('appt-status').value;
  const carerContact = document.getElementById('carer-contact').value;
  const daysCons     = parseInt(document.getElementById('days-consultant').value) || 0;
  const escalation   = document.getElementById('escalation').value;
  const caseload     = parseInt(document.getElementById('caseload').value) || 0;
  const highRisk     = parseInt(document.getElementById('high-risk-count').value) || 0;

  const alerts = [];
  const actions = [];
  const metrics = [];
  let urgencyLevel = 'green';

  const flag = (lvl) => {
    if(lvl==='red') urgencyLevel='red';
    else if(lvl==='amber' && urgencyLevel!=='red') urgencyLevel='amber';
  };

  // ── Metric 1: post-discharge contact window ──
  if(contactMade === 'no' && daysDis > 0){
    const past72 = daysDis >= 3;
    flag(past72 ? 'red' : 'amber');
    alerts.push({
      lvl: past72 ? 'red' : 'amber',
      title: past72
        ? `⚠ No post-discharge contact — ${daysDis} days elapsed`
        : `Contact window closing — ${daysDis} day${daysDis!==1?'s':''} since discharge`,
      body: past72
        ? 'NICE NG53 requires contact within 72 hours of discharge. This window has passed with no documented attempt. Patient is in an unmonitored gap.'
        : 'Contact should be attempted today. The 72-hour window closes soon.',
      action: 'Attempt contact now. Document outcome (reached / not reached / left message) in EHR before end of shift.'
    });
    actions.push(`Contact ${patId} immediately — no post-discharge outreach documented after ${daysDis} day${daysDis!==1?'s':''}.`);
  } else if(contactMade === 'yes'){
    alerts.push({lvl:'green', title:'Post-discharge contact documented', body:'Outreach has been made and recorded. Confirm risk status was assessed during that contact.', action:''});
  }

  // ── Metric 2: missed appointment ──
  if(apptStatus === 'missed-no-response'){
    flag('red');
    alerts.push({
      lvl:'red',
      title:'Missed appointment — no outreach response documented',
      body:'A missed appointment with no same-day outreach is a silent failure point. SAMHSA identifies this as a pre-agreed escalation trigger. The patient has dropped from active monitoring.',
      action:'Attempt outreach today. If no response after 2 attempts within 48 hours, escalate to team lead and consider Mobile Crisis Team activation.'
    });
    actions.push(`Missed appointment for ${patId}: initiate outreach now and document each attempt with timestamp.`);
  } else if(apptStatus === 'missed-responded'){
    flag('amber');
    alerts.push({lvl:'amber', title:'Missed appointment — outreach in progress', body:'Outreach has been attempted. Ensure outcome is documented and next contact is scheduled.', action:'Confirm outreach outcome is recorded and a new appointment date is set.'});
  }

  // ── Metric 3: caregiver contact ──
  if(carerContact === 'no' && daysDis >= 1){
    flag(daysDis >= 3 ? 'red' : 'amber');
    alerts.push({
      lvl: daysDis >= 3 ? 'red' : 'amber',
      title: 'Caregiver not yet contacted post-discharge',
      body: 'NICE NG53 requires carers to be formally identified and informed. An uninformed caregiver cannot act as an early warning system at home.',
      action: `Contact identified caregiver for ${patId} today. Brief them on warning signs and provide crisis contact number.`
    });
    actions.push(`Caregiver contact for ${patId}: brief on warning signs, crisis line, and role in monitoring.`);
  }

  // ── Metric 4: consultant update ──
  if(daysCons > 5){
    flag('red');
    alerts.push({
      lvl:'red',
      title:`Consultant not updated — ${daysCons} days elapsed`,
      body:'Psychiatrist has not received a documented status update in over 5 days. If risk level has changed, clinical decisions may be based on outdated information.',
      action:`Send a status update to the treating consultant for ${patId} today. Document the update and any response.`
    });
    actions.push(`Consultant update overdue for ${patId}: send written status note today.`);
  } else if(daysCons >= 3 && daysCons <= 5){
    flag('amber');
    alerts.push({lvl:'amber', title:`Consultant update due soon — ${daysCons} days since last update`, body:'Plan to update the treating psychiatrist within the next 1–2 days.', action:'Schedule consultant update within 48 hours.'});
  }

  // ── Metric 5: escalation acknowledgement ──
  if(escalation === 'yes-pending'){
    flag('red');
    alerts.push({
      lvl:'red',
      title:'Escalation raised but not acknowledged',
      body:'An unacknowledged escalation is a silent failure — the handover is incomplete until the receiving role confirms receipt. SAMHSA requires warm hand-off with documented acknowledgement.',
      action:'Chase escalation acknowledgement now. If no response within 4 hours, re-escalate to supervisor.'
    });
    actions.push('Follow up on unacknowledged escalation — confirm receipt with receiving role and document.');
  } else if(escalation === 'no-unknown'){
    flag('amber');
    alerts.push({lvl:'amber', title:'Escalation need unclear', body:'Current situation may require escalation but this has not been determined. Review patient status against your pre-agreed risk thresholds.', action:'Review risk threshold criteria and determine whether escalation is required before end of shift.'});
  }

  // ── Metric 6: high-risk with no activity (proxy from contact + days) ──
  if(contactMade === 'no' && daysDis >= 2 && apptStatus !== 'attended'){
    const alreadyFlagged = alerts.some(a => a.lvl === 'red' && a.title.includes('No post-discharge'));
    if(!alreadyFlagged){
      flag('red');
      alerts.push({lvl:'red', title:'High-risk patient: no documented activity for 48+ hours', body:'Patient appears as an active case but no contact, outreach, or documentation has been recorded in over 48 hours. This is the gap between "case open" and "case monitored".',
        action:'Review case immediately and document current status.'});
    }
  }

  // ── Caseload capacity ──
  const ratio = caseload > 0 ? Math.round((highRisk/caseload)*100) : 0;
  const barColor = ratio >= 40 ? '#A32D2D' : ratio >= 30 ? '#EF9F27' : '#0F6E56';
  document.getElementById('cap-bar').style.width = Math.min(ratio,100)+'%';
  document.getElementById('cap-bar').style.background = barColor;
  document.getElementById('cap-pct').textContent = caseload > 0 ? ratio+'%' : '—';
  document.getElementById('cap-note').textContent = ratio >= 40
    ? 'High-risk ratio is critically high. Capacity to complete time-sensitive coordination steps for all patients is at risk. Flag to supervisor.'
    : ratio >= 30
    ? 'High-risk ratio approaching threshold (30%). Monitor closely and prioritise most time-sensitive cases first.'
    : caseload > 0 ? 'Caseload high-risk ratio is within manageable range.'
    : 'Enter caseload figures to see capacity indicator.';

  // ── Metrics summary ──
  metrics.push({label:'Days since discharge', val:daysDis+'d', pill: daysDis>=3&&contactMade==='no' ? 'red' : 'green'});
  metrics.push({label:'Post-discharge contact', val: contactMade==='yes'?'Done':contactMade==='no'?'Not made':'—', pill: contactMade==='no'?'red':'green'});
  metrics.push({label:'Follow-up appointment', val: apptStatus==='attended'?'Attended':apptStatus==='missed-no-response'?'Missed — no response':apptStatus==='missed-responded'?'Missed — outreach done':apptStatus==='upcoming'?'Upcoming':'—',
    pill: apptStatus==='missed-no-response'?'red':apptStatus==='missed-responded'?'amber':apptStatus==='attended'?'green':'gray'});
  metrics.push({label:'Caregiver contacted', val: carerContact==='yes'?'Yes':carerContact==='no'?'No':carerContact==='na'?'N/A':'—', pill: carerContact==='no'?'amber':'green'});
  metrics.push({label:'Days since consultant update', val: daysCons+'d', pill: daysCons>5?'red':daysCons>=3?'amber':'green'});
  metrics.push({label:'Escalation status', val: escalation==='yes-ack'?'Acknowledged':escalation==='yes-pending'?'Pending':escalation==='no-needed'?'Not needed':escalation==='no-unknown'?'Unclear':'—',
    pill: escalation==='yes-pending'?'red':escalation==='no-unknown'?'amber':'green'});
  metrics.push({label:'Active caseload', val: caseload>0?caseload+' patients':'—', pill: caseload>40?'amber':'green'});
  metrics.push({label:'High-risk ratio', val: caseload>0?ratio+'%':'—', pill: ratio>=40?'red':ratio>=30?'amber':'green'});

  // ── Render ──
  const ac = document.getElementById('alerts-container');
  ac.innerHTML = alerts.length ? alerts.map(a=>`
    <div class="alert-card alert-${a.lvl}">
      <div class="alert-title">${a.title}</div>
      <div class="alert-body">${a.body}</div>
      ${a.action?`<span class="alert-action">→ ${a.action}</span>`:''}
    </div>`).join('') : '<div class="empty">No coordination gaps detected for this patient.</div>';

  document.getElementById('metrics-container').innerHTML = metrics.map(m=>`
    <div class="metric-row">
      <span class="metric-label">${m.label}</span>
      <span style="display:flex;align-items:center;gap:8px">
        <span class="metric-val">${m.val}</span>
        <span class="pill pill-${m.pill}">${m.pill==='red'?'Flag':m.pill==='amber'?'Watch':m.pill==='green'?'OK':'—'}</span>
      </span>
    </div>`).join('');

  const abox = document.getElementById('actions-container');
  abox.innerHTML = actions.length ? actions.map((a,i)=>`
    <div style="display:flex;gap:10px;align-items:flex-start;padding:7px 0;border-bottom:1px solid var(--color-border-tertiary);font-size:13px">
      <span style="font-weight:500;color:var(--color-text-danger);min-width:18px">${i+1}.</span>
      <span>${a}</span>
    </div>`).join('') : '<div class="empty">No urgent actions required at this time.</div>';

  const badge = document.getElementById('cm-badge');
  badge.textContent = urgencyLevel==='red'?'Urgent gaps detected':urgencyLevel==='amber'?'Review required':'All checks clear';
  badge.className = 'badge '+(urgencyLevel==='red'?'badge-red':urgencyLevel==='amber'?'badge-amber':'badge-green');

  document.getElementById('output').style.display='block';
}
</script>
