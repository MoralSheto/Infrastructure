# מדריך הגדרת Disaster Recovery (DR) Replica Setup

מדריך זה מרכז את ההנחיות והפרמטרים להגדרת רפליקציית DR (Disaster Recovery) בין שרתים ומרכזי נתונים (Data Centers).

---

## 📋 שלבי הביצוע (Step-by-Step)

1. **איתור הגדרות מקור:**  
   עבור לבלוק הערות (Notes) של המכונה בעלת הרפליקה המבוקשת והעתק את מחרוזת ההגדרות.

2. **החלת ההגדרות בשרת המקור:**  
   עבור לשרת המקור (Source Server) והדבק את הגדרות התצורה בתוך שדה ה-**Notes**.

3. **סנכרון:**  
   מתין לשרת עד לסיום תהליך הרפליקציה.

4. **הרשאות:**  
   עבור להגדרות הרשאות (Permissions) ושייך את השרת לאיש הצוות הרלוונטי.

---

## ⚙️ תצורת ברירת מחדל (Standard Configuration)

לשימוש כללי, ניתן להשתמש במחרוזת ברירת המחדל הבאה (יש להחליף את `{datacenter}` ביעד המבוקש):

```text
repResync=5,repTargets={datacenter},repTargetSuffix=replica,repSnapsHourly=2,repSnapsDaily=4,repSnapsWeekly=1,rep=on
```

**דוגמה להגדרה מוכנה (לדוגמה: יעד `eu-ml`):**
```text
repResync=5,repTargets=eu-ml,repTargetSuffix=replica,repSnapsHourly=2,repSnapsDaily=4,repSnapsWeekly=1,rep=on
```

---

## 📖 מילון פרמטרים והסברים

| פרמטר | תיאור | ערכי ברירת מחדל / הערות |
| :--- | :--- | :--- |
| `repTargets` | יעד/י הרפליקציה (Data Centers) | לדוגמה: `il-pt`, `il-rh`, `eu-fr` |
| `repResync` | מרווח זמן לסנכרון מחדש (בדקות) | ברירת מחדל: `5` (טווח מורשה: 5–1440) |
| `repSnapsHourly` | מספר סנאפשוטים שעתיים לשמירה | ברירת מחדל: `2` (עבור 24 השעות האחרונות) |
| `repSnapsDaily` | מספר סנאפשוטים יומיים לשמירה | ברירת מחדל: `4` (עבור 7 הימים האחרונים) |
| `repSnapsWeekly` | מספר שבועות לשמירת סנאפשוט שבועי יחיד | ברירת מחדל: `1` |
| `repTargetSuffix` | סיומת שתתווסף לשם המכונה ביעד | ברירת מחדל: `replica` (לדוגמה: `vmname-[il-pt]-dr`) |
| `rep` | הפעלת מנגנון הרפליקציה | `on` |

---

## 🌐 הגדרת מיפוי רשתות (VLAN Modification)

במידה ונדרש שינוי/מיפוי של VLAN בין דאטה-סנטר המקור לדאטה-סנטר היעד, יש להשתמש במבנה הבא:

```text
repVlanModify{datacenter}={VLAN_מקור}>{VLAN_יעד}
```

### דוגמה:
שינוי VLAN עבור דאטה-סנטר `il-rh` מ-VLAN **282** בשרת המקור ל-VLAN **307** ביעד:
```text
repVlanModifyil-rh=282>307
```