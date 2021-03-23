
Read [JSONL](https://jsonlines.org/) formatted data into [pandas](https://pandas.pydata.org/) and run stats.

```python
#!/usr/bin/env python3

import sys
import jsonlines
import pandas as pd

input_file = sys.argv[1]

lines = jsonlines.open(input_file)
df = pd.DataFrame(lines.iter(type=dict))

print("mean model byte size: {mean:.2f}".format(mean = df["json_byte_count"].mean()))
```

[jq](https://stedolan.github.io/jq/manual/) is very good at generating JSONL.

```bash
{ for f in pages/*; do cat $f | jq '.data.psr.product_models.items | map({ model_id: .merchant_product_model_id, json_byte_count: tojson | utf8bytelength }) | .[]' -c; done } > model-byte-sizes.jsonl
```
