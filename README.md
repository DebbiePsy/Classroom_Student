<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>📱 智慧學生互動端 PRO</title>

<style>
body {
    margin: 0;
    padding: 18px;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    background:
        radial-gradient(circle at 20% 20%, #1b6fff22, transparent 40%),
        radial-gradient(circle at 80% 30%, #8a2be222, transparent 40%),
        radial-gradient(circle at 50% 80%, #00d4ff22, transparent 40%),
        #0b1220;
    color: white;
}

/* 🧊 主卡片 */
.container {
    max-width: 720px;
    margin: auto;
    background: rgba(255,255,255,0.06);
    backdrop-filter: blur(18px);
    border: 1px solid rgba(255,255,255,0.15);
    border-radius: 24px;
    padding: 26px;
    box-shadow: 0 25px 70px rgba(0,0,0,0.45);
}

/* 標題 */
h2 {
    text-align: center;
    margin: 0 0 16px 0;
    font-size: 22px;
    background: linear-gradient(90deg,#4facfe,#00f2fe);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}

/* 🎒 輸入區塊（升級重點） */
label {
    font-weight: 600;
    display: block;
    margin-bottom: 8px;
    opacity: 0.9;
}

/* ✨ 輸入框 UI 升級 */
input[type="text"] {
    width: 100%;
    padding: 16px 16px;
    border-radius: 14px;
    border: 1px solid rgba(255,255,255,0.15);
    font-size: 16px;
    text-align: center;
    font-weight: 600;
    background: rgba(255,255,255,0.06);
    color: white;
    outline: none;
    box-sizing: border-box;
    transition: all 0.25s ease;
    box-shadow: inset 0 0 0 rgba(0,0,0,0);
}

/* 🌟 focus 發光（重點升級） */
input[type="text"]:focus {
    border-color: rgba(79,172,254,0.8);
    box-shadow: 0 0 0 4px rgba(79,172,254,0.15);
    background: rgba(255,255,255,0.09);
}

/* ⏳ 等待區 */
#wait-msg {
    margin-top: 14px;
    background: rgba(0,123,255,0.15);
    padding: 22px;
    border-radius: 16px;
    text-align: center;
    font-weight: bold;
    border: 1px dashed rgba(255,255,255,0.3);
}

/* 📦 題目區 */
#task-area {
    display: none;
    margin-top: 18px;
    background: rgba(255,255,255,0.05);
    padding: 18px;
    border-radius: 18px;
}

/* 題目 */
#question-text {
    font-size: 18px;
    margin-bottom: 10px;
}

/* 提示 */
.hint-text {
    font-size: 13px;
    opacity: 0.8;
    margin-bottom: 10px;
    color: #ffd43b;
}

/* 🔘 選項卡片 */
.option-label {
    display: flex;
    align-items: center;
    padding: 14px;
    margin: 10px 0;
    background: rgba(255,255,255,0.08);
    border: 1px solid rgba(255,255,255,0.15);
    border-radius: 14px;
    cursor: pointer;
    transition: 0.2s;
    font-weight: bold;
}

.option-label:hover {
    background: rgba(255,255,255,0.16);
    transform: translateY(-1px);
}

/* input */
.opt-input {
    width: 18px;
    height: 18px;
    margin-right: 12px;
}

/* ✏️ 文字輸入 */
textarea {
    width: 100%;
    padding: 14px;
    border-radius: 14px;
    border: 1px solid rgba(255,255,255,0.2);
    resize: none;
    font-size: 15px;
    background: rgba(255,255,255,0.05);
    color: white;
    box-sizing: border-box;
}

/* 🔒 鎖定提示 */
.lock-banner {
    display: none;
    background: rgba(255,0,0,0.65);
    padding: 10px;
    border-radius: 10px;
    text-align: center;
    font-weight: bold;
    margin-bottom: 12px;
}

/* 🚀 按鈕升級 */
button {
    width: 100%;
    padding: 16px;
    margin-top: 18px;
    border-radius: 14px;
    border: none;
    font-size: 17px;
    font-weight: bold;
    cursor: pointer;
    background: linear-gradient(135deg,#51cf66,#37b24d);
    color: white;
    box-shadow: 0 10px 25px rgba(55,178,77,0.25);
    transition: all 0.2s ease;
}

/* hover 效果 */
button:hover {
    transform: translateY(-1px);
    box-shadow: 0 14px 30px rgba(55,178,77,0.35);
}

/* disabled */
button:disabled {
    background: #495057;
    box-shadow: none;
    transform: none;
}

/* 📱 RWD */
@media (max-width: 768px) {
    .container { padding: 18px; }
    h2 { font-size: 20px; }
    #question-text { font-size: 16px; }
    button { font-size: 16px; }
}
</style>

<script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.8.0/firebase-database-compat.js"></script>
</head>

<body>

<div class="container">

<h2>📱 學生智慧互動端</h2>

<label>🎒 姓名 / 學號</label>
<input type="text" id="student_name" placeholder="例如：6A 01">

<div id="wait-msg">⏳ 耐心等待老師發布題目...</div>

<div id="task-area">

<div class="lock-banner" id="lock-msg">🔒 已停止作答</div>

<h3 id="question-text"></h3>
<div class="hint-text" id="hint-msg"></div>

<div id="options-container"></div>

<div id="text-container" style="display:none;">
<textarea id="student_essay" rows="3"></textarea>
</div>

<button id="submit-btn" onclick="submitAnswer()">✨ 提交答案</button>

</div>
</div>

<script>
const firebaseConfig = {
apiKey:"AIzaSyD5O7AWuOhufFBTZaveUxUkkpDhu-Rghj4",
authDomain:"workshop-926dc.firebaseapp.com",
databaseURL:"https://workshop-926dc-default-rtdb.asia-southeast1.firebasedatabase.app/",
projectId:"workshop-926dc"
};

firebase.initializeApp(firebaseConfig);
const db=firebase.database();

let currentTask=null;
let locked=false;

/* 題目 */
db.ref('current_task').on('value',s=>{
currentTask=s.val();

if(!currentTask){
document.getElementById("task-area").style.display="none";
document.getElementById("wait-msg").style.display="block";
return;
}

document.getElementById("task-area").style.display="block";
document.getElementById("wait-msg").style.display="none";

document.getElementById("question-text").innerHTML=currentTask.question;

const opt=document.getElementById("options-container");
const text=document.getElementById("text-container");

opt.innerHTML="";

if(currentTask.type==="text"){
opt.style.display="none";
text.style.display="block";
document.getElementById("hint-msg").innerText="請輸入你的想法";
}
else{
opt.style.display="block";
text.style.display="none";

currentTask.options.forEach(o=>{
const div=document.createElement("label");
div.className="option-label";

div.innerHTML=currentTask.type==="single"
? `<input type="radio" name="a" class="opt-input" value="${o}">${o}`
: `<input type="checkbox" class="opt-input cb" value="${o}">${o}`;

opt.appendChild(div);
});
}
});

/* lock */
db.ref('is_locked').on('value',s=>{
locked=!!s.val();
document.getElementById("submit-btn").disabled=locked;
document.getElementById("lock-msg").style.display=locked?"block":"none";
});

/* submit */
function submitAnswer(){
const name=document.getElementById("student_name").value.trim();
if(!name) return alert("請輸入名字");

db.ref('responses/'+name).once('value',snap=>{
if(snap.exists()) return alert("已提交過");

let ans=[];

if(currentTask.type==="text"){
ans.push(document.getElementById("student_essay").value);
}else if(currentTask.type==="single"){
const r=document.querySelector('input[type=radio]:checked');
if(!r) return alert("請選擇");
ans.push(r.value);
}else{
document.querySelectorAll(".cb:checked").forEach(c=>ans.push(c.value));
}

db.ref('responses/'+name).set(ans);
alert("提交成功");
});
}
</script>

</body>
</html>
