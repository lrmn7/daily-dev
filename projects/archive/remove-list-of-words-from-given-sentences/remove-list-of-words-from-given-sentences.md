**Last updated**: October 26, 2019 | at  11:35 PM



## SNIPPETS CODE

```py
import re

datasets = ["aaaaa bbbbb cccc", "ccccc bbbb xxxxxxx", "xxxxxx zzzz zaaaaaa"]
hapus_kata = ["aaaaa", "zzzz"]

for idx_data in range(len(datasets)):
  for idx_kata in range(len(hapus_kata)):
    kata_baru = re.sub(rf"\b{hapus_kata[idx_kata]}\b", '', datasets[idx_data]).strip()
    datasets[idx_data] = kata_baru
```