<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Produtiv.me - Diário de Produtividade</title>
  <style>
    * {
      box-sizing: border-box;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      margin: 0; 
      padding: 0;
      background: var(--bg); 
      color: var(--text);
      transition: all 0.3s ease;
      min-height: 100vh;
      -webkit-touch-callout: none;
      -webkit-user-select: none;
      -khtml-user-select: none;
      -moz-user-select: none;
      -ms-user-select: none;
      user-select: none;
    }

    :root {
      --bg: #f8f9fa;
      --card: #ffffff;
      --text: #212529;
      --primary: #28a745;
      --secondary: #e9ecef;
      --danger: #dc3545;
      --warning: #ffc107;
      --shadow: rgba(0,0,0,0.1);
    }

    [data-theme="dark"] {
      --bg: #0d1117;
      --card: #161b22;
      --text: #f0f6fc;
      --primary: #238636;
      --secondary: #30363d;
      --danger: #f85149;
      --warning: #d29922;
      --shadow: rgba(0,0,0,0.3);
    }

    header {
      text-align: center;
      padding: 1.5rem 1rem;
      background: var(--primary);
      color: white;
      position: sticky;
      top: 0;
      z-index: 100;
      box-shadow: 0 2px 10px var(--shadow);
    }

    header h1 {
      margin: 0;
      font-size: 1.8rem;
      font-weight: 700;
    }

    header p {
      margin: 0.5rem 0;
      opacity: 0.9;
      font-size: 0.9rem;
    }

    #currentDate {
      font-size: 0.85rem;
      margin-top: 0.5rem;
      opacity: 0.8;
      font-weight: 500;
    }

    .container {
      max-width: 100%;
      margin: 0;
      padding: 1rem;
      padding-bottom: 2rem;
    }

    .card {
      background: var(--card);
      padding: 1.2rem;
      border-radius: 16px;
      margin-bottom: 1rem;
      box-shadow: 0 2px 12px var(--shadow);
      border: 1px solid var(--secondary);
    }

    .task-input {
      display: flex;
      flex-direction: column;
      gap: 1rem;
    }

    .task-input input {
      width: 100%;
      padding: 1rem;
      border: 2px solid var(--secondary);
      border-radius: 12px;
      background: var(--card);
      color: var(--text);
      font-size: 1rem;
      outline: none;
      transition: border-color 0.2s;
    }

    .task-input input:focus {
      border-color: var(--primary);
    }

    .task-input button {
      width: 100%;
      padding: 1rem;
      background: var(--primary);
      border: none;
      border-radius: 12px;
      color: white;
      cursor: pointer;
      font-size: 1rem;
      font-weight: 600;
      transition: all 0.2s;
      touch-action: manipulation;
    }

    .task-input button:active {
      transform: scale(0.98);
      background: #218838;
    }

    .tasks {
      list-style: none;
      padding: 0;
      margin: 0;
    }

    .tasks li {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1rem 0;
      border-bottom: 1px solid var(--secondary);
      min-height: 60px;
    }

    .tasks li:last-child {
      border-bottom: none;
    }

    .tasks li span {
      cursor: pointer;
      flex: 1;
      margin-right: 1rem;
      font-size: 1rem;
      line-height: 1.4;
      padding: 0.5rem 0;
      word-break: break-word;
    }

    .tasks li.completed span {
      text-decoration: line-through;
      color: #6c757d;
      opacity: 0.7;
    }

    .task-buttons {
      display: flex;
      gap: 0.5rem;
      flex-shrink: 0;
    }

    .task-buttons button {
      background: var(--secondary);
      border: none;
      cursor: pointer;
      padding: 0.75rem;
      border-radius: 10px;
      font-size: 1.2rem;
      min-width: 44px;
      min-height: 44px;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.2s;
      touch-action: manipulation;
    }

    .task-buttons button:active {
      transform: scale(0.95);
    }

    .task-buttons .edit-btn:hover,
    .task-buttons .edit-btn:active {
      background: var(--warning);
      color: white;
    }

    .task-buttons .delete-btn:hover,
    .task-buttons .delete-btn:active {
      background: var(--danger);
      color: white;
    }

    .progress h3 {
      margin: 0 0 1rem 0;
      font-size: 1.1rem;
      color: var(--text);
    }

    .progress-bar {
      height: 24px;
      background: var(--secondary);
      border-radius: 12px;
      overflow: hidden;
      position: relative;
    }

    .progress-fill {
      height: 100%;
      background: linear-gradient(90deg, var(--primary), #20c997);
      width: 0%;
      transition: width 0.5s ease;
      border-radius: 12px;
    }

    .stats {
      margin-top: 1rem;
      font-size: 0.9rem;
      text-align: center;
      color: var(--text);
      opacity: 0.8;
    }

    .theme-toggle {
      cursor: pointer;
      padding: 1rem;
      background: var(--secondary);
      border-radius: 12px;
      text-align: center;
      transition: all 0.2s;
      font-weight: 500;
      touch-action: manipulation;
    }

    .theme-toggle:active {
      transform: scale(0.98);
    }

    .empty-state {
      text-align: center;
      padding: 2rem;
      color: var(--text);
      opacity: 0.6;
    }

    .empty-state h3 {
      margin: 0 0 0.5rem 0;
      font-size: 1.2rem;
      color: var(--primary);
      font-weight: 600;
    }

    .empty-state p {
      margin: 0;
      font-size: 0.9rem;
    }

    #motivationalMessage {
      animation: fadeIn 0.5s ease-in;
      background: rgba(40, 167, 69, 0.1);
      padding: 0.8rem;
      border-radius: 8px;
      border-left: 4px solid var(--primary);
      text-align: center; 
      margin-top: 1rem; 
      font-style: italic; 
      color: var(--primary); 
      font-weight: 500; 
      display: none;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .celebration {
      animation: bounce 0.6s ease-in-out;
    }

    @keyframes bounce {
      0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
      40% { transform: translateY(-10px); }
      60% { transform: translateY(-5px); }
    }

    /* Melhorias para touch */
    @media (hover: none) and (pointer: coarse) {
      .task-buttons button:hover {
        background: var(--secondary);
        color: inherit;
      }
    }

    /* Responsividade para tablets */
    @media (min-width: 768px) {
      .container {
        max-width: 600px;
        margin: 0 auto;
      }
      
      .task-input {
        flex-direction: row;
        align-items: stretch;
      }
      
      .task-input input {
        flex: 1;
      }
      
      .task-input button {
        width: auto;
        padding: 1rem 2rem;
        white-space: nowrap;
      }
    }
  </style>
</head>
<body>
  <header>
    <h1>Produtiv.me</h1>
    <p>Seu diário de produtividade</p>
    <div id="currentDate"></div>
  </header>

  <div class="container">
    <div class="card task-input">
      <input type="text" id="taskInput" placeholder="Digite sua tarefa..." autocomplete="off">
      <button onclick="addTask()" id="addButton">Adicionar Tarefa</button>
    </div>

    <div class="card" id="taskCard" style="display: none;">
      <ul class="tasks" id="taskList"></ul>
    </div>

    <div class="card" id="emptyState">
      <div class="empty-state">
        <h3 id="emptyQuote">✨ Comece seu dia!</h3>
        <p>Adicione sua primeira tarefa para começar a ser mais produtivo</p>
      </div>
    </div>

    <div class="card progress">
      <h3>📈 Progresso do Dia</h3>
      <div class="progress-bar">
        <div class="progress-fill" id="progressFill"></div>
      </div>
      <div class="stats" id="stats">Nenhuma tarefa ainda</div>
      <div id="motivationalMessage"></div>
    </div>

    <div class="card theme-toggle" onclick="toggleTheme()">
      <span id="themeText">🌙 Modo Escuro</span>
    </div>
  </div>

  <script>
    let tasks = [];
    let lastDate = null;
    
    // Frases motivacionais
    const motivationalQuotes = {
      empty: [
        "✨ Cada grande jornada começa com um pequeno passo!",
        "🚀 Hoje é o dia perfeito para começar algo incrível!",
        "💪 Você tem o poder de transformar este dia!",
        "🌟 Pequenas ações levam a grandes resultados!",
        "🔥 Sua produtividade começa agora!",
        "⭐ Acredite em você e comece a primeira tarefa!"
      ],
      progress: [
        "🎯 Continue assim, você está no caminho certo!",
        "💎 Cada tarefa concluída é uma vitória!",
        "🏆 Você está construindo algo incrível!",
        "🌈 Progresso é progresso, não importa o tamanho!",
        "⚡ Sua determinação está dando frutos!",
        "🎊 Parabéns! Você está fazendo acontecer!"
      ],
      completed: [
        "🎉 Incrível! Você completou tudo hoje!",
        "👑 Você é uma máquina de produtividade!",
        "🏅 Dia 100% produtivo! Você merece aplausos!",
        "🌟 Parabéns! Você arrasou hoje!",
        "🚀 Missão cumprida! Você é fantástico!",
        "💯 Perfeição alcançada! Descanse merecidamente!"
      ],
      newDay: [
        "🌅 Novo dia, novas oportunidades de brilhar!",
        "☀️ Bom dia! Pronto para mais conquistas?",
        "🌱 Cada dia é uma nova chance de crescer!",
        "✨ O universo está conspirando a seu favor hoje!",
        "🎯 Hoje você pode superar suas expectativas!",
        "🔥 Desperte o gigante que existe em você!"
      ]
    };
    
    function getRandomQuote(category) {
      const quotes = motivationalQuotes[category];
      if (!quotes || quotes.length === 0) return "";
      return quotes[Math.floor(Math.random() * quotes.length)];
    }
    
    // Funções globais para serem chamadas pelos botões
    window.toggleTask = function(index) {
      if (index < 0 || index >= tasks.length) return;
      tasks[index].completed = !tasks[index].completed;
      saveTasks();
      renderTasks();
    };
    
    window.editTask = function(index) {
      if (index < 0 || index >= tasks.length) return;
      const currentTask = tasks[index];
      const newText = prompt("✏️ Editar tarefa:", currentTask.text);
      if (newText !== null && newText.trim() !== "" && newText.trim() !== currentTask.text) {
        tasks[index].text = newText.trim();
        saveTasks();
        renderTasks();
      }
    };
    
    window.deleteTask = function(index) {
      if (index < 0 || index >= tasks.length) return;
      if (confirm("🗑️ Tem certeza que deseja excluir esta tarefa?")) {
        tasks.splice(index, 1);
        saveTasks();
        renderTasks();
      }
    };

    function getTodayDateString() {
      const today = new Date();
      return today.toDateString();
    }

    function updateCurrentDate() {
      try {
        const today = new Date();
        const options = { 
          weekday: 'long', 
          year: 'numeric', 
          month: 'long', 
          day: 'numeric' 
        };
        const dateString = today.toLocaleDateString('pt-BR', options);
        const dateElement = document.getElementById('currentDate');
        if (dateElement) {
          dateElement.textContent = dateString;
        }
      } catch (error) {
        console.error('Erro ao atualizar data:', error);
      }
    }

    function loadTasks() {
      try {
        const savedTasks = localStorage.getItem('produtiv_tasks');
        const savedDate = localStorage.getItem('produtiv_last_date');
        const currentDate = getTodayDateString();
        
        if (savedTasks) {
          const parsedTasks = JSON.parse(savedTasks);
          if (Array.isArray(parsedTasks)) {
            tasks = parsedTasks;
          }
        }
        
        // Se é um novo dia, desmarcar todas as tarefas concluídas
        if (savedDate && savedDate !== currentDate) {
          tasks = tasks.map(task => ({
            ...task,
            completed: false
          }));
          console.log('✨ Novo dia! Tarefas desmarcadas para um novo começo');
          showNewDayMessage();
        }
        
        lastDate = currentDate;
        localStorage.setItem('produtiv_last_date', currentDate);
        saveTasks();
      } catch (error) {
        console.error('Erro ao carregar tarefas:', error);
        tasks = [];
      }
    }
    
    function showNewDayMessage() {
      try {
        const message = document.getElementById('motivationalMessage');
        if (message) {
          const quote = getRandomQuote('newDay');
          message.textContent = quote;
          message.style.display = 'block';
          message.classList.add('celebration');
          
          setTimeout(() => {
            message.classList.remove('celebration');
          }, 600);
          
          setTimeout(() => {
            message.style.display = 'none';
          }, 5000);
        }
      } catch (error) {
        console.error('Erro ao mostrar mensagem de novo dia:', error);
      }
    }

    function saveTasks() {
      try {
        localStorage.setItem('produtiv_tasks', JSON.stringify(tasks));
        localStorage.setItem('produtiv_last_date', getTodayDateString());
      } catch (error) {
        console.error('Erro ao salvar tarefas:', error);
      }
    }

    function renderTasks() {
      try {
        const taskList = document.getElementById("taskList");
        const taskCard = document.getElementById("taskCard");
        const emptyState = document.getElementById("emptyState");
        const emptyQuote = document.getElementById("emptyQuote");
        
        if (!taskList || !taskCard || !emptyState || !emptyQuote) {
          console.error('Elementos não encontrados');
          return;
        }
        
        taskList.innerHTML = "";
        
        if (tasks.length === 0) {
          taskCard.style.display = "none";
          emptyState.style.display = "block";
          emptyQuote.textContent = getRandomQuote('empty');
        } else {
          taskCard.style.display = "block";
          emptyState.style.display = "none";
          
          tasks.forEach((task, index) => {
            if (!task || typeof task.text !== 'string') return;
            
            const li = document.createElement("li");
            if (task.completed) {
              li.className = "completed";
            }
            
            // Criar span para o texto da tarefa
            const taskSpan = document.createElement("span");
            taskSpan.textContent = task.text;
            taskSpan.onclick = function() { toggleTask(index); };
            
            // Criar div para os botões
            const buttonsDiv = document.createElement("div");
            buttonsDiv.className = "task-buttons";
            
            // Botão de editar
            const editBtn = document.createElement("button");
            editBtn.className = "edit-btn";
            editBtn.innerHTML = "✏️";
            editBtn.title = "Editar tarefa";
            editBtn.onclick = function() { editTask(index); };
            
            // Botão de excluir
            const deleteBtn = document.createElement("button");
            deleteBtn.className = "delete-btn";
            deleteBtn.innerHTML = "🗑️";
            deleteBtn.title = "Excluir tarefa";
            deleteBtn.onclick = function() { deleteTask(index); };
            
            buttonsDiv.appendChild(editBtn);
            buttonsDiv.appendChild(deleteBtn);
            
            li.appendChild(taskSpan);
            li.appendChild(buttonsDiv);
            taskList.appendChild(li);
          });
        }
        
        updateProgress();
      } catch (error) {
        console.error('Erro ao renderizar tarefas:', error);
      }
    }

    function addTask() {
      try {
        const input = document.getElementById("taskInput");
        if (!input) return;
        
        const taskText = input.value.trim();
        
        if (taskText === "") {
          input.focus();
          return;
        }
        
        tasks.push({ 
          text: taskText, 
          completed: false,
          createdAt: new Date().toISOString()
        });
        
        input.value = "";
        saveTasks();
        renderTasks();
        
        // Scroll suave para a nova tarefa
        setTimeout(() => {
          const taskCard = document.getElementById("taskCard");
          if (taskCard && taskCard.style.display !== "none") {
            taskCard.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
          }
        }, 100);
      } catch (error) {
        console.error('Erro ao adicionar tarefa:', error);
      }
    }

    function updateProgress() {
      try {
        const progressFill = document.getElementById("progressFill");
        const stats = document.getElementById("stats");
        const motivationalMessage = document.getElementById("motivationalMessage");
        
        if (!progressFill || !stats || !motivationalMessage) return;
        
        const total = tasks.length;
        const completed = tasks.filter(task => task && task.completed).length;
        const percent = total === 0 ? 0 : Math.round((completed / total) * 100);
        
        progressFill.style.width = percent + "%";
        
        if (total === 0) {
          stats.textContent = "Nenhuma tarefa ainda";
          motivationalMessage.style.display = "none";
        } else {
          const pending = total - completed;
          stats.textContent = `📊 Total: ${total} | ✅ Concluídas: ${completed} | ⏳ Pendentes: ${pending} (${percent}%)`;
          
          // Mostrar frases motivacionais baseadas no progresso
          if (percent === 100) {
            const quote = getRandomQuote('completed');
            motivationalMessage.textContent = quote;
            motivationalMessage.style.display = 'block';
            motivationalMessage.classList.add('celebration');
            setTimeout(() => motivationalMessage.classList.remove('celebration'), 600);
          } else if (completed > 0) {
            const quote = getRandomQuote('progress');
            motivationalMessage.textContent = quote;
            motivationalMessage.style.display = 'block';
          } else {
            motivationalMessage.style.display = 'none';
          }
        }
      } catch (error) {
        console.error('Erro ao atualizar progresso:', error);
      }
    }

    function toggleTheme() {
      try {
        const body = document.body;
        const themeText = document.getElementById("themeText");
        
        if (!themeText) return;
        
        if (body.getAttribute("data-theme") === "dark") {
          body.removeAttribute("data-theme");
          themeText.textContent = "🌙 Modo Escuro";
          localStorage.setItem('produtiv_theme', 'light');
        } else {
          body.setAttribute("data-theme", "dark");
          themeText.textContent = "☀️ Modo Claro";
          localStorage.setItem('produtiv_theme', 'dark');
        }
      } catch (error) {
        console.error('Erro ao alternar tema:', error);
      }
    }

    function loadTheme() {
      try {
        const savedTheme = localStorage.getItem('produtiv_theme');
        const themeText = document.getElementById("themeText");
        
        if (!themeText) return;
        
        if (savedTheme === 'dark') {
          document.body.setAttribute("data-theme", "dark");
          themeText.textContent = "☀️ Modo Claro";
        } else {
          themeText.textContent = "🌙 Modo Escuro";
        }
      } catch (error) {
        console.error('Erro ao carregar tema:', error);
      }
    }

    // Event listeners
    document.addEventListener('DOMContentLoaded', function() {
      try {
        const taskInput = document.getElementById("taskInput");
        const addButton = document.getElementById("addButton");
        
        if (taskInput) {
          taskInput.addEventListener('keypress', function(event) {
            if (event.key === 'Enter') {
              event.preventDefault();
              addTask();
            }
          });
        }
        
        if (addButton && navigator.vibrate) {
          addButton.addEventListener('touchstart', function() {
            navigator.vibrate(10);
          });
        }
        
        // Inicializar aplicação
        updateCurrentDate();
        loadTasks();
        loadTheme();
        renderTasks();
        
        // Verificar mudança de dia a cada minuto
        setInterval(() => {
          try {
            const currentDate = getTodayDateString();
            if (lastDate && lastDate !== currentDate) {
              console.log('🌅 Novo dia detectado!');
              loadTasks();
              renderTasks();
              updateCurrentDate();
            }
          } catch (error) {
            console.error('Erro na verificação de novo dia:', error);
          }
        }, 60000);
        
      } catch (error) {
        console.error('Erro na inicialização:', error);
      }
    });
  </script>
</body>
</html>
