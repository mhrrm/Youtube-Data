# YouTube Konuşmalarından Parkinson Hastalığının Sınıflandırılması

## Genel Bakış

Bu depo, herkese açık YouTube videolarındaki konuşmaların Parkinson hastalığı (PD) ve sağlıklı kontrol (HC) olarak ikili sınıflandırılmasına yönelik bir araştırma veri kümesiyle ilişkili üst verileri sunmaktadır. Çalışmada beş saniyelik konuşma segmentleri kullanılmaktadır. Üst veriler, segment kayıtlarını, bunların sınıf etiketlerini ve ilgili kaynak videoların URL adreslerini tanımlamaktadır.

**Bu depoda WAV dosyaları, çıkarılmış ses klipleri veya başka herhangi bir ses kaydı dağıtılmamaktadır.** Kaynak video bağlantıları ve mevcut segment üst verileri, araştırmanın şeffaflığını, izlenebilirliğini ve yeniden üretilebilirliğini desteklemek amacıyla paylaşılmaktadır. Ses verilerini yeniden oluşturmak isteyen araştırmacılar, özgün videolara bağımsız olarak hukuka uygun erişim sağlamalı ve belgelenmiş ses çıkarma ve ön işleme prosedürünü izlemelidir.

## Veri Kümesine Genel Bakış

Veri kümesi, segment düzeyinde dengeli olan **2.470 adet beş saniyelik segment kaydı** içermektedir:

| Sınıf | Etiket | Segment sayısı | Nominal segment süresi |
|---|---|---:|---:|
| Parkinson hastalığı | PD | 1.235 | 1:42:55 |
| Sağlıklı kontrol | HC | 1.235 | 1:42:55 |
| **Toplam** | — | **2.470** | **3:25:50** |

## Depodaki Dosya ve Üst Veri Yapısı

Mevcut birleştirilmiş üst veri çalışma kitabı şöyledir:

- `Youtube_URL_Dagitilmis_2470.xlsx`
  - **`Sayfa1`**: Segment düzeyindeki üst veriler (2.470 satır).
  - **`Dagitim Kontrolu`**: Her sınıf için 1.235 kayıt oluşturmak üzere yapılan düzenlemeler sonrasında kaynak video URL’lerinin talep edilen ve uygulanan tekrar sayılarını karşılaştıran denetim sayfası. Bu sayfa bir dağılım denetimidir; segment zaman damgalarını veya bağımsız olarak doğrulanmış konuşmacı kimliklerini içermez.

### `Sayfa1` İçindeki Sütunlar

| Sütun | Anlamı | Örnek |
|---|---|---|
| `speaker_id` | Mevcut çalışma kitabındaki kayıt tanımlayıcısı; **gerçek dünyadaki benzersiz bir konuşmacıya ait doğrulanmış kimlik değildir** | `P01`, `H01` |
| `class` | İkili sınıf ataması | `PD`, `HC` |
| `youtube_url` | İlişkili kaynak YouTube videosunun bağlantısı | `https://www.youtube.com/watch?v=...` |

Her satır bir segment kaydına karşılık gelir. `speaker_id` alanı, kayıt etiketlerinin sırasını (`P01`–`P1235` ve `H01`–`H1235`) izler ve ayrı bir doğrulanmış konuşmacı eşlemesi olmadan, konuşmacıdan bağımsız değerlendirmede katılımcıları gruplandırmak için **kullanılmamalıdır**.

Mevcut çalışma kitabı, **kayıt başına `start` ve `end` zaman damgalarını**, segmentlerin özgün video içindeki kesin zaman konumlarını veya kayıtları bireysel konuşmacılarla ilişkilendiren doğrulanmış bir eşlemeyi **içermemektedir**. Bu nedenle üst veriler, kaynak videoların ve segment kayıtlarının belirlenmesini destekler; ancak **tek başına her beş saniyelik ses aralığının tam olarak yeniden çıkarılmasına olanak sağlamaz**. Verilerin bütünüyle yeniden oluşturulabilmesi için gelecekte yayımlanacak üst verilerde, her segmentin başlangıç ve bitiş zamanı (veya başlangıç zamanı ile süresi), ilgili ses çıkarma parametreleri ve konuşmacı grup kimlikleri belirtilmelidir.

## Etiket Eşlemesi

İkili sınıflandırma için etiketler aşağıdaki gibi kodlanabilir:

```text
HC = 0  # Sağlıklı kontrol
PD = 1  # Parkinson hastalığı
```

Bunlar, veri kümesindeki kayıtlara atanmış araştırma etiketleridir; yalnızca video üst verilerine dayanılarak bağımsız biçimde doğrulanmış klinik tanılar olarak yorumlanmamalıdır.

## Ses Çıkarma ve Ön İşleme

Deneylerin tekrarlanması için öngörülen işlem akışı şöyledir:

1. Videonun erişilebilirliğinin devam etmesi ve geçerli izinler koşuluyla, `youtube_url` üzerinden özgün videoya erişin.
2. Özgün segmentasyon günlüğünü veya bağımsız olarak belgelenmiş başlangıç/bitiş zaman damgalarını kullanarak hedef konuşma aralığını belirleyin (**bunlar mevcut çalışma kitabında bulunmamaktadır**).
3. Listede art arda gelen kayıtların bir videodaki ardışık zaman aralıklarını temsil ettiğini varsaymadan, belirtilen beş saniyelik segmenti çıkarın.
4. Deneysel düzenekle uyumluysa sesi mono biçime dönüştürün ve hedef örnekleme hızına (örneğin 16 kHz) yeniden örnekleyin.
5. Model girdisini beş saniye uzunluğunda tutarak belgelenmiş normalizasyon, kırpma ve doldurma kurallarını uygulayın.
6. Sınıfı `class` sütunundan atayın ve kaynak bağlantısını koruyun.

Yukarıdaki örnekleme hızı ve ön işleme seçenekleri, örnek bir tekrarlama iş akışını tanımlamaktadır; kesin değerler, yazılım sürümleri ve segmentasyon kararları deneyle birlikte raporlanmalıdır. URL tekrar denetimi, segmentlerin kesin başlangıç konumlarını çıkarsamak için kullanılmamalıdır.

## Değerlendirme ve Veri Sızıntısı

Dağılım **segment düzeyinde** dengelidir; konuşmacı düzeyinde dengeli olması gerekmez. Aynı videodan veya konuşmacıdan elde edilen birden fazla segment istatistiksel olarak birbirine bağımlıdır. Bu nedenle, satır düzeyinde rastgele bir eğitim/test ayrımı aşırı iyimser sonuçlar üretebilir.

**Konuşmacıdan bağımsız** değerlendirme için araştırmacılar öncelikle konuşmacı kimliklerini doğrulamalı ve **aynı kişiye ait tüm segmentleri** aynı çapraz doğrulama katında gruplandırmalıdır. Bu grup tanımlayıcılarıyla `StratifiedGroupKFold` veya grupları dikkate alan eşdeğer bir protokol kullanılabilir. Yalnızca kaynak video URL’sine göre gruplandırma, aynı videodan kaynaklanan veri sızıntısını azaltabilir; ancak bir konuşmacı birden fazla videoda yer alıyorsa konuşmacı bağımsızlığını garanti etmez. Bu çalışma kitabındaki satır düzeyindeki `speaker_id` değerlerini konuşmacı grubu tanımlayıcısı olarak **kullanmayın**.

Her sınıftaki farklı ve doğrulanmış konuşmacı sayısının, video sayısının, her kattaki sınıf ve konuşmacı dağılımlarının, eğitim/test gruplandırma kurallarının ve metriklerin kat ortalamalarından mı yoksa birleştirilmiş kat dışı tahminlerden mi hesaplandığının raporlanması önerilir.

## Önerilen Değerlendirme Metrikleri

Doğruluk, kesinlik, geri çağırma/duyarlılık, özgüllük, F1 skoru, ROC-AUC ve karışıklık matrisleri; değerlendirme protokolü ve uygun durumlarda belirsizlik tahminleriyle birlikte raporlanabilir.

## Yeniden Üretilebilirlik ve Erişilebilirlik Beyanı

**Özgün WAV ses dosyaları ve çıkarılmış beş saniyelik ses segmentleri paylaşılmamaktadır.** Bunun yerine depo, izlenebilirliği ve yeniden üretilebilirliği desteklemek için kaynak YouTube video URL’lerini, segment kaydı tanımlayıcılarını, sınıf atamalarını ve URL dağılım denetimini sunmaktadır. Özgün videolar, ilgili yayıncıların barındırma ve kontrolü altında kalmaktadır. Erişilebilirlik ve içerik değişebilir veya videolar kaldırılabilir.

**Mevcut sınırlılık:** Kayıtlar beş saniyelik segmentleri temsil etse de yayımlanan çalışma kitabında, segmentlerin tam olarak çıkarılması ve konuşmacı düzeyinde veri sızıntısı olmadan deneylerin tekrarlanması için gerekli olan segment başına kaynak video zaman konumları veya doğrulanmış konuşmacı kimlikleri bulunmamaktadır. Tam yeniden üretilebilirlik hedefleniyorsa bunlar ayrı bir segmentasyon ve konuşmacı eşleme dökümünde sunulmalıdır. Özgün bağlantıların sağlanması, bağımsız olarak indirilen bir ses dosyasının özgün analizi tam olarak yeniden üretmesini garanti etmez.

## Etik, Telif Hakkı ve Hukuki Hususlar

Kaynak videolar ve bu videoların sesleri, ilgili içerik üreticilerinin haklarına, YouTube’un hizmet şartlarına ve yürürlükteki gizlilik ve veri koruma mevzuatına tabidir. Bir URL’nin paylaşılması, görsel-işitsel içeriği indirme, yeniden kullanma veya yeniden dağıtma izni vermez. Araştırmacılar, kendi veri işleme faaliyetleri için uygun izinlere veya hukuki dayanağa sahip olduklarından emin olmakla sorumludur. Burada hiçbir özgün WAV dosyası yeniden dağıtılmamaktadır.

## Sorumluluk Reddi

Bu üst veriler yalnızca bilimsel araştırma ve yöntemsel değerlendirme amacıyla sunulmaktadır. Bu kayıtlar kullanılarak eğitilen bir sınıflandırma modeli, klinik tanı cihazı değildir ve uygun dış doğrulama ve uzman gözetimi olmadan klinik tarama, tanı veya tedavi kararları için kullanılmamalıdır.
