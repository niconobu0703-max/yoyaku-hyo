<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>予約表</title>

<style>
*{
  box-sizing:border-box;
}

body{
  margin:0;
  font-family:-apple-system,BlinkMacSystemFont,"Noto Sans JP",sans-serif;
  background:#fff;
  color:#111;
}

button,input,select{
  font:inherit;
}

button{
  cursor:pointer;
}

/* =========================
   上部
========================= */

.topbar{
  position:sticky;
  top:0;
  z-index:20;
  background:#fff;
  border-bottom:1px solid #ddd;
  padding:12px;
}

.nav{
  display:grid;
  grid-template-columns:52px 1fr 52px 90px 90px;
  gap:8px;
  align-items:center;
}

.nav button,
.datebox{
  height:52px;
  border-radius:12px;
  border:1px solid #ccc;
}

.nav button{
  background:#222;
  color:#fff;
  font-size:28px;
}

.nav .action{
  font-size:16px;
}

.datebox{
  display:flex;
  align-items:center;
  justify-content:center;
  background:#f5f5f5;
  font-size:20px;
}

/* =========================
   予約表
========================= */

.wrap{
  padding:16px 10px 50px;
}

.table-wrap{
  overflow:auto;
  border:1px solid #ddd;
  border-radius:16px;
}

.schedule{
  border-collapse:collapse;
  width:max-content;
  min-width:100%;
}

.schedule th,
.schedule td{
  border:1px solid #ddd;
}

.schedule th{
  height:70px;
  background:#fafafa;
  font-size:18px;
}

.time-head{
  width:105px;
  min-width:105px;
  position:sticky;
  left:0;
  z-index:5;
}

.girl-head{
  min-width:310px;
}

.time{
  width:105px;
  min-width:105px;
  text-align:center;
  background:#fafafa;
  color:#666;
  position:sticky;
  left:0;
  z-index:4;
}

.slot{
  height:110px;
  padding:7px;
}

/* 予約中 */

.res-card{
  width:100%;
  height:100%;
  border:0;
  border-radius:11px;
  background:#dff1ff;
  color:#0785e8;
  text-align:left;
  padding:10px;
}

.res-card strong{
  display:block;
  font-size:18px;
}

.res-card span{
  display:block;
  margin-top:5px;
  font-size:15px;
}

.res-card small{
  display:block;
  margin-top:5px;
  color:#666;
}

/* 予約可能 */

.add-card{
  width:100%;
  height:100%;
  border:2px dashed #ccc;
  border-radius:11px;
  background:#fff;
  color:#777;
  font-size:19px;
}

/* 予約不可 */

.unavailable{
  width:100%;
  height:100%;
  border-radius:11px;
  display:flex;
  align-items:center;
  justify-content:center;
  background:#eee;
  color:#aaa;
  font-size:16px;
}

/* =========================
   モーダル
========================= */

.modal-back{
  position:fixed;
  inset:0;
  z-index:50;
  background:rgba(0,0,0,.5);
  display:none;
  align-items:flex-end;
}

.modal-back.show{
  display:flex;
}

.modal{
  background:#fff;
  width:min(100%,850px);
  max-height:92vh;
  overflow:auto;
  border-radius:25px 25px 0 0;
  padding:24px 18px 30px;
}

.modal h2{
  margin:0 0 20px;
  font-size:27px;
}

.field{
  margin-bottom:17px;
}

.field label{
  display:block;
  margin-bottom:7px;
  color:#555;
}

.field input,
.field select{
  width:100%;
  height:54px;
  border:1px solid #ccc;
  border-radius:11px;
  padding:0 12px;
  background:#fff;
}

.modal-actions{
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:12px;
  margin-top:20px;
}

.btn{
  height:54px;
  border:0;
  border-radius:11px;
  background:#eee;
  color:#0785e8;
}

.btn.black{
  background:#222;
  color:#fff;
}

.error{
  display:none;
  background:#fff0f0;
  color:#c00;
  border-radius:10px;
  padding:11px;
  margin-bottom:14px;
}

.error.show{
  display:block;
}

.info{
  background:#f5f5f5;
  border-radius:10px;
  padding:11px;
  color:#555;
  margin-bottom:14px;
}

/* =========================
   設定
========================= */

.settings-list{
  display:flex;
  flex-direction:column;
  gap:9px;
}

.setting-row{
  display:grid;
  grid-template-columns:1fr 75px;
  gap:8px;
}

.setting-row.course{
  grid-template-columns:1fr 100px 110px 75px;
}

.setting-row input{
  min-width:0;
  height:50px;
  border:1px solid #ccc;
  border-radius:10px;
  padding:0 10px;
}

.remove{
  height:50px;
  border:0;
  border-radius:10px;
  background:#eee;
  color:#0785e8;
}

.add-line{
  margin-top:10px;
  width:100%;
}

/* =========================
   集計
========================= */

.summary-item{
  border:1px solid #ddd;
  border-radius:10px;
  padding:12px;
  margin-bottom:9px;
}

.total{
  font-size:21px;
  font-weight:bold;
  margin-top:15px;
}

/* =========================
   スマホ
========================= */

@media(max-width:700px){

  .nav{
    grid-template-columns:48px 1fr 48px 78px 78px;
  }

  .nav .action{
    font-size:14px;
  }

  .girl-head{
    min-width:280px;
  }

  .setting-row.course{
    grid-template-columns:1fr 90px 100px;
  }

  .setting-row.course .remove{
    grid-column:1/-1;
  }
}
</style>
</head>

<body>

<!-- =========================
     上部
========================= -->

<div class="topbar">

  <div class="nav">

    <button id="prevDay">‹</button>

    <div id="dateBox" class="datebox"></div>

    <button id="nextDay">›</button>

    <button id="summaryBtn" class="action">
      集計
    </button>

    <button id="settingsBtn" class="action">
      設定
    </button>

  </div>

</div>


<!-- =========================
     予約表
========================= -->

<div class="wrap">

  <div class="table-wrap">

    <table class="schedule">

      <thead>
        <tr id="headRow"></tr>
      </thead>

      <tbody id="scheduleBody"></tbody>

    </table>

  </div>

</div>


<!-- =========================
     予約モーダル
========================= -->

<div id="reservationBack" class="modal-back">

  <div class="modal">

    <h2 id="reservationTitle">
      新しい予約
    </h2>

    <div id="reservationError" class="error"></div>


    <div class="field">

      <label>
        お客様
      </label>

      <input
        id="customerInput"
        placeholder="お客様名"
      >

    </div>


    <div class="field">

      <label>
        女の子
      </label>

      <select id="girlSelect"></select>

    </div>


    <div class="field">

      <label>
        開始時間（1分単位）
      </label>

      <input
        id="timeInput"
        type="time"
        step="60"
      >

    </div>


    <div class="field">

      <label>
        コース
      </label>

      <select id="courseSelect"></select>

    </div>


    <div
      id="reservationHint"
      class="info">
    </div>


    <div class="modal-actions">

      <button
        class="btn"
        id="cancelReservation">
        キャンセル
      </button>

      <button
        class="btn black"
        id="saveReservation">
        保存
      </button>

    </div>

  </div>

</div>


<!-- =========================
     設定モーダル
========================= -->

<div id="settingsBack" class="modal-back">

  <div class="modal">

    <h2>
      設定
    </h2>


    <div class="field">

      <label>
        女の子
      </label>

      <div
        id="girlsSettings"
        class="settings-list">
      </div>

      <button
        class="btn add-line"
        id="addGirl">
        ＋ 女の子を追加
      </button>

    </div>


    <div class="field">

      <label>
        コース（コース名・分数・料金）
      </label>

      <div
        id="coursesSettings"
        class="settings-list">
      </div>

      <button
        class="btn add-line"
        id="addCourse">
        ＋ コースを追加
      </button>

    </div>


    <div class="modal-actions">

      <button
        class="btn"
        id="cancelSettings">
        キャンセル
      </button>

      <button
        class="btn black"
        id="saveSettings">
        設定を保存
      </button>

    </div>

  </div>

</div>


<!-- =========================
     集計モーダル
========================= -->

<div id="summaryBack" class="modal-back">

  <div class="modal">

    <h2>
      集計
    </h2>

    <div id="summaryContent"></div>

    <div class="modal-actions">

      <button
        class="btn black"
        id="closeSummary">
        閉じる
      </button>

    </div>

  </div>

</div>


<script>

"use strict";


/*
============================================================
予約ルール
============================================================

★予約開始時間は「1分単位」

10:50
10:51
10:52
10:53
10:54
...

全部予約可能。


★予約終了後は10分空ける


例①

10:00〜10:40 予約

10:40 ❌
10:41 ❌
10:42 ❌
10:43 ❌
10:44 ❌
10:45 ❌
10:46 ❌
10:47 ❌
10:48 ❌
10:49 ❌

10:50 ⭕


例②

10:53から60分予約

10:53〜11:53

11:53 ❌
11:54 ❌
11:55 ❌
11:56 ❌
11:57 ❌
11:58 ❌
11:59 ❌
12:00 ❌
12:01 ❌
12:02 ❌

12:03 ⭕


★つまり

「終了時間 + 10分」

から次の予約が可能。


★予約表の表示は10分刻み。

ただし予約そのものの開始時間は1分単位。


============================================================
*/


const START_TIME = 10 * 60;

const END_TIME = 24 * 60;


/*
  ★ここが重要

  10分空ける
*/

const GAP_MINUTES = 10;


const DEFAULT_GIRLS = [
  "ことねさん",
  "ナヨンさん"
];


const DEFAULT_COURSES = [

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

];


const SETTINGS_KEY =
  "yoyaku_settings_final_v6";


const RES_PREFIX =
  "yoyaku_reservations_final_v6_";


let settings =
  loadSettings();


let currentDate =
  new Date();


let reservations = [];


let editingId =
  null;


/* =========================================================
   共通
========================================================= */

function pad(n){

  return String(n).padStart(2,"0");

}


function dateKey(d){

  return (
    d.getFullYear()
    + "-"
    + pad(d.getMonth()+1)
    + "-"
    + pad(d.getDate())
  );

}


function formatDate(d){

  return (
    d.getFullYear()
    + "/"
    + pad(d.getMonth()+1)
    + "/"
    + pad(d.getDate())
  );

}


function minutesToTime(m){

  return (
    pad(Math.floor(m/60))
    + ":"
    + pad(m%60)
  );

}


function timeToMinutes(v){

  const m =
    String(v || "")
      .match(/^(\d{1,2}):(\d{2})$/);

  return m
    ? Number(m[1])*60 + Number(m[2])
    : NaN;

}


function escapeHtml(v){

  return String(v ?? "")

    .replace(/&/g,"&amp;")

    .replace(/</g,"&lt;")

    .replace(/>/g,"&gt;")

    .replace(/"/g,"&quot;")

    .replace(/'/g,"&#039;");

}


function yen(n){

  return (
    "¥"
    + Number(n || 0)
      .toLocaleString("ja-JP")
  );

}


/* =========================================================
   設定
========================================================= */

function normalizeSettings(s){

  const girls =
    (Array.isArray(s?.girls)
      ? s.girls
      : DEFAULT_GIRLS)

      .map(String)
      .map(x => x.trim())
      .filter(Boolean);


  const courses =
    (Array.isArray(s?.courses)
      ? s.courses
      : DEFAULT_COURSES)

      .map(c => ({

        name:
          String(c.name ?? "").trim(),

        minutes:
          Number(c.minutes),

        price:
          Number(c.price)

      }))

      .filter(c =>
        c.name &&
        Number.isFinite(c.minutes) &&
        c.minutes > 0
      );


  return {

    girls:
      [...new Set(
        girls.length
          ? girls
          : DEFAULT_GIRLS
      )],

    courses:
      courses.length
        ? courses
        : structuredClone(DEFAULT_COURSES)

  };

}


function loadSettings(){

  try{

    const raw =
      localStorage.getItem(
        SETTINGS_KEY
      );

    return normalizeSettings(
      raw
        ? JSON.parse(raw)
        : {}
    );

  }catch(e){

    return normalizeSettings({});

  }

}


function saveSettingsStorage(){

  localStorage.setItem(
    SETTINGS_KEY,
    JSON.stringify(settings)
  );

}


/* =========================================================
   予約データ
========================================================= */

function loadReservations(){

  try{

    const raw =
      localStorage.getItem(
        RES_PREFIX + dateKey(currentDate)
      );

    reservations =
      raw
        ? JSON.parse(raw)
        : [];

    if(!Array.isArray(reservations)){

      reservations = [];

    }

  }catch(e){

    reservations = [];

  }

}


function saveReservations(){

  localStorage.setItem(

    RES_PREFIX + dateKey(currentDate),

    JSON.stringify(reservations)

  );

}


function getCourse(name){

  return settings.courses.find(
    c =>
      String(c.name)
      ===
      String(name)
  ) || null;

}


function getDuration(r){

  const c =
    getCourse(r.course);

  return c
    ? Number(c.minutes)
    : Number(r.minutes) || 0;

}


function getEnd(r){

  return (
    timeToMinutes(r.time)
    +
    getDuration(r)
  );

}


/* =========================================================
   ★予約可能判定
========================================================= */

function isAvailable(
  girl,
  start,
  duration,
  excludeId = null
){

  const newStart =
    Number(start);

  const newEnd =
    newStart + Number(duration);


  if(
    !Number.isFinite(newStart)
    ||
    !Number.isFinite(newEnd)
  ){

    return false;

  }


  if(
    newStart < START_TIME
    ||
    newEnd > END_TIME
  ){

    return false;

  }


  for(const r of reservations){

    if(
      String(r.girl)
      !==
      String(girl)
    ){

      continue;

    }


    /*
      編集中の自分自身は除外
    */

    if(
      excludeId !== null
      &&
      String(r.id)
      ===
      String(excludeId)
    ){

      continue;

    }


    const oldStart =
      timeToMinutes(r.time);


    const oldEnd =
      getEnd(r);


    /*
    ========================================================
    既存予約より後に入れる場合

    既存
    10:00〜10:40

    新規
    10:50 → OK

    10:49 → NG
    ========================================================
    */

    if(newStart >= oldStart){

      if(
        newStart
        <
        oldEnd + GAP_MINUTES
      ){

        return false;

      }

    }


    /*
    ========================================================
    既存予約より前に入れる場合

    既存
    12:00〜13:00

    新規60分

    10:50〜11:50 → OK
    11:00〜12:00 → NG

    「新規終了 + 10分」
    が既存開始以下ならOK
    ========================================================
    */

    if(newStart < oldStart){

      if(
        newEnd + GAP_MINUTES
        >
        oldStart
      ){

        return false;

      }

    }


    /*
    念のため重複もチェック
    */

    if(
      newStart < oldEnd
      &&
      newEnd > oldStart
    ){

      return false;

    }

  }


  return true;

}


/* =========================================================
   予約表の表示
========================================================= */

function coveringReservation(
  girl,
  t
){

  return reservations.find(r => {

    if(
      String(r.girl)
      !==
      String(girl)
    ){

      return false;

    }


    const start =
      timeToMinutes(r.time);


    const end =
      getEnd(r);


    return (
      t >= start
      &&
      t < end
    );

  }) || null;

}


function isGridAvailable(
  girl,
  t
){

  /*
    表示は10分刻み。

    その10分地点から
    どれかのコースが
    予約可能なら＋予約。
  */

  return settings.courses.some(
    course =>
      isAvailable(
        girl,
        t,
        course.minutes
      )
  );

}


/* =========================================================
   描画
========================================================= */

function render(){

  document.getElementById(
    "dateBox"
  ).textContent =
    formatDate(currentDate);


  loadReservations();


  document.getElementById(
    "headRow"
  ).innerHTML =

    '<th class="time-head">時間</th>'

    +

    settings.girls
      .map(g =>
        `<th class="girl-head">
          ${escapeHtml(g)}
        </th>`
      )
      .join("");


  const body =
    document.getElementById(
      "scheduleBody"
    );


  body.innerHTML = "";


  /*
    表示は10分刻み
  */

  for(
    let t = START_TIME;
    t < END_TIME;
    t += 10
  ){

    const tr =
      document.createElement("tr");


    const timeTd =
      document.createElement("td");

    timeTd.className =
      "time";

    timeTd.textContent =
      minutesToTime(t);

    tr.appendChild(timeTd);


    for(
      const girl
      of settings.girls
    ){

      const td =
        document.createElement("td");

      td.className =
        "slot";


      const r =
        coveringReservation(
          girl,
          t
        );


      /*
      予約中
      */

      if(r){

        const button =
          document.createElement(
            "button"
          );

        button.className =
          "res-card";


        button.innerHTML =

          `<strong>
            ${escapeHtml(r.time)}
            〜
            ${escapeHtml(
              minutesToTime(
                getEnd(r)
              )
            )}
          </strong>`

          +

          `<span>
            ${escapeHtml(
              r.customer || "お客様"
            )}
            /
            ${escapeHtml(r.course)}
          </span>`

          +

          `<small>
            タップで編集
          </small>`;


        button.onclick =
          () => openEdit(r.id);


        td.appendChild(button);

      }


      /*
      予約可能
      */

      else if(
        isGridAvailable(
          girl,
          t
        )
      ){

        const button =
          document.createElement(
            "button"
          );

        button.className =
          "add-card";

        button.textContent =
          "＋ 予約";


        button.onclick =
          () =>
            openNew(
              girl,
              t
            );


        td.appendChild(button);

      }


      /*
      予約不可
      */

      else{

        const div =
          document.createElement(
            "div"
          );

        div.className =
          "unavailable";

        div.textContent =
          "予約不可";


        td.appendChild(div);

      }


      tr.appendChild(td);

    }


    body.appendChild(tr);

  }

}


/* =========================================================
   予約モーダル
========================================================= */

function fillGirlSelect(){

  const select =
    document.getElementById(
      "girlSelect"
    );


  select.innerHTML =
    settings.girls

      .map(g =>
        `<option value="${escapeHtml(g)}">
          ${escapeHtml(g)}
        </option>`
      )

      .join("");

}


function fillCourseSelect(){

  const select =
    document.getElementById(
      "courseSelect"
    );


  select.innerHTML =
    settings.courses

      .map(c =>
        `<option value="${escapeHtml(c.name)}">
          ${escapeHtml(c.name)}
          （${c.minutes}分 /
          ${yen(c.price)}）
        </option>`
      )

      .join("");

}


function updateHint(){

  const girl =
    document.getElementById(
      "girlSelect"
    ).value;


  const time =
    document.getElementById(
      "timeInput"
    ).value;


  const course =
    getCourse(
      document.getElementById(
        "courseSelect"
      ).value
    );


  const hint =
    document.getElementById(
      "reservationHint"
    );


  if(
    !course
    ||
    !time
  ){

    hint.textContent =
      "";

    return;

  }


  const start =
    timeToMinutes(time);


  const end =
    start + course.minutes;


  const ok =
    isAvailable(
      girl,
      start,
      course.minutes,
      editingId
    );


  hint.textContent =

    `${time}〜${minutesToTime(end)}`
    +
    `　／　${course.minutes}分`
    +
    `　／　${yen(course.price)}`
    +
    (ok
      ? ""
      : "　⚠ この時間は予約できません");

}


function clearError(){

  const e =
    document.getElementById(
      "reservationError"
    );


  e.classList.remove("show");

  e.textContent = "";

}


function showError(message){

  const e =
    document.getElementById(
      "reservationError"
    );


  e.textContent =
    message;


  e.classList.add("show");

}


function openNew(
  girl,
  time
){

  editingId =
    null;


  clearError();


  document.getElementById(
    "reservationTitle"
  ).textContent =
    "新しい予約";


  document.getElementById(
    "customerInput"
  ).value =
    "";


  fillGirlSelect();

  fillCourseSelect();


  document.getElementById(
    "girlSelect"
  ).value =
    girl;


  document.getElementById(
    "timeInput"
  ).value =
    minutesToTime(time);


  document.getElementById(
    "reservationBack"
  ).classList.add("show");


  updateHint();

}


function openEdit(id){

  const r =
    reservations.find(
      x =>
        String(x.id)
        ===
        String(id)
    );


  if(!r){

    return;

  }


  editingId =
    r.id;


  clearError();


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
    "girlSelect"
  ).value =
    r.girl;


  document.getElementById(
    "timeInput"
  ).value =
    r.time;


  document.getElementById(
    "courseSelect"
  ).value =
    r.course;


  document.getElementById(
    "reservationBack"
  ).classList.add("show");


  updateHint();

}


function closeReservation(){

  document.getElementById(
    "reservationBack"
  ).classList.remove("show");


  editingId =
    null;

}


function saveReservation(){

  clearError();


  const customer =
    document.getElementById(
      "customerInput"
    ).value.trim()
    ||
    "お客様";


  const girl =
    document.getElementById(
      "girlSelect"
    ).value;


  const time =
    document.getElementById(
      "timeInput"
    ).value;


  const courseName =
    document.getElementById(
      "courseSelect"
    ).value;


  const course =
    getCourse(courseName);


  const start =
    timeToMinutes(time);


  if(!course){

    showError(
      "コースを選択してください。"
    );

    return;

  }


  if(!Number.isFinite(start)){

    showError(
      "開始時間を入力してください。"
    );

    return;

  }


  if(
    start < START_TIME
    ||
    start + course.minutes
      > END_TIME
  ){

    showError(
      "営業時間内に収まる時間を選択してください。"
    );

    return;

  }


  /*
    ★最重要

    ここで
    「終了後10分」
    を判定。
  */

  if(
    !isAvailable(
      girl,
      start,
      course.minutes,
      editingId
    )
  ){

    showError(

      "この時間は予約できません。"
      +
      "既存予約の終了時刻から10分後以降、"
      +
      "かつ他の予約と重ならない時間を選択してください。"

    );

    return;

  }


  const item = {

    id:
      editingId
      ||
      (
        Date.now()
        +
        "_"
        +
        Math.random()
          .toString(36)
          .slice(2)
      ),

    customer,

    girl,

    time,

    course:
      course.name

  };


  if(editingId){

    const index =
      reservations.findIndex(
        x =>
          String(x.id)
          ===
          String(editingId)
      );


    if(index >= 0){

      reservations[index] =
        item;

    }

  }

  else{

    reservations.push(item);

  }


  reservations.sort(
    (a,b) =>
      timeToMinutes(a.time)
      -
      timeToMinutes(b.time)
  );


  saveReservations();

  closeReservation();

  render();

}


/* =========================================================
   設定
========================================================= */

function openSettings(){

  renderSettings();

  document.getElementById(
    "settingsBack"
  ).classList.add("show");

}


function closeSettings(){

  document.getElementById(
    "settingsBack"
  ).classList.remove("show");

}


function renderSettings(){

  const girls =
    document.getElementById(
      "girlsSettings"
    );


  girls.innerHTML = "";


  settings.girls.forEach(
    (name,index) => {

      const row =
        document.createElement(
          "div"
        );


      row.className =
        "setting-row";


      row.innerHTML =

        `<input
          data-girl="${index}"
          value="${escapeHtml(name)}"
        >`

        +

        `<button
          class="remove"
          data-rmgirl="${index}">
          削除
        </button>`;


      girls.appendChild(row);

    }
  );


  girls
    .querySelectorAll(
      "[data-rmgirl]"
    )
    .forEach(button => {

      button.onclick = () => {

        if(
          settings.girls.length <= 1
        ){

          alert(
            "女の子は最低1人必要です。"
          );

          return;

        }


        settings.girls.splice(
          Number(
            button.dataset.rmgirl
          ),
          1
        );


        renderSettings();

      };

    });


  const courses =
    document.getElementById(
      "coursesSettings"
    );


  courses.innerHTML = "";


  settings.courses.forEach(
    (course,index) => {

      const row =
        document.createElement(
          "div"
        );


      row.className =
        "setting-row course";


      row.innerHTML =

        `<input
          data-cname="${index}"
          value="${escapeHtml(course.name)}"
          placeholder="コース名"
        >`

        +

        `<input
          data-cmin="${index}"
          type="number"
          min="1"
          value="${course.minutes}"
          placeholder="分"
        >`

        +

        `<input
          data-cprice="${index}"
          type="number"
          min="0"
          value="${course.price}"
          placeholder="料金"
        >`

        +

        `<button
          class="remove"
          data-rmcourse="${index}">
          削除
        </button>`;


      courses.appendChild(row);

    }
  );


  courses
    .querySelectorAll(
      "[data-rmcourse]"
    )
    .forEach(button => {

      button.onclick = () => {

        if(
          settings.courses.length <= 1
        ){

          alert(
            "コースは最低1つ必要です。"
          );

          return;

        }


        settings.courses.splice(
          Number(
            button.dataset.rmcourse
          ),
          1
        );


        renderSettings();

      };

    });

}


function saveSettings(){

  const girls =
    [...document.querySelectorAll(
      "[data-girl]"
    )]

      .map(
        x =>
          x.value.trim()
      )

      .filter(Boolean);


  const courses =
    settings.courses.map(
      (_,index) => ({

        name:
          document.querySelector(
            `[data-cname="${index}"]`
          ).value.trim(),

        minutes:
          Number(
            document.querySelector(
              `[data-cmin="${index}"]`
            ).value
          ),

        price:
          Number(
            document.querySelector(
              `[data-cprice="${index}"]`
            ).value
          )

      })
    );


  if(!girls.length){

    alert(
      "女の子を1人以上設定してください。"
    );

    return;

  }


  if(
    courses.some(
      c =>
        !c.name
        ||
        !Number.isFinite(c.minutes)
        ||
        c.minutes <= 0
    )
  ){

    alert(
      "コース名と分数を正しく入力してください。"
    );

    return;

  }


  settings =
    normalizeSettings({
      girls,
      courses
    });


  saveSettingsStorage();

  closeSettings();

  render();

}


/* =========================================================
   集計
========================================================= */

function openSummary(){

  loadReservations();


  const totals = {};

  let total = 0;


  for(
    const r
    of reservations
  ){

    const course =
      getCourse(r.course);


    const price =
      course
        ? course.price
        : 0;


    total += price;


    if(!totals[r.girl]){

      totals[r.girl] = {
        count:0,
        total:0
      };

    }


    totals[r.girl].count++;

    totals[r.girl].total +=
      price;

  }


  let html = "";


  for(
    const girl
    of settings.girls
  ){

    const x =
      totals[girl]
      ||
      {
        count:0,
        total:0
      };


    html +=

      `<div class="summary-item">

        <strong>
          ${escapeHtml(girl)}
        </strong>

        <br>

        ${x.count}件
       　
        ${yen(x.total)}

      </div>`;

  }


  html +=

    `<div class="total">

      合計
      ${reservations.length}件
      ／
      ${yen(total)}

    </div>`;


  document.getElementById(
    "summaryContent"
  ).innerHTML =
    html;


  document.getElementById(
    "summaryBack"
  ).classList.add("show");

}


/* =========================================================
   ボタン
========================================================= */

document.getElementById(
  "prevDay"
).onclick = () => {

  currentDate.setDate(
    currentDate.getDate() - 1
  );

  render();

};


document.getElementById(
  "nextDay"
).onclick = () => {

  currentDate.setDate(
    currentDate.getDate() + 1
  );

  render();

};


document.getElementById(
  "settingsBtn"
).onclick =
  openSettings;


document.getElementById(
  "summaryBtn"
).onclick =
  openSummary;


document.getElementById(
  "cancelReservation"
).onclick =
  closeReservation;


document.getElementById(
  "saveReservation"
).onclick =
  saveReservation;


document.getElementById(
  "girlSelect"
).onchange =
  updateHint;


document.getElementById(
  "timeInput"
).oninput =
  updateHint;


document.getElementById(
  "courseSelect"
).onchange =
  updateHint;


document.getElementById(
  "cancelSettings"
).onclick =
  closeSettings;


document.getElementById(
  "closeSummary"
).onclick = () => {

  document.getElementById(
    "summaryBack"
  ).classList.remove("show");

};


document.getElementById(
  "addGirl"
).onclick = () => {

  settings.girls.push(
    "新しい女の子"
  );

  renderSettings();

};


document.getElementById(
  "addCourse"
).onclick = () => {

  settings.courses.push({

    name:"新しいコース",

    minutes:40,

    price:10000

  });

  renderSettings();

};


/* =========================================================
   モーダル外側クリック
========================================================= */

for(
  const id
  of [
    "reservationBack",
    "settingsBack",
    "summaryBack"
  ]
){

  document.getElementById(
    id
  ).onclick = e => {

    if(
      e.target !==
      e.currentTarget
    ){

      return;

    }


    if(
      id ===
      "reservationBack"
    ){

      closeReservation();

    }


    if(
      id ===
      "settingsBack"
    ){

      closeSettings();

    }


    if(
      id ===
      "summaryBack"
    ){

      document.getElementById(
        "summaryBack"
      ).classList.remove(
        "show"
      );

    }

  };

}


/* =========================================================
   初期化
========================================================= */

saveSettingsStorage();

render();

</script>

</body>
</html>
