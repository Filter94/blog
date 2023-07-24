[category]: <> (Türkçe)
[date]: <> (2016/12/29)
[title]: <> ([Mirror] Bir Proof of Stake Tasarım Felsefesi)
[pandoc]: <> (--mathjax)

_Bu, <a href="https://medium.com/@VitalikButerin/a-proof-of-stake-design-philosophy-506585978d51">https://medium.com/@VitalikButerin/a-proof-of-stake-design-philosophy-506585978d51</a> adresindeki yazının bir aynası(kopyası)dır._

Ethereum gibi sistemler (ve Bitcoin, NXT, Bitshares vb.), temel olarak yeni bir kriptoekonomik organizma sınıfıdır - merkeziyetsiz, yargı alanı olmayan, tamamen siber uzayda var olan ve kriptografi, ekonomi ve toplumsal fikir birliği (konsensüs) kombinasyonuyla sürdürülen varlıklardır. BitTorrent gibi görünebilirler, ancak BitTorrent'un state (durum) kavramı yoktur - son derece önemli olduğu ortaya çıkan bir ayrım. Bazen [merkeziyetsiz otonom şirketler](https://letstalkbitcoin.com/is-bitcoin-overpaying-for-false-security) olarak tanımlanır, ancak tam anlamıyla şirket de değillerdir - Microsoft'u hard forklayamazsınız. Açık kaynak yazılım projelerine benzeyebilir, ancak tam olarak da öyle değillerdir - bir blok zincirini forklayabilirsiniz, ancak OpenOffice'i forklamak kadar kolay değildir.

Bu kriptoekonomik ağlar birçok farklı aromayla gelir - ASIC-based PoW, GPU-based PoW, naive PoS, delegated PoS, umarım yakında Casper PoS - ve her biri kaçınılmaz olarak kendi temel felsefesiyle birlikte gelir. İyi bilinen bir örnek, proof of work'ün maksimalist vizyonudur, burada "doğru" blok zinciri, madencilerin yaratmak için en büyük miktarda ekonomik sermayeyi harcadığı zincir olarak tanımlanır. Başlangıçta yalnızca bir protokol içi fork choice kuralı olan bu mekanizma, birçok durumda kutsal bir dogma olarak yüceltilmiştir - hash algoritmasını değiştiren protokol hard fork'ları karşısında bile fikrin saf halini ciddi şekilde savunmaya çalışan birinin bir örneği için [Chris DeRose ile benim aramdaki bu Twitter tartışmasına](https://twitter.com/vitalikbuterin/status/687050458301657088) bakın. Bitshares'in [delegated proof of stake'i](https://bitshares.org/technology/delegated-proof-of-stake-consensus/) başka bir tutarlı felsefeyi sunuyor, burada her şey yine tek bir temel ilke üzerinden akıyor, ancak çok daha da basit bir şekilde ifade edilebilir: [hissedarlar oy kullanır](http://docs.bitshares.org/bitshares/dpos.html).

Bu felsefelerin her biri; Nakamoto konsensüsü, sosyal konsensüs, hissedar oyları konsensüsü, kendi sonuçlarına götürür ve kendi terimleriyle bakıldığında oldukça anlamlı olan bir değerler sistemine yol açar - ancak birbirleriyle karşılaştırıldıklarında kesinlikle eleştirilebilirler. Casper konsensüsünün de felsefi bir temeli var, ancak bu şimdiye kadar açık bir şekilde makaleye alınmamıştır.

Ben, Vlad, Dominic, Jae ve diğerleri, proof of stake protokollerinin neden var olduğu ve nasıl tasarlandığı konusunda kendi görüşlerimize sahibiz, ancak burada kişisel olarak nereden geldiğimi açıklamayı amaçlıyorum.

Gözlemlerimi ve ardından direkt sonuçları listeleyeceğim.

<br>

* Kriptografi, 21. yüzyılda gerçekten özeldir çünkü **kriptografi, düşmanca çatışmalarda büyük ölçüde savunanın lehine olmaya devam ettiği çok az alandan biridir**. Kaleleri yok etmek inşa etmekten çok daha kolaydır, adalar savunulabilir ancak yine de saldırılabilir, ancak ortalama bir kişinin ECC anahtarları devlet düzeyindeki aktörlere bile direnecek kadar güvenlidir. Cypherpunk felsefesi, temel olarak, bireyin özerkliğini daha iyi koruyan bir dünya yaratmak için bu değerli asimetriden yararlanmakla ilgilidir ve kriptoekonomi bir dereceye kadar bunun bir uzantısıdır, ancak bu sefer sadece özel iletilerin bütünlüğü ve gizliliği değil, karmaşık koordinasyon ve işbirliği sistemlerinin güvenliği ve canlılığını koruması gerekir. **Kendilerini cypherpunk ruhunun ideolojik mirasçıları olarak gören sistemler, bu temel özelliği korumalı ve sistemi yok etmeleri veya bozmaları, sistemi kullanıp sürdürmekten çok daha pahalı olmalıdır.**

* "Cypherpunk ruhu" sadece idealizmle ilgili değildir; Savunması saldırmaktan daha kolay olan sistemler yapmak da aslında sağlam mühendisliktir.

* **Orta ila uzun zaman ölçeklerinde, insanlar fikir birliğinde (konsensüs) oldukça iyidir**. Bir düşman sınırsız hash gücüne erişimi olsa ve herhangi bir büyük blokzincirine %51 saldırısı gerçekleştirerek tarihin son ayını geri alsa bile, topluluğu bu zincirin doğru olan olduğuna ikna etmek, ana zincirin hash gücünü aşmaktan çok daha zor. Blok explorerları, topluluktaki her güvenilir üyeyi, New York Times'ı, archive.org'u ve internetteki diğer birçok kaynağı yıkmaları gerekecektir; genel olarak, bilgi teknolojisinin yoğun olduğu 21. yüzyılda yeni saldırı zincirinin asıl zincir olduğunu dünyaya ikna etmek, ABD'nin Ay'a inişinin gerçekleşmediğini dünyaya ikna etmek kadar zordur. **Bu sosyal dikkate almalar, blokzinciri topluluğunun kabul edip etmemesine bakılmaksızın, herhangi bir blok zincirini uzun vadede koruyan şeydir** ([dikkate alın,](https://www.reddit.com/r/bitcoinxt/comments/41pbmf/maxwell_considers_changing_the_pow_algorithm_in/) Bitcoin Core sosyal katmanın önemini [kabul ediyor](https://www.reddit.com/r/Bitcoin/comments/3fg0jw/could_a_cartel_of_pool_operators_collude_to/ctoat0d/)).

* Ancak tek başına sosyal konsensüs ile korunan bir blokzinciri, çok verimsiz ve yavaş olacaktır ve bitmeyen anlaşmazlıkların gerçekleşmesi daha kolay olacaktır (tüm zorluklara rağmen, [böyle bir şey yaşandı](http://www.npr.org/sections/money/2011/02/15/131934618/the-island-of-stone-money)); dolayısıyla **ekonomik konsensüs, kısa vadede canlılık ve güvenlik özelliklerinin korunmasında son derece önemli bir rol oynuyor**.

* Proof of Work güvenliği sadece blok ödüllerinden geliyor (Dominic Williams'ın deyimiyle, [üç E'den ikisi eksik](https://twitter.com/dominic_w/status/648330685963370496)), madencilerin teşviki yalnızca gelecekteki blok ödüllerini kaybetme riskiyle geliyor. Bu nedenle, **proof of work mantıksal olarak büyük ödüllerle var olmasına teşvik edilen büyük güç üzerine dayanır**. PoW'da saldırılardan kurtulmak çok zordur: İlk kez gerçekleştiğinde PoW'i değiştirmek için bir hard fork yapabilir ve saldırganın ASIC'lerini işe yaramaz hale getirebilirsiniz, ancak ikinci kez bu seçeneğiniz olmaz ve saldırgan tekrar tekrar saldırabilir. Bu nedenle, madencilik ağı o kadar büyük olmalıdır ki saldırılar düşünülemez hale gelmelidir. X'ten daha küçük saldırganlar, ağın her gün sürekli olarak X harcamasıyla ortaya çıkmaktan (saldırmaktan) vazgeçirilir. **Ben bu mantığı reddediyorum çünkü (i) [ağaçları öldürüyor](http://digiconomist.net/beci) ve (ii) cypherpunk ruhunu tanımayı başaramıyor - saldırı maliyeti ve savunma maliyeti 1:1 oranında, bu yüzden herhangi bir savunma avantajı yok**.

* **Proof of stake, güvenliği ödüllere dayanmak yerine cezalara dayandırarak bu simetriyi kırar**. Doğrulayıcılar (Validator) para ("mevduat") riske eder, sermayelerini kitlemeleri ve node'larını sürdürmeleri ve private key'lerinin güvenliğini sağlamak için önlemler almaları karşılığında bir miktar ödül alır, ancak işlemleri geri alma maliyetinin büyük bir kısmı, bu süre zarfında aldıkları ödüllerden yüzlerce veya binlerce kat daha büyük olan cezalardan kaynaklanır. **Bu nedenle Proof of stake'in tek cümlelik felsefesi, "güvenlik yanan enerjiden gelir" değil, "güvenlik, ekonomik kayba dayalı değer koymaktan gelir" şeklindedir**. Kötü niyetli node'lar, takasın X$ değerinde protokol içi cezalar ödemesini sağlamak için suç ortağı olmadıkça, tutarsız herhangi bir blok veya durum için eşit bir sonlandırma düzeyi elde etmenin mümkün olmadığını kanıtlayabilirseniz, belirli bir blok veya durum X$ güvenliğine sahiptir.

* Teorik olarak, Çoğunluktaki doğrulayıcıların(validator) gizli anlaşmaları bir PoS zincirini devralabilir ve kötü niyetli davranmaya başlayabilir. Ancak, (i) akıllıca bir protokol tasarımıyla, bu tür manipülasyonlar ile ek kar elde etme yetenekleri mümkün olduğunca sınırlanabilir ve daha da önemlisi (ii) eğer yeni doğrulayıcıların katılmasını engellemeye çalışırlar veya %51 saldırısı gerçekleştirirlerse, topluluk basitçe bir hard fork koordine ederek söz konusu kötü niyetli doğrulayıcıların mevduatlarını silme yetisine sahiptir. **Başarılı bir saldırı 50 milyon dolara mal olabilir, ancak sonuçlarının temizlenmesi süreci, [2016.11.25'teki geth/parity konsensüs hatasından](https://blog.ethereum.org/2016/11/25/security-alert-11242016-consensus-bug-geth-v1-4-19-v1-5-2/) *çok daha külfetli* olmayacaktır**. İki gün sonra, blokzinciri ve topluluk eski haline dönerken, saldırganlar 50 milyon dolar daha fakir ve topluluğun geri kalanı muhtemelen daha zengin çünkü saldırı, müteakip arz sıkışıklığı nedeniyle tokenin değerinin *yükselmesine* neden oldu. *İşte* size saldırı/savunma asimetrisi.

* Yukarıdaki ifade, plansız hard fork'ların düzenli bir şekilde meydana geleceği anlamına gelmemelidir. İstenirse, proof of stake'teki *bir tane* 51% saldırısının maliyeti, proof of work'te *kalıcı* 51% saldırısı yapmakla aynı derecede yüksek olabilir ve saldırının yüksek maliyeti ve etkisizliği, pratikte neredeyse hiç denemeye kalkışılmamasını sağlar.

* **Ekonomi her şey değildir.** Bireysel aktörler, protokol dışı nedenlerle davranabilirler, hacklenebilirler, kaçırılabilirler veya sadece bir gün sarhoş olup blokzincirini mahvetmeye ve maliyetini umursamamaya karar verebilirler. Ayrıca, olumlu yönden baktığımızda, **bireylerin ahlaki sınırlamaları ve iletişim eksiklikleri, bir saldırının maliyetini nominal protokol tanımlı değer kaybı seviyesinden çok daha yüksek seviyelere çıkaracaktır**. Bu, güvenemeyeceğimiz bir avantaj, ancak aynı zamanda gereksiz yere görmezden gelmememiz gereken bir avantajdır.

* **Bu nedenle, en iyi protokoller, çeşitli modeller ve varsayımlar altında iyi çalışan protokollerdir **- koordine edilmiş seçimlerle ekonomik rasyonellik, bireysel seçimlerle ekonomik rasyonellik, basit hata toleransı, Bizans hata toleransı (ideal olarak hem uyarlamalı hem de uyarlamasız saldırgan varyantları), [Ariely/Kahneman'dan esinlenmiş davranışsal ekonomik modeller](https://www.amazon.ca/Honest-Truth-About-Dishonesty-Everyone-Especially/dp/0062183613) ("hepimiz biraz hile yaparız") ve mümkünse gerçekçi ve mantıklı düşünülebilen diğer herhangi bir model. **Her iki savunma katmanına da sahip olmak önemlidir: merkezi kartellerin anti-sosyal davranmasını engellemek için ekonomik teşvikler, en başta kartellerin oluşmasını engellemek için anti-merkeziyetçi teşvikler.**

* **Olabildiğince hızlı çalışan konsensüs protokollerinin riskleri vardır ve eğer varsa çok dikkatli bir şekilde yaklaşılmalıdır**, çünkü çok hızlı olma *olasılığı* bunu yapmaya yönelik *teşviklere* bağlıysa, bu kombinasyon **ağ düzeyinde çok yüksek ve sistemik risk oluşturan bir merkezileşme** seviyesini ödüllendirir (örneğin, tüm doğrulayıcıların aynı hosting sağlayıcısından çalışması). Doğrulayıcıların bir mesajı ne kadar hızlı gönderdiğini çok fazla umursamayan konsensüs protokolleri, bunu kabul edilebilir uzun bir zaman aralığı içinde yaptıkları sürece (örneğin, 4-8 saniye gibi, çünkü Ethereum'daki gecikme süresinin genellikle ~500ms-1s olduğunu gözlemledik) böyle endişeler taşımaz. Mümkün olan bir orta nokta, çok hızlı çalışabilen ancak Ethereum'un amca(uncle) mekanizmasına benzer mekanikler taşıyan, bir node'un ağ bağlantısının kolayca ulaşılabilecek bir noktanın ötesine geçmesi durumunda marjinal ödülün oldukça düşük olduğunu sağlayan, protokoller oluşturmaktır.

Buradan itibaren, elbette birçok detay ve detaylarda farklı yollar var, ancak yukarıdakiler en azından benim Casper versiyonumun dayandığı temel prensipleridir. Buradan itibaren, rekabet eden değerler arasında tercihleri tartışabiliriz. ETH'e yıllık %1 oranında bir arz oranı verelim ve düzeltici bir hard fork için 50 milyon dolar maliyet alalım mı, yoksa yıllık %0 oranında bir arz oranı verelim ve düzeltici bir hard fork için 5 milyon dolar maliyet alalım mı? Ekonomik model altında bir protokolün güvenliğini artırmak için hata tolere edilebilirlik modeli altında güvenliğini azaltma değiş tokuşunu ne zaman yaparız? Tahmin edilebilir bir güvenlik seviyesine mi yoksa tahmin edilebilir bir arz seviyesine mi daha çok önem veriyoruz? Bu soruların hepsi başka bir post'un konusu ve bu değerler arasındaki farklı değiş tokuşları *uygulamanın* çeşitli yolları, daha da fazla post'un konusu. Ama onlara da geleceğiz :)