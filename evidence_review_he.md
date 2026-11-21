<!-- RTL rendering: Hebrew narrative and evidence table; LTR bibliography. Markdown inside the wrappers remains active in CommonMark/GFM. -->
<div dir="rtl" lang="he">

# ‏סקירת ראיות ופתרונות קיימים

‏**פרויקט:** זיהוי תמונות וסרטונים שנוצרו או שונו באמצעות AI  
‏**מסמך תכנון מחקרי:** נובמבר 2026 | **היקף:** תמונות וסרטונים, כולל עריכה מקומית ב-AI; לא זיהוי טקסט  
‏**סוג מסמך:** Evidence Review and Alternatives | **מצב:** החלטות תכנון ראשוניות, טרם ביצוע ניסויי הפרויקט

## ‏1. מטרה, היקף ושאלת ההחלטה

‏מטרת הפרויקט היא לבנות מערכת שמסייעת למשתמש להעריך אם תמונה או סרטון מציגים סימנים ליצירה **או לשינוי** באמצעות בינה מלאכותית. המערכת מיועדת בעיקר לבדיקת תוכן חשוד ברשתות חברתיות, אך אינה קובעת אם אירוע אכן התרחש או אם טענה המופיעה לצד התוכן נכונה.

‏**היקף הזיהוי כולל שלושה תרחישים שונים:** (1) תמונה או פריימים שנוצרו במלואם ב-AI (*AI-Generated*); (2) צילום אמיתי שעבר **עריכה מקומית** ב-AI, למשל הוספה, מחיקה או החלפה של אובייקט באמצעות *Inpainting* (*AI-Modified*); (3) שינוי פנים או וידאו מסוג *Deepfake*. אין להניח שמודל שמזהה תרחיש אחד יצליח באחרים [1, 3, 12, 13].

‏**שאלת ההחלטה המרכזית:** כיצד לתכנן פתרון מעשי, נגיש וזהיר שמספק תוצאה שימושית, בודק גם עריכות מקומיות ומאפשר למדוד תרומה טכנולוגית מעבר לשימוש במודל יחיד או בכלי בדיקה קיים?

## ‏2. שיטת החיפוש ובחירת המקורות

‏הסקירה ממוקדת בחמישה סוגי ראיות: (א) מחקר על זיהוי תמונות, הכללה ועמידות; (ב) מחקר על זיהוי ו**מיקום** עריכות מקומיות ב-AI; (ג) מאמרים ומאגרי נתונים על מניפולציות בווידאו; (ד) פתרונות קיימים לצרכנים ולחוקרים; (ה) תקנים ותיעוד רשמי. נבחרו מקורות ראשוניים: פרסומים מכנסים ומכתבי עת, אתרי פרויקטים אקדמיים, דפי מוצר רשמיים ומפרטים טכניים.

‏מילות החיפוש כללו `AI-generated image detection`, `unseen generators`, `image manipulation localization`, `diffusion inpainting detection`, `text-guided inpainting benchmark`, `deepfake benchmark`, `in-the-wild detection`, `video frame sampling`, `AI detector browser extension` ו-`content provenance`. לכל מקור נבדקו הרלוונטיות למשתמשים, סוג הקלט, סוג הפלט (ציון לתמונה לעומת מסכת פיקסלים), המגבלות, זמינות המימוש והשלכה על ההחלטה. הצהרות ביצועים שיווקיות אינן ראיה להצלחה בניסוי עצמאי. מועד בדיקת הקישורים והפרטים הביבליוגרפיים: **21 בנובמבר 2026**. קישורי ה-PDF של NeurIPS [2, 4], Springer [13] ו-CVF [1, 3] אומתו בעזרת דפי הפרסום הרשמיים או קובץ המאמר. את קישורי ה-PDF של CVF [5, 12] התאמתנו למבנה הקישורים הרשמי ולרישומי הפרסום, אך פתיחה אוטומטית ישירה הוחזרה בחסימת 403; נוספו עבורם קובצי PDF חלופיים. דף המודלים האינטראקטיבי [9] אומת גם באמצעות פרסום של האוניברסיטה.

## ‏3. ממצאים לפי נושאים

### ‏3.1 הכללה למודלים חדשים ואיכות המדיה

Ojha, Li ו-Lee [1] בחנו הכללה לגנרטורים שלא נראו באימון. הם השתמשו בתכונות שהופקו מ-`CLIP ViT-L/14` מאומן מראש, ללא אימון מחדש של הייצוג החזותי לזיהוי זיופים, והשוו `nearest neighbor` ל-`linear probing`. במסגרת ההשוואה נבחנו גם ייצוגי `CLIP ResNet-50`, `ImageNet ResNet-50` ו-`ImageNet ViT-B/16`. התוצאה תומכת בבדיקת ייצוגים שונים ובבחינה על גנרטורים שלא נראו, ולא בבחירת מודל לפי דיוק מדווח בלבד. GenImage [2] מוסיף משימות מדידה של `cross-generator` ושל מדיה פגומה: תמונות מטושטשות, דחוסות או ברזולוציה נמוכה. **מסקנה:** נבחר גלאים ונבדוק אותם על מקורות שלא שימשו לכיול ובתנאי עיבוד הדומים לשיתוף ברשתות חברתיות.

### ‏3.2 מניפולציות פנים והוגנות בהערכת וידאו

FaceForensics++ [3] כולל 1,000 סרטוני מקור וארבע שיטות מניפולציה: `DeepFakes`, `Face2Face`, `FaceSwap` ו-`NeuralTextures`. מאגר ההורדה מספק רמות דחיסה `raw/c0` (ללא דחיסה מאבדת), `c23` (דחיסת H.264 קלה) ו-`c40` (דחיסה חזקה), וכן מסכות לזיהוי אזורים ערוכים. DeepfakeBench [4] מאגד תשתית השוואה של 15 שיטות על 9 מאגרים, עם צינור קלט, עיבוד ומדדים אחידים. מאגר Deepfake-Eval-2024 [5] מכיל תוכן שהופץ במהלך 2024; **המאמר פורסם ב-CVPR Workshops 2026** (גרסת arXiv מוקדמת: 2025). המחקר מדגיש את פער ההכללה בין ניסויי מעבדה לתוכן בשטח. לכן נבחן תמונות וסרטונים בנפרד, ונתייחס לסיווג פריימים כבסיס בלבד: הוא אינו מבטיח איתור כל שינוי זמני, תנועתי או פנים.

### ‏3.3 זיהוי עריכות מקומיות: AI-Modified / Inpainting

‏**TruFor** [12] מציע שיטת *Image Forgery Detection and Localization* המשלבת מידע חזותי ברמת RGB עם `Noiseprint++` — ייצוג רגיש לעקבות רעש ועריכה — באמצעות מנגנון מיזוג מבוסס Transformer. בניגוד למסווג של תמונה שלמה, המערכת מחזירה **מפת מיקום ברמת פיקסל**, **מפת אמינות** וציון שלמות לתמונה. המחקר מציג מסגרת כללית למניפולציות, **אך אין להסיק ממנו שכל עריכת Diffusion חדשה תזוהה היטב**.

‏**TGIF2** [13], מאמר מחקר מ-2026 על *Text-Guided Inpainting*, בוחן במיוחד עריכת אזורים בתמונות באמצעות `Stable Diffusion 2`, `SDXL` ו-`FLUX.1`. הוא מבחין בין **Spliced** — אזור שנוצר ומשולב בתמונה המקורית — לבין **Fully Regenerated** — תמונה שעוברת תהליך יצירה מחדש אף שהעריכה מכוונת לאזור מסוים; ובין מסכות סמנטיות למסכות אקראיות. במחקר דווח כי גלאי מיקום, ובהם TruFor, מתקשים במקרים של Fully Regenerated ובהכללה לחלק ממודלי FLUX.1; הניסויים גם מצביעים על תלות אפשרית בגבולות אובייקטים. מכאן **שציון AI לתמונה שלמה אינו תחליף לאיתור אזור ערוך**, וגם מפת חום אינה הוכחה ודאית [12, 13].

‏**השלכה ארכיטקטונית מחייבת:** לצד מסלול הסיווג הגלובלי יתווסף מסלול *Localization* נפרד, שמקבל תמונה ומחזיר `tamper mask`, ציון אמינות ומדד לחשד לעריכה מקומית. שכבת החלטה תאחד את התוצאות **ללא מחיקת ההבחנה** בין יצירה מלאה לבין שינוי מקומי. בדוח יוצגו סוג החשד ואזורי החשד, כאשר קיימים, יחד עם אפשרות של *לא ניתן להכריע*. אם אין מודל מיקום שמצליח על מקרי Inpainting רלוונטיים, לא נציג את יכולת זיהוי העריכות המקומיות כפתורה — נבצע ניסוי ממוקד ונשקול שינוי מודל או התאמה מוגבלת.

### ‏3.4 פתרונות קיימים והמשמעות לחידוש

BitMind [6] מפרסם תוסף דפדפן לזיהוי תוכן AI במהלך גלישה; AI or Not [7] מציע בדיקת תמונות וקלטי מדיה נוספים; F-Secure [8] מאפשר בדיקת תמונות וסרטונים באמצעות העלאה; DeepFake-O-Meter [9] הוא כלי מחקרי המאגד כמה גלאים. לכן אתר העלאה, תוסף דפדפן או הרצת כמה גלאים אינם חידוש מספקים כשלעצמם. התרומה שתיבחן תהיה **שיטת שילוב מוגדרת ומדידה** של גלאי יצירה מלאה, גלאי עריכות מקומיות וגלאי וידאו, תוך טיפול בחוסר הסכמה, בהימנעות מהכרעה ובזמן חישוב. לא נטען מראש לביצועים עדיפים או להיעדר תכונות במוצרים שלא נבדקו עצמאית.

### ‏3.5 ראיות מקור ומגבלות ההסקה

‏תקן `C2PA Technical Specification`, **גרסה 2.4 מאפריל 2026** [10], ותוכנות האימות הפתוחות של Content Authenticity Initiative [11] מאפשרים לבדוק *Content Credentials* חתומים כאשר הם קיימים. זהו ערוץ ראיות משלים, **נפרד** מתוצאת הגלאים: היעדר אישור מקור אינו הוכחה לזיוף, ואישור תקף אינו מוכיח שהתוכן נכון עובדתית. ניתן לשלב בדיקת מקור במערכת, אך אין להשתמש בהיעדר מטא-דאטה כאינדיקציה חיובית ליצירת AI.

## ‏4. טבלת ראיות והשפעה על הפרויקט

<table dir="rtl" lang="he">
<thead><tr><th>מקור</th><th>ראיה טכנית ניתנת לאימות</th><th>מגבלה</th><th>השלכה תכנונית</th></tr></thead>
<tbody>
<tr><td>Ojha, Li, Lee [1]</td><td><code>CLIP ViT-L/14</code>; השוואת <code>nearest neighbor</code> ו-<code>linear probing</code>; בחינת ייצוגי CLIP ו-ImageNet נוספים.</td><td>נבדקו משפחות גנרטורים ונתונים מסוימים, לא עריכות מקומיות מכל סוג.</td><td>גלאי גלובלי מבוסס ייצוג מאומן מראש, עם מבחן לגנרטור שלא נראה.</td></tr>
<tr><td>GenImage [2]</td><td>משימות <code>cross-generator</code> ו-<code>degraded image</code> (טשטוש, דחיסה ורזולוציה נמוכה).</td><td>גודל המאגר וסכנת דליפת דוגמאות/מקורות.</td><td>תת-קבוצה מבוקרת, הפרדה לפי מקור ומבחני JPEG ושינוי גודל.</td></tr>
<tr><td>FaceForensics++ [3]</td><td>1,000 סרטוני מקור; DeepFakes, Face2Face, FaceSwap, NeuralTextures; רמות <code>raw/c0</code>, <code>c23</code>, <code>c40</code> ומסכות.</td><td>ממוקד במניפולציות פנים ובתקופת יצירת המאגר.</td><td>בדיקת וידאו לפי רמת דחיסה והפרדת Deepfake פנים מיצירה מלאה.</td></tr>
<tr><td>DeepfakeBench [4]</td><td>15 שיטות, 9 מאגרים, ניהול נתונים, preprocessing ומדדים אחידים.</td><td>הוגנות מחייבת התאמה לפרוטוקול ולמדיניות החלוקה.</td><td>אותם נתונים, preprocessing ותקציב פריימים בכל השוואה.</td></tr>
<tr><td>Deepfake-Eval-2024 [5]</td><td>תוכן בשטח שנאסף במהלך 2024; פרסום כנס ב-2026.</td><td>התפלגות המאגר והתוויות מגבילות הכללה.</td><td>בדיקה חיצונית ומצב <code>Inconclusive</code> מפורש.</td></tr>
<tr><td>TruFor [12]</td><td>מיזוג RGB עם <code>Noiseprint++</code>; מפת מיקום פיקסלית, מפת אמינות וציון תמונה.</td><td>לא מובטחת הכללה לכל עריכת Inpainting חדשה.</td><td>מסלול <code>Localization</code> ומפת חום לצד סיווג גלובלי.</td></tr>
<tr><td>TGIF2 [13]</td><td>עריכות <code>SD2</code>, <code>SDXL</code>, <code>FLUX.1</code>; השוואת <code>Spliced/FR</code> ומסכות <code>Semantic/Random</code>; דיווחי <code>pixel F1</code>/<code>IoU</code>.</td><td>שונות גדולה בין שיטות העריכה; קושי מובהק ב-Fully Regenerated.</td><td>מערך מבחן עם מסכות אמת והפרדת ביצועי זיהוי ומיקום.</td></tr>
<tr><td>BitMind / F-Secure [6, 8]</td><td>בדיקה במהלך גלישה או העלאת קובצי תמונה/וידאו קיימת כבר בשוק.</td><td>דפי מוצר אינם ניסויי דיוק עצמאיים.</td><td>לא להגדיר העלאה או תוסף כחידוש; למדוד יתרון בשיטה.</td></tr>
<tr><td>DeepFake-O-Meter [9]</td><td>פלטפורמה המאגדת גלאי AI שונים לתמונות, וידאו ואודיו.</td><td>אתר מחקרי אינו הוכחה לקיום API ציבורי חופשי.</td><td>לבדוק מיזוג פשוט כ-Baseline ולהגדיר את ההבדל האלגוריתמי.</td></tr>
<tr><td>C2PA + CAI [10, 11]</td><td>מניפסט חתום ומנגנוני אימות באמצעות SDK.</td><td>אין תמיד אישורי מקור, ואימות לא קובע אמת עובדתית.</td><td>לשקול מודול Provenance נפרד שאינו משנה אוטומטית ציון AI.</td></tr>
<tr><td>Android Media Projection [14]</td><td>נדרשת הסכמת משתמש לכל סשן צילום מסך; ב-Android 14+ קיימות דרישות foreground service.</td><td>רכיב אינטגרציה נוסף עם מגבלות פלטפורמה והרשאות.</td><td>אתר Web כ-MVP; כפתור צף נשאר הרחבה מותנית באב-טיפוס.</td></tr>
</tbody>
</table>

## ‏5. סינתזה: מסקנות והחלטות ראשוניות

‏מן המחקרים עולה שהבעיה המהותית היא **אמינות בתנאים משתנים** ולא רק ציון גבוה על Dataset יחיד. מן המוצרים עולה שכבר קיימת חוויית בדיקה למשתמש ואף שימוש בכמה גלאים. בנוסף, TruFor ו-TGIF2 מראים שהבחנה בין **זיהוי גלובלי** לבין **מיקום עריכה מקומית** היא הכרחית למימוש היקף הפרויקט [12, 13]. השילוב בין סוגי המקורות מכוון לפתרון המתרכז בהשוואה ניסויית של שיטות, בדיווח שקוף של אי-ודאות ובממשק שלא מציג הסתברות כהוכחה.

- ‏**MVP מוצע:** אתר המאפשר העלאת תמונה או סרטון, הפעלת מודלים קיימים, והצגת תוצאה בשלושה מצבי החלטה: **"זוהו סימנים ליצירה/עריכה ב-AI"**, **"לא זוהו סימנים מספיקים"** ו-**"לא ניתן להכריע"**. לצד התוצאה יוצג *סוג החשד* — יצירה מלאה, שינוי מקומי או שינוי פנים — אם יש בסיס להבחין ביניהם. אין לזהות את המצב השני עם "אמיתי בוודאות".
- ‏**מחקר אלגוריתמי:** להשוות מודל יחיד, ממוצע פשוט, משקלים קבועים ומיזוג המושפע מאיכות המדיה/הסכמת גלאים. ספים וכיול ייקבעו על נתוני Validation בלבד; תוצאות מסלול המיקום לא יומרו ללא בדיקה לציון גלובלי שקול.
- ‏**וידאו:** להתחיל מדגימת פריימים אחידה וצבירת ציונים; לבחון בחירה לפי שינוי סצנה/איכות מול אותו מספר פריימים, ובהמשך גלאי ייעודי למניפולציות פנים אם יהיה צורך.
- ‏**ממשק:** להציג תוצאות נפרדות למודלים, מסכות/אזורי חשד אם קיימים, זמן עיבוד, הסבר למגבלות ואזהרה מפני פירוש התוצאה כהוכחה. כפתור צף לאנדרואיד יישאר הרחבה ולא תלות של ה-MVP.

### ‏5.1 חלופות מערכת והעדפתנו

‏מנוע מקומי המבוסס על מודלים פתוחים נבחר כבסיס המחקר, משום שהוא מאפשר שחזור ניסויים, שליטה בעיבוד הנתונים וללא תלות מלאה במכסת API מסחרי. שירותי API יוכלו לשמש להשוואה נקודתית בלבד, בכפוף לתנאי השימוש והתקציב. **מסלול גלובלי** יפיק ציון לכל תמונה; **מסלול Localization** יפיק חשד אזורי, מסכה ואמינות; **מסלול וידאו** יפעיל את המסלולים על פריימים נבחרים ויצבור ראיות ברמת קליפ. **שכבת החלטה והצגה** תשמר את ההבחנה בין יצירה מלאה לעריכה מקומית ותאפשר הימנעות מהכרעה. בדיקת C2PA, אם תתווסף, תוצג בנפרד. אלה רכיבים מוצעים — טרם הוכחה התאמתם הביצועית.

### ‏5.2 Exit Check — אילו החלטות השתנו בעקבות הראיות?

‏**הנחות שהתעדכנו:** (א) ההנחה שסיווג בינארי של כל הקובץ מספיק הוחלפה ב**שלוש תוצאות החלטה הכוללות "לא ניתן להכריע"**, עקב פערי הכללה שתועדו אצל Ojha ו-Deepfake-Eval-2024 [1, 5]. זהו **מענה תכנוני מוצע**, ולא הוכחה שהימנעות מהכרעה כבר שיפרה דיוק. (ב) ההנחה שמסווג גלובלי יוכל לכסות גם עריכות חלקיות הוחלפה ב**מסלול מיקום נפרד**, בעקבות פלט המפה והאמינות של TruFor והקשיים שהוצגו ב-TGIF2 [12, 13]. (ג) האפשרות להתחיל בכפתור צף באנדרואיד הועברה ל**שלב הרחבה**, בעוד אתר Web נבחר ל-MVP: כלים קיימים כבר מציעים חוויית בדיקה [6, 8, 9], ואילו מסלול `MediaProjection` דורש הרשאות והסכמה חוזרת [14]. הבחירה ב-Web היא **הערכת היקף ומשאבים שלנו**, לא מסקנה שניסוי השוואתי מוכיח ש-Web עדיף.

‏**מה יגרום לנו לשנות כיוון בפועל?** אם גלאי המיקום לא יעמוד במדדי מיקום שנקבע מראש על עריכות `Spliced` ו-`Fully Regenerated`, במיוחד תחת מסכות אקראיות [13], נבחן מודל Localization אחר, התאמה מצומצמת או צמצום טענת התמיכה; אם שילוב הגלאים יעלה את שיעור החיוביים-השגויים או לא יועיל ביחס למודל יחיד [1, 4, 5], נשאיר את המודל היחיד כבסיס ונשנה את כלל המיזוג; אם דגימת פריימים מפספסת שינויים קצרים [3, 4], נגדיל דגימה, נבחר דגימה מותאמת או נשלב גלאי וידאו ייעודי; ואם פתרון מקומי לא יעמוד בתקציב זמן או זיכרון, נצמצם את מספר המודלים לפני מעבר לתלות ב-API. **לכל שינוי יישמר מקור, ממצא מדיד, החלטה ונימוק** — בהתאם לשאלת הסיום במצגת הקורס.

## ‏6. תכנית הערכה, מגבלות ושאלות פתוחות

- ‏**לזיהוי ברמת קובץ:** `Precision`, `Recall`, `F1`, `ROC-AUC`, שיעור חיוביים-שגויים, זמן עיבוד, כיול וכיסוי כאשר ניתן להשיב "לא ניתן להכריע". התוצאות יוצגו בנפרד ליצירה מלאה, לשינוי מקומי ולזיוף פנים, ככל שהנתונים מאפשרים.
- ‏**לזיהוי מקומי:** `pixel-level F1` ו-`IoU` של מסכת העריכה; מדדי זיהוי ברמת התמונה; השוואת `Spliced` מול `Fully Regenerated` ומסכות `Semantic` מול `Random` [12, 13]. אין להחליף מדד מיקום ב-ROC-AUC של סיווג תמונה.
- ‏**להוגנות ועמידות:** הפרדה קשיחה בין אימון/אימות/בדיקה, מניעת כפילויות, מניעת הופעת פריימים מאותו סרטון בחלקים שונים, החזקת גנרטורים בחוץ, בדיקות JPEG, שינוי גודל וטשטוש, ואותו תקציב זמן/פריימים לכל חלופה [1–5].
- ‏**לחלופות:** מודל יחיד מול ממוצע לא-משוקלל מול מיזוג מוצע; וידאו בדגימה אחידה מול דגימה מותאמת; ניתוח תוספת מסלול Localization מול סיווג גלובלי בלבד.

‏הסיכונים שנותרו לבירור: רישיונות וזמינות של משקולות מודלים ומאגרי נתונים; כוח חישוב נדרש; התאמת מסכות אמת לאזורי עריכה; שיעור חיוביים-שגויים; בחירת ספי הימנעות; כשלי הכללה לעריכות חדשות; פרטיות קובצי המשתמש. **המסמך מציג ראיות ומסקנות תכנוניות — לא תוצאות ניסויים שכבר נערכו בפרויקט.**

## ‏7. מקורות — מחברים מלאים, כותרות מדויקות וכתובות URL

> ‏במספור נשמרו [1]–[11] מן המסמך המקורי; מקורות חדשים הם [12]–[14]. למאמרים רשומים **כל המחברים** בהתאם לעמוד הפרסום; למוצרים, תקנים ותיעוד טכני מצוין הגוף האחראי. "ללא תאריך" משמעו שהדף הרשמי אינו מציין תאריך פרסום מוסמך, ולא שהמידע ישן. **קישורי המאמרים [1]–[5], [12], [13] מפנים ישירות לקובצי PDF של המאמרים המלאים (לא לעמודי Abstract).** מקורות [6]–[11], [14] אינם מאמרי מחקר, ולכן הקישורים אליהם מפנים לדפי המוצר, המפרט או התיעוד עצמם.

<div dir="ltr" lang="en">

1. **[1]** Utkarsh Ojha, Yuheng Li, Yong Jae Lee (2023). *Towards Universal Fake Image Detectors That Generalize Across Generative Models*. Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pp. 24480–24489. [קריאת המאמר המלא (PDF)](https://openaccess.thecvf.com/content/CVPR2023/papers/Ojha_Towards_Universal_Fake_Image_Detectors_That_Generalize_Across_Generative_Models_CVPR_2023_paper.pdf) — **URL מלא:** https://openaccess.thecvf.com/content/CVPR2023/papers/Ojha_Towards_Universal_Fake_Image_Detectors_That_Generalize_Across_Generative_Models_CVPR_2023_paper.pdf
2. **[2]** Mingjian Zhu, Hanting Chen, Qiangyu Yan, Xudong Huang, Guanyu Lin, Wei Li, Zhijun Tu, Hailin Hu, Jie Hu, Yunhe Wang (2023). *GenImage: A Million-Scale Benchmark for Detecting AI-Generated Image*. Advances in Neural Information Processing Systems 36 (NeurIPS), Datasets and Benchmarks Track. [קריאת המאמר המלא (PDF)](https://proceedings.neurips.cc/paper_files/paper/2023/file/f4d4a021f9051a6c18183b059117e8b5-Paper-Datasets_and_Benchmarks.pdf) — **URL מלא:** https://proceedings.neurips.cc/paper_files/paper/2023/file/f4d4a021f9051a6c18183b059117e8b5-Paper-Datasets_and_Benchmarks.pdf
3. **[3]** Andreas Rössler, Davide Cozzolino, Luisa Verdoliva, Christian Riess, Justus Thies, Matthias Nießner (2019). *FaceForensics++: Learning to Detect Manipulated Facial Images*. Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV). [קריאת המאמר המלא (PDF)](https://openaccess.thecvf.com/content_ICCV_2019/papers/Rossler_FaceForensics_Learning_to_Detect_Manipulated_Facial_Images_ICCV_2019_paper.pdf) — **URL מלא:** https://openaccess.thecvf.com/content_ICCV_2019/papers/Rossler_FaceForensics_Learning_to_Detect_Manipulated_Facial_Images_ICCV_2019_paper.pdf . [תיעוד רמות הדחיסה](https://github.com/ondyari/FaceForensics/blob/master/dataset/README.md) — **URL מלא:** https://github.com/ondyari/FaceForensics/blob/master/dataset/README.md
4. **[4]** Zhiyuan Yan, Yong Zhang, Xinhang Yuan, Siwei Lyu, Baoyuan Wu (2023). *DeepfakeBench: A Comprehensive Benchmark of Deepfake Detection*. Advances in Neural Information Processing Systems 36 (NeurIPS), Datasets and Benchmarks Track. [קריאת המאמר המלא (PDF)](https://proceedings.neurips.cc/paper_files/paper/2023/file/0e735e4b4f07de483cbe250130992726-Paper-Datasets_and_Benchmarks.pdf) — **URL מלא:** https://proceedings.neurips.cc/paper_files/paper/2023/file/0e735e4b4f07de483cbe250130992726-Paper-Datasets_and_Benchmarks.pdf . **PDF חלופי (arXiv):** https://arxiv.org/pdf/2307.01426
5. **[5]** Nuria Alina Chandra, Hannah Lee, Ryan Murtfeldt, Lin Qiu, Arnab Karmakar, Emmanuel Tanumihardja, Kevin Farhat, Ben Caffee, Changyeon Lee, Jongwook Choi, Sejin Paik, Aerin Kim, Oren Etzioni (2026). *Deepfake-Eval-2024: A Multi-Modal In-the-Wild Benchmark of Deepfakes Circulated in 2024*. Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR) Workshops, pp. 10668–10678. **הבהרה:** 2024 היא שנת האיסוף בשם המאגר; 2026 היא שנת פרסום הכנס; ה-preprint פורסם לראשונה ב-2025. [פרסום הכנס המלא (PDF)](https://openaccess.thecvf.com/content/CVPR2026W/APAI/papers/Chandra_Deepfake-Eval-2024_A_Multi-Modal_In-the-Wild_Benchmark_of_Deepfakes_Circulated_in_2024_CVPRW_2026_paper.pdf) — **URL מלא:** https://openaccess.thecvf.com/content/CVPR2026W/APAI/papers/Chandra_Deepfake-Eval-2024_A_Multi-Modal_In-the-Wild_Benchmark_of_Deepfakes_Circulated_in_2024_CVPRW_2026_paper.pdf . **PDF חלופי (גרסת arXiv; עשויה להיות שונה מגרסת הכנס):** https://arxiv.org/pdf/2503.02857
6. **[6]** BitMind Holdings Limited (ללא תאריך פרסום מצוין; האתר מציין © 2026). *AI Detector – Chrome Extension*. [קישור למוצר](https://bitmind.ai/extension) — **URL מלא:** https://bitmind.ai/extension
7. **[7]** AI or Not (ללא תאריך פרסום מצוין). *AI Image Detector – Check If an Image Is AI-Generated*. [קישור למוצר](https://www.aiornot.com/ai-image-detector) — **URL מלא:** https://www.aiornot.com/ai-image-detector
8. **[8]** F-Secure (ללא תאריך פרסום מצוין). *F-Secure Deepfake Detector*. [קישור למוצר](https://www.f-secure.com/en/deepfake-detector) — **URL מלא:** https://www.f-secure.com/en/deepfake-detector
9. **[9]** University at Buffalo, UB Media Forensics Lab (ללא תאריך פרסום מצוין לעמוד המתעדכן). *DeepFake-O-Meter — Detection Models*. [קישור למערכת](https://zinc.cse.buffalo.edu/ubmdfl/deep-o-meter/models) — **URL מלא:** https://zinc.cse.buffalo.edu/ubmdfl/deep-o-meter/models . **קישור מאמת חלופי מטעם האוניברסיטה (2024):** [University at Buffalo — DeepFake-O-Meter](https://www.buffalo.edu/news/releases/2024/09/ub-deepfake-o-meter-democratizes-deepfake-detection.html) — **URL מלא:** https://www.buffalo.edu/news/releases/2024/09/ub-deepfake-o-meter-democratizes-deepfake-detection.html . (עמוד המודלים הוא ממשק אינטראקטיבי הדורש JavaScript; דף האוניברסיטה מסביר את תכלית המערכת.)
10. **[10]** Coalition for Content Provenance and Authenticity (C2PA) (אפריל 2026). *Content Credentials: C2PA Technical Specification*, **version 2.4**. **זהו המפרט הטכני ולא מסמך ה-Explainer.** [קישור למפרט](https://spec.c2pa.org/specifications/specifications/2.4/specs/C2PA_Specification.html) — **URL מלא:** https://spec.c2pa.org/specifications/specifications/2.4/specs/C2PA_Specification.html
11. **[11]** Content Authenticity Initiative (ללא תאריך פרסום מצוין). *CAI open source SDK*. [קישור לתיעוד](https://opensource.contentauthenticity.org/docs/introduction/) — **URL מלא:** https://opensource.contentauthenticity.org/docs/introduction/
12. **[12]** Fabrizio Guillaro, Davide Cozzolino, Avneesh Sud, Nicholas Dufour, Luisa Verdoliva (2023). *TruFor: Leveraging All-Round Clues for Trustworthy Image Forgery Detection and Localization*. Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), pp. 20606–20615. [קריאת המאמר המלא (PDF)](https://openaccess.thecvf.com/content/CVPR2023/papers/Guillaro_TruFor_Leveraging_All-Round_Clues_for_Trustworthy_Image_Forgery_Detection_and_CVPR_2023_paper.pdf) — **URL מלא:** https://openaccess.thecvf.com/content/CVPR2023/papers/Guillaro_TruFor_Leveraging_All-Round_Clues_for_Trustworthy_Image_Forgery_Detection_and_CVPR_2023_paper.pdf . **PDF חלופי (עותק בכנס ששמרו המחברים ב-Zenodo):** https://zenodo.org/records/8139135/files/Guillaro_TruFor_Leveraging_All-Round_Clues_for_Trustworthy_Image_Forgery_Detection_and_CVPR_2023_paper.pdf?download=1
13. **[13]** Hannes Mareen, Dimitrios Karageorgiou, Paschalis Giakoumoglou, Peter Lambert, Symeon Papadopoulos, Glenn Van Wallendael (2026). *TGIF2: extended text-guided inpainting forgery dataset and benchmark*. Journal on Information Security, volume 2026, article 13. **פורסם:** 10 באפריל 2026; **Version of record:** 18 במאי 2026. DOI: 10.1186/s13635-026-00235-9. [קריאת המאמר המלא (PDF)](https://link.springer.com/content/pdf/10.1186/s13635-026-00235-9.pdf) — **URL מלא:** https://link.springer.com/content/pdf/10.1186/s13635-026-00235-9.pdf
14. **[14]** Google, Android Developers (עדכון אחרון: 19 בנובמבר 2026). *Media projection*. Android Developers documentation. [קישור לתיעוד](https://developer.android.com/media/grow/media-projection) — **URL מלא:** https://developer.android.com/media/grow/media-projection

</div>


</div>
