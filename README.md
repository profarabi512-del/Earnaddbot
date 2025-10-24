earnadd-bot 
<!doctype html>
<html lang="bn">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Income Site — Demo</title>
<style>
  :root{--bg:#f6f8fa;--card:#fff;--accent:#ff4d6d;--muted:#666}
  body{font-family: "Noto Sans Bengali", system-ui, Arial; margin:0;background:var(--bg);padding:20px}
  .wrap{max-width:980px;margin:0 auto;display:grid;grid-template-columns:1fr 320px;gap:18px}
  .card{background:var(--card);border-radius:12px;padding:16px;box-shadow:0 6px 18px rgba(10,10,20,0.06)}
  h2{margin:0 0 10px;font-size:18px}
  label{display:block;font-size:13px;margin:8px 0 6px}
  input,select,button,textarea{width:100%;padding:10px;border-radius:8px;border:1px solid #e6e9ef;box-sizing:border-box}
  .btn{background:var(--accent);color:#fff;border:0;padding:10px;border-radius:8px;cursor:pointer;font-weight:600}
  .muted{color:var(--muted);font-size:13px}
  .small{font-size:13px;color:#444}
  .row{display:flex;gap:8px}
  .row > *{flex:1}
  .nav{display:flex;gap:8px;margin-bottom:12px}
  .nav button{flex:1}
  .list{margin-top:10px}
  .tx{padding:10px;border-radius:8px;background:#fbfbfd;border:1px solid #f0f3ff;margin-bottom:8px;display:flex;justify-content:space-between;align-items:center}
  .badge{padding:6px 8px;border-radius:8px;font-weight:700}
  .badge.pending{background:#fff7e6;color:#b06a00;border:1px solid #ffe6b3}
  .badge.paid{background:#e8fff0;color:#087a3f;border:1px solid #bff0cf}
  .small-muted{font-size:12px;color:#888}
  footer{margin-top:18px;text-align:center;color:#999;font-size:13px}
  .admin-login{display:flex;gap:8px;align-items:center}
  @media(max-width:900px){.wrap{grid-template-columns:1fr;}}
</style>
</head>
<body>
<div class="wrap">
  <!-- LEFT: User area -->
  <div class="card" id="userArea">
    <div class="nav">
      <button class="btn" id="showRegister">Register</button>
      <button class="btn" id="showDashboard" style="background:#3b82f6">Dashboard</button>
    </div>

    <!-- Register / Login -->
    <div id="registerView">
      <h2>রেজিস্টার / লগইন (ডেমো)</h2>
      <label>Username (short)</label>
      <input id="regUser" placeholder="example: rahim"/>
      <label>Referral (optional) — তোমার ref code (যদি থাকে)</label>
      <input id="regRef" placeholder="ref=username"/>
      <div style="display:flex;gap:8px;margin-top:10px">
        <button class="btn" id="doRegister">Register</button>
        <button class="btn" id="doLogin" style="background:#6b7280">Login</button>
      </div>
      <p class="muted" style="margin-top:12px">রেজিস্টার হলে রেফারারকে স্বয়ংক্রিয়ভাবে <strong>১০৳</strong> কমিশন যোগ হবে (ডেমো)।</p>
      <hr style="margin:12px 0"/>
      <h2>Admin Login</h2>
      <div class="admin-login">
        <input id="adminPass" placeholder="admin password"/>
        <button class="btn" id="adminLogin" style="background:#111827">Admin</button>
      </div>
      <p class="small-muted">Admin পাসওয়ার্ড (ডেমো): <strong>admin123</strong></p>
    </div>

    <!-- Dashboard -->
    <div id="dashboardView" style="display:none">
      <div style="display:flex;justify-content:space-between;align-items:center">
        <h2>ড্যাশবোর্ড</h2>
        <div class="small-muted">User: <span id="meName"></span></div>
      </div>

      <div style="display:flex;gap:12px;margin-top:8px">
        <div style="flex:1" class="card">
          <div class="small-muted">Balance</div>
          <div style="font-size:22px;font-weight:800">৳ <span id="myBalance">0</span></div>
          <div class="small-muted">Referral Link</div>
          <input id="myRefLink" readonly/>
          <div style="margin-top:8px">
            <button class="btn" id="copyRef">Copy Link</button>
          </div>
        </div>

        <div style="width:220px" class="card">
          <div class="small-muted">Quick Actions</div>
          <div style="margin-top:8px">
            <button class="btn" id="toWithdraw">Withdraw</button>
          </div>
          <div style="margin-top:8px">
            <button class="btn" id="simulateRef" style="background:#10b981">Simulate new user via your ref</button>
            <div class="small-muted" style="margin-top:6px">ডেমো: নতুন ইউজার রেজিস্টার করলে রেফারার পাবে ১০৳</div>
          </div>
        </div>
      </div>

      <!-- Withdraw Form -->
      <div id="withdrawForm" style="margin-top:12px;display:none" class="card">
        <h3>Withdraw Request</h3>
        <label>Amount (৳) — Minimum 50৳</label>
        <input id="withdrawAmount" type="number" min="50" placeholder="50"/>
        <label>Method</label>
        <select id="withdrawMethod">
          <option value="bkash">Bkash</option>
          <option value="nagad">Nagad</option>
          <option value="bank">Bank</option>
        </select>
        <label>Account / Mobile (01XXXXXXXXX)</label>
        <input id="withdrawAccount" placeholder="01XXXXXXXXX or account no"/>
        <div style="display:flex;gap:8px;margin-top:10px">
          <button class="btn" id="submitWithdraw">Submit Withdraw</button>
          <button class="btn" id="cancelWithdraw" style="background:#6b7280">Cancel</button>
        </div>
        <p class="small-muted" style="margin-top:8px">নোট: এখানে সাবমিশন হলে অ্যাডমিন Pending তালিকায় যাবে। অ্যাডমিন আইডি ভেরিফাই করে Send Money করলে পেইড হিসেবে চিহ্নিত হবে।</p>
      </div>

      <!-- Withdraw History -->
      <div class="card" style="margin-top:12px">
        <h3>Withdraw History</h3>
        <div id="historyList"></div>
      </div>
    </div>
  </div>

  <!-- RIGHT: Admin panel -->
  <div id="adminArea" class="card">
    <h2>Admin Panel</h2>
    <div id="adminControls">
      <div><strong>Logged in as Admin</strong></div>
      <div style="margin-top:8px">
        <button class="btn" id="refreshData" style="background:#2563eb">Refresh</button>
      </div>

      <h3 style="margin-top:12px">Pending Withdrawals</h3>
      <div id="pendingList"></div>

      <h3 style="margin-top:12px">Users</h3>
      <div id="usersList"></div>
    </div>
    <div id="adminNot" style="display:none">
      <p class="small-muted">Admin লগইন করো ডাটা দেখতে। (ডেমো পাসওয়ার্ড admin123)</p>
    </div>
  </div>
</div>

<footer>Demo — এই ফাইলটি কেবল ক্লায়েন্ট সাইড ডেমো। বাস্তব প্রোডাকশনে ব্যাকএন্ড ও পেমেন্ট ইন্টিগ্রেশন দরকার।</footer>

<script>
/*
Simple client-side demo using localStorage.
Structure:
- users: {username: {name, balance, verified:false, referrals:[], refBy:null}}
- withdraws: [{id, user, amount, method, account, status:'pending'|'paid', created_at, admin_note}]
- currentUser: username
- admin password: admin123
- Referral bonus: 10৳ to referrer on new registration via ref param.
- Minimum withdraw: 50৳
*/

const STORAGE_KEY = 'income_demo_v1';
const state = {
  users: {}, withdraws: [], currentUser: null, adminLogged: false
};

function saveState(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); }
function loadState(){
  const raw = localStorage.getItem(STORAGE_KEY);
  if(raw){ try{ Object.assign(state, JSON.parse(raw)); }catch(e){ console.error(e) } }
}
function uid(){ return 'tx'+Date.now()+Math.floor(Math.random()*999); }

loadState();
renderViews();

/* Helpers */
function ensureUser(u){
  if(!state.users[u]){
    state.users[u] = {name:u, balance:0, verified:false, referrals:[], refBy:null};
    saveState();
  }
}
function formatMoney(n){ return Number(n).toFixed(0); }

/* UI wiring */
document.getElementById('showRegister').onclick = ()=>{
  document.getElementById('registerView').style.display = 'block';
  document.getElementById('dashboardView').style.display = 'none';
};
document.getElementById('showDashboard').onclick = ()=>{
  if(!state.currentUser){ alert('প্রথমে লগইন/রেজিস্টার কর'); return; }
  renderDashboard();
  document.getElementById('registerView').style.display = 'none';
  document.getElementById('dashboardView').style.display = 'block';
};

document.getElementById('doRegister').onclick = ()=>{
  const u = (document.getElementById('regUser').value||'').trim();
  const ref = (document.getElementById('regRef').value||'').trim();
  if(!u){ alert('Username দিন'); return; }
  if(state.users[u]){ alert('Username already exists — use login'); return; }
  // create user
  state.users[u] = {name: u, balance: 0, verified:false, referrals:[], refBy:null};
  // handle referral
  if(ref && state.users[ref]){
    state.users[ref].balance = (state.users[ref].balance||0) + 10; // referral commission 10৳
    state.users[u].refBy = ref;
    state.users[ref].referrals.push(u);
    alert('Registered. Referrer '+ref+' got 10৳ commission (demo).');
  } else {
    alert('Registered without ref.');
  }
  state.currentUser = u;
  saveState();
  renderDashboard();
  document.getElementById('registerView').style.display='none';
  document.getElementById('dashboardView').style.display='block';
};

document.getElementById('doLogin').onclick = ()=>{
  const u = (document.getElementById('regUser').value||'').trim();
  if(!u){ alert('Username দিন'); return; }
  if(!state.users[u]){ alert('User not found — register first'); return; }
  state.currentUser = u;
  saveState();
  renderDashboard();
  document.getElementById('registerView').style.display='none';
  document.getElementById('dashboardView').style.display='block';
};

document.getElementById('adminLogin').onclick = ()=>{
  const p = document.getElementById('adminPass').value||'';
  if(p === 'admin123'){ state.adminLogged = true; saveState(); alert('Admin logged in'); renderAdmin(); }
  else alert('Wrong admin password');
};

document.getElementById('toWithdraw').onclick = ()=>{
  document.getElementById('withdrawForm').style.display = 'block';
  window.scrollTo({top:0,behavior:'smooth'});
};
document.getElementById('cancelWithdraw').onclick = ()=>{ document.getElementById('withdrawForm').style.display='none'; };

document.getElementById('submitWithdraw').onclick = ()=>{
  const amt = Number(document.getElementById('withdrawAmount').value||0);
  const method = document.getElementById('withdrawMethod').value;
  const account = (document.getElementById('withdrawAccount').value||'').trim();
  if(!state.currentUser){ alert('লগইন করুন'); return; }
  if(!amt || amt < 50){ alert('সর্বনিম্ন উইথড্র ৫০৳'); return; }
  const userBal = state.users[state.currentUser].balance || 0;
  if(amt > userBal){ alert('তোমার ব্যালেন্সের চেয়ে বেশি অনুরোধ করা হয়েছে'); return; }
  if(!account){ alert('Account/Mobile দিন'); return; }
  // create withdraw
  const tx = { id: uid
