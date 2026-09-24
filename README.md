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
  font-family:
    -apple-system,
    BlinkMacSystemFont,
    "Segoe UI",
    "Hiragino Kaku Gothic ProN",
    "Yu Gothic",
    Meiryo,
    sans-serif;
  background:#f4f5f7;
  color:#222;
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

.header-top{
  display:flex;
  align-items:center;
  gap:8px;
  flex-wrap:wrap;
}

.header h1{
  margin:0;
  font-size:20px;
  flex:1;
}

.date-area{
  display:flex;
  align-items:center;
  gap:5px;
}

.date-area input{
  padding:7px;
  border:1px solid #ccc;
  border-radius:6px;
}

.btn{
  border:0;
  border-radius:7px;
  padding:8px 12px;
  background:#333;
  color:#fff;
}

.btn.gray{
  background:#777;
}

.btn.blue{
  background:#1976d2;
}

.btn.green{
  background:#2e7d32;
}

.btn.red{
  background:#c62828;
}

.btn.small{
  padding:5px 8px;
  font-size:12px;
}

.main{
  padding:10px;
}

.info{
  background:#fff;
  border-radius:8px;
  padding:10px;
  margin-bottom:10px;
  border:1px solid #ddd;
}

.info-row{
  display:flex;
  gap:15px;
  flex-wrap:wrap;
  font-size:14px;
}

.schedule-wrap{
  background:#fff;
  border:1px solid #ddd;
  border-radius:8px;
  overflow:auto;
}

.schedule{
  min-width:700px;
}

.schedule-head,
.schedule-row{
  display:grid;
  grid-template-columns:70px repeat(var(--girl-count), minmax(180px,1fr));
}

.schedule-head{
  position:sticky;
  top:61px;
  z-index:20;
  background:#eee;
  border-bottom:1px solid #ccc;
}

.head-time,
.head-girl{
  padding:8px;
  text-align:center;
  font-weight:bold;
  border-right:1px solid #ddd;
}

.time-cell{
  min-height:58px;
  padding:5px;
  border-right:1px solid #ddd;
  border-bottom:1px solid #ddd;
  text-align:center;
  font-size:12px;
  background:#fafafa;
}

.slot{
  min-height:58px;
  padding:4px;
  border-right:1px solid #ddd;
  border-bottom:1px solid #ddd;
  position:relative;
}

.slot-btn{
  width:100%;
  min-height:48px;
  border:0;
  border-radius:6px;
  background:#e8f5e9;
  color:#2e7d32;
  font-weight:bold;
}

.slot-btn:hover{
  background:#c8e6c9;
}

.slot-unavailable{
  width:100%;
  min-height:48px;
  display:flex;
  justify-content:center;
  align-items:center;
  border-radius:6px;
  background:#eeeeee;
  color:#999;
  font-size:12px;
}

.reservation{
  width:100%;
  min-height:48px;
  border-radius:6px;
  padding:5px;
  background:#dbeafe;
  border:1px solid #90caf9;
  text-align:left;
  cursor:pointer;
}

.reservation strong{
  display:block;
  font-size:13px;
}

.reservation span{
  display:block;
  font-size:11px;
  margin-top:2px;
}

.modal-bg{
  position:fixed;
  inset:0;
  background:rgba(0,0,0,.5);
  display:none;
  align-items:center;
  justify-content:center;
  z-index:1000;
  padding:15px;
}

.modal-bg.show{
  display:flex;
}

.modal{
  width:min(520px,100%);
  max-height:90vh;
  overflow:auto;
  background:#fff;
  border-radius:12px;
  padding:18px;
  box-shadow:0 10px 40px rgba(0,0,0,.25);
}

.modal h2{
  margin:0 0 15px;
  font-size:20px;
}

.form-group{
  margin-bottom:13px;
}

.form-group label{
  display:block;
  margin-bottom:5px;
  font-weight:bold;
  font-size:13px;
}

.form-group input,
.form-group select{
  width:100%;
  padding:10px;
  border:1px solid #ccc;
  border-radius:7px;
  background:#fff;
}

.modal-buttons{
  display:flex;
  gap:8px;
  flex-wrap:wrap;
  margin-top:15px;
}

.error{
  background:#ffebee;
  color:#c62828;
  border:1px solid #ef9a9a;
  border-radius:7px;
  padding:9px;
  font-size:13px;
  margin-top:8px;
  display:none;
}

.error.show{
  display:block;
}

.list-item{
  border:1px solid #ddd;
  border-radius:8px;
  padding:10px;
  margin-bottom:8px;
  background:#fafafa;
}

.list-main{
  display:flex;
  justify-content:space-between;
  gap:10px;
  flex-wrap:wrap;
}

.list-info{
  flex:1;
}

.list-actions{
  display:flex;
  gap:5px;
  align-items:center;
}

.settings-section{
  margin-bottom:20px;
}

.settings-section h3{
  margin:0 0 10px;
}

.settings-item{
  display:flex;
  gap:6px;
  margin-bottom:7px;
}

.settings-item input{
  flex:1;
  min-width:0;
  padding:8px;
  border:1px solid #ccc;
  border-radius:6px;
}

.summary-table{
  width:100%;
  border-collapse:collapse;
}

.summary-table th,
.summary-table td{
  border:1px solid #ddd;
  padding:7px;
  text-align:center;
  font-size:13px;
}

.summary-table th{
  background:#eee;
}

.empty{
  text-align:center;
  padding:20px;
  color:#888;
}

.notice{
  padding:9px;
  border-radius:7px;
  background:#fff8e1;
  border:1px solid #ffe082;
  color:#795548;
  font-size:12px;
  margin-bottom:10px;
}

@media(max-width:700px){

  .header h1{
    width:100%;
    flex:none;
  }

  .header-top{
    gap:5px;
  }

  .date-area{
    width:100%;
  }

  .date-area input{
    flex:1;
  }

  .schedule-head{
    top:105px;
  }

  .schedule{
    min-width:650px;
  }

  .schedule-head,
  .schedule-row{
    grid-template-columns:60px repeat(var(--girl-count), minmax(160px,1fr));
  }
}
</style>
</head>

<body>

<header class="header">

  <div class="header-top">

    <h1>予約表</h1>

    <div class="date-area">
      <button class="btn small gray" id="prevDay">‹</button>
      <input type="date" id="dateInput">
      <button class="btn small gray" id="nextDay">›</button>
    </div>

    <button class="btn blue small" id="newReservationBtn">
      ＋予約
    </button>

    <button class="btn small" id="listBtn">
      予約一覧
    </button>

    <button class="btn small" id="summaryBtn">
      集計
    </button>

    <button class="btn small" id="settingsBtn">
      設定
    </button>

  </div>

</header>


<main class="main">

  <div class="notice">
    同じ女の子の予約は、前の予約終了時刻から
    <strong>5分以上</strong>空ける必要があります。
  </div>

  <div class="info">
    <div class="info-row">
      <div>
        予約件数：
        <strong id="reservationCount">0</strong>件
      </div>

      <div>
        売上：
        <strong id="totalSales">¥0</strong>
      </div>
    </div>
  </div>

  <div class="schedule-wrap">
    <div
      class="schedule"
      id="schedule"
      style="--girl-count:2;"
    ></div>
  </div>

</main>


<!-- =========================
     予約モーダル
========================= -->

<div class="modal-bg" id="reservationModal">

  <div class="modal">

    <h2 id="reservationModalTitle">
      予約登録
    </h2>

    <div class="form-group">
      <label>お客様名</label>
      <input
        type="text"
        id="customerInput"
        placeholder="お客様名"
      >
    </div>

    <div class="form-group">
      <label>女の子</label>
      <select id="girlInput"></select>
    </div>

    <div class="form-group">
      <label>開始時間</label>
      <input
        type="time"
        id="timeInput"
        step="300"
      >
    </div>

    <div class="form-group">
      <label>コース</label>
      <select id="courseInput"></select>
    </div>

    <div
      class="error"
      id="reservationError"
    ></div>

    <div class="modal-buttons">

      <button
        class="btn green"
        id="saveReservationBtn"
      >
        保存
      </button>

      <button
        class="btn gray"
        id="cancelReservationBtn"
      >
        キャンセル
      </button>

      <button
        class="btn red"
        id="deleteReservationBtn"
        style="display:none;"
      >
        削除
      </button>

    </div>

  </div>

</div>


<!-- =========================
     予約一覧
========================= -->

<div class="modal-bg" id="listModal">

  <div class="modal">

    <h2>予約一覧</h2>

    <div id="reservationList"></div>

    <div class="modal-buttons">

      <button
        class="btn gray"
        id="closeListBtn"
      >
        閉じる
      </button>

    </div>

  </div>

</div>


<!-- =========================
     集計
========================= -->

<div class="modal-bg" id="summaryModal">

  <div class="modal">

    <h2>集計</h2>

    <div id="summaryContent"></div>

    <div class="modal-buttons">

      <button
        class="btn gray"
        id="closeSummaryBtn"
      >
        閉じる
      </button>

    </div>

  </div>

</div>


<!-- =========================
     設定
========================= -->

<div class="modal-bg" id="settingsModal">

  <div class="modal">

    <h2>設定</h2>

    <div class="settings-section">

      <h3>女の子</h3>

      <div id="girlsSettings"></div>

      <button
        class="btn blue small"
        id="addGirlBtn"
      >
        ＋女の子追加
      </button>

    </div>


    <div class="settings-section">

      <h3>コース</h3>

      <div id="coursesSettings"></div>

      <button
        class="btn blue small"
        id="addCourseBtn"
      >
        ＋コース追加
      </button>

    </div>


    <div class="modal-buttons">

      <button
        class="btn green"
        id="saveSettingsBtn"
      >
        設定を保存
      </button>

      <button
        class="btn gray"
        id="closeSettingsBtn"
      >
        閉じる
      </button>

    </div>

  </div>

</div>


<script>

"use strict";

/* =====================================================
   基本設定
===================================================== */

const START_TIME = 10 * 60;
const END_TIME   = 24 * 60;

/*
  ここが今回一番重要。

  前の予約が10:00～10:40なら

  10:40 → 不可
  10:45 → 不可
  10:50 → OK

  という5分間隔。
*/
const GAP = 5;


/* =====================================================
   初期設定
===================================================== */

const DEFAULT_GIRLS = [
  "ことねさん",
  "ナヨンさん"
];

const DEFAULT_COURSES = [
  {
    name: "40分",
    minutes: 40,
    price: 8000
  },
  {
    name: "60分",
    minutes: 60,
    price: 11000
  },
  {
    name: "90分",
    minutes: 90,
    price: 16000
  },
  {
    name: "100分",
    minutes: 100,
    price: 18000
  },
  {
    name: "120分",
    minutes: 120,
    price: 22000
  }
];


/* =====================================================
   状態
===================================================== */

let girls = loadGirls();
let courses = loadCourses();

let currentDate = "";
let reservations = [];

let editingReservationId = null;


/* =====================================================
   DOM
===================================================== */

const dateInput =
  document.getElementById("dateInput");

const schedule =
  document.getElementById("schedule");

const reservationModal =
  document.getElementById("reservationModal");

const listModal =
  document.getElementById("listModal");

const summaryModal =
  document.getElementById("summaryModal");

const settingsModal =
  document.getElementById("settingsModal");

const customerInput =
  document.getElementById("customerInput");

const girlInput =
  document.getElementById("girlInput");

const timeInput =
  document.getElementById("timeInput");

const courseInput =
  document.getElementById("courseInput");

const reservationError =
  document.getElementById("reservationError");

const reservationModalTitle =
  document.getElementById("reservationModalTitle");

const deleteReservationBtn =
  document.getElementById("deleteReservationBtn");


/* =====================================================
   localStorage
===================================================== */

function getGirlsKey(){
  return "reservation_girls";
}

function getCoursesKey(){
  return "reservation_courses";
}

function getReservationKey(date){
  return "reservations_" + date;
}


function loadGirls(){

  try{

    const data =
      localStorage.getItem(getGirlsKey());

    if(!data){
      return [...DEFAULT_GIRLS];
    }

    const parsed = JSON.parse(data);

    if(
      !Array.isArray(parsed) ||
      parsed.length === 0
    ){
      return [...DEFAULT_GIRLS];
    }

    return parsed;

  }catch(e){

    return [...DEFAULT_GIRLS];

  }
}


function loadCourses(){

  try{

    const data =
      localStorage.getItem(getCoursesKey());

    if(!data){
      return DEFAULT_COURSES.map(x => ({...x}));
    }

    const parsed = JSON.parse(data);

    if(
      !Array.isArray(parsed) ||
      parsed.length === 0
    ){
      return DEFAULT_COURSES.map(x => ({...x}));
    }

    return parsed;

  }catch(e){

    return DEFAULT_COURSES.map(x => ({...x}));

  }
}


function saveGirls(){

  localStorage.setItem(
    getGirlsKey(),
    JSON.stringify(girls)
  );

}


function saveCourses(){

  localStorage.setItem(
    getCoursesKey(),
    JSON.stringify(courses)
  );

}


function loadReservations(){

  try{

    const data =
      localStorage.getItem(
        getReservationKey(currentDate)
      );

    if(!data){
      return [];
    }

    const parsed = JSON.parse(data);

    return Array.isArray(parsed)
      ? parsed
      : [];

  }catch(e){

    return [];

  }
}


function saveReservations(){

  localStorage.setItem(
    getReservationKey(currentDate),
    JSON.stringify(reservations)
  );

}


/* =====================================================
   日付
===================================================== */

function getTodayString(){

  const d = new Date();

  const y = d.getFullYear();

  const m =
    String(d.getMonth() + 1)
      .padStart(2,"0");

  const day =
    String(d.getDate())
      .padStart(2,"0");

  return `${y}-${m}-${day}`;
}


function changeDate(date){

  currentDate = date;

  dateInput.value = date;

  reservations =
    loadReservations();

  render();

}


/* =====================================================
   時刻変換
===================================================== */

function timeToMinutes(time){

  if(!time){
    return NaN;
  }

  const parts = String(time).split(":");

  if(parts.length !== 2){
    return NaN;
  }

  const h = Number(parts[0]);
  const m = Number(parts[1]);

  if(
    !Number.isFinite(h) ||
    !Number.isFinite(m)
  ){
    return NaN;
  }

  return h * 60 + m;
}


function minutesToTime(minutes){

  const h =
    Math.floor(minutes / 60);

  const m =
    minutes % 60;

  return (
    String(h).padStart(2,"0") +
    ":" +
    String(m).padStart(2,"0")
  );

}


function formatYen(value){

  return "¥" +
    Number(value || 0)
      .toLocaleString("ja-JP");

}


/* =====================================================
   HTMLエスケープ
===================================================== */

function escapeHtml(value){

  return String(value ?? "")
    .replace(/&/g,"&amp;")
    .replace(/</g,"&lt;")
    .replace(/>/g,"&gt;")
    .replace(/"/g,"&quot;")
    .replace(/'/g,"&#039;");

}


/* =====================================================
   ID
===================================================== */

function createId(){

  return (
    Date.now().toString(36) +
    Math.random()
      .toString(36)
      .substring(2,8)
  );

}


/* =====================================================
   コース検索
===================================================== */

function getCourseByMinutes(minutes){

  return courses.find(
    c => Number(c.minutes) === Number(minutes)
  );

}


/* =====================================================
   予約終了時刻
===================================================== */

function getReservationEnd(reservation){

  const start =
    timeToMinutes(reservation.time);

  const duration =
    Number(reservation.course);

  return start + duration;

}


/* =====================================================
   ★★★ 予約可能判定 ★★★
=====================================================

   ここが今回の重要部分。

   例：

   既存予約
   10:00～10:40

   新規予約
   10:40 → NG
   10:45 → NG
   10:50 → OK

   逆方向も判定する。

   既存予約
   11:00～

   新規予約を10:20から40分
   10:20～11:00

   これは11:00ぴったりなのでNG。

   10:15～10:55なら、
   11:00まで5分空くのでOK。
===================================================== */

function isReservationTimeAvailable(
  girl,
  startTime,
  duration,
  excludeId = null
){

  const newStart =
    timeToMinutes(startTime);

  const newDuration =
    Number(duration);

  if(
    !Number.isFinite(newStart) ||
    !Number.isFinite(newDuration) ||
    newDuration <= 0
  ){
    return false;
  }

  const newEnd =
    newStart + newDuration;


  /*
    営業時間外チェック
  */

  if(newStart < START_TIME){
    return false;
  }

  if(newEnd > END_TIME){
    return false;
  }


  /*
    同じ女の子の予約だけ調べる
  */

  for(const oldReservation of reservations){

    if(
      excludeId &&
      String(oldReservation.id) === String(excludeId)
    ){
      continue;
    }

    if(
      String(oldReservation.girl) !==
      String(girl)
    ){
      continue;
    }

    const oldStart =
      timeToMinutes(oldReservation.time);

    const oldEnd =
      getReservationEnd(oldReservation);


    /*
      新規予約が既存予約より完全に前
    */

    const canBeBefore =
      newEnd + GAP <= oldStart;


    /*
      新規予約が既存予約より完全に後
    */

    const canBeAfter =
      newStart >= oldEnd + GAP;


    /*
      前でも後でもない
      ↓
      重なっている
      または5分空いていない
    */

    if(
      !canBeBefore &&
      !canBeAfter
    ){
      return false;
    }

  }

  return true;
}


/* =====================================================
   ★ グリッド上の予約可能判定
=====================================================

   グリッドは10分単位。

   コースを選んでいない状態でも、
   その時間に「何か1つでもコースを入れられるか」
   を確認する。

   例：

   10:00～10:40予約

   10:40
   → どのコースでも5分空かない
   → 予約不可

   10:50
   → 40分コースなら入る
   → ＋予約
===================================================== */

function isGridTimeAvailable(
  girl,
  gridTime
){

  for(const course of courses){

    if(
      isReservationTimeAvailable(
        girl,
        gridTime,
        Number(course.minutes)
      )
    ){
      return true;
    }

  }

  return false;
}


/* =====================================================
   予約フォーム
===================================================== */

function populateGirlSelect(){

  girlInput.innerHTML = "";

  girls.forEach(girl => {

    const option =
      document.createElement("option");

    option.value = girl;
    option.textContent = girl;

    girlInput.appendChild(option);

  });

}


function populateCourseSelect(){

  courseInput.innerHTML = "";

  courses.forEach(course => {

    const option =
      document.createElement("option");

    option.value =
      String(course.minutes);

    option.textContent =
      `${course.name} ${formatYen(course.price)}`;

    courseInput.appendChild(option);

  });

}


function openNewReservation(
  girl = null,
  time = null
){

  editingReservationId = null;

  reservationModalTitle.textContent =
    "予約登録";

  customerInput.value = "";

  populateGirlSelect();
  populateCourseSelect();


  if(girl && girls.includes(girl)){

    girlInput.value = girl;

  }else if(girls.length){

    girlInput.value = girls[0];

  }


  if(time){

    timeInput.value = time;

  }else{

    timeInput.value = "10:00";

  }


  if(courses.length){

    courseInput.value =
      String(courses[0].minutes);

  }


  deleteReservationBtn.style.display =
    "none";

  clearReservationError();

  reservationModal.classList.add("show");

}


function openEditReservation(id){

  const reservation =
    reservations.find(
      r => String(r.id) === String(id)
    );

  if(!reservation){
    return;
  }

  editingReservationId =
    reservation.id;

  reservationModalTitle.textContent =
    "予約編集";

  customerInput.value =
    reservation.customer || "";

  populateGirlSelect();
  populateCourseSelect();

  girlInput.value =
    reservation.girl;

  timeInput.value =
    reservation.time;

  courseInput.value =
    String(reservation.course);

  deleteReservationBtn.style.display =
    "inline-block";

  clearReservationError();

  reservationModal.classList.add("show");

}


function closeReservationModal(){

  reservationModal.classList.remove("show");

  editingReservationId = null;

  clearReservationError();

}


function showReservationError(message){

  reservationError.textContent =
    message;

  reservationError.classList.add("show");

}


function clearReservationError(){

  reservationError.textContent = "";

  reservationError.classList.remove("show");

}


/* =====================================================
   予約保存
===================================================== */

function saveReservation(){

  clearReservationError();

  const customer =
    customerInput.value.trim();

  const girl =
    girlInput.value;

  const time =
    timeInput.value;

  const duration =
    Number(courseInput.value);


  if(!girl){

    showReservationError(
      "女の子を選択してください。"
    );

    return;
  }


  if(!time){

    showReservationError(
      "開始時間を入力してください。"
    );

    return;
  }


  if(!duration){

    showReservationError(
      "コースを選択してください。"
    );

    return;
  }


  const start =
    timeToMinutes(time);

  const end =
    start + duration;


  if(start < START_TIME){

    showReservationError(
      `${minutesToTime(START_TIME)}以降で入力してください。`
    );

    return;
  }


  if(end > END_TIME){

    showReservationError(
      "営業時間内に収まる時間で予約してください。"
    );

    return;
  }


  /*
    ★ここで5分ルールを強制
  */

  const available =
    isReservationTimeAvailable(
      girl,
      time,
      duration,
      editingReservationId
    );


  if(!available){

    /*
      ぶつかっている予約を探して、
      具体的な理由を表示
    */

    const conflict =
      findConflictReservation(
        girl,
        time,
        duration,
        editingReservationId
      );

    if(conflict){

      const oldStart =
        timeToMinutes(conflict.time);

      const oldEnd =
        getReservationEnd(conflict);

      const oldEndWithGap =
        oldEnd + GAP;


      if(start >= oldStart){

        showReservationError(
          `${conflict.time}～${minutesToTime(oldEnd)}に予約があります。` +
          `次に予約できるのは${minutesToTime(oldEndWithGap)}以降です。`
        );

      }else{

        showReservationError(
          `${conflict.time}～${minutesToTime(oldEnd)}に予約があります。` +
          `この予約は開始時間またはコースを変更してください。`
        );

      }

    }else{

      showReservationError(
        "同じ女の子の予約と重なっています。5分以上空けてください。"
      );

    }

    return;
  }


  /*
    編集
  */

  if(editingReservationId){

    const index =
      reservations.findIndex(
        r =>
          String(r.id) ===
          String(editingReservationId)
      );

    if(index !== -1){

      reservations[index] = {
        ...reservations[index],
        customer,
        girl,
        time,
        course: duration
      };

    }

  }else{

    /*
      新規
    */

    reservations.push({

      id:createId(),

      customer,

      girl,

      time,

      course:duration,

      createdAt:Date.now()

    });

  }


  saveReservations();

  closeReservationModal();

  render();

}


/* =====================================================
   競合予約を探す
===================================================== */

function findConflictReservation(
  girl,
  time,
  duration,
  excludeId = null
){

  const newStart =
    timeToMinutes(time);

  const newEnd =
    newStart + Number(duration);


  for(const reservation of reservations){

    if(
      excludeId &&
      String(reservation.id) === String(excludeId)
    ){
      continue;
    }

    if(
      String(reservation.girl) !==
      String(girl)
    ){
      continue;
    }

    const oldStart =
      timeToMinutes(reservation.time);

    const oldEnd =
      getReservationEnd(reservation);


    const canBeBefore =
      newEnd + GAP <= oldStart;

    const canBeAfter =
      newStart >= oldEnd + GAP;


    if(
      !canBeBefore &&
      !canBeAfter
    ){
      return reservation;
    }

  }

  return null;
}


/* =====================================================
   削除
===================================================== */

function deleteCurrentReservation(){

  if(!editingReservationId){
    return;
  }

  const reservation =
    reservations.find(
      r =>
        String(r.id) ===
        String(editingReservationId)
    );

  if(!reservation){
    return;
  }


  const ok =
    confirm(
      `${reservation.customer || "この予約"}を削除しますか？`
    );

  if(!ok){
    return;
  }


  reservations =
    reservations.filter(
      r =>
        String(r.id) !==
        String(editingReservationId)
    );


  saveReservations();

  closeReservationModal();

  render();

}


/* =====================================================
   スケジュール描画
===================================================== */

function render(){

  schedule.style.setProperty(
    "--girl-count",
    Math.max(girls.length,1)
  );


  let html = "";


  /*
    ヘッダー
  */

  html += `
    <div class="schedule-head">

      <div class="head-time">
        時間
      </div>
  `;


  girls.forEach(girl => {

    html += `
      <div class="head-girl">
        ${escapeHtml(girl)}
      </div>
    `;

  });


  html += `
    </div>
  `;


  /*
    10分刻み
  */

  for(
    let minutes = START_TIME;
    minutes < END_TIME;
    minutes += 10
  ){

    const time =
      minutesToTime(minutes);


    html += `
      <div class="schedule-row">

        <div class="time-cell">
          ${time}
        </div>
    `;


    girls.forEach(girl => {

      const reservation =
        getReservationStartingAt(
          girl,
          time
        );


      /*
        この時間から始まる予約がある
      */

      if(reservation){

        const end =
          getReservationEnd(reservation);

        const course =
          getCourseByMinutes(
            reservation.course
          );

        html += `
          <div class="slot">

            <button
              class="reservation"
              type="button"
              data-edit-id="${escapeHtml(reservation.id)}"
            >

              <strong>
                ${escapeHtml(
                  reservation.customer || "予約"
                )}
              </strong>

              <span>
                ${escapeHtml(reservation.time)}
                ～${escapeHtml(minutesToTime(end))}
              </span>

              <span>
                ${escapeHtml(
                  course
                    ? course.name
                    : `${reservation.course}分`
                )}
              </span>

            </button>

          </div>
        `;

        return;
      }


      /*
        既存予約の途中なら、
        予約不可表示
      */

      if(isInsideExistingReservation(
        girl,
        minutes
      )){

        html += `
          <div class="slot">

            <div class="slot-unavailable">
              予約不可
            </div>

          </div>
        `;

        return;
      }


      /*
        その時間から予約を開始できるか
      */

      if(
        isGridTimeAvailable(
          girl,
          time
        )
      ){

        html += `
          <div class="slot">

            <button
              class="slot-btn"
              type="button"
              data-new-girl="${escapeHtml(girl)}"
              data-new-time="${time}"
            >
              ＋予約
            </button>

          </div>
        `;

      }else{

        html += `
          <div class="slot">

            <div class="slot-unavailable">
              予約不可
            </div>

          </div>
        `;

      }

    });


    html += `
      </div>
    `;

  }


  schedule.innerHTML = html;


  /*
    イベント
  */

  schedule
    .querySelectorAll("[data-edit-id]")
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


  schedule
    .querySelectorAll("[data-new-girl]")
    .forEach(button => {

      button.addEventListener(
        "click",
        () => {

          openNewReservation(
            button.dataset.newGirl,
            button.dataset.newTime
          );

        }
      );

    });


  updateInfo();

}


/* =====================================================
   開始時刻に予約があるか
===================================================== */

function getReservationStartingAt(
  girl,
  time
){

  return reservations.find(
    reservation =>
      String(reservation.girl) ===
      String(girl) &&
      String(reservation.time) ===
      String(time)
  );

}


/* =====================================================
   既存予約の中にいるか
=====================================================

   例：

   10:00～10:40

   10:00 → 予約
   10:10 → 不可
   10:20 → 不可
   10:30 → 不可
   10:40 → 不可
   10:50 → 予約可能

===================================================== */

function isInsideExistingReservation(
  girl,
  gridMinutes
){

  for(const reservation of reservations){

    if(
      String(reservation.girl) !==
      String(girl)
    ){
      continue;
    }


    const start =
      timeToMinutes(reservation.time);

    const end =
      getReservationEnd(reservation);


    /*
      予約終了時間そのものも
      GAPを考慮して不可にする。

      10:00～10:40なら

      10:40～10:50 → 不可
    */

    if(
      gridMinutes >= start &&
      gridMinutes < end + GAP
    ){

      /*
        ただし開始時刻そのものは
        予約ボタンとして表示する必要がある。
      */

      if(gridMinutes === start){
        return false;
      }

      return true;
    }

  }

  return false;
}


/* =====================================================
   情報
===================================================== */

function updateInfo(){

  const count =
    reservations.length;

  const sales =
    reservations.reduce(
      (sum,reservation) => {

        const course =
          getCourseByMinutes(
            reservation.course
          );

        return sum +
          Number(course?.price || 0);

      },
      0
    );


  document.getElementById(
    "reservationCount"
  ).textContent = count;


  document.getElementById(
    "totalSales"
  ).textContent = formatYen(sales);

}


/* =====================================================
   予約一覧
===================================================== */

function openList(){

  renderReservationList();

  listModal.classList.add("show");

}


function renderReservationList(){

  const container =
    document.getElementById(
      "reservationList"
    );


  if(reservations.length === 0){

    container.innerHTML =
      `<div class="empty">予約はありません。</div>`;

    return;
  }


  const sorted =
    [...reservations].sort(
      (a,b) =>
        timeToMinutes(a.time) -
        timeToMinutes(b.time)
    );


  let html = "";


  sorted.forEach(reservation => {

    const end =
      getReservationEnd(reservation);

    const course =
      getCourseByMinutes(
        reservation.course
      );


    html += `
      <div class="list-item">

        <div class="list-main">

          <div class="list-info">

            <strong>
              ${escapeHtml(
                reservation.customer || "名前なし"
              )}
            </strong>

            <div>
              ${escapeHtml(reservation.girl)}
            </div>

            <div>
              ${escapeHtml(reservation.time)}
              ～${escapeHtml(minutesToTime(end))}
            </div>

            <div>
              ${escapeHtml(
                course
                  ? course.name
                  : `${reservation.course}分`
              )}
              ${
                course
                  ? " / " + formatYen(course.price)
                  : ""
              }
            </div>

          </div>

          <div class="list-actions">

            <button
              class="btn blue small"
              type="button"
              data-list-edit="${escapeHtml(reservation.id)}"
            >
              編集
            </button>

          </div>

        </div>

      </div>
    `;

  });


  container.innerHTML = html;


  container
    .querySelectorAll("[data-list-edit]")
    .forEach(button => {

      button.addEventListener(
        "click",
        () => {

          listModal.classList.remove("show");

          openEditReservation(
            button.dataset.listEdit
          );

        }
      );

    });

}


/* =====================================================
   集計
===================================================== */

function openSummary(){

  renderSummary();

  summaryModal.classList.add("show");

}


function renderSummary(){

  const container =
    document.getElementById(
      "summaryContent"
    );


  /*
    女の子ごと
  */

  const girlData =
    girls.map(girl => {

      const list =
        reservations.filter(
          r =>
            String(r.girl) ===
            String(girl)
        );


      const sales =
        list.reduce(
          (sum,r) => {

            const course =
              getCourseByMinutes(r.course);

            return sum +
              Number(course?.price || 0);

          },
          0
        );


      return {
        girl,
        count:list.length,
        sales
      };

    });


  /*
    コースごと
  */

  const courseData =
    courses.map(course => {

      const list =
        reservations.filter(
          r =>
            Number(r.course) ===
            Number(course.minutes)
        );


      const sales =
        list.length *
        Number(course.price || 0);


      return {
        course,
        count:list.length,
        sales
      };

    });


  const total =
    reservations.reduce(
      (sum,r) => {

        const course =
          getCourseByMinutes(r.course);

        return sum +
          Number(course?.price || 0);

      },
      0
    );


  let html = `

    <h3>全体</h3>

    <table class="summary-table">

      <tr>
        <th>予約件数</th>
        <th>売上</th>
      </tr>

      <tr>
        <td>${reservations.length}件</td>
        <td>${formatYen(total)}</td>
      </tr>

    </table>


    <br>


    <h3>女の子別</h3>

    <table class="summary-table">

      <tr>
        <th>女の子</th>
        <th>件数</th>
        <th>売上</th>
        <th>割合</th>
      </tr>

  `;


  girlData.forEach(data => {

    const count =
      reservations.length > 0
        ? data.count /
          reservations.length *
          100
        : 0;


    html += `

      <tr>

        <td>
          ${escapeHtml(data.girl)}
        </td>

        <td>
          ${data.count}
        </td>

        <td>
          ${formatYen(data.sales)}
        </td>

        <td>
          ${count.toFixed(1)}%
        </td>

      </tr>

    `;

  });


  html += `

    </table>


    <br>


    <h3>コース別</h3>

    <table class="summary-table">

      <tr>
        <th>コース</th>
        <th>件数</th>
        <th>売上</th>
        <th>割合</th>
      </tr>

  `;


  courseData.forEach(data => {

    const count =
      reservations.length > 0
        ? data.count /
          reservations.length *
          100
        : 0;


    html += `

      <tr>

        <td>
          ${escapeHtml(data.course.name)}
        </td>

        <td>
          ${data.count}
        </td>

        <td>
          ${formatYen(data.sales)}
        </td>

        <td>
          ${count.toFixed(1)}%
        </td>

      </tr>

    `;

  });


  html += `
    </table>
  `;


  container.innerHTML = html;

}


/* =====================================================
   設定
===================================================== */

function openSettings(){

  renderSettings();

  settingsModal.classList.add("show");

}


function renderSettings(){

  renderGirlsSettings();

  renderCoursesSettings();

}


function renderGirlsSettings(){

  const container =
    document.getElementById(
      "girlsSettings"
    );


  container.innerHTML = "";


  girls.forEach((girl,index) => {

    const div =
      document.createElement("div");

    div.className =
      "settings-item";


    div.innerHTML = `

      <input
        type="text"
        value="${escapeHtml(girl)}"
        data-girl-index="${index}"
      >

      <button
        type="button"
        class="btn red small"
        data-delete-girl="${index}"
      >
        削除
      </button>

    `;


    container.appendChild(div);

  });


  container
    .querySelectorAll("[data-delete-girl]")
    .forEach(button => {

      button.addEventListener(
        "click",
        () => {

          const index =
            Number(
              button.dataset.deleteGirl
            );


          if(girls.length <= 1){

            alert(
              "女の子は最低1人必要です。"
            );

            return;

          }


          const ok =
            confirm(
              `${girls[index]}を削除しますか？`
            );


          if(!ok){
            return;
          }


          girls.splice(index,1);

          renderSettings();

        }
      );

    });

}


function renderCoursesSettings(){

  const container =
    document.getElementById(
      "coursesSettings"
    );


  container.innerHTML = "";


  courses.forEach((course,index) => {

    const div =
      document.createElement("div");

    div.className =
      "settings-item";


    div.innerHTML = `

      <input
        type="text"
        value="${escapeHtml(course.name)}"
        placeholder="コース名"
        data-course-name="${index}"
      >

      <input
        type="number"
        value="${Number(course.minutes)}"
        min="1"
        placeholder="分"
        style="max-width:90px;"
        data-course-minutes="${index}"
      >

      <input
        type="number"
        value="${Number(course.price)}"
        min="0"
        placeholder="料金"
        style="max-width:110px;"
        data-course-price="${index}"
      >

      <button
        type="button"
        class="btn red small"
        data-delete-course="${index}"
      >
        削除
      </button>

    `;


    container.appendChild(div);

  });


  container
    .querySelectorAll("[data-delete-course]")
    .forEach(button => {

      button.addEventListener(
        "click",
        () => {

          const index =
            Number(
              button.dataset.deleteCourse
            );


          if(courses.length <= 1){

            alert(
              "コースは最低1つ必要です。"
            );

            return;

          }


          courses.splice(index,1);

          renderSettings();

        }
      );

    });

}


/* =====================================================
   設定保存
===================================================== */

function saveSettings(){

  const girlInputs =
    document.querySelectorAll(
      "[data-girl-index]"
    );


  const newGirls = [];


  girlInputs.forEach(input => {

    const value =
      input.value.trim();

    if(value){
      newGirls.push(value);
    }

  });


  if(newGirls.length === 0){

    alert(
      "女の子を1人以上設定してください。"
    );

    return;

  }


  const courseNames =
    document.querySelectorAll(
      "[data-course-name]"
    );

  const courseMinutes =
    document.querySelectorAll(
      "[data-course-minutes]"
    );

  const coursePrices =
    document.querySelectorAll(
      "[data-course-price]"
    );


  const newCourses = [];


  for(
    let i = 0;
    i < courseNames.length;
    i++
  ){

    const name =
      courseNames[i].value.trim();

    const minutes =
      Number(courseMinutes[i].value);

    const price =
      Number(coursePrices[i].value);


    if(!name){
      alert("コース名を入力してください。");
      return;
    }


    if(
      !Number.isFinite(minutes) ||
      minutes <= 0
    ){

      alert(
        "コース時間を正しく入力してください。"
      );

      return;
    }


    if(
      !Number.isFinite(price) ||
      price < 0
    ){

      alert(
        "料金を正しく入力してください。"
      );

      return;
    }


    newCourses.push({
      name,
      minutes,
      price
    });

  }


  if(newCourses.length === 0){

    alert(
      "コースを1つ以上設定してください。"
    );

    return;

  }


  /*
    同じ時間のコースを重複させない
  */

  const minutesSet =
    new Set();


  for(const course of newCourses){

    if(minutesSet.has(course.minutes)){

      alert(
        "同じ時間のコースが重複しています。"
      );

      return;

    }

    minutesSet.add(course.minutes);

  }


  girls = newGirls;

  courses = newCourses;


  saveGirls();
  saveCourses();


  populateGirlSelect();
  populateCourseSelect();


  settingsModal.classList.remove("show");

  render();

}


/* =====================================================
   女の子追加
===================================================== */

function addGirl(){

  const name =
    prompt(
      "女の子の名前を入力してください。"
    );


  if(name === null){
    return;
  }


  const value =
    name.trim();


  if(!value){
    return;
  }


  if(girls.includes(value)){

    alert(
      "同じ名前がすでにあります。"
    );

    return;

  }


  girls.push(value);

  renderSettings();

}


/* =====================================================
   コース追加
===================================================== */

function addCourse(){

  const name =
    prompt(
      "コース名を入力してください。",
      "新コース"
    );


  if(name === null){
    return;
  }


  const minutesText =
    prompt(
      "コース時間（分）を入力してください。",
      "40"
    );


  if(minutesText === null){
    return;
  }


  const priceText =
    prompt(
      "料金を入力してください。",
      "8000"
    );


  if(priceText === null){
    return;
  }


  const minutes =
    Number(minutesText);

  const price =
    Number(priceText);


  if(
    !name.trim() ||
    !Number.isFinite(minutes) ||
    minutes <= 0 ||
    !Number.isFinite(price) ||
    price < 0
  ){

    alert(
      "入力内容が正しくありません。"
    );

    return;

  }


  if(
    courses.some(
      c =>
        Number(c.minutes) ===
        Number(minutes)
    )
  ){

    alert(
      "同じ時間のコースがすでにあります。"
    );

    return;

  }


  courses.push({
    name:name.trim(),
    minutes,
    price
  });


  renderSettings();

}


/* =====================================================
   前後の日付
===================================================== */

function moveDate(days){

  const d =
    new Date(
      currentDate + "T00:00:00"
    );


  d.setDate(
    d.getDate() + days
  );


  const y =
    d.getFullYear();

  const m =
    String(d.getMonth()+1)
      .padStart(2,"0");

  const day =
    String(d.getDate())
      .padStart(2,"0");


  changeDate(
    `${y}-${m}-${day}`
  );

}


/* =====================================================
   フォーム変更時のリアルタイムチェック
===================================================== */

function checkCurrentForm(){

  if(
    !girlInput.value ||
    !timeInput.value ||
    !courseInput.value
  ){
    clearReservationError();
    return;
  }


  const available =
    isReservationTimeAvailable(
      girlInput.value,
      timeInput.value,
      Number(courseInput.value),
      editingReservationId
    );


  if(!available){

    const conflict =
      findConflictReservation(
        girlInput.value,
        timeInput.value,
        Number(courseInput.value),
        editingReservationId
      );


    if(conflict){

      const oldEnd =
        getReservationEnd(conflict);

      const next =
        oldEnd + GAP;


      showReservationError(
        `この時間は予約できません。` +
        `${conflict.time}～${minutesToTime(oldEnd)}の予約があるため、` +
        `${minutesToTime(next)}以降にしてください。`
      );

    }else{

      showReservationError(
        "この時間は予約できません。"
      );

    }

  }else{

    clearReservationError();

  }

}


/* =====================================================
   イベント
===================================================== */

document
  .getElementById("prevDay")
  .addEventListener(
    "click",
    () => moveDate(-1)
  );


document
  .getElementById("nextDay")
  .addEventListener(
    "click",
    () => moveDate(1)
  );


dateInput.addEventListener(
  "change",
  () => {

    if(dateInput.value){
      changeDate(dateInput.value);
    }

  }
);


document
  .getElementById("newReservationBtn")
  .addEventListener(
    "click",
    () => openNewReservation()
  );


document
  .getElementById("saveReservationBtn")
  .addEventListener(
    "click",
    saveReservation
  );


document
  .getElementById("cancelReservationBtn")
  .addEventListener(
    "click",
    closeReservationModal
  );


deleteReservationBtn.addEventListener(
  "click",
  deleteCurrentReservation
);


document
  .getElementById("listBtn")
  .addEventListener(
    "click",
    openList
  );


document
  .getElementById("closeListBtn")
  .addEventListener(
    "click",
    () => listModal.classList.remove("show")
  );


document
  .getElementById("summaryBtn")
  .addEventListener(
    "click",
    openSummary
  );


document
  .getElementById("closeSummaryBtn")
  .addEventListener(
    "click",
    () => summaryModal.classList.remove("show")
  );


document
  .getElementById("settingsBtn")
  .addEventListener(
    "click",
    openSettings
  );


document
  .getElementById("closeSettingsBtn")
  .addEventListener(
    "click",
    () => settingsModal.classList.remove("show")
  );


document
  .getElementById("saveSettingsBtn")
  .addEventListener(
    "click",
    saveSettings
  );


document
  .getElementById("addGirlBtn")
  .addEventListener(
    "click",
    addGirl
  );


document
  .getElementById("addCourseBtn")
  .addEventListener(
    "click",
    addCourse
  );


girlInput.addEventListener(
  "change",
  checkCurrentForm
);


timeInput.addEventListener(
  "input",
  checkCurrentForm
);


courseInput.addEventListener(
  "change",
  checkCurrentForm
);


/*
  モーダル外側をクリックして閉じる
*/

reservationModal.addEventListener(
  "click",
  event => {

    if(event.target === reservationModal){
      closeReservationModal();
    }

  }
);


listModal.addEventListener(
  "click",
  event => {

    if(event.target === listModal){
      listModal.classList.remove("show");
    }

  }
);


summaryModal.addEventListener(
  "click",
  event => {

    if(event.target === summaryModal){
      summaryModal.classList.remove("show");
    }

  }
);


settingsModal.addEventListener(
  "click",
  event => {

    if(event.target === settingsModal){
      settingsModal.classList.remove("show");
    }

  }
);


/* =====================================================
   初期化
===================================================== */

(function init(){

  currentDate =
    getTodayString();

  dateInput.value =
    currentDate;

  reservations =
    loadReservations();

  populateGirlSelect();

  populateCourseSelect();

  render();

})();

</script>

</body>
</html>
