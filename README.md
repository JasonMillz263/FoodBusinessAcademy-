<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Food Business Academy - Student Login</title>

  <!-- Firebase COMPAT SDKs -->
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f9f9f9;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }
    .login-container {
      max-width: 400px;
      width: 100%;
      padding: 30px;
      background: #fff;
      border-radius: 10px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    }
    input {
      width: 100%;
      padding: 12px;
      margin-bottom: 12px;
      border: 1px solid #ccc;
      border-radius: 6px;
      font-size: 16px;
    }
    button {
      width: 100%;
      padding: 12px;
      border: none;
      border-radius: 6px;
      font-size: 16px;
      cursor: pointer;
    }
    #loginBtn {
      background: #0074D9;
      color: white;
      margin-bottom: 10px;
    }
    #forgotBtn {
      background: #555;
      color: white;
    }
    #message {
      margin-top: 15px;
      text-align: center;
      color: red;
    }
    h2 {
      text-align: center;
      margin-bottom: 20px;
    }
  </style>
</head>

<body>

<div class="login-container">
  <h2>Student Login</h2>

  <input id="username" type="text" placeholder="Username">
  <input id="password" type="password" placeholder="Password">

  <button id="loginBtn" onclick="login()">Login</button>
  <button id="forgotBtn" onclick="forgotPassword()">Forgot Password</button>

  <p id="message"></p>
</div>

<script>
  // Firebase config
  const firebaseConfig = {
    apiKey: "AIzaSyBAx6F9q-jBoKAGV_UNNVZj1_1f8ccVu98",
    authDomain: "foodbusinessacademy-f1c60.firebaseapp.com",
    projectId: "foodbusinessacademy-f1c60",
    storageBucket: "foodbusinessacademy-f1c60.firebasestorage.app",
    messagingSenderId: "676124097300",
    appId: "1:676124097300:web:e9949d6dfa21ff214de001"
  };

  // Initialize Firebase
  firebase.initializeApp(firebaseConfig);

  const auth = firebase.auth();
  const db = firebase.firestore();
  const message = document.getElementById("message");

  // ================= LOGIN =================
  async function login() {
    const username = document.getElementById("username").value.trim();
    const pass = document.getElementById("password").value.trim();

    if (!username || !pass) {
      message.innerText = "Please enter username and password.";
      return;
    }

    const email = username + "@students.com";

    try {
      const userCredential = await auth.signInWithEmailAndPassword(email, pass);
      const uid = userCredential.user.uid;

      // Verify student exists
      const doc = await db.collection("Students Login").doc(uid).get();
      if (!doc.exists) {
        message.innerText = "Student record not found.";
        return;
      }

      // Redirect to dashboard
      window.location.href =
        "https://foodbusinessacademdy.blogspot.com/p/student-dashboard.html";

    } catch (error) {
      message.innerText = error.message;
    }
  }

  // ================= FORGOT PASSWORD =================
  async function forgotPassword() {
    const username = document.getElementById("username").value.trim();

    if (!username) {
      message.innerText = "Enter your username to reset password.";
      return;
    }

    const email = username + "@students.com";

    try {
      await auth.sendPasswordResetEmail(email);
      message.style.color = "green";
      message.innerText = "Password reset email sent. Check your inbox.";
    } catch (error) {
      message.style.color = "red";
      message.innerText = error.message;
    }
  }
</script>

</body>
</html>
