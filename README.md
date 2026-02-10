<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Advanced UniLMS | Professional Edition</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
/* ================= GLOBAL & THEME VARIABLES ================= */
:root {
  --primary: #6366f1;
  --secondary: #ec4899;
  --success: #22c55e;
  --warning: #f59e0b;
  --danger: #ef4444;
  --bg-app: #f1f5f9;
  --bg-card: #ffffff;
  --sidebar: #1e293b;
  --text-main: #1e293b;
  --text-light: #64748b;
  --transition: all 0.3s ease;
}

body.dark {
  --bg-app: #0f172a;
  --bg-card: #1e293b;
  --text-main: #f8fafc;
  --text-light: #94a3b8;
}

/* ================= BASE STYLES ================= */
* { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
body { margin: 0; background: var(--bg-app); color: var(--text-main); transition: var(--transition); }
.hidden { display: none !important; }
button { cursor: pointer; border: none; transition: var(--transition); }

/* ================= LAYOUT ================= */
header {
  background: var(--bg-card);
  padding: 15px 25px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 2px 10px rgba(0,0,0,0.05);
  position: sticky;
  top: 0;
  z-index: 100;
}

.container { display: flex; }

.sidebar {
  width: 240px;
  background: var(--sidebar);
  color: #fff;
  min-height: 100vh;
  padding-top: 20px;
  transition: var(--transition);
  position: fixed;
  z-index: 200;
}

.sidebar.collapsed { transform: translateX(-100%); }

.sidebar ul { list-style: none; padding: 0; }
.sidebar li {
  padding: 15px 25px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 10px;
  color: #cbd5e1;
}
.sidebar li:hover, .sidebar li.active {
  background: var(--primary);
  color: #fff;
}

main { flex: 1; padding: 30px; margin-left: 240px; transition: var(--transition); }
main.full { margin-left: 0; }

/* ================= COMPONENTS ================= */
.card {
  background: var(--bg-card);
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 12px;
  box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1);
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 20px;
}

/* Advanced Course Styling */
.course-card {
  border-top: 4px solid var(--primary);
  transition: transform 0.2s;
}
.course-card:hover { transform: translateY(-5px); }

.progress-bar {
  background: #e2e8f0;
  height: 8px;
  border-radius: 4px;
  margin: 10px 0;
  overflow: hidden;
}
.progress-fill { background: var(--primary); height: 100%; transition: width 0.5s; }

.status {
  padding: 4px 10px;
  border-radius: 20px;
  font-size: 11px;
  font-weight: bold;
  text-transform: uppercase;
}
.active-status { background: #dcfce7; color: #166534; }
.comp-status { background: #e0f2fe; color: #0369a1; }

.alert {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: var(--sidebar);
  color: #fff;
  padding: 12px 24px;
  border-radius: 8px;
  animation: slideUp 0.3s ease-out;
  z-index: 1000;
}

@keyframes slideUp { from { transform: translateY(100px); } to { transform: translateY(0); } }

/* ================= FORM ELEMENTS ================= */
input {
  width: 100%;
  padding: 12px;
  margin-bottom: 15px;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  background: var(--bg-card);
  color: var(--text-main);
}
.btn-primary {
  background: var(--primary);
  color: white;
  padding: 10px 20px;
  border-radius: 8px;
  font-weight: bold;
}
</style>
</head>

<body>

<div id="alertBox"></div>

<section id="loginPage" style="display: flex; align-items: center; justify-content: center; height: 100vh; background: var(--sidebar);">
  <div class="card" style="width: 350px;">
    <h2 style="text-align:center; color: var(--primary);">UniLMS Login</h2>
    <p style="text-align:center; font-size: 14px; color: var(--text-light);">Enter any credentials to enter</p>
    <input id="username" placeholder="Username">
    <input id="password" type="password" placeholder="Password">
    <div id="loginError" style="color:var(--danger); font-size:12px; margin-bottom:10px;"></div>
    <button id="loginBtn" class="btn-primary" style="width: 100%;">Sign In</button>
  </div>
</section>

<div id="app" class="hidden">
  <nav class="sidebar" id="sidebar">
    <div style="padding: 20px; font-weight: bold; font-size: 20px; color: var(--primary);">LMS PORTAL</div>
    <ul>
      <li data-page="dashboard" class="active">📊 Dashboard</li>
      <li data-page="courses">📚 My Courses</li>
      <li data-page="assignments">📝 Assignments</li>
      <li data-page="profile">👤 Profile</li>
    </ul>
  </nav>

  <div id="main-wrapper" style="width: 100%;">
    <header>
      <button id="menuBtn" style="background:none; font-size: 24px; color: var(--text-main);">☰</button>
      <div style="display:flex; align-items:center; gap: 15px;">
        <button id="themeToggle" style="background:none; font-size: 20px;">🌙</button>
        <span id="userGreet" style="font-weight:bold;">Welcome, Student</span>
      </div>
    </header>

    <main id="mainContent">
      <section id="dashboard">
        <h2>Dashboard Overview</h2>
        <div class="grid" id="dashboardCards"></div>
      </section>

      <section id="courses" class="hidden">
        <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom: 20px;">
          <h2>Advanced Course Catalog</h2>
          <input id="courseSearch" placeholder="Search courses..." style="width: 250px; margin-bottom: 0;">
        </div>
        <div class="grid" id="courseList"></div>
      </section>

      <section id="assignments" class="hidden">
        <h2>Upcoming Assignments</h2>
        <div id="assignmentList"></div>
      </section>

      <section id="profile" class="hidden">
        <h2>Student Profile</h2>
        <div class="card" style="max-width: 500px;">
          <label>Full Name</label>
          <input id="profName" value="John Doe" disabled>
          <label>University Email</label>
          <input id="profEmail" value="john.doe@university.edu" disabled>
          <button id="editProfileBtn" class="btn-primary">Edit Profile</button>
        </div>
      </section>
    </main>
  </div>
</div>

<script>
/* ================= DATA MODELS ================= */
const DATA = {
  stats: [
    { label: "Completed Courses", val: "4", icon: "✅" },
    { label: "Pending Tasks", val: "3", icon: "⏳" },
    { label: "Current GPA", val: "3.85", icon: "🎓" },
    { label: "Attendance", val: "94%", icon: "📅" }
  ],
  courses: [
    { id: 1, title: "Web Architecture", instructor: "Dr. Aris", progress: 85, status: "Active" },
    { id: 2, title: "Data Structures", instructor: "Prof. Snape", progress: 100, status: "Completed" },
    { id: 3, title: "UI/UX Principles", instructor: "Ivy Chen", progress: 40, status: "Active" },
    { id: 4, title: "Database Security", instructor: "Mark Otto", progress: 15, status: "Active" }
  ],
  assignments: [
    { id: 101, title: "JavaScript DOM Lab", due: "2026-02-25", status: "Pending" },
    { id: 102, title: "Final CSS Project", due: "2026-01-10", status: "Late" }
  ]
};

/* ================= CORE UTILS ================= */
const showAlert = (msg) => {
  const alert = document.createElement("div");
  alert.className = "alert";
  alert.textContent = msg;
  document.getElementById('alertBox').appendChild(alert);
  setTimeout(() => alert.remove(), 3000);
};

/* ================= TASK 1: LOGIN ================= */
document.getElementById('loginBtn').addEventListener('click', () => {
  const u = document.getElementById('username').value.trim();
  const p = document.getElementById('password').value.trim();
  if (!u || !p) {
    document.getElementById('loginError').textContent = "Required fields missing!";
    return;
  }
  document.getElementById('loginPage').classList.add('hidden');
  document.getElementById('app').classList.remove('hidden');
  document.getElementById('userGreet').textContent = `Welcome, ${u}`;
  showAlert("Login Successful");
  initApp();
});

/* ================= TASK 2 & 9: NAVIGATION ================= */
menuBtn.addEventListener('click', () => {
  sidebar.classList.toggle('collapsed');
  document.querySelector('main').classList.toggle('full');
});

document.querySelectorAll('.sidebar li').forEach(item => {
  item.addEventListener('click', () => {
    // UI Update
    document.querySelectorAll('.sidebar li').forEach(li => li.classList.remove('active'));
    item.classList.add('active');
    
    // Page Toggle
    const target = item.dataset.page;
    document.querySelectorAll('main section').forEach(s => s.classList.add('hidden'));
    document.getElementById(target).classList.remove('hidden');
  });
});

/* ================= TASK 3, 4, 5: CONTENT RENDERING ================= */
function initApp() {
  // Render Dashboard
  const dashGrid = document.getElementById('dashboardCards');
  dashGrid.innerHTML = "";
  DATA.stats.forEach(s => {
    dashGrid.innerHTML += `
      <div class="card" style="text-align:center">
        <div style="font-size:24px">${s.icon}</div>
        <h2 style="margin:10px 0">${s.val}</h2>
        <p style="color:var(--text-light); margin:0">${s.label}</p>
      </div>`;
  });

  renderCourses();
  renderAssignments();
}

// Advanced Course Rendering (Task 4)
function renderCourses(filter = "") {
  const list = document.getElementById('courseList');
  list.innerHTML = "";
  
  const filtered = DATA.courses.filter(c => c.title.toLowerCase().includes(filter.toLowerCase()));
  
  filtered.forEach(c => {
    const card = document.createElement('div');
    card.className = "card course-card";
    const statusCls = c.status === "Active" ? "active-status" : "comp-status";
    
    card.innerHTML = `
      <div style="display:flex; justify-content:space-between; margin-bottom:10px">
        <span class="status ${statusCls}">${c.status}</span>
        <small style="color:var(--text-light)">ID: #00${c.id}</small>
      </div>
      <h3 style="margin:0">${c.title}</h3>
      <p style="font-size:13px; color:var(--text-light)">Instructor: ${c.instructor}</p>
      <div class="progress-bar"><div class="progress-fill" style="width:${c.progress}%"></div></div>
      <small>Course Progress: ${c.progress}%</small>
    `;
    list.appendChild(card);
  });
}

// Assignment Logic (Task 5 & 6)
function renderAssignments() {
  const list = document.getElementById('assignmentList');
  list.innerHTML = "";
  
  DATA.assignments.forEach(a => {
    const div = document.createElement('div');
    div.className = "card";
    const isLate = new Date() > new Date(a.due) && a.status === "Pending";
    
    div.innerHTML = `
      <div style="display:flex; justify-content:space-between; align-items:center">
        <div>
          <h4 style="margin:0">${a.title}</h4>
          <small>Deadline: ${a.due}</small>
        </div>
        <div style="text-align:right">
          <span class="status ${isLate ? 'comp-status' : 'active-status'}" style="background:${isLate?'#fee2e2':'#fef3c7'}; color:${isLate?'#991b1b':'#92400e'}">
            ${isLate ? 'LATE' : a.status}
          </span>
          <button class="btn-primary" style="margin-left:10px" id="sub-${a.id}" ${a.status === 'Submitted' ? 'disabled' : ''}>
            ${a.status === 'Submitted' ? 'Turned In' : 'Submit'}
          </button>
        </div>
      </div>
    `;
    
    div.querySelector('button').onclick = (e) => {
      a.status = "Submitted";
      showAlert("Assignment Submitted Successfully!");
      renderAssignments();
    };
    list.appendChild(div);
  });
}

// Search Filter (Task 4 requirement)
document.getElementById('courseSearch').addEventListener('input', (e) => {
  renderCourses(e.target.value);
});

/* ================= TASK 7: PROFILE ================= */
let isEditing = false;
document.getElementById('editProfileBtn').addEventListener('click', (e) => {
  isEditing = !isEditing;
  document.getElementById('profName').disabled = !isEditing;
  document.getElementById('profEmail').disabled = !isEditing;
  e.target.textContent = isEditing ? "Save Changes" : "Edit Profile";
  if(!isEditing) showAlert("Profile Information Updated");
});

/* ================= TASK 8: THEME ================= */
const themeToggle = document.getElementById('themeToggle');
const setTheme = (t) => {
  document.body.classList.toggle('dark', t === 'dark');
  themeToggle.textContent = t === 'dark' ? "☀️" : "🌙";
  localStorage.setItem('lms_theme', t);
};

themeToggle.addEventListener('click', () => {
  const current = document.body.classList.contains('dark') ? 'light' : 'dark';
  setTheme(current);
});

// Init theme on load
setTheme(localStorage.getItem('lms_theme') || 'light');

</script>
</body>
</html>
