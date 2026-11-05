
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Notes - Mac Style</title>
<style>
* { box-sizing: border-box; }
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", sans-serif;
  background: #f5f5f7;
  color: #222;
  height: 100vh;
  overflow: hidden;
}
.app {
  display: flex;
  height: 100vh;
  background: #fff;
}
.sidebar {
  width: 245px;
  background: #f2f2f4;
  padding: 22px 15px;
  border-right: 1px solid #ddd;
  display: flex;
  flex-direction: column;
}
.brand {
  font-size: 21px;
  font-weight: 700;
  margin: 5px 8px 25px;
}
.search {
  width: 100%;
  padding: 10px;
  border: none;
  border-radius: 9px;
  background: #e4e4e7;
  outline: none;
  margin-bottom: 22px;
}
.section {
  font-size: 12px;
  color: #888;
  font-weight: bold;
  margin: 12px 8px;
  text-transform: uppercase;
}
.folder {
  padding: 10px;
  border-radius: 8px;
  cursor: pointer;
  margin-bottom: 4px;
  color: #444;
}
.folder.active, .folder:hover {
  background: #dedee2;
}
.sidebar-bottom {
  margin-top: auto;
  display: flex;
  gap: 8px;
}
button {
  border: none;
  cursor: pointer;
  font: inherit;
}
.side-button {
  background: #e2e2e5;
  padding: 10px;
  border-radius: 9px;
  flex: 1;
}
.notes-list {
  width: 270px;
  border-right: 1px solid #eee;
  padding: 20px 12px;
  overflow-y: auto;
}
.list-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 8px 15px;
}
.list-header h3 { font-size: 17px; }
.add {
  background: #f5c94e;
  border-radius: 8px;
  padding: 8px 12px;
  font-size: 19px;
}
.note-card {
  padding: 13px;
  border-radius: 10px;
  cursor: pointer;
  margin-bottom: 5px; 
}
.note-card.active, .note-card:hover {
  background: #f2e7c7;
}
.note-card strong {
  display: block;
  font-size: 14px;
  margin-bottom: 5px;
}
.note-card small {
  color: #888;
  font-size: 11px;
}
.note-card p {
  color: #777;
  font-size: 12px;
  margin: 5px 0 0;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.editor {
  flex: 1;
  padding: 35px 7%;
  overflow-y: auto;
  position: relative;
}
.toolbar {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  margin-bottom: 35px;
}
.tool {
  background: #f1f1f1;
  padding: 9px 13px;
  border-radius: 8px;
}
#title {
  width: 100%;
  border: none;
  outline: none;
  font-size: 30px;
  font-weight: bold;
  margin-bottom: 12px;
  background: transparent;
  color: inherit;
}
#content {
  width: 100%;
  min-height: 60vh;
  outline: none;
  font-size: 16px;
  line-height: 1.8;
  color: inherit;
  white-space: pre-wrap;
  overflow-wrap: anywhere;
}
#content:empty:before {
  content: "Empieza a escribir...";
  color: #aaa;
}
.date {
  color: #999;
  font-size: 12px;
  margin-bottom: 25px;
}
.empty {
  color: #999;
  text-align: center;
  margin-top: 100px;
}
body.dark {
  background: #202020;
  color: #eee;
}
.dark .app { background: #242424; }
.dark .sidebar { background: #303030; border-color: #444; }
.dark .notes-list { border-color: #444; }
.dark .search, .dark .side-button, .dark .tool { background: #414141; color: white; }
.dark .folder.active, .dark .folder:hover { background: #484848; }
.dark .note-card.active, .dark .note-card:hover { background: #51482f; }
.dark #title, .dark #content { color: #eee; }
@media(max-width:650px) {
  .sidebar { width: 65px; padding: 15px 7px; }
  .brand { font-size: 0; }
  .brand:after { content: "🍎"; font-size: 22px; }
  .search, .section, .folder span { display: none; }
  .folder { text-align: center; }
  .notes-list { width: 125px; padding: 12px 5px; }
  .editor { padding: 25px 15px; }
  #title { font-size: 23px; }
  .note-card { padding: 9px 5px; }
}
</style>
</head>
<body>

<div class="app">

  <aside class="sidebar">
    <div class="brand">Notes</div>
    <input class="search" id="search"
      placeholder="⌕ Buscar notas..."
      oninput="renderNotes()">
    <div class="section">Carpetas</div>
    <div class="folder active" onclick="setFolder('Todas')">
      📁 <span>Todas las notas</span>
    </div>
    <div class="folder" onclick="setFolder('Personal')">
      👤 <span>Personal</span>
    </div>
    <div class="folder" onclick="setFolder('Ideas')">
      💡 <span>Ideas</span>
    </div>
    <div class="folder" onclick="setFolder('Trabajo')">
      💼 <span>Trabajo</span>
    </div>
    <div class="sidebar-bottom">
      <button class="side-button" onclick="toggleTheme()">☾</button>
      <button class="side-button" onclick="newFolder()">＋</button>
    </div>
  </aside>

  <section class="notes-list">
    <div class="list-header">
      <h3>Notas</h3>
      <button class="add" onclick="newNote()">＋</button>
    </div>
    <div id="notes"></div>
  </section>

  <main class="editor">
    <div class="toolbar">
      <button class="tool" onclick="formatText('bold')">B</button>
      <button class="tool" onclick="formatText('italic')"><i>I</i></button>
      <button class="tool" onclick="formatText('insertUnorderedList')">☷</button>
      <button class="tool" onclick="deleteNote()">🗑</button>
    </div>
    <div class="date" id="date"></div>
    <input id="title" placeholder="Título de la nota">
    <div id="content" contenteditable="true"></div>
  </main>

</div>

<script>
let notes = JSON.parse(localStorage.getItem("macNotes") || "[]");
let current = null;
let folder = "Todas";

function save() {
  localStorage.setItem("macNotes", JSON.stringify(notes));
}

function newNote() {
  const note = {
    id: Date.now(),
    title: "Nueva nota",
    content: "",
    folder: folder === "Todas" ? "Personal" : folder,
    pinned: false,
    date: new Date().toLocaleString()
  };

  notes.unshift(note);
  current = note.id;
  save();
  renderNotes();
  loadNote();
  document.getElementById("title").focus();
}

function loadNote() {
  const note = notes.find(n => n.id === current);

  if (!note) {
    document.getElementById("title").value = "";
    document.getElementById("content").innerHTML = "";
    document.getElementById("date").textContent = "";
    return;
  }

  document.getElementById("title").value = note.title;
  document.getElementById("content").innerHTML = note.content;
  document.getElementById("date").textContent =
    "Última modificación: " + note.date;
}

function updateNote() {
  const note = notes.find(n => n.id === current);
  if (!note) return;

  note.title = document.getElementById("title").value;
  note.content = document.getElementById("content").innerHTML;
  note.date = new Date().toLocaleString();

  save();
  renderNotes();
  document.getElementById("date").textContent =
    "Última modificación: " + note.date;
}

document.getElementById("title").addEventListener("input", updateNote);
document.getElementById("content").addEventListener("input", updateNote);

function renderNotes() {
  const container = document.getElementById("notes");
  const search = document.getElementById("search").value.toLowerCase();

  container.innerHTML = "";

  let filtered = notes.filter(n => {
    const matchesFolder = folder === "Todas" || n.folder === folder;
    const matchesSearch =
      n.title.toLowerCase().includes(search) ||
      n.content.toLowerCase().includes(search);

    return matchesFolder && matchesSearch;
  });

  filtered.sort((a,b) => Number(b.pinned)-Number(a.pinned));

  filtered.forEach(note => {
    const card = document.createElement("div");
    card.className = "note-card" +
      (note.id === current ? " active" : "");

    const title = document.createElement("strong");
    title.textContent = (note.pinned ? "📌 " : "") +
      (note.title || "Sin título");

    const date = document.createElement("small");
    date.textContent = note.date;

    const preview = document.createElement("p");
    preview.textContent = note.content.replace(/<[^>]*>/g, "");

    card.append(title, date, preview);

    card.onclick = () => {
      current = note.id;
      loadNote();
      renderNotes();
    };

    card.oncontextmenu = e => {
      e.preventDefault();
      note.pinned = !note.pinned;
      save();
      renderNotes();
    };

    container.appendChild(card);
  });

  if (!filtered.length) {
    container.innerHTML = '<p class="empty">Sin notas</p>';
  }
}

function setFolder(name) {
  folder = name;
  document.querySelectorAll(".folder").forEach(el =>
    el.classList.remove("active")
  );
  event.currentTarget.classList.add("active");
  renderNotes();
}

function deleteNote() {
  if (!current) return;
  notes = notes.filter(n => n.id !== current);
  current = null;
  save();
  loadNote();
  renderNotes();
}

function toggleTheme() {
  document.body.classList.toggle("dark");
  localStorage.setItem(
    "macTheme",
    document.body.classList.contains("dark") ? "dark" : "light"
  );
}

function formatText(command) {
  document.getElementById("content").focus();
  document.execCommand(command, false, null);
  updateNote();
}

function newFolder() {
  const name = prompt("Nombre de la nueva carpeta:");
  if (!name) return;

  const element = document.createElement("div");
  element.className = "folder";
  element.textContent = "📁 " + name;
  element.onclick = () => setFolder(name);

  document.querySelector(".section").after(element);
}

if (localStorage.getItem("macTheme") === "dark") {
  document.body.classList.add("dark");
}

if (notes.length) {
  current = notes[0].id;
  loadNote();
} else {
  newNote();
}

renderNotes();
</script>
</body>
</html>
