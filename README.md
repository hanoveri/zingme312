<!DOCTYPE html>
<html>
<head>
    <title>Tu Tiên Truyện</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>

<header>
    <h1>🔥 Tu Tiên Truyện</h1>
    <button id="loginBtn">Đăng nhập Google</button>
</header>

<section id="profile">
    <h3 id="username"></h3>
    <div class="tuvi-bar">
        <div id="tuviFill"></div>
    </div>
    <p>Tu Vi: <span id="tuvi">0</span></p>
</section>

<section id="story">
    <h2>Chương 1</h2>
    <p id="content">
        Đây là nội dung truyện mẫu...
    </p>
    <button id="readBtn">Đọc xong (+50 Tu Vi)</button>
</section>

<script type="module" src="js/firebase.js"></script>
<script type="module" src="js/auth.js"></script>
<script type="module" src="js/tuvi.js"></script>
<script type="module" src="js/story.js"></script>
<script type="module" src="js/ui.js"></script>

</body>
</html>
body {
    background: #0f0f1a;
    color: white;
    font-family: Arial;
    padding: 20px;
}

header {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

button {
    background: #6a00ff;
    border: none;
    padding: 10px 20px;
    color: white;
    cursor: pointer;
    border-radius: 6px;
}

.tuvi-bar {
    width: 100%;
    height: 20px;
    background: #222;
    border-radius: 10px;
    margin: 10px 0;
}

#tuviFill {
    height: 100%;
    width: 0%;
    background: linear-gradient(90deg, #6a00ff, #00d4ff);
    border-radius: 10px;
}

import { initializeApp } from 
"https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";

import { getAuth } from 
"https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";

import { getFirestore } from 
"https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

const firebaseConfig = {
  apiKey: "YOUR_KEY",
  authDomain: "YOUR_DOMAIN",
  projectId: "YOUR_ID",
};

export const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getFirestore(app);

import { auth } from "./firebase.js";
import { GoogleAuthProvider, signInWithPopup } from 
"https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";

const provider = new GoogleAuthProvider();

document.getElementById("loginBtn").onclick = async () => {
    const result = await signInWithPopup(auth, provider);
    alert("Đăng nhập thành công!");
};

import { db, auth } from "./firebase.js";
import { doc, getDoc, setDoc, updateDoc } from 
"https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

export async function loadTuVi(uid) {
    const ref = doc(db, "users", uid);
    const snap = await getDoc(ref);

    if (!snap.exists()) {
        await setDoc(ref, { tuvi: 0 });
        return 0;
    }

    return snap.data().tuvi;
}

export async function addTuVi(uid, amount) {
    const ref = doc(db, "users", uid);
    await updateDoc(ref, {
        tuvi: (await loadTuVi(uid)) + amount
    });
}

import { auth } from "./firebase.js";
import { addTuVi } from "./tuvi.js";

document.getElementById("readBtn").onclick = async () => {
    if (!auth.currentUser) {
        alert("Đăng nhập trước!");
        return;
    }

    await addTuVi(auth.currentUser.uid, 50);
    alert("+50 Tu Vi");
};

import { auth } from "./firebase.js";
import { loadTuVi } from "./tuvi.js";

auth.onAuthStateChanged(async (user) => {
    if (user) {
        document.getElementById("username").innerText = user.displayName;

        const tuvi = await loadTuVi(user.uid);
        document.getElementById("tuvi").innerText = tuvi;

        const percent = Math.min(tuvi / 1000 * 100, 100);
        document.getElementById("tuviFill").style.width = percent + "%";
    }
});
