<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="stylecss/style.css">
    <style>
        .container{
    max-width:960px;
    margin:36px auto;
    padding:20px;
}
header h1{
    margin:0 0 8px;
    font-size:1.4rem
}
header p{
    margin:0;
    color:#6b7280;
}
.drop1{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:18px;
    margin-top:18px
}
.card{
    background:#fff;
    border-radius:12px;
    padding:14px;
    box-shadow:0 6px 18px rgba(15,23,42,0.06)
}
.meta{
    font-size:0.85rem;
    color:#6b7280;
    margin-top:8px
}
.items{
    display:flex;
    flex-wrap:wrap;
    gap:8px
}
.item{
    padding:8px 12px;
    border-radius:8px;
    border:1px dashed #d1d5db;
    background:#fff;
    cursor:grab;
    user-select:none
} 
.item [aria-grabbed="true"] {
    outline:2px dashed #2563eb;
    cursor:grabbing
}
.list{
    display:flex;
    flex-wrap:wrap;
    gap:8px
}
.btn{
    padding:6px 12px;
    border-radius:8px;
    border:none;
    background-color: #2563eb;
    color:white;
    cursor:pointer;
}
.hint{
    color:#6b7280;
    font-size:0.9rem;
    gap:10px;
}
.zone-title{
    font-weight:600;
    margin-bottom:6px;
    color:#6b7280;
}
.drop2{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:12px
}
.dropzone{
    min-height:160px;
    border-radius:10px;
    border:2px dashed #e6edf8;
    padding:12px;
    display:flex;
    flex-direction:column;
    gap:8px;
    align-items:flex-start
}

    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>HTML5 Drag & Drop — Mô tả và Thiết kế</h1>
            <p>Ví dụ thực tế có hỗ trợ bàn phím và fallback cho cảm ứng. Kéo thả các phần tử vào vùng nhận.</p>
        </header>
        <section class="drop1">
          <div class="card">
            <h2>Các phần tử có thể kéo</h2>
            <p class="meta">Sử dụng chuột hoặc Tab để chọn.</p>
            <div class="items" role="list" id="palette">
              <button class="item" role="listitem" draggable="true" aria-grabbed="false">Táo</button>
              <button class="item" role="listitem" draggable="true" aria-grabbed="false">Chuối</button>
              <button class="item" role="listitem" draggable="true"   aria-grabbed="false">Cà rốt</button>
              <button class="item" role="listitem" draggable="true" aria-grabbed="false">Cải bó xôi</button>
            </div><br>
            <div class="controls">
              <div class="hint">Kéo vào một trong hai vùng bên phải</div>
              <button id="reset" class="btn" type="button">Đặt lại</button>
            </div>
          </div>
        <div>
        <div class="card" style="margin-bottom:12px">
          <h2>Khu vực nhận</h2>
          <div class="drop2">
            <div class="dropzone" tabindex="0" aria-dropeffect="none"  aria-label="Vùng nhận trái cây">
              <div class="zone-title">Trái cây</div>
              <div class="list" role="list" aria-live="polite"></div>
            </div>
            <div class="dropzone" tabindex="0" aria-dropeffect="none" aria-label="Vùng nhận rau">
              <div class="zone-title">Rau củ</div>
              <div class="list" role="list" aria-live="polite"></div>
            </div>
          </div>
        </div>
      <script>
      document.addEventListener("DOMContentLoaded", () => {
        const items = document.querySelectorAll(".item");
        const zones = document.querySelectorAll(".dropzone");
        const resetBtn = document.getElementById("reset");
        let draggedItem = null;
      //kéo  
      items.forEach(item => {
          item.addEventListener("dragstart", e => {
          draggedItem = item;
          e.dataTransfer.effectAllowed = "move";
          e.dataTransfer.setData("text/plain", item.textContent);
          item.setAttribute("aria-grabbed", "true");
        });
        item.addEventListener("dragend", () => {
          item.setAttribute("aria-grabbed", "false");
          draggedItem = null;
        });
      });
      //vùng nhận
      zones.forEach(zone => {
          zone.addEventListener("dragover", e => {
            e.preventDefault(); //thả
            zone.classList.add("over");
          });
          zone.addEventListener("dragleave", () => {
            zone.classList.remove("over");
          });
          zone.addEventListener("drop", e => {
            e.preventDefault();
            zone.classList.remove("over");
            if (draggedItem) {
              zone.querySelector(".list").appendChild(draggedItem);
              draggedItem.setAttribute("aria-grabbed", "false");
              draggedItem = null;
            }
          });
      });
        //reset
        resetBtn.addEventListener("click", () => {
            const palette = document.getElementById("palette");
              document.querySelectorAll(".dropzone .item").forEach(it => {
                palette.appendChild(it);
                });
        });
      });
      </script>
    </div>
</body>
</html>
