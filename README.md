# Veri Ön İşleme ve Özellik Mühendisliği Günlüğü (Data Preprocessing & Feature Engineering)

Bu depo, makine öğrenmesi modellerini eğitmeden önce ham veriyi temizlemek, dengelemek ve algoritmaların anlayabileceği matematiksel formata getirmek için kullanılan **Veri Ön İşleme (Data Preprocessing)** ve **Kategorik Veri Dönüştürme (Encoding)** tekniklerini içermektedir.

"Çöp girerse, çöp çıkar (Garbage in, garbage out)" ilkesinden yola çıkarak, bu aşamada verinin istatistiksel dağılımını korumak ve modellerin yanlılık (bias) göstermesini engellemek amaçlanmıştır.

---

## 📂 Proje Yapısı

```text
├── 01_Encoding_Techniques.ipynb       # Kategorik verileri sayısallaştırma çalışmaları
├── 02_Resampling_and_SMOTE.ipynb     # Dengesiz veri setlerini dengeleme çalışmaları
└── README.md                         # Proje dokümantasyonu ve teorik özet
```

🧠 Teorik Özet ve Öğrenimler
1. Yeniden Örnekleme (Resampling) Teknikleri
Dengesiz veri setlerinde (imbalanced data) sınıf dağılımını eşitlemek için uygulanan stratejiler:

Rastgele Upsampling: Azınlık sınıfındaki verileri birebir kopyalar. En büyük riski modele aynı verileri tekrar tekrar göstererek Overfitting (Aşırı Öğrenme/Ezberleme) yaratmasıdır.

Downsampling: Çoğunluk sınıfından rastgele veri siler. En büyük riski kıymetli istatistiksel varyansların yok olmasına yani Bilgi Kaybına (Information Loss) yol açmasıdır.

SMOTE (Synthetic Minority Over-sampling Technique): Azınlık sınıfındaki noktaların en yakın komşularını (k-NN) bulur, aralarında çizgiler çeker ve bu çizgiler üzerinde tamamen yeni, sentetik veri noktaları üretir. Ezber riskini azaltır.

⚠️ SMOTE'un Kör Noktaları ve Riskleri

Gürültüyü Çoğaltmak: Çoğunluk sınıfının arasına sızmış aykırı (outlier) bir azınlık verisi varsa, SMOTE bunun etrafına da veri üretir ve iki sınıfı ayıran Karar Sınırını (Decision Boundary) bulandırır.

Hayalet Profiller (İnterpolasyon Hatası): Sadece matematiksel ortalamaya odaklandığı için, gerçek dünyada (örneğin tıbbi veya fiziksel olarak) asla var olamayacak saçma sentetik profiller üretebilir.

🛡️ Gelişmiş Temizlik Yöntemleri

SMOTE-Tomek: Sınırda burun buruna gelmiş, modeli yanıltacak çoğunluk ve azınlık noktalarını (Tomek Links) silerek yüzeysel bir sınır temizliği yapar.

SMOTE-ENN: Yanlış mahalleye düşmüş, etrafı farklı sınıflarla sarılmış gürültülü noktaları acımasızca silen daha agresif ve derinlemesine bir temizlik yöntemidir.

🚨 ÖLÜMCÜL HATA: Veri Sızıntısı (Data Leakage)

Veri ön işlemedeki en büyük tuzak, resampling işlemlerini veriyi Train/Test olarak bölmeden önce tüm veri setine uygulamaktır. Eğer bölmeden önce SMOTE yaparsanız; algoritma, Test setine gitmesi gereken noktaların konumlarına bakarak Train setine sentetik veriler üretir. Model, henüz görmemesi gereken sınav sorularının cevaplarını eğitim aşamasında "kopya çekmiş" olur. Kağıt üzerinde başarı %99 görünür ancak gerçek dünyada model tamamen çuvallar.

Doğru Akış Sıralaması:

Veriyi train_test_split ile böl.

Test setini kasaya kilitle (asla dokunma).

Sadece Train setine SMOTE/Encoding uygula.

Modeli eğit ve en son kilitli Test setiyle sına.
