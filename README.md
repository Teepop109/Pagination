# Pagination
HTML และ PHP แบบผสมกัน (PHP Embedded in HTML) ซึ่งใช้ในการแสดงรายการลิงค์ (pagination) สำหรับการเชื่อมโยงไปยังหน้าต่างๆ ที่มีอยู่ในอาร์เรย์ urls ต่อไปนี้คือคำอธิบายทีละบรรทัด:

<main>: เปิดแท็กหลักของหน้าเว็บ ซึ่งใช้ในการจัดโครงสร้างเนื้อหาหลักของ HTML.

<ul>: เปิดแท็กของรายการไม่เรียงลำดับ (unordered list) สำหรับแสดงรายการลิงค์.

<? var count = urls.length; ?>: ประกาศตัวแปร count ที่เก็บค่าความยาวของอาร์เรย์ urls ซึ่งคือจำนวนของ URL ที่มีอยู่ในอาร์เรย์นี้.

<? for(var i = 0; i < count; i++) { ?>: ใช้การวนลูป for เพื่อทำงานกับทุกๆ ค่าในอาร์เรย์ urls. ตัวแปร i จะเริ่มจาก 0 และเพิ่มขึ้นทีละ 1 จนถึงค่าของ count - 1.

<li><a href='<?= urls[i] ?>'>หน้า <?=i + 1 ?></a></li>: สำหรับแต่ละรอบของลูป, สร้างรายการ <li> และภายในนั้นสร้างลิงก์ <a>.

href='<?= urls[i] ?>': ใช้ urls[i] เป็นลิงก์ที่จะแสดงให้ผู้ใช้คลิก.
หน้า <?=i + 1 ?>: แสดงข้อความว่า "หน้า 1", "หน้า 2", "หน้า 3", เป็นต้น (โดยการเพิ่ม 1 เข้าไปใน i เพื่อให้เริ่มนับจาก 1 แทนที่จะเริ่มจาก 0).
<? } ?>: ปิดลูป for ที่เริ่มต้นไว้.

</ul>: ปิดแท็กของรายการไม่เรียงลำดับ.

</main>: ปิดแท็ก <main> ที่เปิดไว้ก่อนหน้านี้.
  
## code.gs
```
function doGet(e) {
  if(e.parameter.page){
    var pageName = e.parameter.page.trim().toLowerCase();
    if (pageName !== "home"){
      var template = HtmlService.createTemplateFromFile(pageName);
      template.url = getPageUrl();
      return template.evaluate();
    }else{
      return homePage();
    }
  }else{
    return homePage();
  }
}

function homePage(){
  var pages = ["page1", "page2", "page3", "page4"];
  var urls = pages.map(function(name){
    return getPageUrl(name);
  });
  var template = HtmlService.createTemplateFromFile("home");
  template.urls = urls;
  return template.evaluate();
}

function getPageUrl(name){
  if (name){
    var url = ScriptApp.getService().getUrl();
    return url + "?page=" + name;
  }else{
    return ScriptApp.getService().getUrl();
  }
}

function test(){
  Logger.log(ScriptApp.getService().getUrl());
}
```

## home.html
```
<!DOCTYPE html>
<html lang="th">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>หน้าเมนูหลัก</title>
    <base target="_top">
    <style>
      body {
        font-family: 'Arial', sans-serif;
        background-color: #f4f4f9;
        color: #333;
        margin: 0;
        padding: 0;
      }
      header {
        background-color: #4CAF50;
        color: white;
        padding: 15px;
        text-align: center;
      }
      h1 {
        font-size: 1.8em;
        margin: 0;
      }
      ul {
        list-style-type: none;
        padding: 0;
        margin: 20px;
      }
      li {
        margin: 8px 0;
        background-color: #fff;
        border-radius: 5px;
        box-shadow: 0 1px 3px rgba(0,0,0,0.1);
      }
      a {
        text-decoration: none;
        color: #333;
        display: block;
        padding: 10px;
        font-size: 1.1em;
        text-align: center;
      }
      a:hover {
        background-color: #4CAF50;
        color: white;
      }
      footer {
        background-color: #333;
        color: white;
        text-align: center;
        padding: 8px;
        position: fixed;
        width: 100%;
        bottom: 0;
      }
    </style>
  </head>
  <body>

    <header>
      <h1>เมนูหลัก</h1>
    </header>

    <main>
      <ul>
        <? var count = urls.length; ?>
        <? for(var i = 0; i < count; i++) { ?>
          <li><a href='<?= urls[i] ?>'>หน้า <?=i + 1 ?></a></li>
        <? } ?>
      </ul>
    </main>

    <footer>
      <p>© 2024 เว็บไซต์ของเรา</p>
    </footer>
    

  </body>
</html>
```

## page1
```
<!DOCTYPE html>
<html>
  <head>
    <base target="_top">
    <style>
      /* จัดกลางเนื้อหาทั้งหมด */
      body {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        margin: 0;
        text-align: center;
        font-family: Arial, sans-serif;
      }

      /* กำหนดขนาดของการ์ด */
      .card {
        width: 25rem;  /* ขยายขนาดการ์ด */
        margin-top: 20px;
        display: inline-block;
      }

      /* เพิ่มช่องว่างให้กับ container */
      .container {
        margin-top: 20px;
      }

      /* กำหนดขนาดของข้อความหัวเรื่อง */
      h2 {
        font-size: 2rem;
        margin-bottom: 20px;
      }

      /* เพิ่มขนาดตัวหนังสือใน card */
      .card-title, .card-text {
        font-size: 1.2rem;
      }

      /* ปรับขนาดของลิงก์กลับหน้าหลัก */
      a {
        font-size: 1.5rem;
        text-decoration: none;
        color: #007bff;
      }

      a:hover {
        text-decoration: underline;
      }
    </style>
  </head>
  <body>

    <!-- ข้อมูลที่จะแสดงในกล่อง -->
    <div class="container">
      <h2>ข้อมูลผู้ใช้</h2>
      <div class="card">
        <img class="card-img-top" src="https://via.placeholder.com/150" alt="Card image cap">
        <div class="card-body">
          <h5 class="card-title">ชื่อ: นายสมชาย</h5>
          <p class="card-text">รหัส: 12345678</p>
          <p class="card-text">แผนก: ฝ่ายเทคโนโลยีสารสนเทศ</p>
        </div>
      </div>

      <!-- ปุ่มกลับหน้าหลัก -->
      <h1>หน้า 1</h1>
      <p><a href='<?= url ?>'>กลับหน้าหลัก</a></p>
    </div>

  </body>
</html>

```
## page2

```
      <!-- ปุ่มกลับหน้าหลัก -->
      <h1>หน้า 1</h1>
      <p><a href='<?= url ?>'>กลับหน้าหลัก</a></p>
    </div>
```
## แบบฝึกหัดปรับเเต่งเว็บไซต์ให้สมบูรณ์โดยมี pagination ประกอบ
