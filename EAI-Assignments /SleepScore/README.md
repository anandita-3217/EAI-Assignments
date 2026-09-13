# Agent for calculating the Sleep Score of a given location

## Course: Essentials of Artificial Intelligence

## Author
- **Name**: Anandita Dakshayani Garimella
- **Roll No**: SE26MAID034
- 

## Objective
To implement an agent to calculate the sleep score of a person using various lifestyle and psychological  etrics.

## Dataset

- **Source:** [Sleep Health and Lifestyle Dataset](https://www.kaggle.com/datasets/uom190346a/sleep-health-and-lifestyle-dataset)
- Main File Used: `sleep_data/Sleep_health_and_lifestyle_dataset.csv  `


## Metrics Used:
- Age — used to determine age-appropriate sleep duration and expected resting heart rate
- Sleep Duration (hours) — compared against an age-adjusted target
- Quality of Sleep (self-rated, 1–10)
- Heart Rate (resting, bpm) — compared against an age-adjusted expected baseline
- Stress Level (self-rated, 1–10)
- Sleep Disorder (None / Insomnia / Sleep Apnea)

## Approach / Logic
### SleepScore  
1. **Load data** — Read the CSV into a DataFrame and inspect its shape/columns.
2. **Duration score** — Compare each person's sleep duration to an age-adjusted target (e.g. ~8 hrs for adults), penalizing deviation.
3. **Cycle alignment score** — Estimate how closely total sleep time aligns to whole ~90-minute sleep cycles, since waking mid-cycle is associated with grogginess independent of total hours (per the [Sleep Foundation's Sleep Calculator](https://www.sleepfoundation.org/sleep-calculator)).
4. **Quality score** — Rescale the dataset's 1–10 self-rated sleep quality to a 0–100 scale.
5. **Heart rate score** — Compare resting heart rate to an age-adjusted expected baseline, with a small tolerance band before penalizing.
6. **Penalties** — Subtract flat penalties for diagnosed sleep disorders (Sleep Apnea, Insomnia) and for self-reported stress level.
7.  **Combine** — Weight duration/cycle (30%), quality (30%), and heart rate (20%), subtract penalties, then rescale to a final 0–100 score.
8.  **Sleep Classification** — Map the scores to Very Low, Low, OK, High, and Very High

### Mental Health Score
1. **Stress component** — Invert the 1–10 self-rated stress level to a 0–100 scale (low stress → high score).
2. **Sleep contribution** — Incorporate the already-computed Sleep Score as a secondary factor, reflecting the established link between sleep and mental health.
3. **Combine** — Weight stress at 70% and sleep score at 30% to produce a final 0–100 Mental Health Score.
4. **Classification** — Map the score to one of four categories: Thriving, Good, At Risk, Struggling.
   

**Input:** Per-person records with Age, Sleep Duration, Quality of Sleep, Heart Rate, Stress Level, and Sleep Disorder.
 
**Output:**
1. **Sleep Score** — a numeric score (0–100).
2. **Mental Health Score** — a numeric score (0–100).
3. **Mental Health Category** — one of: `Thriving`, `Good`, `At Risk`, `Struggling`.

## References
- Dataset: https://www.kaggle.com/datasets/uom190346a/sleep-health-and-lifestyle-dataset
- Sleep duration and cycle guidance: https://www.sleepfoundation.org/sleep-calculator
 
