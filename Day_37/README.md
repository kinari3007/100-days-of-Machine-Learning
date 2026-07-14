# Missing Category Imputation

1. What is it?
```
Missing Category Imputation means filling missing values in categorical columns with a new category called "Missing" instead of dropping rows or filling with mode.

Instead of pretending the data isn't missing → you explicitly label it as missing → create a brand new category called "Missing".
```

---

2. Simple Analogy

Survey question: "Rate your garage quality"  
Options: Excellent, Good, Average, Poor, Terrible  
  
Some people left it blank →  

```
Mean/Median imputation: impossible for text categories
Mode imputation: fill with "Average" → lies to model ❌
Missing category: fill with "Missing" → honest signal ✅
```
---
```
Model now learns:
"When GarageQual = Missing → house probably has no garage"
→ This is USEFUL information for predicting sale price!
```

---

3. Why Not Just Use Mode?
```
GarageQual distribution:
TA (Typical)  → 1311 houses  ← mode
Gd (Good)     →   14 houses
Fa (Fair)     →   69 houses
Po (Poor)     →    5 houses
Ex (Excellent)→    3 houses
Missing       →   81 houses  (5.5% missing)

If we fill with mode (TA):
→ 81 houses that have NO garage
→ now look like houses with TYPICAL garage
→ model completely misled ❌

If we fill with "Missing":
→ Model learns: Missing = no garage = lower price
→ This is REAL and USEFUL information ✅
```

---

4. When to Use Missing Category Imputation
```
✅ Use when:
  → Column is CATEGORICAL (text, not numbers)
  → Missing data is NOT random
    (missing = something meaningful like "no garage")
  → Missing % is moderate or high (5-50%)
  → You want model to know data was missing
  → Using tree-based models

❌ Avoid when:
  → Missing is truly random with no meaning
  → Column has too many categories already
    (adding "Missing" makes it worse)
  → Missing % is very low (<1%) → use mode instead
```

---

5. How it Affects Data
```
Before imputation:
GarageQual: [TA, TA, Gd, NaN, TA, NaN, Fa]
Unique categories: 4

After Missing category imputation:
GarageQual: [TA, TA, Gd, Missing, TA, Missing, Fa]
Unique categories: 5  ← "Missing" is a new category

Distribution changes:
→ "Missing" appears as a NEW bar in bar chart
→ Other category proportions stay the same
→ No rows dropped
→ No information lost
```