ז
<img width="646" height="711" alt="image" src="https://github.com/user-attachments/assets/1d016fa3-17e8-402c-82cb-fcd9eafe81a9" />

.

---

## 🧠 המוח מאחורי הנגן: ניתוח קוד ה-JavaScript

הנגן מתבסס על אירועים (Events) ועל מניפולציה של ה-DOM (ממשק התצוגה של הדפדפן).

### 1. ניהול נתונים ומערכים (The Playlist)

בתחילת הדרך, עלינו להגדיר לנגן אילו שירים קיימים. אנחנו משתמשים במערך (Array) ובמשתנה אינדקס.

```javascript
const songs = ["hey", "summer", "ukulele"];
let songIndex = 0; // מצביע על המיקום הנוכחי במערך

```

* **מה לומדים:** מערך הוא רשימה מסודרת. האינדקס מתחיל תמיד מ-`0`, ולכן השיר הראשון הוא במקום ה-`0` והשני במקום ה-`1`.

---

### 2. פונקציית הטעינה הדינמית (`loadSong`)

זוהי אחת הפונקציות החשובות ביותר. היא מחברת בין המידע ב-JS לבין מה שהמשתמש רואה ושומע.

```javascript
function loadSong(song) {
  title.innerText = song; // משנה את כותרת השיר המוצגת
  audio.src = `music/${song}.mp3`; // מעדכן את נתיב קובץ השמע
  cover.src = `images/${song}.jpg`; // מעדכן את תמונת העטיפה
}

```

* **מה לומדים:** שימוש ב-**Template Literals** (סימני ה-Backtick ```). זה מאפשר לנו להזריק משתנים ישירות לתוך מחרוזת טקסט, מה שהופך את הקוד לדינמי וחסכוני.

---

### 3. לוגיקת הנגינה והאנימציה (`playSong` & `pauseSong`)

כאן אנחנו שולטים ב-State (מצב) של הנגן. אנחנו לא רק מפעילים סאונד, אלא גם מעדכנים את העיצוב.

```javascript
function playSong() {
  musicContainer.classList.add("play"); // מוסיף קלאס שמפעיל את סיבוב התמונה ב-CSS
  playBtn.querySelector("i").className = "fas fa-pause"; // מחליף את האייקון ל-Pause
  audio.play(); // פקודה מובנית ב-JS להפעלת שמע
}

```

* **מה לומדים:** ניהול **Classes**. ב-JavaScript מודרני, אנחנו מעדיפים להוסיף או להסיר קלאסים (`classList.add/remove`) במקום לשנות עיצוב ישירות ב-JS. זה שומר על הפרדה נכונה בין העיצוב (CSS) ללוגיקה.

---

### 4. שליטה בזמן ובפס התקדמות (`ontimeupdate`)

פס ההתקדמות חייב לזוז בזמן שהשיר מתנגן. האובייקט `audio` שולח אירוע "עדכון זמן" בכל כמה מילישניות.

```javascript
audio.ontimeupdate = () => {
  const percent = (audio.currentTime / audio.duration) * 100;
  progress.style.width = `${percent}%`; // מעדכן את רוחב הפס הוורוד
};

```

* **מה לומדים:**
* `audio.currentTime`: הזמן הנוכחי בשניות.
* `audio.duration`: אורך השיר הכולל בשניות.
* **מתמטיקה פשוטה:** חלוקת הזמן שעבר בזמן הכולל והכפלה ב-100 נותנת לנו את האחוז המדויק להצגה גרפית.



---

### 5. שליטה בעוצמת הקול (Volume API)

הוספנו סליידר שמאפשר לשנות את עוצמת השמע. זהו חלק מה-Audio API של HTML5.

```javascript
volSlider.oninput = (e) => {
  audio.volume = e.target.value; // מקבל ערך בין 0 ל-1
  volPercent.innerText = `${Math.round(e.target.value * 100)}%`;
};

```

* **מה לומדים:** האירוע `oninput` מזהה כל תזוזה של הסליידר (בניגוד ל-`onchange` שקורה רק כשעוזבים את העכבר). המשתנה `e.target.value` מושך את הערך הנוכחי של הסליידר.

---

### 6. ניווט בין שירים ושימוש במודולו (`%`)

כדי לעבור לשיר הבא או הקודם בצורה מעגלית (כך שאחרי השיר האחרון נחזור לראשון), אנחנו משתמשים באופרטור השארית.

```javascript
nextBtn.onclick = () => {
  songIndex = (songIndex + 1) % songs.length;
  loadSong(songs[songIndex]);
  playSong();
};

```

* **מה לומדים (הטריק של המודולו):** אם יש לנו 3 שירים (`length = 3`):
* באינדקס 2 (השיר האחרון), הפעולה תהיה `(2+1) % 3`.
* התוצאה היא `0` – וזה מחזיר אותנו בדיוק לשיר הראשון!



---

### 7. בניית הפלייליסט הדינמי

במקום לכתוב את רשימת השירים ב-HTML, אנחנו מייצרים אותה דרך ה-JavaScript.

```javascript
songs.forEach((song, index) => {
  const li = document.createElement("li"); // יוצר אלמנט רשימה חדש
  li.innerText = song;
  li.onclick = () => { // מוסיף אירוע לחיצה לכל שיר ברשימה
    songIndex = index;
    loadSong(songs[songIndex]);
    playSong();
  };
  songListUl.appendChild(li); // מזריק את השיר לתוך ה-HTML
});

```

* **מה לומדים:** `createElement` ו-`appendChild` הן השיטות המרכזיות לבניית אתרים דינמיים שבהם התוכן משתנה לפי נתונים (כמו פלייליסט שמתעדכן או רשימת מוצרים בחנות).

---

## 🎨 לסיכום: מה הופך את הנגן ל"חכם"?

השילוב של **האזנה לאירועים** (כמו `onclick` או `onended`) יחד עם **מניפולציה של ה-DOM** (שינוי `src` ו-`innerText`) הוא הבסיס לכל אפליקציית אינטרנט מודרנית.

**השלב הבא עבורך:**
האם תרצה לנסות להוסיף פונקציה שתשמור את השיר האחרון שנוגן כך שגם אם תרענן את הדף, הנגן יזכור איפה עצרת? (זה נעשה באמצעות `localStorage`).
