Ayrıştırma Hesaplama Düşenme Nedir?
Bilişimsel düşünme, mantıklı düşünmenin ve problemleri organize bir şekilde çözmenin bir yoludur. Bir probleme yapılandırılmış bir şekilde yaklaşma ve böyle bir problem için bir sistem veya makine tarafından kolayca gerçekleştirilebilecek bir cevap oluşturma ve ifade etme sürecidir. Bu sadece programcılar için değil, farklı alanlarda da uygulanabilir. Hesaplamalı düşünme bilgisine sahip olmak, çeşitli nicel ve veri ile ilgili problemler çözülebilir ve aynı zamanda size gerçek dünya problemlerini çözmek için bir temel sağlayacaktır. Hesaplamalı düşünme kişiden kişiye farklılık gösterir ve diyelim ki bir kişi bir algoritma kullanmayı düşünüyorsa, farklı kişiler algoritmaları farklı şekillerde düşünecektir, örneğin bir bilim adamı bunu deneysel bir prosedür olarak düşünebilir,

Neden Ayrıştırma Kullanılır?

Karmaşık problemlerin kolayca çözülmesine yardımcı olur.
Ayrıştırılan alt görevler, farklı kişiler veya bir grup kişi tarafından gerçekleştirilebilir veya çözülebilir (eğer kişi sorunun tamamı hakkında yeterli bilgiye sahip değilse).
Problem alt görevlere ayrıldığında her bir alt görev detaylı olarak incelenebilir.

Nerede Kullanılır?

Bilgisayar biliminde ayrıştırmanın en iyi örneği Özyineleme'dir. Özyinelemede, bir problemi basitleştirmek ve daha büyük problemi alt parçalara bölmek ve ardından alt parçaların her birini çözmek için kullanırız. Günlük yaşamınızda kullanılan gerçek hayattaki yaygın örneklerden bazıları aşağıda verilmiştir:

Bilgisayar programlama kullanarak yeni bir oyun yapmak istiyorsanız, oyunu yapmak için oyundaki farklı eylemler için farklı işlevler yazacaksınız ve daha sonra bu işlevler alt programlar veya alt görevler olarak hareket edecek, ardından bu alt görevler tam bir oyun oluşturmak için bir araya getirilmelidir.
Sporda bazen takımlar rakip takımları daha zayıf takımlara böler, aksi takdirde daha güçlü bir bütün halinde birleşirler.
Evinizi temizlemek istediğinizde, görevlerinizi yapmak için yapılacaklar listesi yaparsınız.
Bir Bilim projesi yaparken, arka plan araştırması, deney yapma, tez yazma ve teslim etme vb. gibi çeşitli şeyler yapmamız gerekir. Bu da ayrıştırmanın harika bir örneği olacaktır.
Kişi yeni bir dili ve yeni bir yabancı dili özne, fiil, nesne vb. alt parçalara ayırarak cümle kurmayı öğrenir.
Başka bir örnek matematik problemi olabilir, Aşağıdaki gibi 256.37 sayısı 2*10 2 +5*10 1 +6*10 0 +3*10 -1 +7*10 -2

Bilgisayar biliminde ayrıştırmanın iyi bir örneği, birleştirme sıralama algoritması olabilir. Bu algoritmada diziyi iki parçaya böleriz, iki parça için kendisini çağırırız ve sonra sıralanan iki parçayı tek parçada birleştiririz. Aşağıdaki birleştirme sıralama algoritması , dizinin boyutu 1 olana kadar özyinelemeli olarak 2 parçaya bölündüğünü göstermektedir. 1 olunca, birleştirme işlemi gerçekleştirilir ve dizi tamamen birleştirilir.
MergeSort(dizi[], l, r)

sağ > sol ise

1. Orta noktayı bulma ve arr'ı 2 yarıya bölme  

            orta m = 1 + (sağ – sol) / 2

2. İlk yarı için mergeSort'u arayın:    

            Çağrı birleştirmeSort(dizi, sol, m)

3. İkinci yarı için mergeSort'u arayın:

            Çağrı birleştirmeSort(dizi, m + 1, sağ)

4. 2 ve 3 yarıyı bir arada birleştirmek:

            Çağrı birleştirme(dizi, sol, m, sağ)