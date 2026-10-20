<!doctype html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="robots" content="noindex,nofollow">
<title>فحص البراشية أون لاين</title>
<style>
:root{--bg:#F6F1E7;--ink:#12332B;--card:#fff;--line:#E4DCCB;--g:#1F6B52}
@media (prefers-color-scheme:dark){:root{--bg:#0F1D19;--ink:#EEF3F0;--card:#172A24;--line:#26413A;--g:#2E8B6B;color-scheme:dark}}
body{margin:0;padding:16px;background:var(--bg);color:var(--ink);font:16px/1.7 Tahoma,Arial,sans-serif}
.w{max-width:640px;margin:0 auto}
input{width:100%;min-height:44px;margin:4px 0 10px;padding:0 10px;border:1px solid var(--line);border-radius:10px;background:var(--card);color:var(--ink);font:inherit}
button{min-height:48px;width:100%;border:0;border-radius:12px;background:var(--g);color:#fff;font:inherit;font-weight:700}
#o div{background:var(--card);border:1px solid var(--line);border-radius:10px;padding:8px 12px;margin-top:8px;word-break:break-word}
.ok{border-inline-start:6px solid #2e8b57!important}.bad{border-inline-start:6px solid #d33!important}
</style>
</head>
<body>
<div class="w">
<h2>فحص التطبيق</h2>
<p>اضغط الزر، وبعد ما تخلص ابعت صورة للشاشة كلها.</p>
<label>بريد الأدمن (اختياري لفحص الدخول)<input id="em" type="email" autocomplete="off"></label>
<label>كلمة المرور (اختياري)<input id="pw" type="password" autocomplete="off"></label>
<button id="go" type="button">ابدأ الفحص</button>
<div id="o"></div>
</div>
<script>
var FB="https://firestore.googleapis.com/v1/projects/barashia-online/databases/(default)/documents";
var KEY="AIzaSyAkTo0Bw-UlRK-6KRzIpaPjJbESufhezeA";
var AUTH="https://identitytoolkit.googleapis.com/v1/accounts:signInWithPassword?key="+KEY;
var out=document.getElementById("o");
function log(t,ok){var d=document.createElement("div");d.textContent=t;if(ok===true)d.className="ok";if(ok===false)d.className="bad";out.appendChild(d)}
function J(r){return r.json().catch(function(){return {}})}
function err(d){var e=(d&&d.error)||(Array.isArray(d)&&d[0]&&d[0].error)||d;return JSON.stringify(e).slice(0,220)}
function hint(s){return s===403?" ← القواعد بتمنع (راجع Firestore ← Rules)":s===404?" ← قاعدة Firestore مش موجودة أو معرّف المشروع غلط":s===400?" ← مشكلة في مفتاح الـ API أو في البيانات":s===429?" ← محاولات كتير":""}
async function run(){
  out.innerHTML="";log("بدأ الفحص...");
  try{
    var checks=[["index.html","رفض Firebase","الصفحة العامة"],["admin.html","INVALID_LOGIN_CREDENTIALS","لوحة الإدارة"]];
    for(var i=0;i<checks.length;i++){var c=checks[i],r=await fetch(c[0]+"?t="+Date.now(),{cache:"no-store"}),t=await r.text(),n=t.indexOf(c[1])>-1;log(c[2]+": "+(n?"النسخة الجديدة مرفوعة":"النسخة القديمة (ارفع الجديدة)"),n)}
  }catch(e){log("تعذّر فحص النسخ: "+e.message,false)}
  try{
    var r1=await fetch(FB+":runQuery?key="+KEY,{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify({structuredQuery:{from:[{collectionId:"providers"}],where:{fieldFilter:{field:{fieldPath:"approved"},op:"EQUAL",value:{booleanValue:true}}},limit:1000}})});
    var d1=await J(r1),n1=Array.isArray(d1)?d1.filter(function(x){return x.document}).length:0;
    log("1) قراءة الخدمات الظاهرة: HTTP "+r1.status+(r1.ok?" — عددها "+n1:" — "+err(d1)+hint(r1.status)),r1.ok)
  }catch(e){log("1) قراءة الخدمات: تعذّر الاتصال ("+e.message+")",false)}
  try{
    var r2=await fetch(FB+"/reviews?key="+KEY,{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify({fields:{pid:{stringValue:"TEST"},n:{stringValue:"فحص"},s:{integerValue:"5"},x:{stringValue:"فحص النظام"},t:{stringValue:new Date().toISOString()},approved:{booleanValue:false}}})});
    var d2=await J(r2);
    log("2) إرسال تقييم تجريبي (بيدخل مخفي): HTTP "+r2.status+(r2.ok?" — اتسجّل":" — "+err(d2)+hint(r2.status)),r2.ok)
  }catch(e){log("2) إرسال تقييم: تعذّر الاتصال ("+e.message+")",false)}
  var em=document.getElementById("em").value.trim(),pw=document.getElementById("pw").value;
  if(em&&pw){
    try{var r3=await fetch(AUTH,{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify({email:em,password:pw,returnSecureToken:true})}),d3=await J(r3);
      log("3) فحص الدخول: HTTP "+r3.status+(r3.ok?" — الدخول شغّال":" — "+err(d3)),r3.ok)
    }catch(e){log("3) فحص الدخول: تعذّر الاتصال",false)}
  }else log("3) فحص الدخول: اتخطّى (ما كتبتش بريد وكلمة مرور)");
  log("انتهى الفحص. ابعت صورة للشاشة.")
}
document.getElementById("go").addEventListener("click",run);
</script>
</body>
</html>
