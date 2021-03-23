
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

[jq](https://stedolan.github.io/jq/manual/) is very good at generating JSONL (the `-c` flag will force single-line output).

```bash
{ for f in pages/*; do cat $f | jq -c '.data.psr.product_models.items | map({ model_id: .merchant_product_model_id, json_byte_count: tojson | utf8bytelength }) | .[]'; done } > model-byte-sizes.jsonl
```

first few lines of the resulting file `model-byte-sizes.jsonl`:

```txt
{"model_id":"100059","json_byte_count":16992}
{"model_id":"100169","json_byte_count":186463}
{"model_id":"100171","json_byte_count":46156}
{"model_id":"100223","json_byte_count":39140}
{"model_id":"100275","json_byte_count":27703}
{"model_id":"100457","json_byte_count":7531}
{"model_id":"100459","json_byte_count":7199}
{"model_id":"100463","json_byte_count":47487}
{"model_id":"100495","json_byte_count":13421}
...
```
