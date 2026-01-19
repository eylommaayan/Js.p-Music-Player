🎵 Music Player – נגן מוזיקה ב־HTML, CSS ו־JavaScript

<img width="594" height="414" alt="image" src="https://github.com/user-attachments/assets/a6cd338c-5c4b-4303-b15d-1254e250303a" />


פרויקט זה מציג נגן מוזיקה מודרני ויפה המבוסס על HTML5 Audio API.
הנגן מנגן קבצי מוזיקה מתוך תיקיית music, כולל ממשק אינטראקטיבי, אנימציות וניהול זמן ניגון.

הפרויקט מתאים ללמידה, לתרגול JavaScript, ולהטמעה ככרטיס (Card) בתוך אתר קיים.

✨ תכונות עיקריות

🎧 ניגון מוזיקה באמצעות HTML5 Audio API

▶️ / ⏸️ כפתורי נגן והשהיה (Play / Pause)

⏭️ / ⏮️ מעבר לשיר הבא והקודם

💿 תמונת עטיפה מסתובבת בזמן ניגון

📊 פס התקדמות שמתעדכן בזמן אמת

🖱️ לחיצה על פס ההתקדמות לקפיצה לנקודה בשיר

🔁 מעבר אוטומטי לשיר הבא בסיום השיר

📱 עיצוב רספונסיבי – מותאם גם למסכים קטנים

🧱 עטיפה בכרטיס (Card) – לא משפיע על עיצוב האתר שמארח אותו

🗂️ מבנה הפרויקט
.
├── index.html
├── style.css
├── script.js
├── music/
│   ├── hey.mp3
│   ├── summer.mp3
│   └── ukulele.mp3
└── images/
    ├── hey.jpg
    ├── summer.jpg
    └── ukulele.jpg


📌 חשוב:
שם קובץ המוזיקה ושם תמונת העטיפה חייבים להיות זהים (למשל: ukulele.mp3 ו־ukulele.jpg).

🚀 איך מריצים את הפרויקט

לשכפל את הריפוזיטורי:

git clone https://github.com/your-username/music-player.git


להיכנס לתיקיית הפרויקט:

cd music-player


לפתוח את הקובץ index.html בדפדפן
(או דרך Live Server ב־VS Code)

🛠️ טכנולוגיות בשימוש

HTML5 – מבנה הנגן

CSS3 – עיצוב, אנימציות ו־Responsive

JavaScript (Vanilla) – לוגיקת הנגן

HTML5 Audio API

Font Awesome – אייקונים

🎓 מה לומדים מהפרויקט

עבודה עם audio ב־JavaScript

האזנה לאירועים (timeupdate, ended)

שליטה בזמן ניגון (currentTime, duration)

ניהול state פשוט (נגן פועל / מושהה)

חיבור בין JavaScript ל־CSS (אנימציה לפי מצב)
להלן **מדריך לימודי ל־GitHub (README)** שממש מתמקד **במה שיש בקוד ה-JS** של הנגן: מה כל חלק עושה, אילו אירועים קיימים, ואיך להוסיף שירים.

---

# 🎵 Music Player Card — JavaScript Guide (README)

## 📌 מה הפרויקט עושה (ב־JS)

הפרויקט בונה **נגן מוזיקה בדפדפן** בעזרת `HTMLAudioElement` (תגית `<audio>`), ומוסיף:

* ▶️ Play / ⏸ Pause
* ⏮ Prev / ⏭ Next
* 📊 פס התקדמות (Progress Bar) + קפיצה בזמן בלחיצה
* ⏱ זמן נוכחי / זמן כולל
* 🔊 שליטת Volume (Slider)
* 📃 Playlist (לחיצה על שיר מהרשימה)
* 🌙 Dark Mode (החלפת מצב תצוגה)

---

## 🧠 מבנה ה־JS — לפי שלבים

### 1) תפיסת אלמנטים מה־DOM

בתחילת הקובץ לוקחים את האלמנטים שנצטרך לשליטה:

```js
const musicContainer = document.getElementById("music-container");
const playBtn = document.getElementById("play");
const prevBtn = document.getElementById("prev");
const nextBtn = document.getElementById("next");
const audio = document.getElementById("audio");
const progress = document.getElementById("progress");
const progressContainer = document.getElementById("progress-container");
const title = document.getElementById("title");
const cover = document.getElementById("cover");
```

📌 רעיון מרכזי:
כל פעולה בנגן (כפתור, פס התקדמות, כותרת שיר וכו’) = אלמנט בדף שאנחנו שולטים בו דרך JS.

---

### 2) רשימת השירים + אינדקס

יש מערך שמכיל את שמות השירים:

```js
const songs = ["hey", "summer", "ukulele"];
let songIndex = 2;
```

🔍 איך זה עובד?

* `songs` הוא *הפלייליסט*.
* `songIndex` מצביע על השיר הנוכחי.
* כשלוחצים Next/Prev – משנים את `songIndex`.

---

### 3) טעינת שיר — `loadSong(song)`

הפונקציה טוענת שיר + תמונת קאבר:

```js
function loadSong(song) {
  title.innerText = song;
  audio.src = `music/${song}.mp3`;
  cover.src = `images/${song}.jpg`;
}
```

📌 חובה לשמור על מבנה התיקיות:

* `music/NAME.mp3`
* `images/NAME.jpg`

---

### 4) הפעלה / עצירה (Play / Pause)

שתי פונקציות עיקריות:

```js
function playSong() {
  musicContainer.classList.add("play");
  playBtn.querySelector("i").classList.replace("fa-play", "fa-pause");
  audio.play();
}

function pauseSong() {
  musicContainer.classList.remove("play");
  playBtn.querySelector("i").classList.replace("fa-pause", "fa-play");
  audio.pause();
}
```

מה קורה פה?

* מוסיפים/מורידים class בשם `"play"` (עוזר גם ל־CSS: סיבוב תמונה + פתיחת info bar)
* מחליפים אייקון FontAwesome
* מפעילים/עוצרים את האודיו

---

### 5) אירועים לכפתורים

#### ▶️ Play/Pause Toggle

```js
playBtn.onclick = () =>
  musicContainer.classList.contains("play") ? pauseSong() : playSong();
```

#### ⏮ Prev

```js
prevBtn.onclick = () => {
  songIndex = (songIndex - 1 + songs.length) % songs.length;
  loadSong(songs[songIndex]);
  playSong();
};
```

#### ⏭ Next

```js
nextBtn.onclick = () => {
  songIndex = (songIndex + 1) % songs.length;
  loadSong(songs[songIndex]);
  playSong();
};
```

📌 למה עושים `% songs.length`?
כדי ש־Prev מהשיר הראשון יקפוץ לאחרון, ו־Next מהאחרון יחזור לראשון.

---

### 6) פס התקדמות — `timeupdate`

האירוע `ontimeupdate` קורה בזמן ניגון, כל כמה מילישניות:

```js
audio.ontimeupdate = () => {
  if (!audio.duration) return;
  progress.style.width = `${(audio.currentTime / audio.duration) * 100}%`;
};
```

🧠 הנוסחה:
`currentTime / duration * 100` = כמה אחוז מהשיר עבר.

---

### 7) קפיצה בזמן בלחיצה על פס ההתקדמות

```js
progressContainer.onclick = (e) => {
  audio.currentTime =
    (e.offsetX / progressContainer.clientWidth) * audio.duration;
};
```

מה זה אומר?

* `offsetX` = איפה לחצת בתוך הפס
* מחלקים ברוחב הפס כדי לקבל אחוז
* מכפילים במשך השיר (`audio.duration`) כדי לקבל זמן חדש

---

### 8) מעבר אוטומטי לשיר הבא בסוף

```js
audio.onended = () => nextBtn.click();
```

כששיר נגמר → מפעילים את פעולת Next.

---

## 🔊 Volume — שליטה בעוצמה

האודיו תומך ב־`audio.volume` בין `0` ל־`1`.

דוגמה:

```js
audio.volume = 0.8; // 80%
```

אם יש לך slider:

```js
volumeEl.oninput = (e) => {
  audio.volume = Number(e.target.value);
};
```

---

## ⏱ זמן נוכחי / זמן כולל

שני ערכים חשובים:

* `audio.currentTime` — הזמן הנוכחי
* `audio.duration` — הזמן הכולל

מעדכנים בתצוגה בזמן ניגון:

```js
currentTimeEl.textContent = formatTime(audio.currentTime);
durationEl.textContent = formatTime(audio.duration);
```

---

## 📃 Playlist — בניית רשימת שירים ב־JS

בונים `<li>` דינמי לכל שיר:

```js
songs.forEach((song, idx) => {
  const li = document.createElement("li");
  li.onclick = () => {
    songIndex = idx;
    loadSong(songs[songIndex]);
    playSong();
  };
});
```

✅ לחיצה על שיר → טוענת אותו ומנגנת.

---

## 🌙 Dark Mode — החלפת מצב תצוגה

משנים class על הכרטיס:

```js
darkToggle.onclick = () => {
  musicCard.classList.toggle("dark");
};
```

ה־CSS כבר דואג לשינוי הצבעים.

---

## ➕ איך מוסיפים שיר חדש

1. להוסיף קובץ:

* `music/newSong.mp3`
* `images/newSong.jpg`

2. להוסיף אותו למערך:

```js
const songs = ["hey", "summer", "ukulele", "newSong"];
```

זהו ✅ הפלייליסט יתעדכן.

---

## 🧪 תרגילים לתרגול (ממש מומלץ)

1. הוסף כפתור 🔁 Repeat (שיר חוזר)
2. הוסף Shuffle (רנדומלי)
3. שמור Dark Mode ו־Volume ב־`localStorage`
4. הוסף קיצורי מקלדת (רווח Play/Pause)

---


📄 רישיון

הפרויקט פתוח לשימוש לימודי ופרטי.
אפשר לשפר, להרחיב וללמוד ממנו בחופשיות.

