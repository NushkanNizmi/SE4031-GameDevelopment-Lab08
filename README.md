# 🧪 LAB 08 (2 Hours) — Objectives + Timer + Completion Feedback

Includes scripts + where to attach

---

## Goal

Objective flow:

- Collect 3 Gems  
- Collect 1 Key  
- Hit 3 Targets  
- Read 1 Lore  

Then enable completion feedback.

---

## A) Add Objective + Timer texts to HUD

HUD_Canvas: add TMP texts:

- TXT_Objective (Objective text)  
- TXT_Timer (Time text)

---

## B) ObjectiveManager.cs

Create empty object: ObjectiveManager  

Create script:

```csharp
using TMPro;
using UnityEngine;

public class ObjectiveManager : MonoBehaviour
{
    public static ObjectiveManager Instance { get; private set; }

    public TMP_Text objectiveText;

    public int gemsRequired = 3;
    public int keysRequired = 1;
    public int targetsRequired = 3;

    private bool loreRead;

    void Awake()
    {
        if (Instance != null && Instance != this) { Destroy(gameObject); return; }
        Instance = this;
    }

    void Start() => UpdateObjective();

    public void MarkLoreRead()
    {
        loreRead = true;
        UpdateObjective();
    }

    public void UpdateObjective()
    {
        if (!GameManager.Instance || !objectiveText) return;

        if (GameManager.Instance.gems < gemsRequired)
        {
            objectiveText.text = $"Objective: Collect Gems ({GameManager.Instance.gems}/{gemsRequired})";
            return;
        }
        if (GameManager.Instance.keys < keysRequired)
        {
            objectiveText.text = $"Objective: Collect Keys ({GameManager.Instance.keys}/{keysRequired})";
            return;
        }
        if (GameManager.Instance.targetsHit < targetsRequired)
        {
            objectiveText.text = $"Objective: Hit Targets ({GameManager.Instance.targetsHit}/{targetsRequired})";
            return;
        }
        if (!loreRead)
        {
            objectiveText.text = "Objective: Read 1 Lore Tablet";
            return;
        }

        objectiveText.text = "Objective: COMPLETED! Go to Exit!";
    }

    public bool Completed()
    {
        if (!GameManager.Instance) return false;
        return GameManager.Instance.gems >= gemsRequired &&
               GameManager.Instance.keys >= keysRequired &&
               GameManager.Instance.targetsHit >= targetsRequired &&
               loreRead;
    }
}
```
Attach to ObjectiveManager object
Drag TXT_Objective into objectiveText

---

## C) Update LoreObject to mark lore read

In `LoreObject.ShowLore()` add:

```csharp
if (ObjectiveManager.Instance) ObjectiveManager.Instance.MarkLoreRead();
```
---

## D) Make GameManager auto-update objectives

At the end of `RefreshUI()` add:

```csharp
if (ObjectiveManager.Instance) ObjectiveManager.Instance.UpdateObjective();
```
---

## E) TimerManager.cs

Create empty object TimerManager. Script:

```csharp
using TMPro;
using UnityEngine;

public class TimerManager : MonoBehaviour
{
    public TMP_Text timerText;
    public float startTime = 120f;
    float t;

    void Start(){ t = startTime; }

    void Update()
    {
        t -= Time.deltaTime;
        if (t < 0) t = 0;

        int m = Mathf.FloorToInt(t / 60);
        int s = Mathf.FloorToInt(t % 60);
        timerTex
```
Attach to TimerManager
Drag TXT_Timer into timerText

---

## F) Completion feedback (Light)

Add Point Light → rename **CompletionLight**

Set intensity = 0

Create empty object **CompletionController** and attach:

```csharp
using UnityEngine;

public class CompletionController : MonoBehaviour
{
    public Light completionLight;
    bool done;

    void Update()
    {
        if (done) return;
        if (ObjectiveManager.Instance && ObjectiveManager.Instance.Completed())
        {
            done = true;
            completionLight.intensity = 5f;
        }
    }
}
```

Assign CompletionLight into completionLight


---

## Submission Task

Show:

- Objective updates  
- Timer working  
- Completion light turning on
  
---

⚠️ **Important Note**

Students must submit **all project files to GitHub** along with the demo video.

Both of the following are mandatory:

- Complete Unity project pushed to a GitHub repository  
- Demo video file uploaded or shared with the submission  

Submissions without the GitHub project files will be considered incomplete.

---

---

## Next Lab

Lab 09: Exit door + reset + final mini game.

---

