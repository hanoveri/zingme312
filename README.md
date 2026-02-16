<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Tu Tiên Truyện</title>

<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg,#0f0f1a,#1a0033);
    color: white;
}

header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px 20px;
    background: #111;
    box-shadow: 0 0 10px #6a00ff;
}

button {
    background: linear-gradient(90deg,#6a00ff,#00d4ff);
    border: none;
    padding: 10px 20px;
    color: white;
    cursor: pointer;
    border-radius: 8px;
    font-weight: bold;
}

.container {
    max-width: 900px;
    margin: auto;
    padding: 20px;
}

.profile {
    background: rgba(255,255,255,0.05);
    padding: 15px;
    border-radius: 10px;
    margin-bottom: 20px;
}

.tuvi-bar {
    width: 100%;
    height: 20px;
    background: #222;
    border-radius: 10px;
    margin-top: 10px;
    overflow: hidden;
}

#tuviFill {
    height: 100%;
    width: 0%;
    background: linear-gradient(90deg,#6a00ff,#00d4ff);
    transition: 0.5s;
}

.story {
    background: rgba(255,255,255,0.05);
    padding: 20px;
    border-radius: 10px;
    line-height: 1.6;
}
</style>
</head>

<body>

<header>
    <h2>🔥 Tu Tiên Truyện</h2>
    <button id="loginBtn">Đăng nhập Google</button>
</header>

<div class="container">

<div class="profile">
    <h3 id="username">Chưa đăng nhập</h3>
    <p>Tu Vi: <span id="tuvi">0</span></p>
    <div class="tuvi-bar">
        <div id="tuviFill"></div>
    </div>
</div>

<div class="story">
    <h2>Chương 1: Khởi đầu tu luyện</h2>
    <p>
        Trên đỉnh núi cao, thiếu niên nhìn xuống phàm trần...
        Hắn thề phải nghịch thiên cải mệnh, bước lên con đường tu tiên.
        Linh khí vận chuyển, đan điền rung động.
    </p>
    <br>
    <button id="readBtn">Đọc xong (+50 Tu Vi)</button>
</div>

</div>

<script type="module">

import { initializeApp } from 
"https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";

import { 
getAuth, 
GoogleAuthProvider, 
signInWithPopup, 
onAuthStateChanged 
} from "https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";

import { 
getFirestore, 
doc, 
getDoc, 
setDoc, 
updateDoc 
} from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

/* ==============================
   THAY THÔNG TIN FIREBASE Ở ĐÂY
================================= */
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);

const loginBtn = document.getElementById("loginBtn");
const usernameEl = document.getElementById("username");
const tuviEl = document.getElementById("tuvi");
const tuviFill = document.getElementById("tuviFill");
const readBtn = document.getElementById("readBtn");

const provider = new GoogleAuthProvider();

loginBtn.onclick = async () => {
    await signInWithPopup(auth, provider);
};

/* ==============================
   LOAD USER DATA
================================= */

async function loadTuVi(uid) {
    const ref = doc(db, "users", uid);
    const snap = await getDoc(ref);

    if (!snap.exists()) {
        await setDoc(ref, { tuvi: 0 });
        return 0;
    }

    return snap.data().tuvi;
}

async function addTuVi(uid, amount) {
    const ref = doc(db, "users", uid);
    const current = await loadTuVi(uid);
    await updateDoc(ref, {
        tuvi: current + amount
    });
}

/* ==============================
   UPDATE UI
================================= */

async function updateUI(user) {
    usernameEl.innerText = user.displayName;

    const tuvi = await loadTuVi(user.uid);
    tuviEl.innerText = tuvi;

    const percent = Math.min((tuvi % 1000) / 1000 * 100, 100);
    tuviFill.style.width = percent + "%";
}

onAuthStateChanged(auth, async (user) => {
    if (user) {
        updateUI(user);
    }
});

/* ==============================
   READ STORY
================================= */

readBtn.onclick = async () => {
    if (!auth.currentUser) {
        alert("Đăng nhập trước!");
        return;
    }

    await addTuVi(auth.currentUser.uid, 50);
    await updateUI(auth.currentUser);
};

</script>

</body>
</html>
