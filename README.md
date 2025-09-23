<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8">
  <title>Dziennik</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; }
    .hidden { display: none; }
    .box { border: 1px solid #aaa; padding: 15px; margin: 10px 0; }
    h2 { margin-top: 0; }
  </style>
</head>
<body>

  <h1>Dziennik online</h1>

  <!-- Logowanie -->
  <div id="loginBox" class="box">
    <h2>Logowanie</h2>
    <label>Użytkownik: <input id="username" type="text"></label><br><br>
    <label>Hasło: <input id="password" type="password"></label><br><br>
    <button onclick="login()">Zaloguj</button>
  </div>

  <!-- Panel nauczyciela -->
  <div id="teacherBox" class="box hidden">
    <h2>Panel nauczyciela</h2>
    <label>Uczeń: <input id="studentName" type="text"></label><br><br>
    <label>Ocena: <input id="studentGrade" type="text"></label><br><br>
    <button onclick="addGrade()">Dodaj ocenę</button>
    <button onclick="logout()">Wyloguj</button>
  </div>

  <!-- Panel ucznia -->
  <div id="studentBox" class="box hidden">
    <h2>Panel ucznia</h2>
    <p><b>Twoje oceny:</b></p>
    <ul id="gradesList"></ul>
    <button onclick="logout()">Wyloguj</button>
  </div>

  <script>
    const users = {
      "nauczyciel": "1234",
      "uczen": "abcd"
    };

    function login() {
      const username = document.getElementById("username").value;
      const password = document.getElementById("password").value;

      if (users[username] === password) {
        document.getElementById("loginBox").classList.add("hidden");

        if (username === "nauczyciel") {
          document.getElementById("teacherBox").classList.remove("hidden");
          localStorage.setItem("currentUser", "nauczyciel");
        } else {
          document.getElementById("studentBox").classList.remove("hidden");
          localStorage.setItem("currentUser", username);
          loadGrades(username);
        }
      } else {
        alert("Błędny login lub hasło!");
      }
    }

    function logout() {
      localStorage.removeItem("currentUser");
      document.getElementById("loginBox").classList.remove("hidden");
      document.getElementById("teacherBox").classList.add("hidden");
      document.getElementById("studentBox").classList.add("hidden");
    }

    function addGrade() {
      const student = document.getElementById("studentName").value;
      const grade = document.getElementById("studentGrade").value;

      if (!student || !grade) {
        alert("Podaj ucznia i ocenę!");
        return;
      }

      let allGrades = JSON.parse(localStorage.getItem("grades")) || {};
      if (!allGrades[student]) {
        allGrades[student] = [];
      }
      allGrades[student].push(grade);

      localStorage.setItem("grades", JSON.stringify(allGrades));
      alert("Dodano ocenę dla: " + student);
    }

    function loadGrades(student) {
      const allGrades = JSON.parse(localStorage.getItem("grades")) || {};
      const grades = allGrades[student] || [];
      const list = document.getElementById("gradesList");
      list.innerHTML = "";
      grades.forEach(g => {
        const li = document.createElement("li");
        li.textContent = g;
        list.appendChild(li);
      });
    }
  </script>

</body>
</html>
