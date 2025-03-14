<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>YutaPay ポイント管理</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; margin: 20px; }
        input, button { margin: 5px; padding: 10px; font-size: 16px; }
        .menu { position: fixed; top: 10px; right: 10px; background: #333; color: white; padding: 10px; border-radius: 5px; cursor: pointer; }
        .admin-panel, .add-points-form { 
            display: none; position: fixed; top: 50px; right: 10px; background: white; padding: 20px; border: 1px solid #ccc; 
        }
    </style>
</head>
<body>
    <h2>YutaPay ポイント管理</h2>
    <p>現在のポイント: <span id="points">0</span></p>

    <input type="number" id="amount" placeholder="ポイント数">
    <button onclick="usePoints()">ポイントを使う</button>

    <div class="menu" onclick="toggleAdminPanel()">☰ メニュー</div>

    <div class="admin-panel" id="adminPanel">
        <h3>管理者モード</h3>
        <input type="password" id="password" placeholder="パスワード">
        <button onclick="verifyPassword()">ログイン</button>
        <div id="adminControls" style="display: none;">
            <button onclick="showAddPointsForm()">ポイント追加</button>
        </div>
    </div>

    <div class="add-points-form" id="addPointsForm">
        <h3>ポイント追加</h3>
        <input type="number" id="addAmount" placeholder="追加するポイント数">
        <button onclick="addPoints()">追加する</button>
        <button onclick="hideAddPointsForm()">閉じる</button>
    </div>

    <script>
        // ローカルストレージからポイントを取得（なければ0）
        let points = localStorage.getItem("yutapay_points") ? parseInt(localStorage.getItem("yutapay_points")) : 0;
        document.getElementById("points").textContent = points;

        function updatePoints() {
            document.getElementById("points").textContent = points;
            localStorage.setItem("yutapay_points", points); // ローカルストレージに保存
        }

        function usePoints() {
            let amount = parseInt(document.getElementById("amount").value);
            if (isNaN(amount) || amount <= 0) {
                alert("正しいポイント数を入力してください");
                return;
            }
            if (amount > points) {
                alert("ポイントが足りません！");
                return;
            }
            points -= amount;
            updatePoints();
        }

        function toggleAdminPanel() {
            let panel = document.getElementById("adminPanel");
            panel.style.display = (panel.style.display === "block") ? "none" : "block";
        }

        function verifyPassword() {
            let password = document.getElementById("password").value;
            if (password === "yutashop123") {
                document.getElementById("adminControls").style.display = "block";
                alert("管理者モードが有効になりました！");
            } else {
                alert("パスワードが違います！");
            }
        }

        function showAddPointsForm() {
            document.getElementById("addPointsForm").style.display = "block";
        }

        function hideAddPointsForm() {
            document.getElementById("addPointsForm").style.display = "none";
        }

        function addPoints() {
            let amount = parseInt(document.getElementById("addAmount").value);
            if (isNaN(amount) || amount <= 0) {
                alert("正しいポイント数を入力してください");
                return;
            }
            points += amount;
            updatePoints();
            alert("ポイントが追加されました！");
            hideAddPointsForm();
        }
    </script>
</body>
</html>