<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>مهندس محاسبات</title>

<style>
*{box-sizing:border-box}
body{
 margin:0;
 font-family:Tahoma,Arial,sans-serif;
 background:#101827;
 color:white;
}
header{
 background:#172b46;
 padding:22px 12px;
 text-align:center;
 border-bottom:3px solid #16c7a0;
}
header h1{margin:0;color:#16c7a0}
header p{color:#cbd5e1}
select,input,button{
 width:100%;
 padding:13px;
 border-radius:9px;
 border:none;
 margin:6px 0;
 font-size:16px;
}
select,input{background:#26364c;color:white}
button{
 background:#16a085;
 color:white;
 cursor:pointer;
 font-weight:bold;
}
button:active{opacity:.7}
.container{
 max-width:600px;
 margin:auto;
 padding:15px;
}
.card{
 background:#1c2b40;
 padding:17px;
 border-radius:14px;
 margin:15px 0;
}
h2{color:#16c7a0;font-size:20px}
.result{
 background:#102d2b;
 color:#4ef0c0;
 padding:15px;
 border-radius:9px;
 margin-top:10px;
 font-size:18px;
 text-align:center;
}
.grid{
 display:grid;
 grid-template-columns:repeat(2,1fr);
 gap:8px;
}
footer{text-align:center;color:#94a3b8;padding:25px}
</style>
</head>

<body>

<header>
<h1>مهندس محاسبات</h1>
<p>محاسبات هوشمند ریاضی و ساختمانی</p>
</header>

<div class="container">

<div class="card">
<h2>🌐 انتخاب زبان</h2>
<select id="language" onchange="changeLanguage()">
<option value="fa">دری</option>
<option value="ps">پشتو</option>
<option value="en">English</option>
</select>
</div>

<div class="card">
<h2>🧮 ماشین حساب عادی</h2>
<input id="display" type="text" readonly placeholder="0">

<div class="grid">
<button onclick="add('7')">7</button>
<button onclick="add('8')">8</button>
<button onclick="add('9')">9</button>
<button onclick="add('/')">÷</button>

<button onclick="add('4')">4</button>
<button onclick="add('5')">5</button>
<button onclick="add('6')">6</button>
<button onclick="add('*')">×</button>

<button onclick="add('1')">1</button>
<button onclick="add('2')">2</button>
<button onclick="add('3')">3</button>
<button onclick="add('-')">−</button>

<button onclick="add('0')">0</button>
<button onclick="add('.')">.</button>
<button onclick="calculate()">=</button>
<button onclick="add('+')">+</button>
</div>
<button onclick="clearDisplay()" style="background:#c0392b">پاک کردن</button>
</div>

<div class="card">
<h2>📐 محاسبه مساحت مستطیل</h2>
<label>طول (متر)</label>
<input type="number" id="length" placeholder="طول">
<label>عرض (متر)</label>
<input type="number" id="width" placeholder="عرض">
<button onclick="area()">محاسبه مساحت</button>
<div class="result" id="areaResult">نتیجه</div>
</div>

<div class="card">
<h2>📦 محاسبه حجم</h2>
<label>طول (متر)</label>
<input type="number" id="vl">
<label>عرض (متر)</label>
<input type="number" id="vw">
<label>ارتفاع (متر)</label>
<input type="number" id="vh">
<button onclick="volume()">محاسبه حجم</button>
<div class="result" id="volumeResult">نتیجه</div>
</div>

<div class="card">
<h2>🧱 محاسبه تعداد خشت دیوار</h2>
<label>طول دیوار (متر)</label>
<input type="number" id="wl">
<label>ارتفاع دیوار (متر)</label>
<input type="number" id="wh">
<label>طول خشت (سانتی‌متر)</label>
<input type="number" id="bl">
<label>ارتفاع خشت (سانتی‌متر)</label>
<input type="number" id="bh">
<label>ضخامت بند گِل (سانتی‌متر)</label>
<input type="number" id="joint" value="1">
<button onclick="bricks()">محاسبه تعداد خشت</button>
<div class="result" id="brickResult">نتیجه</div>
</div>

<footer>
مهندس محاسبات | Engineer Calculations<br>
نسخه 1.0
</footer>

</div>

<script>
function add(value){
 document.getElementById('display').value+=value;
}

function clearDisplay(){
 document.getElementById('display').value='';
}

function calculate(){
 const d=document.getElementById('display');
 try{
  const s=d.value;
  if(!s || !/^[0-9+\-*/.() ]+$/.test(s)) throw Error();
  const result=Function('"use strict";return ('+s+')')();
  if(!Number.isFinite(result)) throw Error();
  d.value=Number(result.toPrecision(12));
 }catch(e){
  d.value='خطا';
 }
}

function getNumbers(ids){
 return ids.map(id=>Number(document.getElementById(id).value));
}

function area(){
 const [l,w]=getNumbers(['length','width']);
 const r=document.getElementById('areaResult');
 if(l<=0||w<=0||!Number.isFinite(l+w)){
  r.innerText='اعداد معتبر وارد کنید';
  return;
 }
 r.innerText='مساحت: '+(l*w).toFixed(3)+' متر مربع';
}

function volume(){
 const [l,w,h]=getNumbers(['vl','vw','vh']);
 const r=document.getElementById('volumeResult');
 if(l<=0||w<=0||h<=0||!Number.isFinite(l+w+h)){
  r.innerText='اعداد معتبر وارد کنید';
  return;
 }
 r.innerText='حجم: '+(l*w*h).toFixed(3)+' متر مکعب';
}

function bricks(){
 const [l,h,bl,bh,j]=getNumbers(['wl','wh','bl','bh','joint']);
 const r=document.getElementById('brickResult');
 if(l<=0||h<=0||bl<=0||bh<=0||j<0||!Number.isFinite(l+h+bl+bh+j)){
  r.innerText='اعداد معتبر وارد کنید';
  return;
 }
 const brickArea=((bl+j)*(bh+j))/10000;
 const wallArea=l*h;
 const count=Math.ceil(wallArea/brickArea);
 r.innerText='تعداد تقریبی خشت: '+count.toLocaleString('en-US')+' عدد';
}

function changeLanguage(){
 const lang=document.getElementById('language').value;
 document.documentElement.lang=lang;
 document.documentElement.dir=lang==='en'?'ltr':'rtl';
 alert(lang==='en'?'English selected':lang==='ps'?'پښتو ژبه وټاکل شوه':'زبان دری انتخاب شد');
}
</script>

</body>
</html>
