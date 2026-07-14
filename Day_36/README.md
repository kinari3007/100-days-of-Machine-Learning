# Missing Value Imputation Using Mean/Median & Arbitrary Imputation Method

### What is missing value imputation
```
Missing Value Imputation is the process of filling in missing (NaN) values in a dataset with substitute values so that ML models can work with the data.

ML models cannot handle NaN values — you must deal with them before training.
```

---

### What do values go missing 
```
Real world reasons:
→ Survey respondent skipped a question  
→ Sensor malfunction → no reading recorded  
→ Patient didn't show up for follow-up test  
→ Data entry error → field left blank  
→ Feature didn't exist for that record  
   (new customer → no purchase history)  
```

---

### Types or Missingness
```
MCAR — Missing Completely At Random  
→ No pattern to missingness  
→ Pure random chance  
→ Example: Random sensor failures  
→ Safe to use mean/median imputation  
  
MAR — Missing At Random  
→ Missingness depends on OTHER columns  
→ Example: Older passengers less likely  
           to report income  
→ Need careful imputation  

MNAR — Missing Not At Random  
→ Missingness depends on the MISSING VALUE itself  
→ Example: High earners hide their salary  
→ Hardest to handle → arbitrary imputation helps  
```

---

### Two types of technique covered
```
Imputation Techniques  
│  
├── Mean / Median Imputation  
│     → fill with average or middle value  
│     → preserves natural data distribution  
│  
└── Arbitrary Value Imputation  
      → fill with unusual value (99, -1, 999)  
      → signals to model that data was missing  
```

---

## Mean / Median Imputation — Deep Dive

### What is it?
```
Fill missing values with the mean (average) or median (middle value) of that column, calculated from training data only.  
Age column: [22, 25, NaN, 30, 28, NaN, 26]  
```

Mean   = (22+25+30+28+26)/5 = 26.2  
Median = middle of [22,25,26,28,30] = 26  
  
Fill with mean   → [22, 25, 26.2, 30, 28, 26.2, 26]  
Fill with median → [22, 25, 26,   30, 28, 26,   26]  
```
Mean vs Median — When to Use Which  
MEAN:  
✅ Data is normally distributed (bell curve)  
✅ No significant outliers  
✅ Skewness is close to 0  
❌ Pulled by outliers  
   [22, 25, 28, 200] → mean=68.75 → wrong!  
  
MEDIAN:  
✅ Data is skewed (skewness > 1)  
✅ Outliers present  
✅ More robust and stable  
   [22, 25, 28, 200] → median=26.5 → correct!  
```

--- 
 
```
Quick rule:  
Skewness < 1 → Mean  
Skewness > 1 → Median  
Big gap between mean and median → use Median  
```

---

### How it Affects Your Data  
```
Effect 1 — Variance DECREASES  
  Original variance   : 204.35  
  After median impute : 161.99  (-20.7%)  
  After mean impute   : 161.81  (-20.8%)  
  
  Why? All NaN replaced with ONE value  
  → less spread → lower variance  
  → acceptable reduction ✅  
  
Effect 2 — Distribution gets a spike  
  KDE plot shows a bump at mean/median value  
  → all missing values pile at one point  
  → slightly distorts shape  
  → generally acceptable ⚠️  
  
Effect 3 — Correlation slightly weakens  
  Age vs Fare original  : 0.0926  
  After mean imputation : 0.0881  ← tiny change ✅  
  → relationships mostly preserved ✅  
```