# Filtering out uninformative features and variants that are not predicted to be structurally damaging

```python
import pandas as pd
```

```python
data = pd.read_csv('predicted_outcomes.csv')
```

removes columns that contain only zero values; leaves the ones with damaging structural features

```python
pos_col = data.loc[:, (data != 0).any(axis=0)]
```

 removes variants that are predicted to be NOT deleterious buy SWAAT

```python
pos_swaat_pred = pos_col.loc[(data['swaat_prediction'] == 1)]
```

extract variants with damaging dG values

```python
bad_dG = pos_col[(pos_col['dG'] > 0.500) | (pos_col['dG'] < -0.500)]
```

extract variants with damaging dS values

```python
bad_dS = pos_col[(pos_col['dS'] > 0.500) | (pos_col['dS'] < -0.500)]
```

extract variants with both damaging dG and dS values

```python
combined = [bad_dG, bad_dS]
combined = pd.concat(combined)
bad_dG_dS = combined[combined.duplicated()]
bad_dG_dS
```

get the results for all perfomed commands in csv format

```python
d_rf.to_csv('positive_columns.csv', index=False)
sp_deleterious.to_csv('positive_swaat_prediction.csv', index=False)
bad_dG.to_csv('deleterios_dG.csv', index=False)
bad_dS.to_csv('deleterios_dS.csv', index=False)
bad_dG_dS.to_csv('deleterios_dG_and_dS.csv', index=False)
```

