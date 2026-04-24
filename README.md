# LangChain ile Şablonlu Prompt Kullanımı
> PromptTemplate ile dinamik prompt oluşturma ve Groq üzerinde Llama 3.1 modeline gönderme — RAG serisinin 2. adımı

[![Colab'da Aç](https://img.shields.io/badge/Colab'da%20Aç-F9AB00?style=flat-square&logo=googlecolab&logoColor=white)](https://colab.research.google.com/github/yasir237/rag-langchain-2/blob/main/rag_langchain_2.ipynb)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/yasir-alrawi-12814521a/)

---

## Problem

Her seferinde aynı prompt'u elle yazmak hata yaratır ve ölçeklenmez. Onlarca farklı kavramı modele açıklatmak istediğinde her seferinde yeni bir string mi yazacaksın?

## Çözüm

LangChain'in `PromptTemplate` yapısı, prompt'u bir şablona dönüştürür. Değişken kısmı soyutlarsın — model her çağrıda aynı kaliteyi tutturur. Bu yapı, RAG pipeline'larında belge içeriğini dinamik olarak prompt'a enjekte etmenin temelidir.

---

## Zincir Mimarisi

```
Girdi (concept: "autoencoder")
        │
        ▼
┌─────────────────────────────────┐
│         LangChain Zinciri       │
│                                 │
│  PromptTemplate  →  Şablon doldurma  │
│  ChatGroq        →  Model çağrısı    │
└─────────────────────────────────┘
        │
        ▼
  Groq API (Llama 3.1 8B)
        │
        ▼
     Cevap
```

| Bileşen | Görevi |
|---|---|
| `PromptTemplate` | Değişken içeren prompt şablonunu tanımlar |
| `input_variables` | Şablona dışarıdan geçirilecek parametreleri belirtir |
| `chain = prompt \| chat` | LangChain Expression Language (LCEL) ile zincir kurar |
| `chain.invoke(...)` | Zinciri çalıştırır ve cevabı döndürür |

---

## Kullanılan Teknolojiler

![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=flat&logo=langchain&logoColor=white)
![Groq](https://img.shields.io/badge/Groq-F55036?style=flat&logoColor=white)
![Llama](https://img.shields.io/badge/Llama_3.1_8B-0467DF?style=flat&logo=meta&logoColor=white)
![Python](https://img.shields.io/badge/Python_3-3776AB?style=flat&logo=python&logoColor=white)
![Colab](https://img.shields.io/badge/Google_Colab-F9AB00?style=flat&logo=googlecolab&logoColor=white)

---

## Kurulum

```bash
pip install langchain langchain-core langchain-groq
```

### API Anahtarı

Google Colab **Secrets** sekmesine `GROQ_API_KEY` ekle.  
Groq API anahtarı almak için → [console.groq.com](https://console.groq.com)

---

## Kullanım

```python
from langchain_core.prompts import PromptTemplate
from langchain_groq import ChatGroq

template = """
You are an expert data scientist with an expertise in building deep learning models.
Explain the concept of {concept} in a couple of lines
"""

prompt = PromptTemplate(
    input_variables=["concept"],
    template=template,
)

chat = ChatGroq(model="llama-3.1-8b-instant", temperature=0)

chain = prompt | chat

response = chain.invoke({"concept": "autoencoder"})
print(response.content)
```

---

## Seri İçindeki Yeri

Bu notebook, LangChain ile kurulan RAG serisinin **2. adımıdır.**

```
[1] ✅ Mesaj yapısı ve LLM bağlantısı
[2] ✅ PromptTemplate ile şablonlu prompt   ← bu repo
[3]    Çoklu zincir kurma ve zincirleri bağlama
[4]    Metin bölme ve embedding
[5]    Vektör store ve benzerlik araması
[6]    Uçtan uca RAG pipeline
```

Her adım bir sonrakine köprü kuruyor.  
Serinin tamamını takip etmek için LinkedIn profilimi ziyaret edebilirsin 👇

---

## Bağlantı

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/yasir-alrawi-12814521a/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/yasir237)

---

## Lisans

MIT
