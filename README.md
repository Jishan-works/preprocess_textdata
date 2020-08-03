# Preprocessing Text Python Package

## Installation

`pip install git+httpt://github.com/laxmimerit/preprocess_textdata.git`

Uninstall

`pip uninstall preprocess_textdata`

#### How to use it for preprocessing
You have to have installed spacy and python3 to make it work.

Download 'en_core_web_sm' using "python -m spacy download en_core_web_sm" in Anaconda Prompt.

```
def get_clean(x):
    x = str(x).lower().replace('\\', '').replace('_', ' ')
    x = pt.cont_exp(x)
    x = pt.remove_emails(x)
    x = pt.remove_urls(x)
    x = pt.remove_html_tags(x)
    x = pt.remove_rt(x)
    x = pt.remove_accented_chars(x)
    x = pt.remove_special_chars(x)
    x = re.sub("(.)\\1{2,}", "\\1", x)
    return x
```

Use this if you want to use one by one
```
import pandas as pd
import numpy as np
import preprocess_textdata as pt

df = pd.read_csv('imdb_reviews.txt', sep = '\t', header = None)
df.columns = ['reviews', 'sentiment']

# These are series of preprocessing
df['reviews'] = df['reviews'].apply(lambda x: pt.cont_exp(x)) #you're -> you are; i'm -> i am
df['reviews'] = df['reviews'].apply(lambda x: pt.remove_emails(x))
df['reviews'] = df['reviews'].apply(lambda x: pt.remove_html_tags(x))
df['reviews'] = df['reviews'].apply(lambda x: pt.remove_urls(x))

df['reviews'] = df['reviews'].apply(lambda x: pt.remove_special_chars(x))
df['reviews'] = df['reviews'].apply(lambda x: pt.remove_accented_chars(x))
df['reviews'] = df['reviews'].apply(lambda x: pt.make_base(x)) #ran -> run, 
df['reviews'] = df['reviews'].apply(lambda x: pt.spelling_correction(x).raw_sentences[0]) #seplling -> spelling
```

Note: Avoid to use `make_base` and `spelling_correction` for very large dataset otherwise it might take hours to process.