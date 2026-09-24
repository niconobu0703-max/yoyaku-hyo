<!doctype html>
<html lang="ja">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>よやく表</title>

<style>
*{
  box-sizing:border-box;
}

html,
body{
  margin:0;
  padding:0;
  background:#f4f5f7;
  color:#222;
  font-family:
    -apple-system,
    BlinkMacSystemFont,
    "Noto Sans JP",
    "Hiragino Kaku Gothic ProN",
    Meiryo,
    sans-serif;
}

button,
input,
select{
  font:inherit;
}

button{
  cursor:pointer;
}

.header{
  position:sticky;
  top:0;
  z-index:100;
  background:#fff;
  border-bottom:1px solid #ddd;
  padding:10px;
}

.header-row{
  display:flex;
  align-items:center;
  gap:8px;
  flex-wrap:wrap;
}

.title{
  font-size:20px;
  font-weight:800;
  margin-right:auto;
}

.date-input{
  font-weight:700;
  padding:8px;
  border:1px solid #ccc;
  border-radius:9px;
}

.btn{
  border:1px solid #ccc;
  background:#fff;
  color:#222;
  border-radius:9px;
  padding:8px 12px;
}

.btn:hover{
  opacity:.85;
}

.btn-primary{
  background:#222;
  color:#fff;
  border-color:#222;
}

.btn-danger{
  color:#b00020;
  background:#fff3f3;
  border-color:#e2a0a0;
}

.wrap{
  max-width:1200px;
  margin:0 auto;
  padding:10px;
}

.notice{
  background:#fff;
  border:1px solid #ddd;
  border-radius:12px;
  padding:10px;
  margin-bottom:10px;
  line-height:1.7;
}

.table-wrap{
  overflow:auto;
  background:#fff;
  border:1px solid #ddd;
  border-radius:12px;
}

.schedule{
  min-width:700px;
}

.row{
  display:grid;
  grid-template-columns:
    70px
    repeat(2,minmax(280px,1fr));
  min-height:58px;
  border-bottom:1px solid #eee;
}

.row:last-child{
  border-bottom:0;
}

.cell{
  position:relative;
  padding:6px;
  border-right:1px solid #eee;
}

.cell:last-child{
  border-right:0;
}

.head{
  position:sticky;
  top:59px;
  z-index:50;
  background:#fff;
  text-align:center;
  font-weight:800;
  padding:12px 5px;
}

.time-cell{
  display:flex;
  justify-content:center;
  align-items:flex-start;
  padding-top:12px;
  background:#fafafa;
  font-weight:700;
}

.slot-button{
  width:100%;
  min-height:44px;
  border:1px dashed #ccc;
  background:#fff;
  border-radius:8px;
  padding:6px;
  text-align:left;
}

.slot-button:hover{
  background:#fafafa;
}

.reservation-card{
  border:1px solid #999;
  background:#fff;
  border-radius:8px;
  padding:7px;
}

.reservation-time{
  font-weight:800;
}

.reservation-info{
  color:#555;
  font-size:13px;
  margin-top:3px;
}

.busy-button{
  border-style:solid;
}

.list{
  margin-top:15px;
}

.list-title{
  font-size:18px;
  margin:10px 0;
}

.list-item{
  background:#fff;
  border:1px solid #ddd;
  border-radius:11px;
  padding:10px;
  margin-bottom:8px;
}

.list-main{
  font-weight:800;
}

.list-sub{
  font-size:13px;
  color:#666;
  margin-top:4px;
}

.list-buttons{
  display:flex;
  gap:7px;
  margin-top:8px;
}

.empty{
  text-align:center;
  color:#777;
  padding:20px;
  background:#fff;
  border-radius:10px;
}

.modal{
  position:fixed;
  inset:0;
  z-index:500;
  display:none;
  align-items:center;
  justify-content:center;
  padding:15px;
  background:rgba(0,0,0,.65);
}

.modal.open{
  display:flex;
}

.modal-box{
  width:min(550px,100%);
  max-height:92vh;
  overflow:auto;
  background:#fff;
  border-radius:16px;
  padding:18px;
}

.modal-title{
  margin-top:0;
}

.field{
  margin:13px 0;
}

.field label{
  display:block;
  font-weight:700;
  margin-bottom:5px;
}

.field input,
.field select{
  width:100%;
  padding:10px;
  border:1px solid #ccc;
  border-radius:9px;
  background:#fff;
}

.two-column{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:10px;
}

.end-time{
  background:#f7f7f7;
  border-radius:10px;
  padding:10px;
  margin:10px 0;
}

.error{
  color:#b00020;
  font-weight:700;
  min-height:24px;
  line-height:1.5;
}

.modal-buttons{
  display:flex;
  justify-content:flex-end;
  flex-wrap:wrap;
  gap:8px;
  margin-top:15px;
}

.course-setting{
  display:grid;
  grid-template-columns:70px 1fr 1fr;
  gap:8px;
  align-items:center;
  margin-bottom:8px;
}

.course-setting input{
  width:100%;
  padding:8px;
  border:1px solid #ccc;
  border-radius:8px;
}

.summary-box{
  background:#f7f7f7;
  border-radius:10px;
  padding:12px;
  margin:8px 0;
  line-height:1.8;
}

@media(max-width:700px){

  .row{
    grid-template-columns:
      58px
      repeat(2,minmax(230px,1fr));
  }

  .head{
    top:105px;
  }

  .two-column{
    grid-template-columns:1fr;
  }

}
</style>
</head>

<body>

<header class="header">

  <div class="header-row">

    <div class="title">
      📅 よやく表
    </div>

    <input
      id="dateInput"
      class="date-input"
      type="date"
    >

    <button
      id="todayButton"
      class="btn"
      type="button"
    >
      今日
    </button>

    <button
      id="settingsButton"
      class="btn"
      type="button"
    >
      ⚙️ 設定
    </button>

    <button
      id="summaryButton"
      class="btn btn-primary"
      type="button"
    >
      📊 集計
    </button>

  </div>

</header>


<main class="wrap">

  <div class="notice">

    <b>予約ルール</b>

    <br>

    ・予約時間は1分単位で入力できます。

    <br>

    ・表示は10分刻みです。

    <br>

    ・同じ女の子は予約と予約の間を5分空けます。

    <br>

    ・別の女の子なら同じ時間でも予約できます。

    <br>

    ・24:00を超える予約はできません。

  </div>


  <div class="table-wrap">

    <div
      id="schedule"
      class="schedule"
    ></div>

  </div>


  <section class="list">

    <h2 class="list-title">
      📋 この日の予約一覧
    </h2>

    <div id="reservationList"></div>

  </section>

</main>


<!-- =========================================================
     予約モーダル
     ========================================================= -->

<div
  id="reservationModal"
  class="modal"
>

  <div class="modal-box">

    <h2
      id="reservationModalTitle"
      class="modal-title"
    >
      新しい予約
    </h2>


    <div class="field">

      <label>
        女の子
      </label>

      <select
        id="girlSelect"
      ></select>

    </div>


    <div class="two-column">

      <div class="field">

        <label>
          開始時間
        </label>

        <input
          id="timeInput"
          type="time"
          step="60"
          min="10:00"
          max="23:59"
        >

      </div>


      <div class="field">

        <label>
          コース
        </label>

        <select
          id="courseSelect"
        ></select>

      </div>

    </div>


    <div
      id="endTimeText"
      class="end-time"
    ></div>


    <div class="field">

      <label>
        お客さん名（任意）
      </label>

      <input
        id="customerInput"
        type="text"
        placeholder="例：山田さん"
      >

    </div>


    <div
      id="reservationError"
      class="error"
    ></div>


    <div class="modal-buttons">

      <button
        id="cancelReservationButton"
        class="btn"
        type="button"
      >
        キャンセル
      </button>


      <button
        id="deleteReservationButton"
        class="btn btn-danger"
        type="button"
      >
        🗑️ 削除
      </button>


      <button
        id="saveReservationButton"
        class="btn btn-primary"
        type="button"
      >
        保存
      </button>

    </div>

  </div>

</div>


<!-- =========================================================
     設定モーダル
     ========================================================= -->

<div
  id="settingsModal"
  class="modal"
>

  <div class="modal-box">

    <h2 class="modal-title">
      ⚙️ 設定
    </h2>


    <div class="field">

      <label>
        女の子
      </label>

      <input
        id="girlsInput"
        type="text"
        placeholder="ことねさん,ナヨンさん"
      >

    </div>


    <h3>
      コース設定
    </h3>


    <div id="courseSettings"></div>


    <div class="modal-buttons">

      <button
        id="cancelSettingsButton"
        class="btn"
        type="button"
      >
        キャンセル
      </button>


      <button
        id="saveSettingsButton"
        class="btn btn-primary"
        type="button"
      >
        設定を保存
      </button>

    </div>

  </div>

</div>


<!-- =========================================================
     集計モーダル
     ========================================================= -->

<div
  id="summaryModal"
  class="modal"
>

  <div class="modal-box">

    <h2 class="modal-title">
      📊 日別集計
    </h2>


    <div
      id="summaryContent"
    ></div>


    <div class="modal-buttons">

      <button
        id="closeSummaryButton"
        class="btn"
        type="button"
      >
        閉じる
      </button>

    </div>

  </div>

</div>


<script>
"use strict";


/* =========================================================
   基本設定
   ========================================================= */

const GRID_START_MINUTES = 10 * 60;

const GRID_END_MINUTES = 24 * 60;

const REQUIRED_GAP_MINUTES = 5;

const COURSE_LIST = [
  40,
  60,
  90,
  100,
  120
];


const DEFAULT_SETTINGS = {

  girls:[
    "ことねさん",
    "ナヨンさん"
  ],

  courses:{

    40:{
      price:0,
      share:0
    },

    60:{
      price:0,
      share:0
    },

    90:{
      price:0,
      share:0
    },

    100:{
      price:0,
      share:0
    },

    120:{
      price:0,
      share:0
    }

  }

};


/* =========================================================
   グローバル変数
   ========================================================= */

let settings = loadSettings();

let reservations = [];

let editingReservationId = null;


/* =========================================================
   日付
   ========================================================= */

function getTodayString(){

  const now = new Date();

  const year =
    now.getFullYear();

  const month =
    String(now.getMonth() + 1)
      .padStart(2,"0");

  const day =
    String(now.getDate())
      .padStart(2,"0");

  return (
    year +
    "-" +
    month +
    "-" +
    day
  );

}


function getSelectedDate(){

  const input =
    document.getElementById(
      "dateInput"
    );

  return input.value || getTodayString();

}


/* =========================================================
   localStorage キー
   ========================================================= */

function getReservationStorageKey(){

  return (
    "yoyaku_reservations_" +
    getSelectedDate()
  );

}


function getSettingsStorageKey(){

  return "yoyaku_settings";

}


/* =========================================================
   設定読み込み
   ========================================================= */

function loadSettings(){

  try{

    const raw =
      localStorage.getItem(
        getSettingsStorageKey()
      );

    if(!raw){

      return JSON.parse(
        JSON.stringify(
          DEFAULT_SETTINGS
        )
      );

    }


    const saved =
      JSON.parse(raw);


    const result = {
      girls:[],
      courses:{}
    };


    if(
      Array.isArray(saved.girls) &&
      saved.girls.length > 0
    ){

      result.girls =
        saved.girls
          .map(x => String(x).trim())
          .filter(Boolean);

    }


    if(result.girls.length === 0){

      result.girls =
        DEFAULT_SETTINGS.girls.slice();

    }


    for(
      const minutes of COURSE_LIST
    ){

      const old =
        saved.courses &&
        saved.courses[minutes]
          ? saved.courses[minutes]
          : {};


      result.courses[minutes] = {

        price:
          Number(old.price) || 0,

        share:
          Number(old.share) || 0

      };

    }


    return result;

  }catch(error){

    return JSON.parse(
      JSON.stringify(
        DEFAULT_SETTINGS
      )
    );

  }

}


/* =========================================================
   設定保存
   ========================================================= */

function saveSettings(){

  localStorage.setItem(

    getSettingsStorageKey(),

    JSON.stringify(settings)

  );

}


/* =========================================================
   予約読み込み
   ========================================================= */

function loadReservations(){

  try{

    const raw =
      localStorage.getItem(
        getReservationStorageKey()
      );


    if(!raw){

      reservations = [];

      return;

    }


    const data =
      JSON.parse(raw);


    if(Array.isArray(data)){

      reservations = data;

    }else{

      reservations = [];

    }

  }catch(error){

    reservations = [];

  }

}


/* =========================================================
   予約保存
   ========================================================= */

function saveReservations(){

  localStorage.setItem(

    getReservationStorageKey(),

    JSON.stringify(reservations)

  );

}


/* =========================================================
   時間 → 分
   ========================================================= */

function timeToMinutes(time){

  if(
    typeof time !== "string"
  ){

    return NaN;

  }


  const parts =
    time.split(":");


  if(parts.length !== 2){

    return NaN;

  }


  const hour =
    Number(parts[0]);

  const minute =
    Number(parts[1]);


  if(
    !Number.isFinite(hour) ||
    !Number.isFinite(minute)
  ){

    return NaN;

  }


  return (
    hour * 60 +
    minute
  );

}


/* =========================================================
   分 → 時間
   ========================================================= */

function minutesToTime(minutes){

  const hour =
    Math.floor(
      minutes / 60
    );

  const minute =
    minutes % 60;


  return (
    String(hour).padStart(2,"0") +
    ":" +
    String(minute).padStart(2,"0")
  );

}


/* =========================================================
   終了時間
   ========================================================= */

function getEndMinutes(
  startTime,
  course
){

  return (
    timeToMinutes(startTime) +
    Number(course)
  );

}


function getEndTime(
  startTime,
  course
){

  return minutesToTime(
    getEndMinutes(
      startTime,
      course
    )
  );

}


/* =========================================================
   HTMLエスケープ
   ========================================================= */

function escapeHTML(value){

  return String(
    value ?? ""
  )

  .replaceAll(
    "&",
    "&amp;"
  )

  .replaceAll(
    "<",
    "&lt;"
  )

  .replaceAll(
    ">",
    "&gt;"
  )

  .replaceAll(
    '"',
    "&quot;"
  )

  .replaceAll(
    "'",
    "&#039;"
  );

}


/* =========================================================
   ID生成
   ========================================================= */

function createReservationId(){

  return (
    Date.now() +
    "_" +
    Math.random()
      .toString(36)
      .slice(2)
  );

}


/* =========================================================
   重要
   同じ女の子の5分間隔チェック
   ========================================================= */

function isOverlapping(
  girl,
  time,
  course,
  ignoreId
){

  const newStart =
    timeToMinutes(time);

  const newEnd =
    newStart +
    Number(course);


  return reservations.some(
    reservation => {

      /*
       * 違う女の子ならOK
       */
      if(
        reservation.girl !== girl
      ){

        return false;

      }


      /*
       * 編集中の自分自身は除外
       */
      if(
        ignoreId !== null &&
        ignoreId !== undefined &&
        String(reservation.id) ===
          String(ignoreId)
      ){

        return false;

      }


      const existingStart =
        timeToMinutes(
          reservation.time
        );


      const existingEnd =
        existingStart +
        Number(
          reservation.course
        );


      /*
       * ここが5分ルール。
       *
       * 既存：
       * 10:00〜11:00
       *
       * 新規：
       * 11:00〜12:00
       *
       * 11:00 < 11:05
       * なのでNG。
       *
       * 11:05〜12:05なら
       *
       * 11:05 < 11:05
       * がfalseになるためOK。
       */

      return (

        newStart <
        existingEnd +
        REQUIRED_GAP_MINUTES

        &&

        newEnd +
        REQUIRED_GAP_MINUTES >
        existingStart

      );

    }
  );

}


/* =========================================================
   女の子セレクト
   ========================================================= */

function renderGirlSelect(
  selectedGirl
){

  const select =
    document.getElementById(
      "girlSelect"
    );


  select.innerHTML =
    settings.girls
      .map(girl => {

        const selected =
          girl === selectedGirl
            ? "selected"
            : "";


        return `
          <option
            value="${escapeHTML(girl)}"
            ${selected}
          >
            ${escapeHTML(girl)}
          </option>
        `;

      })
      .join("");

}


/* =========================================================
   コースセレクト
   ========================================================= */

function renderCourseSelect(
  selectedCourse
){

  const select =
    document.getElementById(
      "courseSelect"
    );


  select.innerHTML =
    COURSE_LIST
      .map(minutes => {

        const config =
          settings.courses[minutes];


        const price =
          Number(
            config.price
          ) || 0;


        const selected =
          Number(selectedCourse) ===
          minutes
            ? "selected"
            : "";


        return `
          <option
            value="${minutes}"
            ${selected}
          >
            ${minutes}分
            ${
              price > 0
                ? `（${price.toLocaleString()}円）`
                : ""
            }
          </option>
        `;

      })
      .join("");

}


/* =========================================================
   新規予約
   ========================================================= */

function openNewReservation(
  defaultTime
){

  editingReservationId =
    null;


  document.getElementById(
    "reservationModalTitle"
  ).textContent =
    "新しい予約";


  document.getElementById(
    "deleteReservationButton"
  ).style.display =
    "none";


  document.getElementById(
    "reservationError"
  ).textContent =
    "";


  renderGirlSelect(
    settings.girls[0]
  );


  renderCourseSelect(
    60
  );


  document.getElementById(
    "timeInput"
  ).value =
    defaultTime ||
    "10:00";


  document.getElementById(
    "customerInput"
  ).value =
    "";


  updateEndTimeText();


  document.getElementById(
    "reservationModal"
  ).classList.add("open");

}


/* =========================================================
   予約編集
   ========================================================= */

function openEditReservation(
  reservationId
){

  const reservation =
    reservations.find(
      item =>
        String(item.id) ===
        String(reservationId)
    );


  if(!reservation){

    alert(
      "予約が見つかりません。"
    );

    return;

  }


  editingReservationId =
    reservation.id;


  document.getElementById(
    "reservationModalTitle"
  ).textContent =
    "予約を編集";


  document.getElementById(
    "deleteReservationButton"
  ).style.display =
    "inline-block";


  document.getElementById(
    "reservationError"
  ).textContent =
    "";


  renderGirlSelect(
    reservation.girl
  );


  renderCourseSelect(
    reservation.course
  );


  document.getElementById(
    "timeInput"
  ).value =
    reservation.time;


  document.getElementById(
    "customerInput"
  ).value =
    reservation.customer ||
    "";


  updateEndTimeText();


  document.getElementById(
    "reservationModal"
  ).classList.add("open");

}


/* =========================================================
   予約モーダル閉じる
   ========================================================= */

function closeReservationModal(){

  editingReservationId =
    null;


  document.getElementById(
    "reservationModal"
  ).classList.remove("open");


  document.getElementById(
    "reservationError"
  ).textContent =
    "";

}


/* =========================================================
   終了時間表示
   ========================================================= */

function updateEndTimeText(){

  const time =
    document.getElementById(
      "timeInput"
    ).value;


  const course =
    Number(
      document.getElementById(
        "courseSelect"
      ).value
    );


  if(
    !time ||
    !course
  ){

    document.getElementById(
      "endTimeText"
    ).textContent =
      "";

    return;

  }


  const start =
    timeToMinutes(time);


  const end =
    start + course;


  if(
    end > GRID_END_MINUTES
  ){

    document.getElementById(
      "endTimeText"
    ).innerHTML =
      `
        <b style="color:#b00020;">
          ⚠️ 24:00を超えています
        </b>
      `;

    return;

  }


  document.getElementById(
    "endTimeText"
  ).textContent =
    `${time} ～ ${minutesToTime(end)}（${course}分）`;

}


/* =========================================================
   予約保存
   ========================================================= */

function saveReservation(){

  const girl =
    document.getElementById(
      "girlSelect"
    ).value;


  const time =
    document.getElementById(
      "timeInput"
    ).value;


  const course =
    Number(
      document.getElementById(
        "courseSelect"
      ).value
    );


  const customer =
    document.getElementById(
      "customerInput"
    ).value
      .trim();


  const error =
    document.getElementById(
      "reservationError"
    );


  error.textContent =
    "";


  if(!girl){

    error.textContent =
      "女の子を選択してください。";

    return;

  }


  if(!time){

    error.textContent =
      "開始時間を入力してください。";

    return;

  }


  const start =
    timeToMinutes(time);


  if(!Number.isFinite(start)){

    error.textContent =
      "時間が正しくありません。";

    return;

  }


  if(
    start < GRID_START_MINUTES
  ){

    error.textContent =
      "10:00より前は予約できません。";

    return;

  }


  if(
    start >= GRID_END_MINUTES
  ){

    error.textContent =
      "24:00は開始時間にできません。";

    return;

  }


  if(
    !COURSE_LIST.includes(course)
  ){

    error.textContent =
      "コースが正しくありません。";

    return;

  }


  const end =
    start + course;


  if(
    end > GRID_END_MINUTES
  ){

    error.textContent =
      `24:00を超えるため予約できません。終了予定：${minutesToTime(end)}`;

    return;

  }


  /*
   * 同じ女の子の5分ルール
   */
  if(
    isOverlapping(
      girl,
      time,
      course,
      editingReservationId
    )
  ){

    error.textContent =
      "同じ女の子の予約とは5分以上空けてください。";

    return;

  }


  /*
   * 新規
   */
  if(
    editingReservationId === null
  ){

    reservations.push({

      id:
        createReservationId(),

      girl:
        girl,

      time:
        time,

      course:
        course,

      customer:
        customer

    });

  }

  /*
   * 編集
   */
  else{

    const target =
      reservations.find(
        item =>
          String(item.id) ===
          String(editingReservationId)
      );


    if(!target){

      error.textContent =
        "編集対象の予約が見つかりません。";

      return;

    }


    target.girl =
      girl;


    target.time =
      time;


    target.course =
      course;


    target.customer =
      customer;

  }


  /*
   * 時間順
   */
  reservations.sort(
    (a,b) =>
      timeToMinutes(a.time) -
      timeToMinutes(b.time)
  );


  saveReservations();


  closeReservationModal();


  render();


}


/* =========================================================
   予約削除
   ========================================================= */

function deleteReservation(){

  if(
    editingReservationId === null ||
    editingReservationId === undefined
  ){

    return;

  }


  const target =
    reservations.find(
      item =>
        String(item.id) ===
        String(editingReservationId)
    );


  if(!target){

    alert(
      "削除する予約が見つかりません。"
    );

    return;

  }


  const end =
    getEndTime(
      target.time,
      target.course
    );


  const answer =
    confirm(
      `${target.girl} ${target.time}～${end}の予約を削除しますか？`
    );


  if(!answer){

    return;

  }


  const id =
    String(
      editingReservationId
    );


  reservations =
    reservations.filter(
      item =>
        String(item.id) !==
        id
    );


  saveReservations();


  editingReservationId =
    null;


  document.getElementById(
    "reservationModal"
  ).classList.remove("open");


  render();


  alert(
    "予約を削除しました。"
  );

}


/* =========================================================
   スケジュール描画
   ========================================================= */

function renderSchedule(){

  const root =
    document.getElementById(
      "schedule"
    );


  let html = "";


  /*
   * ヘッダー
   */

  html += `
    <div class="row">

      <div class="cell head">
        時間
      </div>
  `;


  for(
    const girl of settings.girls
  ){

    html += `
      <div class="cell head">
        ${escapeHTML(girl)}
      </div>
    `;

  }


  html += `
    </div>
  `;


  /*
   * 10分刻み
   */

  for(
    let minute =
      GRID_START_MINUTES;

    minute <
      GRID_END_MINUTES;

    minute += 10
  ){

    html += `
      <div class="row">

        <div class="cell time-cell">
          ${minutesToTime(minute)}
        </div>
    `;


    for(
      const girl of settings.girls
    ){

      /*
       * この10分枠から始まる予約
       *
       * 10:05なら
       * 10:00の行に表示
       *
       * 10:17なら
       * 10:10の行に表示
       */

      const startingReservations =
        reservations.filter(
          reservation => {

            if(
              reservation.girl !==
              girl
            ){

              return false;

            }


            const start =
              timeToMinutes(
                reservation.time
              );


            return (
              start >= minute &&
              start < minute + 10
            );

          }
        );


      /*
       * すでに予約中の時間
       */

      const coveringReservation =
        reservations.find(
          reservation => {

            if(
              reservation.girl !==
              girl
            ){

              return false;

            }


            const start =
              timeToMinutes(
                reservation.time
              );


            const end =
              start +
              Number(
                reservation.course
              );


            return (
              start < minute &&
              end > minute
            );

          }
        );


      html += `
        <div class="cell">
      `;


      /*
       * 開始予約がある
       */

      if(
        startingReservations.length > 0
      ){

        for(
          const reservation
          of startingReservations
        ){

          const end =
            getEndTime(
              reservation.time,
              reservation.course
            );


          html += `
            <button
              type="button"
              class="slot-button busy-button"
              data-edit-id="${escapeHTML(reservation.id)}"
            >

              <div class="reservation-card">

                <div class="reservation-time">
                  ${escapeHTML(reservation.time)}
                  ～ 
                  ${escapeHTML(end)}
                </div>

                <div class="reservation-info">
                  ${Number(reservation.course)}分
                  ${
                    reservation.customer
                      ? " ・ " +
                        escapeHTML(
                          reservation.customer
                        )
                      : ""
                  }
                </div>

              </div>

            </button>
          `;

        }

      }


      /*
       * 予約中
       */

      else if(
        coveringReservation
      ){

        const end =
          getEndTime(
            coveringReservation.time,
            coveringReservation.course
          );


        html += `
          <button
            type="button"
            class="slot-button busy-button"
            data-edit-id="${escapeHTML(coveringReservation.id)}"
          >

            <div class="reservation-card">

              <div class="reservation-time">
                予約中
              </div>

              <div class="reservation-info">
                ${escapeHTML(coveringReservation.time)}
                ～
                ${escapeHTML(end)}
              </div>

            </div>

          </button>
        `;

      }


      /*
       * 空き
       */

      else{

        html += `
          <button
            type="button"
            class="slot-button"
            data-new-time="${minutesToTime(minute)}"
          >
            ＋ 予約
          </button>
        `;

      }


      html += `
        </div>
      `;

    }


    html += `
      </div>
    `;

  }


  root.innerHTML =
    html;


  /*
   * 編集ボタン
   */

  root
    .querySelectorAll(
      "[data-edit-id]"
    )
    .forEach(button => {

      button.addEventListener(
        "click",
        () => {

          openEditReservation(
            button.dataset.editId
          );

        }
      );

    });


  /*
   * 新規予約ボタン
   */

  root
    .querySelectorAll(
      "[data-new-time]"
    )
    .forEach(button => {

      button.addEventListener(
        "click",
        () => {

          openNewReservation(
            button.dataset.newTime
          );

        }
      );

    });

}


/* =========================================================
   予約一覧
   ========================================================= */

function renderReservationList(){

  const root =
    document.getElementById(
      "reservationList"
    );


  if(
    reservations.length === 0
  ){

    root.innerHTML =
      `
        <div class="empty">
          この日の予約はありません。
        </div>
      `;

    return;

  }


  const sorted =
    [...reservations].sort(
      (a,b) =>
        timeToMinutes(a.time) -
        timeToMinutes(b.time)
    );


  root.innerHTML =
    sorted
      .map(reservation => {

        const end =
          getEndTime(
            reservation.time,
            reservation.course
          );


        return `

          <div class="list-item">

            <div class="list-main">

              ${escapeHTML(reservation.time)}
              ～
              ${escapeHTML(end)}

              ・

              ${escapeHTML(reservation.girl)}

            </div>


            <div class="list-sub">

              ${Number(reservation.course)}分

              ${
                reservation.customer
                  ? " ・ お客さん：" +
                    escapeHTML(
                      reservation.customer
                    )
                  : ""
              }

            </div>


            <div class="list-buttons">

              <button
                type="button"
                class="btn"
                data-list-edit="${escapeHTML(reservation.id)}"
              >
                ✏️ 編集・削除
              </button>

            </div>

          </div>

        `;

      })
      .join("");


  root
    .querySelectorAll(
      "[data-list-edit]"
    )
    .forEach(button => {

      button.addEventListener(
        "click",
        () => {

          openEditReservation(
            button.dataset.listEdit
          );

        }
      );

    });

}


/* =========================================================
   全体描画
   ========================================================= */

function render(){

  settings =
    loadSettings();


  loadReservations();


  renderSchedule();


  renderReservationList();

}


/* =========================================================
   設定画面を開く
   ========================================================= */

function openSettings(){

  document.getElementById(
    "girlsInput"
  ).value =
    settings.girls.join(",");


  const root =
    document.getElementById(
      "courseSettings"
    );


  root.innerHTML =
    COURSE_LIST
      .map(minutes => {

        const course =
          settings.courses[minutes];


        return `

          <div class="course-setting">

            <b>
              ${minutes}分
            </b>

            <input
              type="number"
              min="0"
              step="100"
              data-price="${minutes}"
              value="${Number(course.price) || 0}"
              placeholder="料金"
            >

            <input
              type="number"
              min="0"
              step="1"
              data-share="${minutes}"
              value="${Number(course.share) || 0}"
              placeholder="取り分"
            >

          </div>

        `;

      })
      .join("");


  document.getElementById(
    "settingsModal"
  ).classList.add("open");

}


/* =========================================================
   設定画面を閉じる
   ========================================================= */

function closeSettings(){

  document.getElementById(
    "settingsModal"
  ).classList.remove("open");

}


/* =========================================================
   設定保存
   ========================================================= */

function saveSettingsFromModal(){

  const girlsText =
    document.getElementById(
      "girlsInput"
    ).value;


  const girls =
    girlsText
      .split(",")
      .map(
        item =>
          item.trim()
      )
      .filter(Boolean);


  if(
    girls.length === 0
  ){

    alert(
      "女の子を1人以上入力してください。"
    );

    return;

  }


  const courses = {};


  for(
    const minutes of COURSE_LIST
  ){

    const priceInput =
      document.querySelector(
        `[data-price="${minutes}"]`
      );


    const shareInput =
      document.querySelector(
        `[data-share="${minutes}"]`
      );


    courses[minutes] = {

      price:
        Math.max(
          0,
          Number(priceInput.value) || 0
        ),

      share:
        Math.max(
          0,
          Number(shareInput.value) || 0
        )

    };

  }


  settings = {

    girls:
      girls,

    courses:
      courses

  };


  saveSettings();


  closeSettings();


  render();

}


/* =========================================================
   集計
   ========================================================= */

function openSummary(){

  let totalCount =
    reservations.length;


  let totalMinutes =
    0;


  let totalSales =
    0;


  let totalShare =
    0;


  const girlSummary = {};


  for(
    const girl of settings.girls
  ){

    girlSummary[girl] = {

      count:0,

      minutes:0,

      sales:0,

      share:0

    };

  }


  for(
    const reservation
    of reservations
  ){

    const course =
      Number(
        reservation.course
      );


    const config =
      settings.courses[course]
      || {
        price:0,
        share:0
      };


    const price =
      Number(config.price) || 0;


    const share =
      Number(config.share) || 0;


    totalMinutes +=
      course;


    totalSales +=
      price;


    totalShare +=
      share;


    if(
      !girlSummary[
        reservation.girl
      ]
    ){

      girlSummary[
        reservation.girl
      ] = {

        count:0,

        minutes:0,

        sales:0,

        share:0

      };

    }


    girlSummary[
      reservation.girl
    ].count++;


    girlSummary[
      reservation.girl
    ].minutes +=
      course;


    girlSummary[
      reservation.girl
    ].sales +=
      price;


    girlSummary[
      reservation.girl
    ].share +=
      share;

  }


  let html = `

    <div class="summary-box">

      <b>
        日付
      </b>

      ：${escapeHTML(getSelectedDate())}

    </div>


    <div class="summary-box">

      <b>
        予約件数
      </b>

      ：${totalCount}件

      <br>

      <b>
        合計時間
      </b>

      ：${totalMinutes}分

      <br>

      <b>
        売上
      </b>

      ：${totalSales.toLocaleString()}円

      <br>

      <b>
        取り分
      </b>

      ：${totalShare.toLocaleString()}円

    </div>


    <h3>
      女の子別
    </h3>

  `;


  for(
    const [girl,data]
    of Object.entries(
      girlSummary
    )
  ){

    html += `

      <div class="summary-box">

        <b>
          ${escapeHTML(girl)}
        </b>

        <br>

        件数：
        ${data.count}件

        <br>

        時間：
        ${data.minutes}分

        <br>

        売上：
        ${data.sales.toLocaleString()}円

        <br>

        取り分：
        ${data.share.toLocaleString()}円

      </div>

    `;

  }


  document.getElementById(
    "summaryContent"
  ).innerHTML =
    html;


  document.getElementById(
    "summaryModal"
  ).classList.add("open");

}


/* =========================================================
   集計を閉じる
   ========================================================= */

function closeSummary(){

  document.getElementById(
    "summaryModal"
  ).classList.remove("open");

}


/* =========================================================
   今日
   ========================================================= */

function goToday(){

  document.getElementById(
    "dateInput"
  ).value =
    getTodayString();


  render();

}


/* =========================================================
   日付変更
   ========================================================= */

document
  .getElementById("dateInput")
  .addEventListener(
    "change",
    () => {

      render();

    }
  );


/* =========================================================
   今日ボタン
   ========================================================= */

document
  .getElementById("todayButton")
  .addEventListener(
    "click",
    goToday
  );


/* =========================================================
   設定ボタン
   ========================================================= */

document
  .getElementById("settingsButton")
  .addEventListener(
    "click",
    openSettings
  );


/* =========================================================
   集計ボタン
   ========================================================= */

document
  .getElementById("summaryButton")
  .addEventListener(
    "click",
    openSummary
  );


/* =========================================================
   予約キャンセル
   ========================================================= */

document
  .getElementById(
    "cancelReservationButton"
  )
  .addEventListener(
    "click",
    closeReservationModal
  );


/* =========================================================
   予約保存
   ========================================================= */

document
  .getElementById(
    "saveReservationButton"
  )
  .addEventListener(
    "click",
    saveReservation
  );


/* =========================================================
   予約削除
   ========================================================= */

document
  .getElementById(
    "deleteReservationButton"
  )
  .addEventListener(
    "click",
    deleteReservation
  );


/* =========================================================
   時間変更
   ========================================================= */

document
  .getElementById(
    "timeInput"
  )
  .addEventListener(
    "input",
    updateEndTimeText
  );


/* =========================================================
   コース変更
   ========================================================= */

document
  .getElementById(
    "courseSelect"
  )
  .addEventListener(
    "change",
    updateEndTimeText
  );


/* =========================================================
   設定キャンセル
   ========================================================= */

document
  .getElementById(
    "cancelSettingsButton"
  )
  .addEventListener(
    "click",
    closeSettings
  );


/* =========================================================
   設定保存
   ========================================================= */

document
  .getElementById(
    "saveSettingsButton"
  )
  .addEventListener(
    "click",
    saveSettingsFromModal
  );


/* =========================================================
   集計閉じる
   ========================================================= */

document
  .getElementById(
    "closeSummaryButton"
  )
  .addEventListener(
    "click",
    closeSummary
  );


/* =========================================================
   モーダル外側クリック
   ========================================================= */

document
  .getElementById(
    "reservationModal"
  )
  .addEventListener(
    "click",
    event => {

      if(
        event.target.id ===
        "reservationModal"
      ){

        closeReservationModal();

      }

    }
  );


document
  .getElementById(
    "settingsModal"
  )
  .addEventListener(
    "click",
    event => {

      if(
        event.target.id ===
        "settingsModal"
      ){

        closeSettings();

      }

    }
  );


document
  .getElementById(
    "summaryModal"
  )
  .addEventListener(
    "click",
    event => {

      if(
        event.target.id ===
        "summaryModal"
      ){

        closeSummary();

      }

    }
  );


/* =========================================================
   初期化
   ========================================================= */

document.getElementById(
  "dateInput"
).value =
  getTodayString();


render();


/* =========================================================
   END
   ========================================================= */
</script>

</body>
</html>
