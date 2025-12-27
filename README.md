<!DOCTYPE html>
<html lang="fa">
<head>
<meta charset="UTF-8">
<title>پویش مردمی | ارسال پیام انتقادی</title>
<style>
body {
  direction: rtl;
  font-family: tahoma;
  background: #f2f2f2;
  margin: 0;
}
.container {
  max-width: 640px;
  margin: 40px auto;
  background: #fff;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}
h2 {
  margin-bottom: 10px;
  font-size: 20px;
  color: #333;
}
p.description {
  font-size: 14px;
  color: #666;
  margin-bottom: 20px;
}
input, textarea {
  width: 100%;
  padding: 12px;
  margin-top: 8px;
  margin-bottom: 16px;
  border-radius: 6px;
  border: 1px solid #ccc;
  font-size: 14px;
}
textarea {
  height: 180px;
  resize: vertical;
}
button {
  width: 100%;
  padding: 14px;
  background: #1976d2;
  color: #fff;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
}
#status {
  margin-top: 20px;
  font-size: 14px;
  color: #444;
}
</style>
</head>
<body>

<div class="container">
  <h2>ارسال پیام انتقادی به مسئولین</h2>
  <p class="description">
    این بخش برای ثبت اعتراضات، انتقادات و مشکلات واقعی مردم طراحی شده است. پیام شما باید محترمانه، مستند و شفاف باشد تا قابلیت پیگیری و انتشار داشته باشد.
  </p>

  <input id="name" placeholder="نام و نام خانوادگی">
  <input id="title" placeholder="عنوان / نقش شما (مثلاً شهروند، دانشجو، کسبه)">
  <textarea id="message" placeholder="شرح دقیق مشکل، انتقاد یا درخواست شما از مسئولین"></textarea>

  <button onclick="send()">ارسال پیام</button>
  <div id="status"></div>
</div>

<script>
const WORKER_URL = "https://example-worker.yourdomain.workers.dev/"; // آدرس واقعی API یا Worker

function genCode(){
  return "PX-" + Math.random().toString(16).slice(2,8).toUpperCase();
}

async function send(){
  const name = document.getElementById("name").value.trim();
  const title = document.getElementById("title").value.trim();
  const message = document.getElementById("message").value.trim();
  const status = document.getElementById("status");

  status.innerHTML = ""; // پاک‌سازی وضعیت قبلی

  if(!name || !title || !message){
    status.innerHTML = "⚠️ لطفاً همه فیلدها را کامل و دقیق پر کنید.";
    return;
  }

  if(message.length < 40){
    status.innerHTML = "⚠️ متن پیام بسیار کوتاه است. لطفاً جزئیات بیشتری بنویسید.";
    return;
  }

  const code = genCode();

  try {
    const res = await fetch(WORKER_URL, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ name, title, message, code })
    });

    if(res.ok){
      status.innerHTML = `
        ✅ پیام شما با موفقیت ثبت شد.<br>
        کد پیگیری: <strong>${code}</strong><br>
        این کد را برای پیگیری یا انتشار نزد خود نگه دارید.
      `;
      document.getElementById("name").value = "";
      document.getElementById("title").value = "";
      document.getElementById("message").value = "";
    } else {
      status.innerHTML = "❌ خطا در ارسال پیام. لطفاً بعداً دوباره تلاش کنید.";
    }
  } catch (e) {
    status.innerHTML = "❌ اتصال به سرور برقرار نشد.";
  }
}
</script>

</body>
</html>
