## Complete Case Analysis (CCA)

1. What is it?
```
Complete Case Analysis (CCA) is the simplest strategy for handling missing data — you simply drop all rows that have ANY missing value and keep only the "complete" rows.
```

"Complete case" = a row where every single column has a value — no NaN anywhere.

--- 

### Scenarios where to use or not to use CCA

CCA is safe when:  

✅ Missing data is less than 5% per column  
✅ Data is Missing Completely At Random (MCAR - Missing Completely at Random)  
 → missingness has NO pattern, purely random  
✅ You have enough data after dropping

CCA is DANGEROUS when:  

❌ Missing data is more than 5% per column  
❌ Missing data has a pattern (certain groups missing more)  
❌ Dropping rows makes dataset too small  
❌ Missing data is NOT random (biased missingness)  

---

### The core problem CCA solves

ML models cannot handle NaN values
→ you must deal with missing data before training

Options:
1. CCA          → drop rows with missing values (simplest)
2. Imputation   → fill missing values with mean/median/mode
3. Prediction   → use ML to predict missing values

CCA is the FIRST thing to try → simplest → sometimes good enough

---

### Advantage / Disadvantage

**Advantage**
```
1. Easy to implement as no data manipulation required
2. Preserves varibable distribution (id data is MCAR, then the distribution of the reduced dataset should match the distribution in the original dataset)
```

**Disadvantage**
```
1. It can exclude a large fraction of the original dataset (if missing data is abundant)
2. Excluded observations could be informative for the analysis (if data is not missing at random)
3. When using our models in production, the model will not know how to handle missing data
```
