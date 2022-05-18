# Görüntülerden Hava Durumu Kümeleme Problemi
## Özet

Görüntülerden hava durumu kümeleme problemi, hareket halindeki otonom arabalardan alınan görüntülerin aşağıdaki beş hava durumu kategorisinden birinde sınıfladırılmasıdır.
Beş hava durumu kategorisi aşağıdaki gibidir:
1.	Hava açık
2.	Yol biraz ıslak ve hava biraz bulutlu
3.	Islak yol, bulutlu hava ve hafif yağmur
4.	Alacakaranlıkta veya şafak vaktinde yağmurlu hava.
5.	Gece şiddetli yağmur ve ıslak yollar.

Projenin açıklamasını içeren web sitesindeki veri setini bu beş hava durumu kategorisinde kümelemek için otonom arabalardan alınan görüntülerin özelliklerini Keras framework’ünün InceptionV3, VGG16 ve MobileNet gibi görüntü sınıflandırma modelleriyle çıkarttık. Çıkardığımız bu özelliklere göre görüntüleri beş ayrı kümeye ayırmak için k-Means algoritmasını kullandık.

K-Means algoritmasının bu görüntü sınıflandırma modellerinin hangisiyle daha iyi çalıştığını ölçmek için Silhoutte Score ve Calinski-Harabasz Index performans metriklerini kullandık.

Sonuç olarak MobileNet görüntü sınıflandırma modelinin bu 5 kümeyi daha iyi ayırdığı kanısına vardık.
## Giriş
Görüntülerden hava durumu kümeleme problemi, hareket halindeki otonom arabalardan alınan görüntülerin beş hava durumu kategorisinden birinde sınıfladırılmasıdır.

Problemi çözerken benzer problemler ve çözümleri hakkında bir literatür taraması yaptık. Yaptığımız bu taramada kaggle üzerinde günlük hava durumu verilerinin k-Means algoritmasıyla soğuk, sıcak ve kuru hava olacak şekilde üç kümeye ayrılmasını sağlayan bir çalışmayı ve towardsdatascience üzerinde bulunan bir blogun InceptionV3 image classification modeli ve KMeans algoritmasını kullanarak kedi ve köpek kümelemesini yapan bir çalışmayı örnek aldık.

Sınıflandırma problemlerinde etiketli veri ya da eğitim verisi olmadığında bu problemi çözmek için benzer görüntüleri kümeleyebiliriz. 

Kümeleme(Clustering) benzer özellikteki verileri grupladığımız denetimsiz bir makine öğrenmesidir. Bunu girdileri yorumlayıp özellik uzayından doğal -kümeler- gruplar oluşturarak yapar.

Kümeleme algoritmalarının ne kadar başarılı çalıştığını ölçmek için bazı performans metrikleri vardır. Bu performans metrikleri genel olarak küme içi(intra-cluster) uzaklıların en az ve kümeler arası(inter-cluster) uzaklıkların en fazla olmasına göre skorlar oluşturur. Biz de bu performans metriklerinden bizim geliştirdiğimiz modele uygun olan iki tanesi ile modelimizin performansını ölçtük.
## Yöntem
Sınıflandırma ve Regresyon görevleri Denetimli Öğrenme denilen şeyi oluştururken, Kümeleme Denetimsiz Öğrenme görevlerinin çoğunu oluşturur. Bu iki makro alan arasındaki fark, kullanılan veri türünde yatmaktadır. Denetimli Öğrenmede örnekler kategorik bir etiketle (Sınıflandırma) veya sayısal bir değerle (Regresyon) etiketlenirken, Denetimsiz Öğrenmede örnekler etiketlenmez, bu da gerçekleştirmeyi ve değerlendirmeyi nispeten karmaşık bir görev haline getirir. 
## Image Classification Models in Keras

### VGG16
Çok fazla hyperparam yerine daha sade bir yapı kullanılır

Sinir ağları mimarisini sadeleştirir
 
16 ifadesi parametreli 16 katmanı olduğu anlamına gelir
 
138m parametresi vardır bu da normale göre oldukça fazladır.

### Inceptionv3
Inception v3 Google'ın Inception Convolutional Neural Network'ün üçüncü baskısıdır

Görüntü analizine ve nesne algılamaya yardımcı olmak için oluşturulan bir sinir ağıdır.

Inceptionv3'ün tasarımı, daha derin ağlara izin verirken aynı zamanda parametre sayısının çok fazla büyümesini önlemeyi amaçlıyor.

### Mobilenet
Mobilenet, Normal CNN'ler tarafından yapılan normal evrişimden farklı olan Derinlik evrişim ve nokta evrişim fikrini kullanır. Bu evrişim yolları, karşılaştırma ve tanıma süresini çok azalttığından, çok kısa sürede daha iyi bir yanıt sağlar.

Ağlarda aynı derinliğe sahip düzenli kıvrımlara sahip ağ ile karşılaştırıldığında parametre sayısını önemli ölçüde azaltır ve bu, CNN'nin görüntüleri tahmin etme verimliliğini arttırır, dolayısıyla mobil sistemlerde de rekabet edebilirler ve oldukça sık kullanılırlar. 

Düşük hesaplama tüketimi ve hızlı çalışma hızıyla cep telefonlarında çalışırlar, bu nedenle mobil terminallerdeki uygulamalar için çok uygundurlar.
