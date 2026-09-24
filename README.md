<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>予約表</title>

<style>
*{
  box-sizing:border-box;
}

body{
  margin:0;
  font-family:-apple-system,BlinkMacSystemFont,"Helvetica Neue",Arial,sans-serif;
  background:#f5f5f5;
  color:#222;
}

button,
input,
select{
  font:inherit;
}

.header{
  position:sticky;
  top:0;
  z-index:20;
  background:#fff;
  border-bottom:1px solid #ddd;
  padding:10px;
}

.header-row{
  display:flex;
  align-items:center;
  justify-content:space-between;
  gap:6px;
}

.date-box{
  display:flex;
  align-items:center;
  gap:5px;
}

.date-box input{
  width:145px;
  padding:8px;
  border:1px solid #ccc;
  border-radius:8px;
}

.top-btn{
  border:0;
  border-radius:8px;
  padding:8px 10px;
  background:#222;
  color:#fff;
}

.schedule{
  padding:10px;
  overflow-x:auto;
}

.grid{
  display:grid;
  grid-template-columns:60px repeat(2,minmax(150px,1fr));
  background:#fff;
  border:1px solid #ddd;
  border-radius:10px;
  overflow:hidden;
  min-width:360px;
}

.cell{
  min-height:58px;
  border-right:1px solid #ddd;
  border-bottom:1px solid #ddd;
  padding:5px;
}

.header-cell{
  min-height:42px;
  background:#fafafa;
  font-weight:bold;
  display:flex;
  align-items:center;
  justify-content:center;
  text-align:center;
}

.time-cell{
  color:#666;
  font-size:12px;
  display:flex;
  justify-content:center;
  align-items:flex-start;
  padding-top:9px;
}

.empty-btn{
  width:100%;
  min-height:45px;
  border:1px dashed #ccc;
  border-radius:7px;
  background:#fafafa;
  color:#777;
}

.unavailable-btn{
  width:100%;
  min-height:45px;
  border:1px solid #eee;
  border-radius:7px;
  background:#eee;
  color:#aaa;
  font-size:12px;
}

.reservation{
  width:100%;
  min-height:45px;
  border:0;
  border-radius:7px;
  background:#dff2ff;
  padding:5px;
  text-align:left;
}

.r-time{
  font-weight:bold;
  font-size:13px;
}

.r-info{
  font-size:11px;
  margin-top:2px;
}

.r-ratio{
  font-size:10px;
  margin-top:2px;
  color:#555;
}

.bottom-area{
  padding:10px;
}

.big-btn{
  width:100%;
  border:0;
  border-radius:10px;
  padding:13px;
  background:#222;
  color:#fff;
}

.list{
  margin-top:10px;
}

.list-item{
  background:#fff;
  border:1px solid #ddd;
  border-radius:10px;
  padding:10px;
  margin-bottom:8px;
}

.list-ratio{
  color:#555;
  font-size:12px;
  margin-top:3px;
}

.modal-bg{
  display:none;
  position:fixed;
  inset:0;
  z-index:100;
  background:rgba(0,0,0,.5);
  align-items:flex-end;
  justify-content:center;
}

.modal-bg.show{
  display:flex;
}

.modal{
  width:100%;
  max-width:650px;
  max-height:90vh;
  overflow:auto;
  background:#fff;
  border-radius:18px 18px 0 0;
  padding:18px;
}

.modal h2{
  margin-top:0;
}

.form-row{
  margin-bottom:14px;
}

.form-row label{
  display:block;
  font-size:13px;
  color:#666;
  margin-bottom:5px;
}

.form-row input,
.form-row select{
  width:100%;
  padding:11px;
  border:1px solid #ccc;
  border-radius:8px;
  background:#fff;
}

.modal-buttons{
  display:flex;
  gap:8px;
  margin-top:15px;
}

.modal-buttons button{
  flex:1;
  padding:12px;
  border:0;
  border-radius:8px;
}

.save-btn{
  background:#222;
  color:#fff;
}

.cancel-btn{
  background:#eee;
}

.delete-btn{
  background:#d9534f;
  color:#fff;
}

.notice{
  background:#fff7d6;
  border:1px solid #f0d875;
  border-radius:8px;
  padding:10px;
  font-size:13px;
  margin-bottom:12px;
}

.small-btn{
  border:0;
  border-radius:7px;
  padding:7px 10px;
  background:#eee;
}

.setting-girl{
  display:flex;
  gap:6px;
  margin-bottom:8px;
}

.setting-girl input{
  flex:1;
  padding:8px;
  border:1px solid #ccc;
  border-radius:7px;
}

.course-head{
  display:grid;
  grid-template-columns:1.1fr .8fr 1fr .8fr .8fr 48px;
  gap:5px;
  margin-bottom:5px;
  font-size:10px;
  color:#777;
  text-align:center;
}

.course-row{
  display:grid;
  grid-template-columns:1.1fr .8fr 1fr .8fr .8fr 48px;
  gap:5px;
  margin-bottom:7px;
  align-items:center;
}

.course-row input{
  width:100%;
  padding:8px 5px;
  border:1px solid #ccc;
  border-radius:7px;
  min-width:0;
}

.course-delete{
  border:0;
  border-radius:7px;
  padding:8px 3px;
  background:#eee;
  font-size:11px;
}

.ratio-preview{
  background:#f7f7f7;
  border-radius:7px;
  padding:7px;
  margin-top:8px;
  font-size:12px;
  text-align:center;
}

.summary-box{
  background:#f7f7f7;
  border-radius:10px;
  padding:12px;
  margin-bottom:8px;
}

.summary-ratio{
  margin-top:5px;
  font-size:13px;
}

.summary-money{
  margin-top:3px;
  font-size:13px;
}

@media(max-width:500px){

  .course-head{
    grid-template-columns:1.1fr .7fr .9fr .7fr .7fr 42px;
    font-size:9px;
  }

  .course-row{
    grid-template-columns:1.1fr .7fr .9fr .7fr .7fr 42px;
  }

  .course-row input{
    font-size:12px;
  }

}
</style>
</head>

<body>

<div class="header">
  <div class="header-row">

    <div class="date-box">

      <button
        class="top-btn"
        onclick="changeDate(-1)"
      >
        ‹
      </button>

      <input
        type="date"
        id="dateInput"
        onchange="loadDate()"
      >

      <button
        class="top-btn"
        onclick="changeDate(1)"
      >
        ›
      </button>

    </div>

    <div>

      <button
        class="top-btn"
        onclick="openSummary()"
      >
        集計
      </button>

      <button
        class="top-btn"
        onclick="openSettings()"
      >
        設定
      </button>

    </div>

  </div>
</div>


<div class="schedule">

  <div
    id="scheduleGrid"
    class="grid"
  ></div>

</div>


<div class="bottom-area">

  <button
    class="big-btn"
    onclick="openNewReservation()"
  >
    ＋ 新しい予約
  </button>

  <div
    id="reservationList"
    class="list"
  ></div>

</div>


<!-- =========================
     予約モーダル
========================= -->

<div
  id="reservationModal"
  class="modal-bg"
>

  <div class="modal">

    <h2 id="reservationTitle">
      新しい予約
    </h2>

    <div class="notice">

      同じ女の子は、前の予約が終了してから
      <strong>5分空ける必要があります。</strong>

    </div>


    <div class="form-row">

      <label>
        お客様名
      </label>

      <input
        id="customerInput"
        type="text"
        placeholder="例：田中様"
      >

    </div>


    <div class="form-row">

      <label>
        女の子
      </label>

      <select
        id="girlInput"
      ></select>

    </div>


    <div class="form-row">

      <label>
        開始時間
      </label>

      <input
        id="timeInput"
        type="time"
        min="10:00"
        max="23:59"
      >

    </div>


    <div class="form-row">

      <label>
        コース
      </label>

      <select
        id="courseInput"
        onchange="updateCoursePreview()"
      ></select>

      <div
        id="coursePreview"
        class="ratio-preview"
      ></div>

    </div>


    <div class="modal-buttons">

      <button
        class="cancel-btn"
        onclick="closeReservationModal()"
      >
        キャンセル
      </button>

      <button
        class="save-btn"
        onclick="saveReservation()"
      >
        保存
      </button>

    </div>


    <div
      class="modal-buttons"
      id="deleteArea"
    >

      <button
        class="delete-btn"
        onclick="deleteReservation()"
      >
        この予約を削除
      </button>

    </div>

  </div>

</div>


<!-- =========================
     設定モーダル
========================= -->

<div
  id="settingsModal"
  class="modal-bg"
>

  <div class="modal">

    <h2>
      設定
    </h2>


    <h3>
      女の子
    </h3>

    <div
      id="girlsSettings"
    ></div>

    <button
      class="small-btn"
      onclick="addGirl()"
    >
      ＋ 女の子を追加
    </button>


    <h3>
      コース
    </h3>

    <div class="course-head">

      <div>
        コース名
      </div>

      <div>
        分
      </div>

      <div>
        料金
      </div>

      <div>
        店％
      </div>

      <div>
        女の子％
      </div>

      <div>
        削除
      </div>

    </div>


    <div
      id="coursesSettings"
    ></div>


    <button
      class="small-btn"
      onclick="addCourse()"
    >
      ＋ コースを追加
    </button>


    <div class="notice" style="margin-top:12px;">

      <strong>割合について</strong><br>

      「店％」と「女の子％」を自由に変更できます。<br>

      例：店50％・女の子50％ → <strong>50：50</strong><br>

      店60％・女の子40％ → <strong>60：40</strong><br>

      合計は100％にしてください。

    </div>


    <div class="modal-buttons">

      <button
        class="cancel-btn"
        onclick="closeSettings()"
      >
        キャンセル
      </button>

      <button
        class="save-btn"
        onclick="saveSettings()"
      >
        設定を保存
      </button>

    </div>

  </div>

</div>


<!-- =========================
     集計モーダル
========================= -->

<div
  id="summaryModal"
  class="modal-bg"
>

  <div class="modal">

    <h2>
      本日の集計
    </h2>

    <div
      id="summaryContent"
    ></div>

    <div class="modal-buttons">

      <button
        class="cancel-btn"
        onclick="closeSummary()"
      >
        閉じる
      </button>

    </div>

  </div>

</div>


<script>

/* =====================================================
   基本設定
===================================================== */

const START_TIME = 10 * 60;

const END_TIME = 24 * 60;

const GAP = 5;


/* =====================================================
   初期設定
===================================================== */

const DEFAULT_SETTINGS = {

  girls:[
    "ことねさん",
    "ナヨンさん"
  ],

  courses:[

    {
      name:"40分",
      minutes:40,
      price:10000,
      shopPercent:50,
      girlPercent:50
    },

    {
      name:"60分",
      minutes:60,
      price:14000,
      shopPercent:50,
      girlPercent:50
    },

    {
      name:"90分",
      minutes:90,
      price:20000,
      shopPercent:50,
      girlPercent:50
    },

    {
      name:"100分",
      minutes:100,
      price:22000,
      shopPercent:50,
      girlPercent:50
    },

    {
      name:"120分",
      minutes:120,
      price:26000,
      shopPercent:50,
      girlPercent:50
    }

  ]

};


/* =====================================================
   設定読み込み
===================================================== */

let settings =
  JSON.parse(
    localStorage.getItem(
      "reservationSettings"
    )
  ) || DEFAULT_SETTINGS;


/*
  古い設定に割合が存在しない場合、
  50：50を追加する。
*/

settings.courses =
  settings.courses.map(
    function(course){

      return {

        name:
          course.name || "コース",

        minutes:
          Number(course.minutes) || 0,

        price:
          Number(course.price) || 0,

        shopPercent:
          course.shopPercent !== undefined
            ? Number(course.shopPercent)
            : 50,

        girlPercent:
          course.girlPercent !== undefined
            ? Number(course.girlPercent)
            : 50

      };

    }
  );


let reservations = [];

let editingId = null;


/* =====================================================
   初期化
===================================================== */

document.addEventListener(
  "DOMContentLoaded",
  function(){

    const now =
      new Date();


    const yyyy =
      now.getFullYear();


    const mm =
      String(
        now.getMonth()+1
      ).padStart(2,"0");


    const dd =
      String(
        now.getDate()
      ).padStart(2,"0");


    document.getElementById(
      "dateInput"
    ).value =
      yyyy + "-" + mm + "-" + dd;


    loadDate();

  }
);


/* =====================================================
   日付
===================================================== */

function getDate(){

  return document.getElementById(
    "dateInput"
  ).value;

}


function getStorageKey(){

  return "reservations_" +
    getDate();

}


function loadDate(){

  const saved =
    localStorage.getItem(
      getStorageKey()
    );


  if(saved){

    try{

      reservations =
        JSON.parse(saved);

    }catch(e){

      reservations = [];

    }

  }else{

    reservations = [];

  }


  renderAll();

}


function saveData(){

  localStorage.setItem(
    getStorageKey(),
    JSON.stringify(
      reservations
    )
  );

}


function changeDate(amount){

  const input =
    document.getElementById(
      "dateInput"
    );


  const date =
    new Date(
      input.value +
      "T00:00:00"
    );


  date.setDate(
    date.getDate() +
    amount
  );


  const yyyy =
    date.getFullYear();


  const mm =
    String(
      date.getMonth()+1
    ).padStart(2,"0");


  const dd =
    String(
      date.getDate()
    ).padStart(2,"0");


  input.value =
    yyyy + "-" + mm + "-" + dd;


  loadDate();

}


/* =====================================================
   時間
===================================================== */

function timeToMinutes(time){

  if(!time){

    return 0;

  }


  const p =
    String(time).split(":");


  return (
    Number(p[0]) * 60 +
    Number(p[1])
  );

}


function minutesToTime(minutes){

  const h =
    Math.floor(
      minutes / 60
    );


  const m =
    minutes % 60;


  return (
    String(h).padStart(2,"0") +
    ":" +
    String(m).padStart(2,"0")
  );

}


/* =====================================================
   コース取得
===================================================== */

function getCourseByMinutes(minutes){

  return settings.courses.find(
    function(course){

      return Number(
        course.minutes
      ) === Number(
        minutes
      );

    }
  );

}


/* =====================================================
   割合表示
===================================================== */

function getRatioText(course){

  if(!course){

    return "50：50";

  }


  const shop =
    Number(
      course.shopPercent
    ) || 0;


  const girl =
    Number(
      course.girlPercent
    ) || 0;


  return (
    shop +
    "：" +
    girl
  );

}


/* =====================================================
   店取り分
===================================================== */

function getShopAmount(course){

  if(!course){

    return 0;

  }


  const price =
    Number(
      course.price
    ) || 0;


  const percent =
    Number(
      course.shopPercent
    ) || 0;


  return Math.round(
    price *
    percent /
    100
  );

}


/* =====================================================
   女の子取り分
===================================================== */

function getGirlAmount(course){

  if(!course){

    return 0;

  }


  const price =
    Number(
      course.price
    ) || 0;


  const percent =
    Number(
      course.girlPercent
    ) || 0;


  return Math.round(
    price *
    percent /
    100
  );

}


/* =====================================================
   5分ルール
===================================================== */

function hasTimeConflict(
  girl,
  newTime,
  newCourse,
  ignoreId
){

  const newStart =
    timeToMinutes(
      newTime
    );


  const newEnd =
    newStart +
    Number(newCourse);


  for(
    let i=0;
    i<reservations.length;
    i++
  ){

    const old =
      reservations[i];


    if(
      String(old.girl) !==
      String(girl)
    ){

      continue;

    }


    if(
      ignoreId !== null &&
      ignoreId !== undefined &&
      String(old.id) ===
      String(ignoreId)
    ){

      continue;

    }


    const oldStart =
      timeToMinutes(
        old.time
      );


    const oldEnd =
      oldStart +
      Number(old.course);


    /*
      完全に前に終わっていて、
      5分以上空いている場合
    */

    if(
      newEnd <=
      oldStart - GAP
    ){

      continue;

    }


    /*
      完全に後ろで、
      5分以上空いている場合
    */

    if(
      newStart >=
      oldEnd + GAP
    ){

      continue;

    }


    /*
      それ以外はNG
    */

    return true;

  }


  return false;

}


/* =====================================================
   表の予約可能判定
===================================================== */

function isGridTimeAvailable(
  girl,
  gridTime
){

  const start =
    timeToMinutes(
      gridTime
    );


  for(
    let i=0;
    i<reservations.length;
    i++
  ){

    const r =
      reservations[i];


    if(
      String(r.girl) !==
      String(girl)
    ){

      continue;

    }


    const oldStart =
      timeToMinutes(
        r.time
      );


    const oldEnd =
      oldStart +
      Number(r.course);


    if(
      start >=
      oldStart - GAP
      &&
      start <
      oldEnd + GAP
    ){

      return false;

    }

  }


  return true;

}


/* =====================================================
   24時チェック
===================================================== */

function goesPastMidnight(
  time,
  course
){

  return (
    timeToMinutes(time) +
    Number(course) >
    END_TIME
  );

}


/* =====================================================
   新規予約
===================================================== */

function openNewReservation(
  presetTime,
  presetGirl
){

  editingId = null;


  document.getElementById(
    "reservationTitle"
  ).textContent =
    "新しい予約";


  document.getElementById(
    "customerInput"
  ).value = "";


  fillGirlSelect();

  fillCourseSelect();


  document.getElementById(
    "timeInput"
  ).value =
    presetTime || "10:00";


  if(presetGirl){

    document.getElementById(
      "girlInput"
    ).value =
      presetGirl;

  }


  document.getElementById(
    "deleteArea"
  ).style.display =
    "none";


  updateCoursePreview();


  document.getElementById(
    "reservationModal"
  ).classList.add("show");

}


/* =====================================================
   編集
===================================================== */

function openEditReservation(id){

  const r =
    reservations.find(
      function(item){

        return String(item.id) ===
          String(id);

      }
    );


  if(!r){

    return;

  }


  editingId =
    r.id;


  document.getElementById(
    "reservationTitle"
  ).textContent =
    "予約を編集";


  document.getElementById(
    "customerInput"
  ).value =
    r.customer || "";


  fillGirlSelect();

  fillCourseSelect();


  document.getElementById(
    "girlInput"
  ).value =
    r.girl;


  document.getElementById(
    "timeInput"
  ).value =
    r.time;


  document.getElementById(
    "courseInput"
  ).value =
    String(r.course);


  document.getElementById(
    "deleteArea"
  ).style.display =
    "flex";


  updateCoursePreview();


  document.getElementById(
    "reservationModal"
  ).classList.add("show");

}


/* =====================================================
   女の子選択
===================================================== */

function fillGirlSelect(){

  const select =
    document.getElementById(
      "girlInput"
    );


  select.innerHTML = "";


  settings.girls.forEach(
    function(girl){

      const option =
        document.createElement(
          "option"
        );


      option.value =
        girl;


      option.textContent =
        girl;


      select.appendChild(
        option
      );

    }
  );

}


/* =====================================================
   コース選択
===================================================== */

function fillCourseSelect(){

  const select =
    document.getElementById(
      "courseInput"
    );


  select.innerHTML = "";


  settings.courses.forEach(
    function(course){

      const option =
        document.createElement(
          "option"
        );


      option.value =
        course.minutes;


      option.textContent =
        course.name +
        " / " +
        getRatioText(course) +
        " / " +
        Number(course.price).toLocaleString() +
        "円";


      select.appendChild(
        option
      );

    }
  );

}


/* =====================================================
   コース割合プレビュー
===================================================== */

function updateCoursePreview(){

  const select =
    document.getElementById(
      "courseInput"
    );


  if(!select){

    return;

  }


  const course =
    getCourseByMinutes(
      Number(
        select.value
      )
    );


  const preview =
    document.getElementById(
      "coursePreview"
    );


  if(!course){

    preview.textContent =
      "";

    return;

  }


  preview.innerHTML =
    "店：" +
    course.shopPercent +
    "%　／　" +
    "女の子：" +
    course.girlPercent +
    "%　→　" +
    "<strong>" +
    getRatioText(course) +
    "</strong>" +
    "　／　" +
    Number(course.price).toLocaleString() +
    "円";

}


/* =====================================================
   予約保存
===================================================== */

function saveReservation(){

  const customer =
    document.getElementById(
      "customerInput"
    ).value.trim();


  const girl =
    document.getElementById(
      "girlInput"
    ).value;


  const time =
    document.getElementById(
      "timeInput"
    ).value;


  const course =
    Number(
      document.getElementById(
        "courseInput"
      ).value
    );


  if(!girl){

    alert(
      "女の子を選択してください。"
    );

    return;

  }


  if(!time){

    alert(
      "開始時間を入力してください。"
    );

    return;

  }


  if(!course){

    alert(
      "コースを選択してください。"
    );

    return;

  }


  const start =
    timeToMinutes(
      time
    );


  if(start < START_TIME){

    alert(
      "10:00以降で入力してください。"
    );

    return;

  }


  if(
    goesPastMidnight(
      time,
      course
    )
  ){

    alert(
      "24:00を超える予約はできません。"
    );

    return;

  }


  /*
    ★ 最終的な強制チェック
  */

  const conflict =
    hasTimeConflict(
      girl,
      time,
      course,
      editingId
    );


  if(conflict){

    alert(
      "予約できません。\n\n" +
      "同じ女の子の予約は、前の予約終了から5分以上空けてください。"
    );

    return;

  }


  const newReservation = {

    id:
      editingId !== null
        ? editingId
        : Date.now(),

    customer:
      customer,

    girl:
      girl,

    time:
      time,

    course:
      course

  };


  if(
    editingId !== null
  ){

    const index =
      reservations.findIndex(
        function(item){

          return String(item.id) ===
            String(editingId);

        }
      );


    if(index !== -1){

      reservations[index] =
        newReservation;

    }

  }else{

    reservations.push(
      newReservation
    );

  }


  reservations.sort(
    function(a,b){

      return (
        timeToMinutes(a.time) -
        timeToMinutes(b.time)
      );

    }
  );


  saveData();

  closeReservationModal();

  renderAll();

}


/* =====================================================
   削除
===================================================== */

function deleteReservation(){

  if(
    editingId === null
  ){

    return;

  }


  const r =
    reservations.find(
      function(item){

        return String(item.id) ===
          String(editingId);

      }
    );


  if(!r){

    return;

  }


  const ok =
    confirm(
      "この予約を削除しますか？\n\n" +
      r.time +
      " " +
      r.girl
    );


  if(!ok){

    return;

  }


  reservations =
    reservations.filter(
      function(item){

        return String(item.id) !==
          String(editingId);

      }
    );


  saveData();

  closeReservationModal();

  renderAll();

}


/* =====================================================
   予約モーダルを閉じる
===================================================== */

function closeReservationModal(){

  document.getElementById(
    "reservationModal"
  ).classList.remove("show");


  editingId = null;

}


/* =====================================================
   予約表
===================================================== */

function renderSchedule(){

  const grid =
    document.getElementById(
      "scheduleGrid"
    );


  grid.innerHTML = "";


  /*
    ヘッダー
  */

  const blank =
    document.createElement(
      "div"
    );


  blank.className =
    "cell header-cell";


  blank.textContent =
    "時間";


  grid.appendChild(
    blank
  );


  settings.girls.forEach(
    function(girl){

      const header =
        document.createElement(
          "div"
        );


      header.className =
        "cell header-cell";


      header.textContent =
        girl;


      grid.appendChild(
        header
      );

    }
  );


  /*
    10分刻み
  */

  for(
    let minute =
      START_TIME;

    minute <
      END_TIME;

    minute += 10
  ){

    const timeCell =
      document.createElement(
        "div"
      );


    timeCell.className =
      "cell time-cell";


    timeCell.textContent =
      minutesToTime(
        minute
      );


    grid.appendChild(
      timeCell
    );


    settings.girls.forEach(
      function(girl){

        const cell =
          document.createElement(
            "div"
          );


        cell.className =
          "cell";


        const reservation =
          reservations.find(
            function(r){

              if(
                String(r.girl) !==
                String(girl)
              ){

                return false;

              }


              const start =
                timeToMinutes(
                  r.time
                );


              const end =
                start +
                Number(r.course);


              const displayStart =
                Math.floor(
                  start / 10
                ) * 10;


              return (
                minute >=
                displayStart
                &&
                minute < end
              );

            }
          );


        if(reservation){

          const button =
            document.createElement(
              "button"
            );


          button.className =
            "reservation";


          const end =
            timeToMinutes(
              reservation.time
            ) +
            Number(
              reservation.course
            );


          const course =
            getCourseByMinutes(
              reservation.course
            );


          button.innerHTML =

            "<div class='r-time'>" +

            reservation.time +

            "〜" +

            minutesToTime(end) +

            "</div>" +

            "<div class='r-info'>" +

            (
              reservation.customer ||
              "お客様"
            ) +

            " / " +

            reservation.course +

            "分 / " +

            (
              course
                ? Number(course.price).toLocaleString()
                : "0"
            ) +

            "円" +

            "</div>" +

            "<div class='r-ratio'>" +

            "店：女の子 " +

            getRatioText(course) +

            "</div>";


          button.onclick =
            function(){

              openEditReservation(
                reservation.id
              );

            };


          cell.appendChild(
            button
          );

        }else{

          const available =
            isGridTimeAvailable(
              girl,
              minutesToTime(
                minute
              )
            );


          if(available){

            const button =
              document.createElement(
                "button"
              );


            button.className =
              "empty-btn";


            button.textContent =
              "＋ 予約";


            button.onclick =
              function(){

                openNewReservation(
                  minutesToTime(
                    minute
                  ),
                  girl
                );

              };


            cell.appendChild(
              button
            );

          }else{

            const button =
              document.createElement(
                "button"
              );


            button.className =
              "unavailable-btn";


            button.textContent =
              "予約不可";


            button.disabled =
              true;


            cell.appendChild(
              button
            );

          }

        }


        grid.appendChild(
          cell
        );

      }
    );

  }

}


/* =====================================================
   予約一覧
===================================================== */

function renderReservationList(){

  const list =
    document.getElementById(
      "reservationList"
    );


  list.innerHTML = "";


  if(
    reservations.length === 0
  ){

    list.innerHTML =
      "<div class='list-item'>" +
      "予約はありません。" +
      "</div>";

    return;

  }


  reservations.forEach(
    function(r){

      const item =
        document.createElement(
          "div"
        );


      item.className =
        "list-item";


      const end =
        timeToMinutes(
          r.time
        ) +
        Number(r.course);


      const course =
        getCourseByMinutes(
          r.course
        );


      item.innerHTML =

        "<strong>" +

        r.time +

        "〜" +

        minutesToTime(end) +

        "</strong><br>" +

        r.girl +

        "<br>" +

        (
          r.customer ||
          "お客様"
        ) +

        " / " +

        r.course +

        "分 / " +

        (
          course
            ? Number(course.price).toLocaleString()
            : "0"
        ) +

        "円" +

        "<div class='list-ratio'>" +

        "店：女の子　" +

        getRatioText(course) +

        "</div>";


      item.onclick =
        function(){

          openEditReservation(
            r.id
          );

        };


      list.appendChild(
        item
      );

    }
  );

}


/* =====================================================
   全体更新
===================================================== */

function renderAll(){

  renderSchedule();

  renderReservationList();

}


/* =====================================================
   設定
===================================================== */

function openSettings(){

  renderSettings();


  document.getElementById(
    "settingsModal"
  ).classList.add("show");

}


function closeSettings(){

  document.getElementById(
    "settingsModal"
  ).classList.remove("show");

}


/* =====================================================
   HTMLエスケープ
===================================================== */

function escapeHtml(value){

  return String(value)

    .replace(
      /&/g,
      "&amp;"
    )

    .replace(
      /</g,
      "&lt;"
    )

    .replace(
      />/g,
      "&gt;"
    )

    .replace(
      /"/g,
      "&quot;"
    )

    .replace(
      /'/g,
      "&#039;"
    );

}


/* =====================================================
   設定画面表示
===================================================== */

function renderSettings(){

  const girls =
    document.getElementById(
      "girlsSettings"
    );


  girls.innerHTML = "";


  settings.girls.forEach(
    function(girl,index){

      const row =
        document.createElement(
          "div"
        );


      row.className =
        "setting-girl";


      row.innerHTML =

        "<input " +

        "type='text' " +

        "value='" +

        escapeHtml(girl) +

        "' " +

        "data-girl='" +

        index +

        "'>" +

        "<button " +

        "class='small-btn' " +

        "onclick='removeGirl(" +

        index +

        ")'>" +

        "削除" +

        "</button>";


      girls.appendChild(
        row
      );

    }
  );


  const courses =
    document.getElementById(
      "coursesSettings"
    );


  courses.innerHTML = "";


  settings.courses.forEach(
    function(course,index){

      const row =
        document.createElement(
          "div"
        );


      row.className =
        "course-row";


      row.innerHTML =

        "<input " +

        "type='text' " +

        "value='" +

        escapeHtml(
          course.name
        ) +

        "' " +

        "data-course-name='" +

        index +

        "'>" +

        "<input " +

        "type='number' " +

        "min='1' " +

        "value='" +

        course.minutes +

        "' " +

        "data-course-minutes='" +

        index +

        "'>" +

        "<input " +

        "type='number' " +

        "min='0' " +

        "value='" +

        course.price +

        "' " +

        "data-course-price='" +

        index +

        "'>" +

        "<input " +

        "type='number' " +

        "min='0' " +

        "max='100' " +

        "value='" +

        course.shopPercent +

        "' " +

        "data-course-shop='" +

        index +

        "'>" +

        "<input " +

        "type='number' " +

        "min='0' " +

        "max='100' " +

        "value='" +

        course.girlPercent +

        "' " +

        "data-course-girl='" +

        index +

        "'>" +

        "<button " +

        "class='course-delete' " +

        "onclick='removeCourse(" +

        index +

        ")'>" +

        "削除" +

        "</button>";


      courses.appendChild(
        row
      );

    }
  );

}


/* =====================================================
   女の子追加
===================================================== */

function addGirl(){

  settings.girls.push(
    "新しい女の子"
  );


  renderSettings();

}


/* =====================================================
   女の子削除
===================================================== */

function removeGirl(index){

  if(
    settings.girls.length <= 1
  ){

    alert(
      "女の子は1人以上必要です。"
    );

    return;

  }


  settings.girls.splice(
    index,
    1
  );


  renderSettings();

}


/* =====================================================
   コース追加
===================================================== */

function addCourse(){

  settings.courses.push({

    name:"新コース",

    minutes:60,

    price:0,

    shopPercent:50,

    girlPercent:50

  });


  renderSettings();

}


/* =====================================================
   コース削除
===================================================== */

function removeCourse(index){

  if(
    settings.courses.length <= 1
  ){

    alert(
      "コースは1つ以上必要です。"
    );

    return;

  }


  settings.courses.splice(
    index,
    1
  );


  renderSettings();

}


/* =====================================================
   設定保存
===================================================== */

function saveSettings(){

  /*
    女の子
  */

  const girlInputs =
    document.querySelectorAll(
      "[data-girl]"
    );


  const newGirls = [];


  girlInputs.forEach(
    function(input){

      const value =
        input.value.trim();


      if(value){

        newGirls.push(
          value
        );

      }

    }
  );


  if(
    newGirls.length === 0
  ){

    alert(
      "女の子を1人以上設定してください。"
    );

    return;

  }


  settings.girls =
    newGirls;


  /*
    コース
  */

  const names =
    document.querySelectorAll(
      "[data-course-name]"
    );


  const minutes =
    document.querySelectorAll(
      "[data-course-minutes]"
    );


  const prices =
    document.querySelectorAll(
      "[data-course-price]"
    );


  const shops =
    document.querySelectorAll(
      "[data-course-shop]"
    );


  const girls =
    document.querySelectorAll(
      "[data-course-girl]"
    );


  const newCourses = [];


  for(
    let i=0;
    i<names.length;
    i++
  ){

    const name =
      names[i]
        .value
        .trim();


    const mins =
      Number(
        minutes[i].value
      );


    const price =
      Number(
        prices[i].value
      );


    const shopPercent =
      Number(
        shops[i].value
      );


    const girlPercent =
      Number(
        girls[i].value
      );


    if(
      !name ||
      mins <= 0
    ){

      continue;

    }


    /*
      割合チェック
    */

    if(
      shopPercent < 0 ||
      shopPercent > 100 ||
      girlPercent < 0 ||
      girlPercent > 100
    ){

      alert(
        name +
        " の割合は0〜100％で入力してください。"
      );

      return;

    }


    if(
      shopPercent +
      girlPercent !==
      100
    ){

      alert(
        name +
        " の割合が100％になっていません。\n\n" +
        "店：" +
        shopPercent +
        "%\n" +
        "女の子：" +
        girlPercent +
        "%\n\n" +
        "合計100％にしてください。"
      );

      return;

    }


    newCourses.push({

      name:
        name,

      minutes:
        mins,

      price:
        price >= 0
          ? price
          : 0,

      shopPercent:
        shopPercent,

      girlPercent:
        girlPercent

    });

  }


  if(
    newCourses.length === 0
  ){

    alert(
      "コースを1つ以上設定してください。"
    );

    return;

  }


  settings.courses =
    newCourses;


  /*
    保存
  */

  localStorage.setItem(
    "reservationSettings",
    JSON.stringify(
      settings
    )
  );


  closeSettings();

  renderAll();

}


/* =====================================================
   集計
===================================================== */

function openSummary(){

  const content =
    document.getElementById(
      "summaryContent"
    );


  content.innerHTML = "";


  let totalSales = 0;

  let totalShop = 0;

  let totalGirl = 0;


  /*
    女の子別集計
  */

  settings.girls.forEach(
    function(girl){

      const list =
        reservations.filter(
          function(r){

            return String(r.girl) ===
              String(girl);

          }
        );


      let sales = 0;

      let shop = 0;

      let girlMoney = 0;


      list.forEach(
        function(r){

          const course =
            getCourseByMinutes(
              r.course
            );


          if(course){

            const price =
              Number(
                course.price
              ) || 0;


            sales +=
              price;


            shop +=
              getShopAmount(
                course
              );


            girlMoney +=
              getGirlAmount(
                course
              );

          }

        }
      );


      totalSales +=
        sales;


      totalShop +=
        shop;


      totalGirl +=
        girlMoney;


      const box =
        document.createElement(
          "div"
        );


      box.className =
        "summary-box";


      box.innerHTML =

        "<strong>" +

        escapeHtml(girl) +

        "</strong><br>" +

        "予約数：" +

        list.length +

        "件<br>" +

        "売上：" +

        sales.toLocaleString() +

        "円" +

        "<div class='summary-money'>" +

        "店取り分：" +

        shop.toLocaleString() +

        "円" +

        "</div>" +

        "<div class='summary-money'>" +

        "女の子取り分：" +

        girlMoney.toLocaleString() +

        "円" +

        "</div>";


      content.appendChild(
        box
      );

    }
  );


  /*
    コース別集計
  */

  const courseTitle =
    document.createElement(
      "h3"
    );


  courseTitle.textContent =
    "コース別";


  content.appendChild(
    courseTitle
  );


  settings.courses.forEach(
    function(course){

      const list =
        reservations.filter(
          function(r){

            return Number(
              r.course
            ) === Number(
              course.minutes
            );

          }
        );


      if(
        list.length === 0
      ){

        return;

      }


      const sales =
        list.length *
        Number(
          course.price
        );


      const shop =
        list.reduce(
          function(sum){

            return (
              sum +
              getShopAmount(
                course
              )
            );

          },
          0
        );


      const girlMoney =
        list.reduce(
          function(sum){

            return (
              sum +
              getGirlAmount(
                course
              )
            );

          },
          0
        );


      const box =
        document.createElement(
          "div"
        );


      box.className =
        "summary-box";


      box.innerHTML =

        "<strong>" +

        escapeHtml(
          course.name
        ) +

        "</strong><br>" +

        "予約数：" +

        list.length +

        "件<br>" +

        "料金：" +

        Number(
          course.price
        ).toLocaleString() +

        "円<br>" +

        "<div class='summary-ratio'>" +

        "店：女の子　" +

        getRatioText(course) +

        "</div>" +

        "<div class='summary-money'>" +

        "売上：" +

        sales.toLocaleString() +

        "円" +

        "</div>" +

        "<div class='summary-money'>" +

        "店取り分：" +

        shop.toLocaleString() +

        "円" +

        "</div>" +

        "<div class='summary-money'>" +

        "女の子取り分：" +

        girlMoney.toLocaleString() +

        "円" +

        "</div>";


      content.appendChild(
        box
      );

    }
  );


  /*
    合計
  */

  const total =
    document.createElement(
      "div"
    );


  total.className =
    "summary-box";


  total.innerHTML =

    "<strong>合計</strong><br>" +

    "予約数：" +

    reservations.length +

    "件<br>" +

    "売上：" +

    totalSales.toLocaleString() +

    "円" +

    "<div class='summary-money'>" +

    "店取り分：" +

    totalShop.toLocaleString() +

    "円" +

    "</div>" +

    "<div class='summary-money'>" +

    "女の子取り分：" +

    totalGirl.toLocaleString() +

    "円" +

    "</div>";


  content.appendChild(
    total
  );


  document.getElementById(
    "summaryModal"
  ).classList.add("show");

}


/* =====================================================
   集計閉じる
===================================================== */

function closeSummary(){

  document.getElementById(
    "summaryModal"
  ).classList.remove("show");

}


/* =====================================================
   モーダル外側タップ
===================================================== */

document.getElementById(
  "reservationModal"
).addEventListener(
  "click",
  function(e){

    if(
      e.target === this
    ){

      closeReservationModal();

    }

  }
);


document.getElementById(
  "settingsModal"
).addEventListener(
  "click",
  function(e){

    if(
      e.target === this
    ){

      closeSettings();

    }

  }
);


document.getElementById(
  "summaryModal"
).addEventListener(
  "click",
  function(e){

    if(
      e.target === this
    ){

      closeSummary();

    }

  }
);

</script>

</body>
</html>
