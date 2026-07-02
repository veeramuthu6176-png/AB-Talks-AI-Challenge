<style>
  :root {
    --bg: #0d1117;
    --card-bg: #161b22;
    --card-bg-hover: #1c2229;
    --border: #2a303a;
    --text-primary: #e6edf3;
    --text-secondary: #9aa4b2;
    --accent: #7c5cff;
    --accent-soft: rgba(124, 92, 255, 0.15);
    --red: #f85149;
    --red-soft: rgba(248, 81, 73, 0.15);
    --orange: #f0883e;
    --orange-soft: rgba(240, 136, 62, 0.15);
    --green: #3fb950;
    --green-soft: rgba(63, 185, 80, 0.15);
    --yellow: #d29922;
    --yellow-soft: rgba(210, 153, 34, 0.15);
    --blue: #58a6ff;
    --blue-soft: rgba(88, 166, 255, 0.15);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text-primary);
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
    padding: 24px;
    line-height: 1.5;
  }

  .dashboard {
    max-width: 1100px;
    margin: 0 auto;
  }

  .header {
    margin-bottom: 28px;
  }

  .header h1 {
    font-size: 26px;
    font-weight: 700;
    letter-spacing: -0.02em;
  }

  .header p {
    color: var(--text-secondary);
    font-size: 14px;
    margin-top: 4px;
  }

  .section {
    margin-bottom: 28px;
  }

  .section-header {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 14px;
    cursor: pointer;
    user-select: none;
  }

  .section-header h2 {
    font-size: 16px;
    font-weight: 600;
    color: var(--text-primary);
  }

  .section-icon {
    width: 30px;
    height: 30px;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 15px;
  }

  .chevron {
    margin-left: auto;
    color: var(--text-secondary);
    transition: transform 0.2s ease;
    font-size: 13px;
  }

  .collapsed .chevron { transform: rotate(-90deg); }
  .collapsed .section-body { display: none; }

  .card {
    background: var(--card-bg);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 18px;
    transition: background 0.15s ease, transform 0.15s ease;
  }

  .card:hover {
    background: var(--card-bg-hover);
  }

  .summary-card p {
    color: var(--text-primary);
    font-size: 14.5px;
  }

  .takeaways-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
    gap: 14px;
  }

  .takeaway-card {
    background: var(--card-bg);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 16px;
    border-left: 3px solid var(--accent);
  }

  .takeaway-card:hover { background: var(--card-bg-hover); }

  .takeaway-card .label {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    color: var(--accent);
    font-weight: 700;
    margin-bottom: 6px;
  }

  .takeaway-card .value {
    font-size: 14px;
    color: var(--text-primary);
  }

  table {
    width: 100%;
    border-collapse: collapse;
    background: var(--card-bg);
    border: 1px solid var(--border);
    border-radius: 12px;
    overflow: hidden;
  }

  thead tr { background: #1c2229; }

  th {
    text-align: left;
    padding: 12px 16px;
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    color: var(--text-secondary);
    font-weight: 700;
    border-bottom: 1px solid var(--border);
  }

  td {
    padding: 14px 16px;
    font-size: 14px;
    border-bottom: 1px solid var(--border);
    color: var(--text-primary);
  }

  tbody tr:last-child td { border-bottom: none; }
  tbody tr:hover { background: var(--card-bg-hover); }

  .badge {
    display: inline-flex;
    align-items: center;
    gap: 5px;
    padding: 4px 10px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 600;
    white-space: nowrap;
  }

  .badge-high { background: var(--red-soft); color: var(--red); }
  .badge-medium { background: var(--orange-soft); color: var(--orange); }
  .badge-low { background: var(--green-soft); color: var(--green); }
  .badge-conflict { background: var(--red-soft); color: var(--red); }
  .badge-question { background: var(--blue-soft); color: var(--blue); }
  .badge-completed { background: var(--green-soft); color: var(--green); }
  .badge-pending { background: var(--yellow-soft); color: var(--yellow); }

  .list-card {
    background: var(--card-bg);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 4px;
  }

  .list-item {
    display: flex;
    align-items: flex-start;
    gap: 12px;
    padding: 14px 16px;
    border-bottom: 1px solid var(--border);
  }

  .list-item:last-child { border-bottom: none; }
  .list-item:hover { background: var(--card-bg-hover); border-radius: 8px; }

  .list-item .icon { font-size: 16px; margin-top: 1px; }

  .list-item .content { flex: 1; }

  .list-item .title { font-size: 14.5px; color: var(--text-primary); font-weight: 500; }

  .list-item .meta { font-size: 12.5px; color: var(--text-secondary); margin-top: 3px; }

  .not-specified {
    color: var(--text-secondary);
    font-style: italic;
    font-size: 13px;
  }

  .notes-card {
    background: var(--card-bg);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 18px;
    font-size: 14px;
    color: var(--text-secondary);
    white-space: pre-line;
  }

  @media (max-width: 640px) {
    body { padding: 14px; }
    .header h1 { font-size: 21px; }
    table, thead, tbody, th, td, tr { display: block; }
    thead { display: none; }
    tbody tr {
      background: var(--card-bg);
      border: 1px solid var(--border);
      border-radius: 12px;
      margin-bottom: 12px;
      padding: 10px 4px;
    }
    td {
      border: none;
      padding: 8px 14px;
      display: flex;
      justify-content: space-between;
      gap: 10px;
    }
    td::before {
      content: attr(data-label);
      font-size: 11px;
      text-transform: uppercase;
      color: var(--text-secondary);
      font-weight: 700;
      flex-shrink: 0;
    }
  }
</style>

<div class="dashboard">

  <div class="header">
    <h1>Brain Dump → Action Plan</h1>
    <p>Sample source: informal team brainstorming notes, product launch planning</p>
  </div>

  <!-- SUMMARY -->
  <div class="section">
    <div class="section-header" onclick="toggleSection(this)">
      <div class="section-icon" style="background: var(--accent-soft); color: var(--accent);">📝</div>
      <h2>Summary</h2>
      <span class="chevron">▾</span>
    </div>
    <div class="section-body">
      <div class="card summary-card">
        <p>Team discussed launch plan for the new mobile app update. Priya to finalize the UI copy by Friday. Backend team (Arjun) raised concerns about API rate limits before the launch date. Marketing wants launch pushed to next month but engineering wants to stick to original date. Budget for ads not yet finalized.</p>
      </div>
    </div>
  </div>

  <!-- KEY TAKEAWAYS -->
  <div class="section">
    <div class="section-header" onclick="toggleSection(this)">
      <div class="section-icon" style="background: var(--blue-soft); color: var(--blue);">💡</div>
      <h2>Key Takeaways</h2>
      <span class="chevron">▾</span>
    </div>
    <div class="section-body">
      <div class="takeaways-grid">
        <div class="takeaway-card">
          <div class="label">Launch Date</div>
          <div class="value">Originally set for August 15th</div>
        </div>
        <div class="takeaway-card">
          <div class="label">UI Copy</div>
          <div class="value">Priya finalizing by Friday</div>
        </div>
        <div class="takeaway-card">
          <div class="label">API Concern</div>
          <div class="value">Rate limits flagged by Arjun as a risk</div>
        </div>
        <div class="takeaway-card">
          <div class="label">Ad Budget</div>
          <div class="value not-specified">Not specified</div>
        </div>
      </div>
    </div>
  </div>

  <!-- ACTION ITEMS -->
  <div class="section">
    <div class="section-header" onclick="toggleSection(this)">
      <div class="section-icon" style="background: var(--green-soft); color: var(--green);">✅</div>
      <h2>Action Items</h2>
      <span class="chevron">▾</span>
    </div>
    <div class="section-body">
      <table>
        <thead>
          <tr>
            <th>Task</th>
            <th>Owner</th>
            <th>Deadline</th>
            <th>Status</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td data-label="Task">Finalize UI copy for app update</td>
            <td data-label="Owner">Priya</td>
            <td data-label="Deadline">Friday</td>
            <td data-label="Status"><span class="badge badge-pending">⏳ Pending</span></td>
          </tr>
          <tr>
            <td data-label="Task">Investigate API rate limit issue</td>
            <td data-label="Owner">Arjun</td>
            <td data-label="Deadline" class="not-specified">Not specified</td>
            <td data-label="Status"><span class="badge badge-high">🔴 High Priority</span></td>
          </tr>
          <tr>
            <td data-label="Task">Confirm ad budget with finance</td>
            <td data-label="Owner" class="not-specified">Not specified</td>
            <td data-label="Deadline" class="not-specified">Not specified</td>
            <td data-label="Status"><span class="badge badge-medium">🟠 Medium Priority</span></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <!-- OPEN QUESTIONS -->
  <div class="section">
    <div class="section-header" onclick="toggleSection(this)">
      <div class="section-icon" style="background: var(--blue-soft); color: var(--blue);">❓</div>
      <h2>Open Questions</h2>
      <span class="chevron">▾</span>
    </div>
    <div class="section-body">
      <div class="list-card">
        <div class="list-item">
          <span class="icon">❓</span>
          <div class="content">
            <div class="title">Should the launch date move to next month or stay as August 15th?</div>
            <div class="meta">Raised by Marketing, unresolved</div>
          </div>
          <span class="badge badge-question">❓ Open Question</span>
        </div>
        <div class="list-item">
          <span class="icon">❓</span>
          <div class="content">
            <div class="title">What is the finalized ad budget?</div>
            <div class="meta not-specified">Owner not specified</div>
          </div>
          <span class="badge badge-question">❓ Open Question</span>
        </div>
      </div>
    </div>
  </div>

  <!-- RISKS / BLOCKERS -->
  <div class="section">
    <div class="section-header" onclick="toggleSection(this)">
      <div class="section-icon" style="background: var(--red-soft); color: var(--red);">⚠️</div>
      <h2>Risks / Blockers</h2>
      <span class="chevron">▾</span>
    </div>
    <div class="section-body">
      <div class="list-card">
        <div class="list-item">
          <span class="icon">🔴</span>
          <div class="content">
            <div class="title">API rate limits may not be resolved before launch date</div>
            <div class="meta">Flagged by Arjun</div>
          </div>
          <span class="badge badge-high">🔴 High Priority</span>
        </div>
      </div>
    </div>
  </div>

  <!-- CONFLICTS -->
  <div class="section">
    <div class="section-header" onclick="toggleSection(this)">
      <div class="section-icon" style="background: var(--red-soft); color: var(--red);">⚠️</div>
      <h2>Conflicts</h2>
      <span class="chevron">▾</span>
    </div>
    <div class="section-body">
      <div class="list-card">
        <div class="list-item">
          <span class="icon">⚠️</span>
          <div class="content">
            <div class="title">Launch date conflict: Marketing wants to push to next month, Engineering wants to keep original August 15th date</div>
            <div class="meta">Not resolved in the discussion</div>
          </div>
          <span class="badge badge-conflict">⚠️ Conflict</span>
        </div>
      </div>
    </div>
  </div>

  <!-- ADDITIONAL NOTES -->
  <div class="section">
    <div class="section-header" onclick="toggleSection(this)">
      <div class="section-icon" style="background: var(--accent-soft); color: var(--accent);">🗒️</div>
      <h2>Additional Notes</h2>
      <span class="chevron">▾</span>
    </div>
    <div class="section-body">
      <div class="notes-card">No additional supporting context was provided beyond the items captured above.</div>
    </div>
  </div>

</div>

<script>
  function toggleSection(header) {
    header.parentElement.classList.toggle('collapsed');
  }
</script>
