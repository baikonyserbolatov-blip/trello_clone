<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Mini Trello Clone — Web Tech Project</title>
<style>
  :root{
    --bg:#f4f7fb;
    --card:#fff;
    --muted:#6b7280;
    --accent:#0ea5a4;
    --danger:#ef4444;
    --shadow: 0 6px 18px rgba(15,23,42,0.08);
    --radius:12px;
    font-family: Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
  }
  html,body{height:100%}
  body{
    margin:0;
    background:linear-gradient(180deg,#f7fbfc 0%,var(--bg) 100%);
    color:#0f172a;
    -webkit-font-smoothing:antialiased;
    -moz-osx-font-smoothing:grayscale;
  }
  .app{
    max-width:1100px;
    margin:28px auto;
    padding:20px;
  }

  /* Auth */
  .card{
    background:var(--card);
    border-radius:16px;
    padding:20px;
    box-shadow:var(--shadow);
  }
  .center{
    display:flex; gap:20px; align-items:center; justify-content:center;
  }
  header{
    display:flex; align-items:center; justify-content:space-between;
    margin-bottom:18px;
  }
  .brand{display:flex;gap:12px;align-items:center}
  .logo{
    width:44px;height:44px;border-radius:10px;background:linear-gradient(135deg,var(--accent),#2563eb);
    display:flex;align-items:center;justify-content:center;color:white;font-weight:700;font-size:18px;box-shadow:0 6px 18px rgba(14,165,164,0.2);
  }
  h1{margin:0;font-size:20px}
  p.lead{margin:4px 0 0;color:var(--muted);font-size:13px}

  /* Forms */
  form { display:flex; flex-direction:column; gap:12px; margin-top:8px }
  input[type="email"], input[type="password"], input[type="text"]{
    padding:10px 12px;border-radius:10px;border:1px solid #e6edf3;font-size:14px;background:#fff;
  }
  button{
    background:var(--accent); color:white;border:0;padding:10px 12px;border-radius:10px;cursor:pointer;font-weight:600;
  }
  button.ghost{background:transparent;color:var(--accent);border:1px solid rgba(14,165,164,0.14)}
  .muted{color:var(--muted);font-size:13px}

  /* Dashboard */
  .topbar{display:flex;align-items:center;justify-content:space-between;gap:12px;margin-bottom:12px}
  .profile{
    display:flex;gap:10px;align-items:center;
  }
  .avatar{
    width:40px;height:40px;border-radius:50%;background:#e2fdfc;color: #064e4e;display:flex;align-items:center;justify-content:center;font-weight:700;
  }
  .boards-grid{
    display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:14px;
  }
  .board-card{padding:14px;border-radius:12px; background:linear-gradient(180deg,#ffffff,#fbfeff); cursor:pointer; min-height:110px; display:flex;flex-direction:column; justify-content:space-between}
  .board-title{font-weight:700}
  .small{font-size:12px;color:var(--muted)}

  /* Board view */
  .board-view{display:flex;gap:12px;align-items:flex-start;overflow-x:auto;padding-bottom:12px}
  .column{
    min-width:260px;background:rgba(255,255,255,0.9);border-radius:12px;padding:10px;box-shadow:0 6px 14px rgba(12,18,29,0.04);
    display:flex;flex-direction:column;gap:10px;
  }
  .col-header{display:flex;align-items:center;justify-content:space-between;gap:8px}
  .col-title{font-weight:700}
  .card-item{
    background:var(--card);padding:10px;border-radius:10px;box-shadow:0 6px 14px rgba(12,18,29,0.04);cursor:grab;
  }
  .card-item.dragging{opacity:0.5}
  .add-btn{padding:8px;border-radius:8px;border:1px dashed #dbeafe;background:transparent;cursor:pointer}
  .trash{background:transparent;border:0;color:var(--danger);cursor:pointer}

  /* small helpers */
  .flex{display:flex;gap:8px;align-items:center}
  .space{flex:1}
  .inline{display:inline-block}
  .link{color:var(--accent);cursor:pointer;text-decoration:underline}
  .hidden{display:none}

  /* responsive */
  @media (max-width:720px){
    .app{padding:12px}
    .board-view{padding:6px}
    .column{min-width:220px}
  }
</style>
</head>
<body>
<div class="app">

  <!-- Header -->
  <header>
    <div class="brand">
      <div class="logo">MT</div>
      <div>
        <h1>Mini Trello Clone</h1>
        <p class="lead">Web Technologies course — project</p>
      </div>
    </div>

    <div id="globalActions" class="flex">
      <!-- dynamic: login/logout/profile -->
    </div>
  </header>

  <!-- AUTH (login/register) -->
  <div id="auth" class="card" style="max-width:780px;margin:auto">
    <div style="display:flex;gap:18px;align-items:flex-start;flex-wrap:wrap">
      <div style="flex:1;min-width:260px">
        <h2>Welcome — <span id="authModeTitle">Login</span></h2>
        <p class="muted" id="authSubtitle">Please login or register to continue.</p>

        <form id="authForm">
          <input id="email" type="email" placeholder="Email" required />
          <input id="password" type="password" placeholder="Password" required />
          <input id="confirmPassword" type="password" placeholder="Confirm password (register only)" class="hidden" />
          <div style="display:flex;gap:8px">
            <button id="authPrimary" type="submit">Login</button>
            <button id="toggleAuth" type="button" class="ghost">Switch to Register</button>
          </div>
          <p class="muted">This is a demo — credentials saved to <code>localStorage</code>.</p>
        </form>
      </div>

      <div style="flex:1;min-width:260px">
        <div style="background:linear-gradient(180deg,#f8fcff,#ffffff);padding:14px;border-radius:12px">
          <h3>How to use</h3>
          <ol style="padding-left:18px;margin:6px 0">
            <li>Create account (Register)</li>
            <li>Create boards in Dashboard</li>
            <li>Open board, add lists/cards and drag & drop</li>
            <li>Everything saved locally to your browser</li>
          </ol>
          <p class="small muted">For team work: one person does UI, екінші — JavaScript логикасын жазсын.</p>
        </div>
      </div>
    </div>
  </div>

  <!-- Dashboard -->
  <div id="dashboard" class="hidden">
    <div class="card">
      <div class="topbar">
        <div class="flex">
          <div class="profile">
            <div id="userAvatar" class="avatar">U</div>
            <div>
              <div id="userEmail" style="font-weight:700">user@example.com</div>
              <div class="small muted" id="userBoardsCount">0 boards</div>
            </div>
          </div>
          <div style="margin-left:18px">
            <button id="newBoardBtn">+ New Board</button>
          </div>
        </div>

        <div class="flex">
          <button id="openProfile" class="ghost">Profile</button>
          <button id="logoutBtn" class="ghost">Logout</button>
        </div>
      </div>

      <h3 style="margin-top:6px">My Boards</h3>
      <div id="boardsGrid" class="boards-grid" style="margin-top:10px"></div>
    </div>
  </div>

  <!-- Board view -->
  <div id="boardViewWrap" class="hidden">
    <div style="display:flex;align-items:center;gap:12px;margin-bottom:10px">
      <button id="backToDash" class="ghost">← Back</button>
      <h2 id="boardTitleDisplay">Board Title</h2>
      <div class="space"></div>
      <button id="renameBoard" class="ghost">Rename</button>
      <button id="deleteBoard" class="trash">Delete Board</button>
    </div>

    <div id="boardView" class="board-view"></div>
  </div>

  <!-- Templates / Modals (simple prompts used in code) -->

</div>

<script>
/*
  Mini Trello Clone
  - Users stored in localStorage key: "mt_users" (array)
  - Logged in user email stored in localStorage key: "mt_current"
  - Each user: { email, password, boards: [ {id, title, lists:[ {id,title,cards:[{id,title,desc}]} ] } ] }
*/

/* ---------- Utilities ---------- */
const uid = ()=> Math.random().toString(36).slice(2,9);
const saveUsers = users => localStorage.setItem('mt_users', JSON.stringify(users||[]));
const loadUsers = ()=> JSON.parse(localStorage.getItem('mt_users') || '[]');
const setCurrent = email => localStorage.setItem('mt_current', email);
const getCurrent = ()=> localStorage.getItem('mt_current');
const clearCurrent = ()=> localStorage.removeItem('mt_current');

function findUser(email){
  const users = loadUsers();
  return users.find(u=>u.email === email);
}
function updateUser(user){
  const users = loadUsers();
  const idx = users.findIndex(u=>u.email === user.email);
  if (idx>=0) users[idx]=user;
  else users.push(user);
  saveUsers(users);
}

/* ---------- Elements ---------- */
const auth = document.getElementById('auth');
const authModeTitle = document.getElementById('authModeTitle');
const authSubtitle = document.getElementById('authSubtitle');
const authForm = document.getElementById('authForm');
const emailInput = document.getElementById('email');
const passwordInput = document.getElementById('password');
const confirmPassword = document.getElementById('confirmPassword');
const toggleAuth = document.getElementById('toggleAuth');
const authPrimary = document.getElementById('authPrimary');

const dashboard = document.getElementById('dashboard');
const boardsGrid = document.getElementById('boardsGrid');
const userEmailEl = document.getElementById('userEmail');
const userAvatar = document.getElementById('userAvatar');
const userBoardsCount = document.getElementById('userBoardsCount');
const newBoardBtn = document.getElementById('newBoardBtn');
const logoutBtn = document.getElementById('logoutBtn');

const boardViewWrap = document.getElementById('boardViewWrap');
const boardView = document.getElementById('boardView');
const boardTitleDisplay = document.getElementById('boardTitleDisplay');
const backToDash = document.getElementById('backToDash');
const deleteBoardBtn = document.getElementById('deleteBoard');
const renameBoardBtn = document.getElementById('renameBoard');

const globalActions = document.getElementById('globalActions');

/* ---------- State ---------- */
let isRegister = false;
let currentUser = null;
let currentBoardId = null;

/* ---------- Init ---------- */
function init(){
  // wire toggle
  toggleAuth.addEventListener('click', ()=>{
    isRegister = !isRegister;
    updateAuthUI();
  });

  authForm.addEventListener('submit', e=>{
    e.preventDefault();
    if (isRegister) doRegister();
    else doLogin();
  });

  newBoardBtn.addEventListener('click', createBoardPrompt);
  logoutBtn.addEventListener('click', ()=>{ clearCurrent(); currentUser=null; showAuth(); });

  backToDash.addEventListener('click', ()=>{ showDashboard(); });
  deleteBoardBtn.addEventListener('click', deleteCurrentBoard);
  renameBoardBtn.addEventListener('click', renameBoardPrompt);

  renderGlobalActions();
  // check current
  const cur = getCurrent();
  if (cur){
    const user = findUser(cur);
    if (user){ signIn(user); return; }
    else { clearCurrent(); showAuth(); }
  } else showAuth();
}
init();

function updateAuthUI(){
  if (isRegister){
    authModeTitle.textContent = 'Register';
    authSubtitle.textContent = 'Create new account';
    confirmPassword.classList.remove('hidden');
    authPrimary.textContent = 'Register';
    toggleAuth.textContent = 'Switch to Login';
  } else {
    authModeTitle.textContent = 'Login';
    authSubtitle.textContent = 'Enter your credentials';
    confirmPassword.classList.add('hidden');
    authPrimary.textContent = 'Login';
    toggleAuth.textContent = 'Switch to Register';
  }
}

/* ---------- Auth actions ---------- */
function doRegister(){
  const email = emailInput.value.trim().toLowerCase();
  const pass = passwordInput.value;
  const pass2 = confirmPassword.value;
  if (!email || !pass) return alert('Email and password required.');
  if (pass !== pass2) return alert('Passwords do not match.');
  const users = loadUsers();
  if (users.some(u=>u.email===email)) return alert('User already exists. Login instead.');
  const user = { email, password: pass, boards: [] };
  users.push(user);
  saveUsers(users);
  setCurrent(email);
  signIn(user);
}

function doLogin(){
  const email = emailInput.value.trim().toLowerCase();
  const pass = passwordInput.value;
  const user = findUser(email);
  if (!user) return alert('User not found. Please register.');
  if (user.password !== pass) return alert('Wrong password.');
  setCurrent(email);
  signIn(user);
}

function signIn(user){
  currentUser = user;
  currentBoardId = null;
  showDashboard();
  renderGlobalActions();
}

function showAuth(){
  auth.classList.remove('hidden');
  dashboard.classList.add('hidden');
  boardViewWrap.classList.add('hidden');
  renderGlobalActions();
}

/* ---------- Global header actions ---------- */
function renderGlobalActions(){
  globalActions.innerHTML = '';
  const cur = getCurrent();
  if (cur){
    const u = findUser(cur);
    const btn = document.createElement('div');
    btn.className='flex';
    const avatar = document.createElement('div'); avatar.className='avatar'; avatar.textContent = u.email[0].toUpperCase();
    const name = document.createElement('div'); name.innerHTML = `<div style="font-weight:700">${u.email}</div><div class="small muted">${(u.boards||[]).length} boards</div>`;
    const logout = document.createElement('button'); logout.className='ghost'; logout.textContent='Logout';
    logout.addEventListener('click', ()=>{ clearCurrent(); currentUser=null; showAuth(); });
    btn.appendChild(avatar); btn.appendChild(name); btn.appendChild(logout);
    globalActions.appendChild(btn);
  } else {
    const l = document.createElement('button'); l.className='ghost'; l.textContent='Login/Register';
    l.addEventListener('click', ()=>{ showAuth(); });
    globalActions.appendChild(l);
  }
}

/* ---------- Dashboard ---------- */
function showDashboard(){
  auth.classList.add('hidden');
  dashboard.classList.remove('hidden');
  boardViewWrap.classList.add('hidden');
  // load user (in case of modifications)
  const cur = getCurrent();
  if (!cur) { showAuth(); return; }
  currentUser = findUser(cur);
  userEmailEl.textContent = currentUser.email;
  userAvatar.textContent = currentUser.email[0].toUpperCase();
  userBoardsCount.textContent = `${(currentUser.boards||[]).length} boards`;
  renderBoards();
}

function renderBoards(){
  boardsGrid.innerHTML = '';
  const boards = currentUser.boards || [];
  if (boards.length === 0){
    boardsGrid.innerHTML = `<div style="grid-column:1/-1;background:linear-gradient(180deg,#ffffff,#fbfbff);padding:18px;border-radius:12px">No boards yet — create one.</div>`;
    return;
  }
  boards.forEach(b=>{
    const el = document.createElement('div'); el.className='board-card';
    el.innerHTML = `<div><div class="board-title">${escapeHtml(b.title)}</div><div class="small muted">${(b.lists||[]).length} lists</div></div>
                    <div style="display:flex;justify-content:space-between;align-items:center">
                      <div class="small muted">${new Date(b.created||Date.now()).toLocaleString()}</div>
                      <div style="display:flex;gap:8px">
                        <button class="ghost openBtn">Open</button>
                        <button class="ghost delBtn">Delete</button>
                      </div>
                    </div>`;
    el.querySelector('.openBtn').addEventListener('click', ()=> openBoard(b.id));
    el.querySelector('.delBtn').addEventListener('click', ()=>{
      if (!confirm('Delete board?')) return;
      currentUser.boards = currentUser.boards.filter(x=>x.id!==b.id);
      updateUser(currentUser);
      showDashboard();
    });
    boardsGrid.appendChild(el);
  });
}

function createBoardPrompt(){
  const title = prompt('Board title')?.trim();
  if (!title) return;
  const board = {
    id: uid(),
    title,
    created: Date.now(),
    lists: [
      { id: uid(), title: 'To Do', cards: [] },
      { id: uid(), title: 'In Progress', cards: [] },
      { id: uid(), title: 'Done', cards: [] }
    ]
  };
  currentUser.boards = currentUser.boards || [];
  currentUser.boards.push(board);
  updateUser(currentUser);
  showDashboard();
  openBoard(board.id);
}

/* ---------- Board view ---------- */
function openBoard(boardId){
  currentBoardId = boardId;
  auth.classList.add('hidden');
  dashboard.classList.add('hidden');
  boardViewWrap.classList.remove('hidden');
  // reload user & board
  currentUser = findUser(getCurrent());
  const board = (currentUser.boards || []).find(b=>b.id===boardId);
  if (!board) { alert('Board not found'); showDashboard(); return; }
  boardTitleDisplay.textContent = board.title;
  renderBoard(board);
}

function renderBoard(board){
  boardView.innerHTML = '';
  board.lists.forEach(list=>{
    const col = document.createElement('div'); col.className='column';
    col.dataset.listId = list.id;
    col.innerHTML = `<div class="col-header"><div class="col-title">${escapeHtml(list.title)}</div>
                     <div class="flex"><button class="add-card add-btn">+ Card</button>
                     <button class="del-list trash" title="Delete list">✕</button></div></div>
                     <div class="cards" style="display:flex;flex-direction:column;gap:8px"></div>
                     <div><button class="add-card-bottom add-btn">+ Add card</button></div>`;
    const cardsWrap = col.querySelector('.cards');
    list.cards.forEach(card=>{
      const ci = document.createElement('div'); ci.className='card-item'; ci.draggable=true;
      ci.dataset.cardId = card.id;
      ci.innerHTML = `<div style="display:flex;justify-content:space-between;align-items:center"><div style="font-weight:600">${escapeHtml(card.title)}</div>
                      <div style="display:flex;gap:8px"><button class="edit small ghost">Edit</button><button class="del small trash">Del</button></div></div>
                      <div class="small muted" style="margin-top:6px">${escapeHtml(card.desc||'')}</div>`;
      cardsWrap.appendChild(ci);

      // card events
      ci.querySelector('.del').addEventListener('click', ()=>{
        if (!confirm('Delete card?')) return;
        removeCard(list.id, card.id);
      });
      ci.querySelector('.edit').addEventListener('click', ()=>{
        editCardPrompt(list.id, card.id);
      });

      // drag events
      ci.addEventListener('dragstart', (ev)=>{
        ev.dataTransfer.setData('text/plain', JSON.stringify({fromList: list.id, cardId: card.id}));
        ci.classList.add('dragging');
      });
      ci.addEventListener('dragend', ()=>{ ci.classList.remove('dragging'); });
    });

    // list buttons
    col.querySelectorAll('.add-card').forEach(b=>{
      b.addEventListener('click', ()=> addCardPrompt(list.id));
    });
    col.querySelector('.del-list').addEventListener('click', ()=>{
      if (!confirm('Delete list?')) return;
      board.lists = board.lists.filter(l=>l.id !== list.id);
      saveBoard(board);
      renderBoard(board);
    });

    // allow drop
    col.addEventListener('dragover', (ev)=>{
      ev.preventDefault();
      col.style.outline = '2px dashed rgba(14,165,164,0.18)';
    });
    col.addEventListener('dragleave', ()=>{ col.style.outline=''; });
    col.addEventListener('drop', (ev)=>{
      ev.preventDefault();
      col.style.outline='';
      try{
        const payload = JSON.parse(ev.dataTransfer.getData('text/plain'));
        moveCard(payload.fromList, list.id, payload.cardId);
      }catch(e){ console.error(e); }
    });

    boardView.appendChild(col);
  });

  // add "add list" column
  const addCol = document.createElement('div'); addCol.className='column';
  addCol.style.minWidth='200px'; addCol.style.alignItems='center'; addCol.style.justifyContent='center';
  addCol.innerHTML = `<button id="addListBtn" style="padding:12px;border-radius:10px;border:1px dashed #c7f0ee;background:transparent;cursor:pointer">+ Add list</button>`;
  addCol.querySelector('#addListBtn').addEventListener('click', addListPrompt);
  boardView.appendChild(addCol);
}

/* ---------- Card / List operations ---------- */
function saveBoard(board){
  // update currentUser
  const users = loadUsers();
  const uidx = users.findIndex(x=>x.email===currentUser.email);
  if (uidx>=0){
    const bidx = users[uidx].boards.findIndex(b=>b.id===board.id);
    if (bidx>=0) users[uidx].boards[bidx]=board;
    else users[uidx].boards.push(board);
    saveUsers(users);
    currentUser = users[uidx];
  } else {
    updateUser(currentUser);
  }
}

function addCardPrompt(listId){
  const title = prompt('Card title');
  if (!title) return;
  const desc = prompt('Card description (optional)') || '';
  const board = (currentUser.boards||[]).find(b=>b.id===currentBoardId);
  const list = board.lists.find(l=>l.id===listId);
  list.cards.push({ id: uid(), title: title.trim(), desc: desc.trim() });
  saveBoard(board);
  renderBoard(board);
}

function editCardPrompt(listId, cardId){
  const board = (currentUser.boards||[]).find(b=>b.id===currentBoardId);
  const list = board.lists.find(l=>l.id===listId);
  const card = list.cards.find(c=>c.id===cardId);
  const newTitle = prompt('Edit title', card.title);
  if (newTitle === null) return;
  const newDesc = prompt('Edit description', card.desc||'') || '';
  card.title = newTitle.trim();
  card.desc = newDesc.trim();
  saveBoard(board);
  renderBoard(board);
}

function removeCard(listId, cardId){
  const board = (currentUser.boards||[]).find(b=>b.id===currentBoardId);
  const list = board.lists.find(l=>l.id===listId);
  list.cards = list.cards.filter(c=>c.id !== cardId);
  saveBoard(board);
  renderBoard(board);
}

function moveCard(fromListId, toListId, cardId){
  if (fromListId === toListId) return;
  const board = (currentUser.boards||[]).find(b=>b.id===currentBoardId);
  const from = board.lists.find(l=>l.id===fromListId);
  const to = board.lists.find(l=>l.id===toListId);
  if (!from || !to) return;
  const idx = from.cards.findIndex(c=>c.id===cardId);
  if (idx < 0) return;
  const [card] = from.cards.splice(idx,1);
  to.cards.push(card);
  saveBoard(board);
  renderBoard(board);
}

/* ---------- Add list / rename / delete board ---------- */
function addListPrompt(){
  const title = prompt('List title')?.trim();
  if (!title) return;
  const board = (currentUser.boards||[]).find(b=>b.id===currentBoardId);
  board.lists.push({ id: uid(), title, cards: [] });
  saveBoard(board);
  renderBoard(board);
}

function deleteCurrentBoard(){
  if (!confirm('Delete this board?')) return;
  currentUser.boards = currentUser.boards.filter(b=>b.id !== currentBoardId);
  updateUser(currentUser);
  showDashboard();
}

function renameBoardPrompt(){
  const board = (currentUser.boards||[]).find(b=>b.id===currentBoardId);
  const nv = prompt('New board title', board.title);
  if (nv === null) return;
  board.title = nv.trim() || board.title;
  saveBoard(board);
  boardTitleDisplay.textContent = board.title;
}

/* ---------- Helpers ---------- */
function escapeHtml(s){
  if (!s) return '';
  return s.replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;').replaceAll('"','&quot;');
}
</script>
</body>
</html>
