```{.python .input  n=1}
# import all of the things
from validate import validate
import pandas as pd
import itables.interactive
from itables import show
```

<div class='outputs' n=1>



</div>

# Validating your sdf file for submission
We have imported the validate function from `validate.py`. This function takes only one argument: the filepath to the sdf file you want to validate.     

The function returns two things:    
- a dictionary containing the results of the validation    
- a True/False value for if the file is valid.     

We have provided a file (compound-set_fragmenstein.sdf') with lots of problems in it, so we can demonstrate how the validation works.     
    
First, we get our results and validation status by running our file through validate.     
    
We can see below that this file is not valid:    

```{.python .input  n=2}
results, valid = validate('compound-set_fragmenstein.sdf')
print(valid)
```

<div class='outputs' n=2>
4 mols detected (including blank mol)
False
RDKit ERROR: [17:26:35] SMILES Parse Error: syntax error while parsing: *CCC(=O)N1CC[NH+](Cc2ccc(CNC(=O)C[NH2+]Cc3c[nH]c4ccc(F)cc34)cc2)CC1lknljnlnln
RDKit ERROR: [17:26:35] SMILES Parse Error: Failed parsing SMILES '*CCC(=O)N1CC[NH+](Cc2ccc(CNC(=O)C[NH2+]Cc3c[nH]c4ccc(F)cc34)cc2)CC1lknljnlnln' for input: '*CCC(=O)N1CC[NH+](Cc2ccc(CNC(=O)C[NH2+]Cc3c[nH]c4ccc(F)cc34)cc2)CC1lknljnlnln'

</div>

# Viewing the problems

Here, we have taken the results dict from validate, and transformed them into a pandas dataframe. We can display this as an interactive table to see what the issues with the file are:

```{.python .input  n=3}
warnings_table = pd.DataFrame.from_dict(results)
show(warnings_table)
```

<div class='outputs' n=3>



</div>
