```{.python .input  n=1}
# import all of the things
from validate import validate
import pandas as pd
import itables.interactive
from itables import show
```

```{.json .output n=1}
[
 {
  "data": {
   "application/javascript": "require.config({\n    paths: {\n        datatables: 'https://cdn.datatables.net/1.10.19/js/jquery.dataTables.min',\n    }\n});\n\n$('head').append('<link rel=\"stylesheet\" type=\"text/css\" \\\n                href = \"https://cdn.datatables.net/1.10.19/css/jquery.dataTables.min.css\" > ');\n\n$('head').append('<style> table td { text-overflow: ellipsis; overflow: hidden; } </style>');\n\n$('head').append(`<script>\nfunction eval_functions(map_or_text) {\n    if (typeof map_or_text === \"string\") {\n        if (map_or_text.startsWith(\"function\")) {\n            try {\n                // Note: parenthesis are required around the whole expression for eval to return a value!\n                // See https://stackoverflow.com/a/7399078/911298.\n                //\n                // eval(\"local_fun = \" + map_or_text) would fail because local_fun is not declared\n                // (using var, let or const would work, but it would only be declared in the local scope\n                // and therefore the value could not be retrieved).\n                const func = eval(\"(\" + map_or_text + \")\");\n                if (typeof func !== \"function\") {\n                    // Note: backquotes are super convenient!\n                    // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals\n                    console.error(\"Evaluated expression \" + map_or_text + \" is not a function (type is \" + typeof func + \")\");\n                    return map_or_text;\n                }\n                // Return the function\n                return func;\n            } catch (e) {\n                // Make sure to print the error with a second argument to console.error().\n                console.error(\"itables was not able to parse \" + map_or_text, e);\n            }\n        }\n    } else if (typeof map_or_text === \"object\") {\n        if (map_or_text instanceof Array) {\n            // Note: \"var\" is now superseded by \"let\" and \"const\".\n            // https://medium.com/javascript-scene/javascript-es6-var-let-or-const-ba58b8dcde75\n            const result = [];\n            // Note: \"for of\" is the best way to iterate through an iterable.\n            // https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of\n            for (const item of map_or_text) {\n                result.push(eval_functions(item));\n            }\n            return result;\n\n            // Alternatively, more functional approach in one line:\n            // return map_or_text.map(eval_functions);\n        } else {\n            const result = {};\n            // Object.keys() is safer than \"for in\" because otherwise you might have keys\n            // that aren't defined in the object itself.\n            //\n            // See https://stackoverflow.com/a/684692/911298.\n            for (const item of Object.keys(map_or_text)) {\n                result[item] = eval_functions(map_or_text[item]);\n            }\n            return result;\n        }\n    }\n\n    return map_or_text;\n}\n</` + 'script>');",
   "text/plain": "<IPython.core.display.Javascript object>"
  },
  "metadata": {},
  "output_type": "display_data"
 }
]
```

# Validating your sdf file for submission
We have imported the validate function from `validate.py`. This function takes
only one argument: the filepath to the sdf file you want to validate.

The function returns two things:
- a dictionary containing the results of the validation
- a True/False value for if the file is valid.

We have provided a file (compound-set_fragmenstein.sdf') with lots of problems
in it, so we can demonstrate how the validation works.

First, we get our results and validation status by running our file through
validate.

We can see below that this file is not valid:

```{.python .input  n=2}
results, valid = validate('compound-set_fragmenstein.sdf')
print(valid)
```

```{.json .output n=2}
[
 {
  "name": "stdout",
  "output_type": "stream",
  "text": "4 mols detected (including blank mol)\nFalse\n"
 },
 {
  "name": "stderr",
  "output_type": "stream",
  "text": "RDKit ERROR: [17:26:35] SMILES Parse Error: syntax error while parsing: *CCC(=O)N1CC[NH+](Cc2ccc(CNC(=O)C[NH2+]Cc3c[nH]c4ccc(F)cc34)cc2)CC1lknljnlnln\nRDKit ERROR: [17:26:35] SMILES Parse Error: Failed parsing SMILES '*CCC(=O)N1CC[NH+](Cc2ccc(CNC(=O)C[NH2+]Cc3c[nH]c4ccc(F)cc34)cc2)CC1lknljnlnln' for input: '*CCC(=O)N1CC[NH+](Cc2ccc(CNC(=O)C[NH2+]Cc3c[nH]c4ccc(F)cc34)cc2)CC1lknljnlnln'\n"
 }
]
```

# Viewing the problems

Here, we have taken the results dict from validate, and transformed them into a
pandas dataframe. We can display this as an interactive table to see what the
issues with the file are:

```{.python .input  n=3}
warnings_table = pd.DataFrame.from_dict(results)
show(warnings_table)
```

```{.json .output n=3}
[
 {
  "data": {
   "text/html": "<div><table id=\"dbe1e1bb-188f-4dc3-9ea8-9beb7d612b9d\" class=\"display\"><thead>\n    <tr style=\"text-align: right;\">\n      \n      <th>molecule_name</th>\n      <th>field</th>\n      <th>warning_string</th>\n    </tr>\n  </thead></table>\n<script type=\"text/javascript\">\nrequire([\"datatables\"], function (datatables) {\n    $(document).ready(function () {        \n        var dt_args = {\"columnDefs\": [{\"width\": \"70px\", \"targets\": \"_all\"}], \"paging\": false, \"data\": [[\"ver_1.0\", \"version\", \"description for version missing\"], [\"ver_1.0\", \"minimised comRMSD\", \"description for minimised comRMSD missing\"], [\"MAK-UNK-176-5_ACR2\", \"ref_pdb\", \"path Mpro-x0692_0/Mpro-x0692_0_apo.pdb does not exist\"], [\"MAK-UNK-176-5_ACR2\", \"original SMILES\", \"invalid SMILES *CCC(=O)N1CC[NH+](Cc2ccc(CNC(=O)C[NH2+]Cc3c[nH]c4ccc(F)cc34)cc2)CC1lknljnlnln\"], [\"MAK-UNK-095-2_NIT3\", \"ref_pdb\", \"path Mpro-x0749_0/Mpro-x0749_0_apo.pdb does not exist\"], [\"MAK-UNK-095-2_NIT3\", \"original SMILES\", \"value for original SMILES missing\"], [\"SAL-INS-1c7-1_ACR2\", \"ref_pdb\", \"path Mpro-x1458_0/Mpro-x1458_0_apo.pdb does not exist\"]]};\n        dt_args = eval_functions(dt_args);\n        table = $('#dbe1e1bb-188f-4dc3-9ea8-9beb7d612b9d').DataTable(dt_args);\n    });\n})\n</script>\n</div>\n",
   "text/plain": "<IPython.core.display.HTML object>"
  },
  "metadata": {},
  "output_type": "display_data"
 }
]
```
