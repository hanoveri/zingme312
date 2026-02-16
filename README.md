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
