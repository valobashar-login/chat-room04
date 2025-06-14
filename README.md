# chat-room04
!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8" />
  <title>সহজ Chat Room</title>
  <style>
    body { font-family: Arial, sans-serif; background:#f2f2f2; }
    #loginBox, #chatBox { max-width: 400px; margin: 50px auto; padding:20px; background:#fff; border-radius:8px; box-shadow:0 0 10px #ccc; }
    #chatBox { display:none; flex-direction: column; height: 400px; }
    #messages { flex-grow:1; border:1px solid #ddd; padding:10px; overflow-y:auto; height: 300px; margin-bottom: 10px; background:#fafafa; }
    input[type=text], input[type=password] { width: 100%; padding: 10px; margin: 8px 0; box-sizing: border-box; }
    button { width: 100%; padding: 10px; background:#4CAF50; color: white; border:none; cursor:pointer; font-size:16px; }
    button:hover { background:#45a049; }
    .message { margin: 5px 0; }
    .message strong { color: #333; }
  </style>
</head>
<body>

  <div id="loginBox">
    <h2>লগইন করো</h2>
    <input type="text" id="username" placeholder="Username" />
    <input type="password" id="password" placeholder="Password" />
    <button onclick="login()">লগইন</button>
    <p id="loginError" style="color:red;"></p>
  </div>

  <div id="chatBox">
    <h2>Chat Room</h2>
    <div id="messages"></div>
    <input type="text" id="messageInput" placeholder="Message লিখো..." />
    <button onclick="sendMessage()">Send</button>
    <button onclick="logout()" style="background:#f44336; margin-top:10px;">Logout</button>
  </div>

<script>
  // Predefined user for demo:
  const validUser = {
    username: "arman",
    password: "1234"
  };

  let currentUser = null;

  function login() {
    const user = document.getElementById('username').value.trim();
    const pass = document.getElementById('password').value.trim();
    const errorEl = document.getElementById('loginError');

    if(user === validUser.username && pass === validUser.password) {
      currentUser = user;
      errorEl.textContent = "";
      document.getElementById('loginBox').style.display = 'none';
      document.getElementById('chatBox').style.display = 'flex';
      loadMessages();
    } else {
      errorEl.textContent = "ভুল ইউজারনেম বা পাসওয়ার্ড!";
    }
  }

  function logout() {
    currentUser = null;
    document.getElementById('loginBox').style.display = 'block';
    document.getElementById('chatBox').style.display = 'none';
    document.getElementById('messages').innerHTML = "";
    document.getElementById('username').value = "";
    document.getElementById('password').value = "";
  }

  function sendMessage() {
    const input = document.getElementById('messageInput');
    const msg = input.value.trim();
    if(msg === "") return;

    const messages = JSON.parse(localStorage.getItem('chatMessages') || '[]');
    messages.push({ user: currentUser, text: msg, time: new Date().toLocaleTimeString() });
    localStorage.setItem('chatMessages', JSON.stringify(messages));

    input.value = "";
    loadMessages();
  }

  function loadMessages() {
    const messages = JSON.parse(localStorage.getItem('chatMessages') || '[]');
    const messagesDiv = document.getElementById('messages');
    messagesDiv.innerHTML = "";
    messages.forEach(m => {
      const div = document.createElement('div');
      div.className = "message";
      div.innerHTML = `<strong>${m.user}:</strong> ${m.text} <small style="color:#888;">(${m.time})</small>`;
      messagesDiv.appendChild(div);
    });
    messagesDiv.scrollTop = messagesDiv.scrollHeight;
  }
</script>

</body>
</html>
