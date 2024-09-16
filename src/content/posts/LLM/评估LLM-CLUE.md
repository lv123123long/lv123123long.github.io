---
title: 评估LLM-GLUE
pubDate: 2024-09-16
categories: ['LLM']
description: ''
---

# 评估LLM-CLUE

## OverView

[CLUEbenchmark/PyCLUE: Python toolkit for Chinese Language Understanding(CLUE) Evaluation benchmark (github.com)](https://github.com/CLUEbenchmark/PyCLUE)



下载数据，然后运行评估代码。这个过程通常相对完善了。[https://github.com/CLUEbenchmark/PyCLUE](https://link.zhihu.com/?target=https%3A//github.com/CLUEbenchmark/PyCLUE) 这个库就是封装好的脚本了。

```text
# 加载保存好的模型路径（train.ipynb中最后打印出的model_file_dict中的'pb_model_file'地址）
pb_model_file = ''
# 测试数据路径（应包含test.txt文件）
data_dir = '/workspace/projects/PyCLUE_Corpus/sentence_pair/afqmc'
def submit_afqmc(predictor, submit_dir):
    test_data_file = os.path.join(data_dir, 'test.txt')
    submit_results = []
    ids = [item[0] for item in predictor.processor.read_file(file_path=test_data_file)]
    labels = [item['prediction'] for item in predictor.predict_from_file(input_file=test_data_file)]
    for index, label in zip(ids, labels):
        submit_results.append('{"id": "%s", "label": "%s"}\n' % (index, label))
    save_path = os.path.join(submit_dir, 'afqmc_predict.json')
    with open(save_path, 'w') as f:
        f.writelines(submit_results)
    return save_path
# 初始化预测器
predictor = Predictor(
    model_file=pb_model_file)
# 生成提交文件
save_path = submit_afqmc(
    predictor=predictor, data_dir=data_dir, submit_dir=pb_model_file)
print(save_path)
```

可以看到，它的核心就是不断的调用训练好的模型，然后把返回结果存储在一个文件中。再提交给服务器。实际上在服务器端会计算一下这个生成结果与标准结果的相似度。

而这个相似度的计算？可能是根据[词嵌入](https://zhida.zhihu.com/search?q=词嵌入&zhida_source=entity&is_preview=1)的相似度、句子嵌入的相似度、结构的相似度、特征的相似度等几方面的综合考量。

下面就是个词嵌入的相似度计算

```text
from transformers import BertModel, BertTokenizer
import torch
from sklearn.metrics.pairwise import cosine_similarity
# 初始化BERT模型和分词器
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
model = BertModel.from_pretrained('bert-base-uncased')
def get_sentence_embedding(sentence):
 # 将句子编码为BERT的输入格式
    inputs = tokenizer(sentence, return_tensors='pt', padding=True, truncation=True)
 # 获取句子的嵌入表示
 with torch.no_grad():
        outputs = model(**inputs)
        last_hidden_states = outputs.last_hidden_state
 # 取CLS标记的嵌入表示作为句子表示
    sentence_embedding = last_hidden_states[:, 0, :].numpy()
 return sentence_embedding
# 计算两个句子的相似度
sentence1 = "The quick brown fox jumps over the lazy dog"
sentence2 = "The fast red fox leaps over the sleepy dog"
embedding1 = get_sentence_embedding(sentence1)
embedding2 = get_sentence_embedding(sentence2)
# 使用余弦相似度计算相似度
similarity = cosine_similarity([embedding1], [embedding2])[0][0]
print(f"Similarity: {similarity}")
```



## 参考

[(97 封私信 / 83 条消息) 如何评估LLM？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/629846408/answer/3388166021)

