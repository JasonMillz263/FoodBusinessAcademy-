<!DOCTYPE html><html>
<head>
  <meta charset="UTF-8">
  <title>Food Business Academy - Student Login</title>  <!-- Firebase COMPAT SDKs -->  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth-compat.js"></script>  <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>  <style>
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
      max-width: 420px;
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
      background: #0074D9;
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
  </style></head><body><div class="login-container">
  <h2>Student Login</h2>  <input id="username" type="text" placeholder="Username" />
  <input id="email" type="email" placeholder="Email" />
  <input id="password" type="password" placeholder="Password" /><button onclick="login()">Login</button>

  <p id="message"></p>
</div><script>
  // ================= FIREBASE CONFIG =================
  const firebaseConfig = {
    apiKey: "AIzaSyBAx6F9q-jBoKAGV_UNNVZj1_1f8ccVu98",
    authDomain: "foodbusinessacademy-f1c60.firebaseapp.com",
    projectId: "foodbusinessacademy-f1c60",
    storageBucket: "foodbusinessacademy-f1c60.appspot.com",
    messagingSenderId: "676124097300",
    appId: "1:676124097300:web:e9949d6dfa21ff214de001"
  };

  firebase.initializeApp(firebaseConfig);

  const auth = firebase.auth();
  const db = firebase.firestore();
  const message = document.getElementById("message");

  // ================= LOGIN FUNCTION =================
  async function login() {
    const username = document.getElementById("username").value.trim();
    const email = document.getElementById("email").value.trim();
    const password = document.getElementById("password").value.trim();

    if (!username || !email || !password) {
      message.innerText = "All fields are required.";
      return;
    }

    try {
      // 1. Sign in with email & password
      const userCredential = await auth.signInWithEmailAndPassword(email, password);
      const uid = userCredential.user.uid;

      // 2. Save / update login data
      const pageUrl = window.location.href;

      const studentRef = db.collection("students").doc(uid);

      const doc = await studentRef.get();

      if (!doc.exists) {
        message.innerText = "Student record not found.";
        return;
      }

      await studentRef.update({
        username: username,
        email: email,
        lastLoginPage: pageUrl,
        lastLoginAt: firebase.firestore.FieldValue.serverTimestamp()
      });

      // 3. Redirect to student dashboard (stored in Firestore)
      const studentData = doc.data();

      if (!studentData.dashboardUrl) {
        message.innerText = "No dashboard URL assigned.";
        return;
      }

      window.location.href = studentData.dashboardUrl;

    } catch (error) {
      message.innerText = error.message;
    }
  }
</script></body>
</html>
