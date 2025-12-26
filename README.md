<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Spooky Tasks Tracker 🎃</title>
  <link href="https://fonts.googleapis.com/css2?family=Creepster&family=Roboto:wght@400;700&display=swap" rel="stylesheet">
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: 'Roboto', sans-serif;
      background: linear-gradient(to bottom, #0a0e27 0%, #1a1a2e 50%, #16213e 100%);
      color: #f0f0f0;
      min-height: 100vh;
      position: relative;
      overflow-x: hidden;
    }

    .stars {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 0;
    }

    .star {
      position: absolute;
      background: white;
      border-radius: 50%;
      animation: twinkle 3s infinite;
    }

    @keyframes twinkle {
      0%, 100% { opacity: 0.3; }
      50% { opacity: 1; }
    }

    @keyframes float {
      0%, 100% { transform: translateY(0px); }
      50% { transform: translateY(-20px); }
    }

    .spooky-element {
      position: fixed;
      font-size: 2.5rem;
      pointer-events: none;
      z-index: 0;
      opacity: 0;
      transition: opacity 0.5s;
    }

    .spooky-element.visible {
      opacity: 1;
    }

    .spooky-element.celebrate {
      animation: flyUp 1.5s ease-out forwards !important;
    }

    @keyframes flyUp {
      0% {
        transform: translateY(0) rotate(0deg) scale(1);
        opacity: 1;
      }
      100% {
        transform: translateY(-120vh) rotate(720deg) scale(0.3);
        opacity: 0;
      }
    }

    @keyframes gentleFloat {
      0%, 100% {
        transform: translateY(0px) translateX(0px);
      }
      25% {
        transform: translateY(-10px) translateX(5px);
      }
      50% {
        transform: translateY(-5px) translateX(-5px);
      }
      75% {
        transform: translateY(-15px) translateX(3px);
      }
    }

    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
      position: relative;
      z-index: 1;
    }

    header {
      text-align: center;
      padding: 40px 20px;
      background: rgba(0, 0, 0, 0.5);
      border-radius: 20px;
      margin-bottom: 30px;
      border: 2px solid #ff6b35;
      box-shadow: 0 0 30px rgba(255, 107, 53, 0.3);
    }

    h1 {
      font-family: 'Creepster', cursive;
      font-size: 3.5rem;
      color: #ff6b35;
      text-shadow: 0 0 20px rgba(255, 107, 53, 0.8), 0 0 40px rgba(255, 107, 53, 0.4);
      margin-bottom: 10px;
      animation: float 3s ease-in-out infinite;
    }

    .subtitle {
      color: #bb86fc;
      font-size: 1.2rem;
    }

    .task-form {
      background: rgba(0, 0, 0, 0.6);
      padding: 30px;
      border-radius: 15px;
      margin-bottom: 30px;
      border: 2px solid #bb86fc;
      box-shadow: 0 0 20px rgba(187, 134, 252, 0.2);
    }

    .form-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 15px;
      margin-bottom: 15px;
    }

    .form-group {
      display: flex;
      flex-direction: column;
    }

    .form-group.full-width {
      grid-column: 1 / -1;
    }

    label {
      color: #bb86fc;
      margin-bottom: 5px;
      font-weight: bold;
      font-size: 0.9rem;
    }

    input, textarea, select {
      background: rgba(255, 255, 255, 0.1);
      border: 1px solid #444;
      border-radius: 8px;
      padding: 10px;
      color: #f0f0f0;
      font-size: 1rem;
      transition: all 0.3s;
    }

    input:focus, textarea:focus, select:focus {
      outline: none;
      border-color: #ff6b35;
      box-shadow: 0 0 10px rgba(255, 107, 53, 0.3);
    }

    textarea {
      resize: vertical;
      min-height: 60px;
    }

    .btn {
      background: linear-gradient(135deg, #ff6b35 0%, #f7931e 100%);
      color: white;
      border: none;
      padding: 12px 30px;
      border-radius: 25px;
      font-size: 1.1rem;
      font-weight: bold;
      cursor: pointer;
      transition: all 0.3s;
      box-shadow: 0 4px 15px rgba(255, 107, 53, 0.4);
    }

    .btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 20px rgba(255, 107, 53, 0.6);
    }

    .filters {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
      flex-wrap: wrap;
      align-items: center;
    }

    .filter-btn {
      background: rgba(187, 134, 252, 0.2);
      color: #bb86fc;
      border: 1px solid #bb86fc;
      padding: 8px 20px;
      border-radius: 20px;
      cursor: pointer;
      transition: all 0.3s;
      font-size: 0.9rem;
    }

    .filter-btn:hover, .filter-btn.active {
      background: #bb86fc;
      color: #0a0e27;
    }

    .search-box {
      flex: 1;
      min-width: 200px;
    }

    .tasks-container {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
      gap: 20px;
      margin-bottom: 40px;
    }

    .task-card {
      background: rgba(0, 0, 0, 0.7);
      border-radius: 15px;
      padding: 20px;
      border: 2px solid #444;
      transition: all 0.3s;
      position: relative;
      overflow: hidden;
    }

    .task-card::before {
      content: '';
      position: absolute;
      top: -2px;
      left: -2px;
      right: -2px;
      bottom: -2px;
      background: linear-gradient(45deg, #ff6b35, #bb86fc);
      border-radius: 15px;
      opacity: 0;
      transition: opacity 0.3s;
      z-index: -1;
    }

    .task-card:hover::before {
      opacity: 0.5;
    }

    .task-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 10px 30px rgba(255, 107, 53, 0.3);
    }

    .task-card.completed {
      opacity: 0.6;
      border-color: #666;
    }

    .task-card.completed .task-title {
      text-decoration: line-through;
    }

    .task-header {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      margin-bottom: 10px;
    }

    .task-title {
      font-size: 1.3rem;
      font-weight: bold;
      color: #ff6b35;
      flex: 1;
      word-break: break-word;
    }

    .priority-badge {
      padding: 4px 12px;
      border-radius: 12px;
      font-size: 0.8rem;
      font-weight: bold;
      margin-left: 10px;
    }

    .priority-high { background: #ff4444; color: white; }
    .priority-medium { background: #ffa500; color: white; }
    .priority-low { background: #4caf50; color: white; }

    .task-description {
      color: #ccc;
      margin-bottom: 15px;
      line-height: 1.5;
    }

    .task-meta {
      display: flex;
      flex-direction: column;
      gap: 8px;
      margin-bottom: 15px;
      font-size: 0.9rem;
    }

    .meta-item {
      display: flex;
      align-items: center;
      gap: 8px;
      color: #bb86fc;
    }

    .countdown {
      color: #ff6b35;
      font-weight: bold;
      animation: pulse 2s infinite;
    }

    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.7; }
    }

    .countdown.urgent {
      color: #ff4444;
      animation: pulse 0.5s infinite;
    }

    .task-actions {
      display: flex;
      gap: 10px;
    }

    .task-actions button {
      flex: 1;
      padding: 8px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 0.9rem;
      transition: all 0.3s;
    }

    .complete-btn {
      background: #4caf50;
      color: white;
    }

    .complete-btn:hover {
      background: #45a049;
    }

    .edit-btn {
      background: #2196f3;
      color: white;
    }

    .edit-btn:hover {
      background: #0b7dda;
    }

    .delete-btn {
      background: #f44336;
      color: white;
    }

    .delete-btn:hover {
      background: #da190b;
    }

    .graveyard {
      margin-top: 40px;
      padding-top: 40px;
      border-top: 2px solid #444;
    }

    .graveyard-title {
      font-family: 'Creepster', cursive;
      font-size: 2rem;
      color: #666;
      text-align: center;
      margin-bottom: 20px;
    }

    .modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.8);
      z-index: 1000;
      justify-content: center;
      align-items: center;
    }

    .modal.active {
      display: flex;
    }

    .modal-content {
      background: #1a1a2e;
      padding: 30px;
      border-radius: 15px;
      border: 2px solid #bb86fc;
      max-width: 600px;
      width: 90%;
      max-height: 90vh;
      overflow-y: auto;
    }

    .modal-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
    }

    .modal-title {
      font-size: 1.5rem;
      color: #ff6b35;
    }

    .close-btn {
      background: none;
      border: none;
      color: #f0f0f0;
      font-size: 1.5rem;
      cursor: pointer;
    }

    .empty-state {
      text-align: center;
      padding: 60px 20px;
      color: #666;
    }

    .empty-state-icon {
      font-size: 4rem;
      margin-bottom: 20px;
    }

    @media (max-width: 768px) {
      h1 {
        font-size: 2.5rem;
      }

      .form-grid {
        grid-template-columns: 1fr;
      }

      .tasks-container {
        grid-template-columns: 1fr;
      }

      .filters {
        flex-direction: column;
        align-items: stretch;
      }
    }
  </style>
</head>
<body>
  <div class="stars" id="stars"></div>
  <div id="spookyElements"></div>
  
  <div class="container">
    <header>
      <h1>🎃 Spooky Tasks Tracker 👻</h1>
      <p class="subtitle">Don't let deadlines haunt you!</p>
    </header>

    <div class="task-form">
      <form id="taskForm">
        <div class="form-grid">
          <div class="form-group full-width">
            <label for="title">Task Title *</label>
            <input type="text" id="title" required placeholder="Enter your spooky task...">
          </div>

          <div class="form-group full-width">
            <label for="description">Description</label>
            <textarea id="description" placeholder="Add details (optional)"></textarea>
          </div>

          <div class="form-group">
            <label for="category">Category *</label>
            <select id="category" required>
              <option value="Assignments">📚 Assignments</option>
              <option value="Deadlines">⏰ Deadlines</option>
              <option value="Reminders">🔔 Reminders</option>
              <option value="Personal">👤 Personal</option>
              <option value="Additional Works">✨ Additional Works</option>
            </select>
          </div>

          <div class="form-group">
            <label for="priority">Priority *</label>
            <select id="priority" required>
              <option value="low">🍬 Low</option>
              <option value="medium">🎃 Medium</option>
              <option value="high">💀 High</option>
            </select>
          </div>

          <div class="form-group">
            <label for="dueDate">Due Date *</label>
            <input type="date" id="dueDate" required>
          </div>

          <div class="form-group">
            <label for="dueTime">Due Time *</label>
            <input type="time" id="dueTime" required>
          </div>
        </div>

        <button type="submit" class="btn">✨ Add Task</button>
      </form>
    </div>

    <div class="filters">
      <button class="filter-btn active" data-filter="all">All Tasks</button>
      <button class="filter-btn" data-filter="Assignments">Assignments</button>
      <button class="filter-btn" data-filter="Deadlines">Deadlines</button>
      <button class="filter-btn" data-filter="Reminders">Reminders</button>
      <button class="filter-btn" data-filter="Personal">Personal</button>
      <input type="text" class="search-box" id="searchBox" placeholder="🔍 Search tasks...">
    </div>

    <div id="activeTasks" class="tasks-container"></div>

    <div class="graveyard">
      <h2 class="graveyard-title">⚰️ Graveyard (Completed Tasks)</h2>
      <div id="completedTasks" class="tasks-container"></div>
    </div>
  </div>

  <div class="modal" id="editModal">
    <div class="modal-content">
      <div class="modal-header">
        <h2 class="modal-title">Edit Task</h2>
        <button class="close-btn" onclick="closeEditModal()">×</button>
      </div>
      <form id="editForm">
        <div class="form-grid">
          <div class="form-group full-width">
            <label for="editTitle">Task Title *</label>
            <input type="text" id="editTitle" required>
          </div>

          <div class="form-group full-width">
            <label for="editDescription">Description</label>
            <textarea id="editDescription"></textarea>
          </div>

          <div class="form-group">
            <label for="editCategory">Category *</label>
            <select id="editCategory" required>
              <option value="Assignments">📚 Assignments</option>
              <option value="Deadlines">⏰ Deadlines</option>
              <option value="Reminders">🔔 Reminders</option>
              <option value="Personal">👤 Personal</option>
              <option value="Additional Works">✨ Additional Works</option>
            </select>
          </div>

          <div class="form-group">
            <label for="editPriority">Priority *</label>
            <select id="editPriority" required>
              <option value="low">🍬 Low</option>
              <option value="medium">🎃 Medium</option>
              <option value="high">💀 High</option>
            </select>
          </div>

          <div class="form-group">
            <label for="editDueDate">Due Date *</label>
            <input type="date" id="editDueDate" required>
          </div>

          <div class="form-group">
            <label for="editDueTime">Due Time *</label>
            <input type="time" id="editDueTime" required>
          </div>
        </div>

        <button type="submit" class="btn">💾 Save Changes</button>
      </form>
    </div>
  </div>

  <script>
    let tasks = [];
    let editingTaskId = null;
    let currentFilter = 'all';
    let searchQuery = '';
    const spookyEmojis = ['🎃', '👻', '🎃', '👻', '🎃', '👻'];

    // Generate stars
    const starsContainer = document.getElementById('stars');
    for (let i = 0; i < 50; i++) {
      const star = document.createElement('div');
      star.className = 'star';
      star.style.width = Math.random() * 3 + 'px';
      star.style.height = star.style.width;
      star.style.left = Math.random() * 100 + '%';
      star.style.top = Math.random() * 100 + '%';
      star.style.animationDelay = Math.random() * 3 + 's';
      starsContainer.appendChild(star);
    }

    // Generate floating pumpkins and ghosts
    function createSpookyElements() {
      const container = document.getElementById('spookyElements');
      container.innerHTML = '';
      
      for (let i = 0; i < 8; i++) {
        const element = document.createElement('div');
        element.className = 'spooky-element';
        element.textContent = spookyEmojis[i % spookyEmojis.length];
        element.style.left = Math.random() * 90 + 5 + '%';
        element.style.top = Math.random() * 80 + 10 + '%';
        element.style.animationDelay = Math.random() * 2 + 's';
        element.style.animationDuration = (Math.random() * 3 + 4) + 's';
        container.appendChild(element);
        
        setTimeout(() => {
          element.classList.add('visible');
          element.style.animation = `gentleFloat ${Math.random() * 3 + 4}s ease-in-out infinite`;
        }, Math.random() * 500);
      }
    }

    // Celebration animation
    function celebrate() {
      const container = document.getElementById('spookyElements');
      const elements = container.querySelectorAll('.spooky-element');
      
      elements.forEach((el, i) => {
        setTimeout(() => {
          el.classList.add('celebrate');
        }, i * 100);
      });

      setTimeout(() => {
        createSpookyElements();
      }, 2000);
    }

    // Load tasks from memory
    function loadTasks() {
      const stored = {};
      tasks = stored.tasks || [];
      renderTasks();
    }

    // Save tasks to memory
    function saveTasks() {
      // Tasks are already in memory, no action needed
    }

    // Add task
    document.getElementById('taskForm').addEventListener('submit', (e) => {
      e.preventDefault();
      
      const task = {
        id: Date.now(),
        title: document.getElementById('title').value,
        description: document.getElementById('description').value,
        category: document.getElementById('category').value,
        priority: document.getElementById('priority').value,
        dueDate: document.getElementById('dueDate').value,
        dueTime: document.getElementById('dueTime').value,
        completed: false,
        createdAt: new Date().toISOString()
      };

      tasks.push(task);
      saveTasks();
      renderTasks();
      celebrate();
      e.target.reset();
    });

    // Render tasks
    function renderTasks() {
      const activeTasks = tasks.filter(t => !t.completed);
      const completedTasks = tasks.filter(t => t.completed);

      renderTaskList(activeTasks, 'activeTasks');
      renderTaskList(completedTasks, 'completedTasks');
    }

    function renderTaskList(taskList, containerId) {
      const container = document.getElementById(containerId);
      
      // Apply filters
      let filtered = taskList;
      
      if (currentFilter !== 'all') {
        filtered = filtered.filter(t => t.category === currentFilter);
      }
      
      if (searchQuery) {
        filtered = filtered.filter(t => 
          t.title.toLowerCase().includes(searchQuery.toLowerCase()) ||
          t.description.toLowerCase().includes(searchQuery.toLowerCase())
        );
      }

      // Sort by due date
      filtered.sort((a, b) => {
        const dateA = new Date(`${a.dueDate}T${a.dueTime}`);
        const dateB = new Date(`${b.dueDate}T${b.dueTime}`);
        return dateA - dateB;
      });

      if (filtered.length === 0) {
        container.innerHTML = `
          <div class="empty-state">
            <div class="empty-state-icon">${containerId === 'activeTasks' ? '👻' : '⚰️'}</div>
            <p>${containerId === 'activeTasks' ? 'No active tasks. Add one to get started!' : 'No completed tasks yet.'}</p>
          </div>
        `;
        return;
      }

      container.innerHTML = filtered.map(task => createTaskCard(task)).join('');
    }

    function createTaskCard(task) {
      const dueDateTime = new Date(`${task.dueDate}T${task.dueTime}`);
      const countdown = getCountdown(dueDateTime);
      const isUrgent = countdown.isUrgent;

      return `
        <div class="task-card ${task.completed ? 'completed' : ''}">
          <div class="task-header">
            <div class="task-title">${task.title}</div>
            <span class="priority-badge priority-${task.priority}">${task.priority.toUpperCase()}</span>
          </div>
          
          ${task.description ? `<div class="task-description">${task.description}</div>` : ''}
          
          <div class="task-meta">
            <div class="meta-item">📁 ${task.category}</div>
            <div class="meta-item">📅 ${formatDate(dueDateTime)}</div>
            <div class="meta-item countdown ${isUrgent ? 'urgent' : ''}">
              ⏰ ${countdown.text}
            </div>
          </div>

          <div class="task-actions">
            <button class="complete-btn" onclick="toggleComplete(${task.id})">
              ${task.completed ? '↩️ Restore' : '✓ Complete'}
            </button>
            <button class="edit-btn" onclick="editTask(${task.id})">✏️ Edit</button>
            <button class="delete-btn" onclick="deleteTask(${task.id})">🗑️ Delete</button>
          </div>
        </div>
      `;
    }

    function getCountdown(dueDate) {
      const now = new Date();
      const diff = dueDate - now;
      
      if (diff < 0) {
        return { text: 'OVERDUE! 💀', isUrgent: true };
      }

      const hours = Math.floor(diff / (1000 * 60 * 60));
      const days = Math.floor(hours / 24);
      const remainingHours = hours % 24;
      const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));

      const isUrgent = hours < 24;

      if (days > 0) {
        return { text: `${days}d ${remainingHours}h left`, isUrgent };
      } else if (hours > 0) {
        return { text: `${hours}h ${minutes}m left`, isUrgent };
      } else {
        return { text: `${minutes}m left`, isUrgent };
      }
    }

    function formatDate(date) {
      return date.toLocaleString('en-US', {
        month: 'short',
        day: 'numeric',
        year: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
      });
    }

    function toggleComplete(id) {
      const task = tasks.find(t => t.id === id);
      if (task) {
        task.completed = !task.completed;
        saveTasks();
        renderTasks();
        celebrate();
      }
    }

    function deleteTask(id) {
      if (confirm('Banish this task to the underworld? 👻')) {
        tasks = tasks.filter(t => t.id !== id);
        saveTasks();
        renderTasks();
      }
    }

    function editTask(id) {
      const task = tasks.find(t => t.id === id);
      if (!task) return;

      editingTaskId = id;
      document.getElementById('editTitle').value = task.title;
      document.getElementById('editDescription').value = task.description;
      document.getElementById('editCategory').value = task.category;
      document.getElementById('editPriority').value = task.priority;
      document.getElementById('editDueDate').value = task.dueDate;
      document.getElementById('editDueTime').value = task.dueTime;

      document.getElementById('editModal').classList.add('active');
    }

    function closeEditModal() {
      document.getElementById('editModal').classList.remove('active');
      editingTaskId = null;
    }

    document.getElementById('editForm').addEventListener('submit', (e) => {
      e.preventDefault();
      
      const task = tasks.find(t => t.id === editingTaskId);
      if (task) {
        task.title = document.getElementById('editTitle').value;
        task.description = document.getElementById('editDescription').value;
        task.category = document.getElementById('editCategory').value;
        task.priority = document.getElementById('editPriority').value;
        task.dueDate = document.getElementById('editDueDate').value;
        task.dueTime = document.getElementById('editDueTime').value;

        saveTasks();
        renderTasks();
        closeEditModal();
      }
    });

    // Filters
    document.querySelectorAll('.filter-btn').forEach(btn => {
      btn.addEventListener('click', () => {
        document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        currentFilter = btn.dataset.filter;
        renderTasks();
      });
    });

    // Search
    document.getElementById('searchBox').addEventListener('input', (e) => {
      searchQuery = e.target.value;
      renderTasks();
    });

    // Update countdowns every minute
    setInterval(renderTasks, 60000);

    // Initialize
    loadTasks();
    createSpookyElements();
  </script>
</body>
</html>
