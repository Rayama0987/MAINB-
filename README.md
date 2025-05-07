<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>乗法公式ゲーム</title>
  <style>
    body { font-family: Arial; padding: 20px; }
    input[type="text"] { width: 300px; font-size: 16px; }
    button { font-size: 16px; margin: 5px 5px 10px 0; }
  </style>
</head>
<body>
  <h1>乗法公式ゲーム</h1>
  <div id="question"></div>
  <input type="text" id="answer" placeholder="例: x²+xy+y²">
  <br>
  <div>
    <button onclick="addTerm('x²')">x²</button>
    <button onclick="addTerm('xy')">xy</button>
    <button onclick="addTerm('y²')">y²</button>
    <button onclick="addTerm('+')">+</button>
    <button onclick="addTerm('-')">−</button>
    <button onclick="clearInput()">消去</button>
  </div>
  <button onclick="checkAnswer()">解答！</button>
  <p id="result"></p>
  <p id="score">0問正解中</p>

  <script>
    let questionCount = 0;
    let correctCount = 0;
    let correctExpansion = "";

    function generateQuestion() {
      const type = Math.floor(Math.random() * 3); // 0, 1, or 2
      const questionDiv = document.getElementById("question");

      if (type === 0) {
        // (ax±by)²
        const a = Math.floor(Math.random() * 10) + 1;
        const b = Math.floor(Math.random() * 10) + 1;
        const plus = Math.random() < 0.5;
        const op = plus ? "+" : "-";
        questionDiv.textContent = `Q${questionCount + 1}: ( ${a}x ${op} ${b}y )² を展開して！`;

        if (plus) {
          correctExpansion = `${a*a}x²+${2*a*b}xy+${b*b}y²`;
        } else {
          correctExpansion = `${a*a}x²-${2*a*b}xy+${b*b}y²`;
        }

      } else if (type === 1) {
        // (ax±by)(cx±dy)
        const a = Math.floor(Math.random() * 10) + 1;
        const b = Math.floor(Math.random() * 10) + 1;
        const c = Math.floor(Math.random() * 10) + 1;
        const d = Math.floor(Math.random() * 10) + 1;
        const plus1 = Math.random() < 0.5;
        const plus2 = Math.random() < 0.5;
        const op1 = plus1 ? "+" : "-";
        const op2 = plus2 ? "+" : "-";

        questionDiv.textContent = `Q${questionCount + 1}: ( ${a}x ${op1} ${b}y )( ${c}x ${op2} ${d}y ) を展開して！`;

        const ac = a * c;
        const ad = a * d;
        const bc = b * c;
        const bd = b * d;
        const middle = (plus1 ? 1 : -1) * ad + (plus2 ? 1 : -1) * bc;
        const signMiddle = middle >= 0 ? "+" : "-";

        const midAbs = Math.abs(middle);
        const signLast = (plus1 === plus2) ? "+" : "-";

        correctExpansion = `${ac}x²${signMiddle}${midAbs}xy${signLast}${bd}y²`;

      } else {
        // (x + y)(x - y)
        questionDiv.textContent = `Q${questionCount + 1}: ( x + y )( x - y ) を展開して！`;
        correctExpansion = "x²-y²";
      }
    }

    function checkAnswer() {
      const userAnswer = document.getElementById("answer").value.replace(/\s+/g, "");
      const result = document.getElementById("result");

      if (userAnswer === correctExpansion) {
        result.textContent = "正解！ 🎉";
        correctCount++;
      } else {
        result.textContent = `不正解 😢 正解は ${correctExpansion}`;
      }

      questionCount++;
      document.getElementById("score").textContent = `${questionCount}問中 ${correctCount}問正解`;

      if (questionCount >= 10) {
        showResult();
      } else {
        document.getElementById("answer").value = "";
        generateQuestion();
      }
    }

    function showResult() {
      const accuracy = (correctCount / 10) * 100;
      if (confirm(`お疲れさま！\n正答数：${correctCount}/10\n正答率：${accuracy.toFixed(1)}%\nもう一度挑戦しますか？`)) {
        questionCount = 0;
        correctCount = 0;
        document.getElementById("answer").value = "";
        document.getElementById("result").textContent = "";
        document.getElementById("score").textContent = "0問正解中";
        generateQuestion();
      } else {
        alert("また遊んでね！");
      }
    }

    function addTerm(term) {
      const input = document.getElementById("answer");
      input.value += term;
      input.focus();
    }

    function clearInput() {
      const input = document.getElementById("answer");
      input.value = "";
      input.focus();
    }

    generateQuestion();
  </script>
</body>
</html>
