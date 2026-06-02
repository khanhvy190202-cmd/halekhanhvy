<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Luyện Phát Âm Tiếng Trung</title>

<style>
body{
    font-family: Arial, sans-serif;
    background:#f4f7fa;
    display:flex;
    justify-content:center;
    align-items:center;
    min-height:100vh;
    margin:0;
}

.container{
    background:white;
    width:500px;
    padding:30px;
    border-radius:15px;
    box-shadow:0 5px 15px rgba(0,0,0,0.1);
    text-align:center;
}

h1{
    color:#d32f2f;
}

.word{
    font-size:60px;
    margin:20px 0;
}

.pinyin{
    font-size:24px;
    color:#666;
}

.meaning{
    font-size:20px;
    margin-top:10px;
}

button{
    padding:12px 20px;
    margin:8px;
    border:none;
    border-radius:8px;
    cursor:pointer;
    font-size:16px;
}

.listen{
    background:#4caf50;
    color:white;
}

.record{
    background:#2196f3;
    color:white;
}

.next{
    background:#ff9800;
    color:white;
}

#result{
    margin-top:20px;
    font-size:18px;
    font-weight:bold;
}

.stats{
    margin-top:20px;
    display:flex;
    justify-content:space-around;
}

.box{
    background:#f5f5f5;
    padding:10px;
    border-radius:10px;
    width:120px;
}
</style>
</head>

<body>

<div class="container">

<h1>🎤 Luyện Phát Âm Tiếng Trung</h1>

<div id="word" class="word">你好</div>

<div id="pinyin" class="pinyin">nǐ hǎo</div>

<div id="meaning" class="meaning">Xin chào</div>

<button class="listen" onclick="playAudio()">
🔊 Nghe mẫu
</button>

<button class="record" onclick="startRecording()">
🎤 Phát âm
</button>

<button class="next" onclick="nextWord()">
➡️ Từ mới
</button>

<div id="result"></div>

<div class="stats">
    <div class="box">
        <h3>✅ Đúng</h3>
        <span id="correct">0</span>
    </div>

    <div class="box">
        <h3>❌ Sai</h3>
        <span id="wrong">0</span>
    </div>
</div>

</div>

<script>

const words = [

{cn:"你好", py:"nǐ hǎo", vi:"Xin chào"},
{cn:"谢谢", py:"xiè xie", vi:"Cảm ơn"},
{cn:"再见", py:"zài jiàn", vi:"Tạm biệt"},
{cn:"老师", py:"lǎo shī", vi:"Giáo viên"},
{cn:"学生", py:"xué sheng", vi:"Học sinh"},
{cn:"中国", py:"zhōng guó", vi:"Trung Quốc"},
{cn:"越南", py:"yuè nán", vi:"Việt Nam"},
{cn:"苹果", py:"píng guǒ", vi:"Táo"},
{cn:"水", py:"shuǐ", vi:"Nước"},
{cn:"茶", py:"chá", vi:"Trà"},
{cn:"猫", py:"māo", vi:"Mèo"},
{cn:"狗", py:"gǒu", vi:"Chó"},
{cn:"花", py:"huā", vi:"Hoa"},
{cn:"树", py:"shù", vi:"Cây"},
{cn:"书", py:"shū", vi:"Sách"},
{cn:"家", py:"jiā", vi:"Nhà"},
{cn:"朋友", py:"péng yǒu", vi:"Bạn bè"},
{cn:"飞机", py:"fēi jī", vi:"Máy bay"},
{cn:"医院", py:"yī yuàn", vi:"Bệnh viện"},
{cn:"学校", py:"xué xiào", vi:"Trường học"}

];

let current = 0;
let correct = 0;
let wrong = 0;

function loadWord(){

document.getElementById("word").innerText =
words[current].cn;

document.getElementById("pinyin").innerText =
words[current].py;

document.getElementById("meaning").innerText =
words[current].vi;

document.getElementById("result").innerHTML = "";
}

function nextWord(){

current =
Math.floor(Math.random()*words.length);

loadWord();
}

function playAudio(){

const speech =
new SpeechSynthesisUtterance(words[current].cn);

speech.lang = "zh-CN";

speech.rate = 0.8;

speechSynthesis.speak(speech);
}

function startRecording(){

if(!('webkitSpeechRecognition' in window)){

alert("Vui lòng dùng Google Chrome.");

return;
}

const recognition =
new webkitSpeechRecognition();

recognition.lang = "zh-CN";
recognition.interimResults = false;
recognition.maxAlternatives = 1;

recognition.start();

document.getElementById("result").innerHTML =
"🎤 Đang nghe...";

recognition.onresult = function(event){

const spoken =
event.results[0][0].transcript.trim();

checkAnswer(spoken);
};

recognition.onerror = function(){

document.getElementById("result").innerHTML =
"⚠️ Không nhận diện được giọng nói.";
};
}

function checkAnswer(spoken){

const answer = words[current].cn;

if(spoken.includes(answer)){

correct++;

document.getElementById("result").innerHTML =
"✅ Chính xác! Bạn nói: " + spoken;

}else{

wrong++;

document.getElementById("result").innerHTML =
"❌ Chưa đúng<br>Đáp án: " + answer +
"<br>Bạn nói: " + spoken;
}

document.getElementById("correct").innerText =
correct;

document.getElementById("wrong").innerText =
wrong;
}

loadWord();

</script>

</body>
</html>
