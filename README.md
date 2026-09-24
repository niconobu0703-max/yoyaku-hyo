<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

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

/* =========================
   ヘッダー
========================= */

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

/* =========================
   予約表
========================= */

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

.empty-btn:active{
  background:#eee;
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

/* =========================
   下部
========================= */

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

.empty-message{
  background:#fff;
  border:1px solid #ddd;
  border-radius:10px;
  padding:15px;
  text-align:center;
  color:#777;
}

/* =========================
   モーダル
========================= */

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
  max-width:600px;
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

.course-row{
  display:grid;
  grid-template-columns:1fr 1fr 1fr auto;
  gap:6px;
  margin-bottom:7px;
}

.course-row input{
  width:100%;
  padding:8px;
  border:1px solid #ccc;
  border-radius:7px;
}

.remove-course-btn{
  border:0;
  border-radius:7px;
  background:#eee;
  padding:0 10px;
}

.summary-box{
  background:#f7f7f7;
  border-radius:10px;
  padding:12px;
  margin-bottom:8px;
}

</style>
</head>

<body>


<!-- =========================
     ヘッダー
========================= -->

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


<!-- =========================
     予約表
========================= -->

<div class="schedule">

  <div
    id="scheduleGrid"
    class="grid"
  ></div>

</div>


<!-- =========================
     新しい予約
========================= -->

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

      同じ女の子は、前の予約終了後に
      <strong>5分間の休憩</strong>
      が必要です。

      <br><br>

      例：

      <strong>10:00〜10:40</strong>

      の40分コースなら、

      <strong>10:40〜10:45</strong>
      が休憩。

      <br>

      表は10分刻みなので、

      <strong>10:40 → 予約不可</strong>

      <br>

      <strong>10:50 → 予約可能</strong>

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
      ></select>

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

    <div
      id="coursesSettings"
    ></div>


    <button
      class="small-btn"
      onclick="addCourse()"
    >
      ＋ コースを追加
    </button>


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


/*
   予約終了後の休憩時間

   10:00〜10:40
   ↓
   10:40〜10:45 休憩
   ↓
   10:45から予約可能

   表は10分刻みなので

   10:40 → 予約不可
   10:50 → 予約可能
*/

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
      price:10000
    },
    {
      name:"60分",
      minutes:60,
      price:14000
    },
    {
      name:"90分",
      minutes:90,
      price:20000
    },
    {
      name:"100分",
      minutes:100,
      price:22000
    },
    {
      name:"120分",
      minutes:120,
      price:26000
    }
  ]

};


/* =====================================================
   設定読み込み
===================================================== */

let settings;

try{

  settings =
    JSON.parse(
      localStorage.getItem("reservationSettings")
    ) || DEFAULT_SETTINGS;

}catch(e){

  settings = DEFAULT_SETTINGS;

}


/* =====================================================
   設定データ安全確認
===================================================== */

if(
  !Array.isArray(settings.girls) ||
  settings.girls.length === 0
){

  settings.girls =
    DEFAULT_SETTINGS.girls.slice();

}


if(
  !Array.isArray(settings.courses) ||
  settings.courses.length === 0
){

  settings.courses =
    DEFAULT_SETTINGS.courses.map(
      function(course){

        return {

          name:course.name,
          minutes:course.minutes,
          price:course.price

        };

      }
    );

}


/* =====================================================
   変数
===================================================== */

let reservations = [];

let editingId = null;


/* =====================================================
   初期化
===================================================== */

document.addEventListener(
  "DOMContentLoaded",
  function(){

    const now = new Date();

    const yyyy =
      now.getFullYear();

    const mm =
      String(
        now.getMonth() + 1
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

  return "reservations_" + getDate();

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

      if(
        !Array.isArray(reservations)
      ){

        reservations = [];

      }

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
    JSON.stringify(reservations)
  );

}


function changeDate(amount){

  const input =
    document.getElementById(
      "dateInput"
    );

  if(!input.value){

    return;
