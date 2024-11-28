---
title: 자주 사용하는 seed 정의 코드
date: 2024-11-28
categories: [Deep Learning]
tags: [Deep Learning]
---
  

pytorch lightning framework 사용하는 경우를 예시로 작성하였다. 주로 사용하는 seed 고정 코드이다.

```
import os
import random
import torch
import numpy as np
from pytorch_lightning import seed_everything

def set_seed(seed):
    os.environ['PYTHONHASHSEED'] = str(seed)
    random.seed(seed)
    np.random.seed(seed)
    torch.manual_seed(seed)
    if torch.cuda.is_available():
        torch.cuda.manual_seed(seed)
        torch.cuda.manual_seed_all(seed)
    torch.backends.cudnn.deterministic = True
    torch.backends.cudnn.benchmark = False
    seed_everything(seed)

seed = 12
set_seed(seed)
```
