---
layout: post
title: "Finetune Pre-trained LMs"
---

Over the weekend, I played with fine-tuning GPT-2 and XLNet (on Colab). Super applause to Huggingface Transformers, it makes all sorts of pre-trained LM extremely accessible. The framework has evolved a lot from a wrapper of pre-trained BERT. It now unifies all models with AutoModel\* with different capabilities, so that we only have to know the key and not care about the API. The repo also contains very handy fine-tuning and inference scripts. 

First I tried text generation with pre-trained GPT-2. Then I followed [this document](https://github.com/huggingface/transformers/tree/master/examples/language-modeling) to fine-tune GPT-2 on WikiText-2. I used the base model which has 117M params. I had to reduce batch size from the default 8 to 2 so that I don't run out of memory, and training speed is about 1.x it/s. The result was good: perplexity dropped below 20 as the tutorial suggests, and the generations were rather reasonable. 

Next, I searched for Chinese pre-trained LMs and found [this awesome project](https://github.com/ymcui/Chinese-XLNet) of Chinese XLNet by HFL. I put together an initial version of the ZJMZ dataset (which is ~1/6 of the final size) and fine-tuned XLNet on it. Encountered a problem where data loading gives an empty dataset, and after some debugging, I found out that the `hfl/chinese-xlnet-base` tokenizer does not have `max_len` set, so the data loader assumed `block_size` as a super large number and chunked the data based on it. For some reason I can use batch size of 8 this time. 

Training was fast: ~8k iterations per epoch with 1.x it/s. But the model overfits quickly (i.e. in one epoch) and perplexity dropped to 1.00000xxx. Interestingly, the tuned model generates garbage: either repeated commas or simply stops, while it is expected to copy the training data. Someone recommends fine-tuning with a 5M-100M dataset, but mine was only 250K. Even when data collection is completed it's probably only gonna be 1.5M. Also tried some "data augmentation": allowing overlap during blocking, but doesn't seem to cure this overfitting problem. 
