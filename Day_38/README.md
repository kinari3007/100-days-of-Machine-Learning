# 🎲 Random Sample Imputation

### 1. What is it?
```
Random Sample Imputation means filling missing values by randomly picking actual observed values from the same column in the training data.

Instead of filling all missing values with ONE fixed value (like mean=29) → you pick different random real values from the existing data for each missing slot.
```

--- 

### 2. Simple Analogy
```
Age column has 148 missing values
Existing ages: [22, 25, 38, 47, 30, 19, 55 ...]

Mean imputation:
  All 148 missing → filled with 29 (mean)
  → 148 identical values → creates spike ❌

Random sample imputation:
  Missing 1 → randomly pick 38 from existing ages
  Missing 2 → randomly pick 22 from existing ages
  Missing 3 → randomly pick 47 from existing ages
  ...
  → 148 different realistic values → no spike ✅
  → distribution stays natural
```

---

### 3. Why is it Better Than Mean/Median?
```
Mean imputation:
  Age variance original : 204.35
  Age variance after    : 161.81  ← reduced by 20%
  Distribution → spike at mean value

Random sample imputation:
  Age variance original : 204.35
  Age variance after    : 208.33  ← almost identical! ✅
  Distribution → smooth, no spike ✅
```

---

Random sample preserves:  
✅ Variance (208 vs 204 → nearly identical)  
✅ Distribution shape (no artificial spike)  
✅ Proportion of each category (for categorical)  

---

### 4. Two Types — Numerical and Categorical
```
Numerical (Age, Fare):
→ randomly sample from existing numerical values
→ preserves distribution shape and variance

Categorical (GarageQual, FireplaceQu):
→ randomly sample from existing categories
→ preserves proportion of each category
→ unlike "Missing" category imputation
   this maintains original category distribution
```

---

### 5. When to Use Random Sample Imputation
```
✅ Use when:
  → Missing data is MCAR (completely random)
  → You want to preserve distribution shape
  → Variance preservation is important
  → Both numerical and categorical columns
  → Missing % is moderate (5-30%)

❌ Avoid when:
  → Missing data has a PATTERN (MNAR)
    → random fill loses that signal
  → Reproducibility matters
    → results change each run (use random_state)
  → Very large datasets
    → sampling overhead can be slow
```

---

### ✅ Pros of Random Sample Imputation
```
✅ Best variance preservation of all methods
   → only +2% change vs -20% for mean/median
✅ No artificial spike in distribution
   → filled values come from real distribution
✅ Works for BOTH numerical and categorical
✅ Preserves category proportions perfectly
   → max 0.15% shift in category %
✅ Filled values are all realistic
   → only uses values that actually existed
✅ Better than mean/median for preserving
   statistical properties of data
```

### ❌ Cons of Random Sample Imputation
```
❌ Does NOT signal missingness to model
   → model doesn't know which values were imputed
❌ Not reproducible without random_state
   → different runs give different results
❌ Weakens relationships between columns
   → Fare vs Age covariance dropped: 71 → 43
❌ Bad for MNAR data
   → randomly filling structured missingness
      destroys the signal in the missing pattern
❌ No sklearn built-in implementation
   → must code manually
❌ For test data → must sample from train
   → can't use test's own distribution
```

------

# 🚩 Missing Indicator

### 1. What is it?
```
Missing Indicator is a technique that adds a new binary column (0/1 or True/False) to flag which rows had missing values — before filling them with imputation.

You first mark WHERE the data was missing → then fill the missing values → model now has BOTH the filled value AND the missingness flag.
```

---

### 2. Simple Analogy
```
Student exam scores:
Roll 1: Score=85   → present    → Score=85,  Was_Missing=0
Roll 2: Score=NaN  → absent     → Score=29,  Was_Missing=1
Roll 3: Score=72   → present    → Score=72,  Was_Missing=0
Roll 4: Score=NaN  → absent     → Score=29,  Was_Missing=1

Without indicator:
  Model sees 85, 29, 72, 29
  → thinks all four students scored similarly
  → has no idea two were absent ❌

With indicator:
  Model sees (85,0), (29,1), (72,0), (29,1)
  → knows rolls 2 and 4 were absent
  → learns: absent students survive differently ✅
```

---

### 3. Why Add a Missing Indicator?
```
Problem with just imputing:
  Age NaN → filled with mean 29
  Model sees 29 → thinks this is a real 29-year-old
  → loses the information that age was unknown ❌

Solution — Missing Indicator:
  Age NaN → filled with 29 AND Age_NA = True
  Model sees (29, True) → knows age was unknown
  → can learn different patterns for unknown age ✅

In Titanic:
  Missing age might mean → crew member, infant, or specific class
  → model learns this signal and improves predictions
```

---

### 4. How it Works
```
Original data:          After Missing Indicator:
Age    Fare             Age    Fare    Age_NA
40.0   27.72    →       40.0   27.72   False   ← age known
NaN    8.71     →       29.0   8.71    True    ← age unknown
47.0   9.00     →       47.0   9.00    False   ← age known
NaN    221.78   →       29.0   221.78  True    ← age unknown

False = 0 = age was present
True  = 1 = age was missing
```

---

### 5. When to Use Missing Indicator
```
✅ Use when:
  → Missing data is NOT random (MNAR/MAR)
  → Fact of missingness carries information
  → Missing % is moderate (5-30%)
  → Using tree-based models
  → Want to combine with any other imputation method

❌ Avoid when:
  → Missing is truly random (MCAR)
    → indicator adds noise, not signal
  → Missing % is very low (<1%)
    → indicator column is almost all zeros
  → Too many columns already
    → adding indicators for all = doubles columns
```

---

### 6. Two Ways to Add Missing Indicator
```
Way 1 → MissingIndicator (standalone)
  → creates indicator separately
  → you manually combine with imputed data

Way 2 → SimpleImputer(add_indicator=True)
  → imputes AND adds indicator in one step
  → cleaner and production ready ✅
```