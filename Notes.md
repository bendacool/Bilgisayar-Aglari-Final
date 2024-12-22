# Modül 14 

### netstat Komutu

Kullanılan protokolleri, yerel adres ve port numaralarını, yabancı adresi ve port numaralarını ve bağlantı durumunu listelemek için aşağıda gösterildiği gibi **netstat** komutu kullanılır.



### TCP Bağlantısı Kurma

- __1. Adım SYN:__ İşlemi başlatan istemci, sunucu ile istemci-sunucu iletişimi kurma talebinde bulunur.
- __2. Adım ACK ve SYN:__ Sunucu, istemciden sunucuya iletişim oturumunu onaylar ve sunucudan istemciye iletişim oturumu talep eder.
- __3. Adım ACK:__ İşlemi başlatan istemci, sunucudan istemciye iletişim oturumunu onaylar.



### Oturum Sonlandırma

Bağlantıyı kapatmak için segment başlığında Son (FIN) kontrol işareti ayarlanmalıdır. Her bir tek yönlü TCP oturumunu bitirmek için FIN segmenti ve ACK segmentinden oluşan iki yollu tokalaşma kullanılır. Dolayısıyla TCP tarafından desteklenen tek bir sohbeti sonlandırmak için iki oturumu da bitirmeye yönelik olarak dört değişim gereklidir. İstemci veya sunucu sonlandırmayı başlatabilir.

- __1. Adım FIN:__ İstemcinin akışta gönderecek daha fazla verisi kalmadığında, FIN bayrağı ayarlanmış bir segment gönderir.
- __2. Adım ACK:__ Sunucu, oturumu istemciden sunucuya sonlandırmak için FIN'in alındığını onaylamak için bir ACK gönderir.
- __3. Adım FIN:__ Sunucu, sunucudan istemciye oturumu sonlandırmak için istemciye bir FIN gönderir.
- __4. Adım ACK:__ İstemci, sunucudan FIN'i onaylamak için bir ACK ile yanıt verir.



### TCP Üç Yollu Tokalaşma Analizi

üç yönlü el sıkışmanın fonksiyonları:
- Hedef aygıtın ağda mevcut olduğunu belirler.
- Hedef cihazın aktif bir hizmete sahip olduğunu ve başlatan istemcinin kullanmayı planladığı hedef port numarasındaki istekleri kabul ettiğini doğrular.
- Hedef cihaza, kaynak istemcinin bu port numarası üzerinde bir iletişim oturumu kurmayı planladığını bildirir.



### Kontrol Bitleri Alanı

- **URG** - Acil işaretçi alanı belirteci
- **ACK** - Bağlantı kurulması ve oturum sonlandırılmasında kullanılan onay bayrağı
- **PSH** - İtme işlevi
- **RST** - Bir hata veya zaman aşımı oluştuğunda bağlantıyı sıfırlama
- **SYN** - Bağlantı kurulmasında kullanılan sıra numaralarını senkronize eder
- **FIN** - Gönderenden daha fazla veri yoksa ve oturum sonlandırılmasında kullanılıyor



### TCP Güvenilirliği - Garantili ve Sipariş Edilen Teslimat

TCP'nin bazı uygulamalar için daha iyi bir protokol olmasının nedeni, teslim edilmeden önce uygun sıralarını göstermek için bırakılan paketleri ve numaraları paketlerini yeniden göndermesidir. TCP, aygıtların aşırı yüklenmemesi için paketlerin akışını korumaya da yardımcı olabilir. Bu konu, TCP'nin bu özelliklerini ayrıntılı olarak kapsar.

Oturum kurulumu sırasında ilk sıra numarası (ISN) ayarlanır. ISN bu oturum için alıcı uygulamaya iletilen baytlara yönelik başlangıç değerini temsil eder. Veriler oturum sırasında iletildikçe, sıra numarası iletilmiş bayt sayısı kadar artılır. Bu veri baytı takibi, her bir segmentin benzersiz şekilde tanımlanmasına ve onaylanmasına olanak tanır. Kayıp segmentler belirlenebilir. ISN birde başlamaz, ancak etkin bir şekilde rastgele bir sayıdır. Bunun amacı, belirli kötü amaçlı saldırıları önlemektir.

TCP Segmentleri Hedefte Yeniden Sıralanır.



### TCP Güvenilirliği - Veri Kaybı ve Yeniden İletme

Örneğin, bilgisayar A 1 ile 10 arasındaki kesimleri B bilgisayarına gönderir. Tüm segmentler 3 ve 4 segmentleri dışında gelirse, bilgisayar B beklenen sonraki segmentin 3 olduğunu belirterek bildirim ile yanıt verir. Host A'nın diğer bölümlerin gelip gelmediği konusunda hiçbir fikri yoktur. Bilgisayar A, bu nedenle, segmentleri 3'ten 10'a yeniden gönderir. Tüm yeniden gönderilen segmentler başarıyla ulaştıysa, 5 ile 10 arasındaki segmentler yinelenmiş olacaktır. Bu da gecikmelere, tıkanıklığa ve verimsizliklere yol açabilir.

Bilgisayar işletim sistemleri bugün genellikle üç yönlü el sıkışma sırasında anlaşılan seçici bildirim (SACK) adı verilen isteğe bağlı bir TCP özelliği kullanır. Her iki bilgisayar da SACK'ı destekliyorsa, alıcı kesimleri de dahil olmak üzere hangi kesimlerin(bayt) alındığını açıkça kabul edebilir. Bu nedenle gönderen bilgisayarın eksik verileri yeniden aktarması gerekir. Örneğin, bilgisayar A 1 ile 10 arası bölümlerini B bilgisayarına gönderir. Tüm segmentler 3 ve 4 hariç gelirse, bilgisayar B, 1 ve 2 (ACK 3) segmentlerini aldığını ve 5 ile 10 (SACK 5-10) seçici olarak kabul edebilir. bilgisayar A yalnızca 3 ve 4 segmentlerini yeniden gönderir.




### TCP Akış Kontrolü - Pencere Boyutu ve Onayları

TCP ayrıca akış kontrolü için mekanizmalar sağlar. Akış kontrolü, hedefin alabileceği ve güvenilir bir şekilde işleyebileceği veri miktarıdır. Akış kontrolü,kaynakla hedef arasındaki veri akışı hızını ayarlayarak TCP iletiminin güvenirliğini korumaya yardımcı olur. TCP başlığı, pencere boyutu adı verilen 16 bitlik bir alan içermektedir. Pencere boyutu, bir alındı bildirimi beklenmeden gönderilebilecek bayt sayısını belirler. Alındı numarası, bir sonraki beklenen baytın numarasıdır.

Örneğin, bir TCP oturumu için ilk pencere boyutu 10,000 bayta ayarlıdır. İlk bayt, bayt numarası 1 ile başlar, son bayt 10000 olarak biter. Bu, PC A'nın gönderme penceresi olarak bilinir. Pencere boyutu her TCP segmentine dahil edilir, böylece hedef, ara bellek kullanılabilirliğine bağlı olarak pencere boyutunu herhangi bir zamanda değiştirebilir.

TCP oturumu üç yönlü el sıkışma sırasında kurulduğunda, ilk pencere boyutu üzerinde anlaşılır. Kaynak aygıt, hedef aygıtın pencere boyutuna göre hedef aygıta gönderilen bayt sayısını sınırlamalıdır. Sadece kaynak cihaz, baytların alındığına dair bir onay aldıktan sonra oturum için daha fazla veri göndermeye devam edebilir. Tipik olarak hedef, bir alındı bildirimi ile yanıtlamadan önce tüm baytların alınmasını beklemeyecektir. Baytlar alınıp işlendikçe, hedef ek bayt göndermeye devam edebileceğini bildirimlerini kaynağı bilgilendirmek için gönderir. Örneğin, PC B'nin bir alındı bildirimi göndermeden önce 10.000 baytın tamamının alınmasını beklememesi normaldir. Bu, PC A'nın gönderme penceresini PC B'den bildirimler aldığı gibi, bir sonraki beklenen bayt olan 2,921 numaralı bir bildirim aldığında PC A gönderme penceresi 2,920 bayt artar. Bu, gönderme penceresi 10.000 bayttan 12.920 olarak değiştirir. PC A, 12.920'deki yeni gönderme penceresinden fazlasını göndermediği sürece PC B'ye 10.000 bayta kadar daha göndermeye devam edebilir. PC B, alınan baytları işlerken onaylar.

Gönderen bir hedef ve kaynak gönderme penceresinin sürekli ayarlanması, kayan pencereler olarak bilinir. 

**Not:** Günümüzde cihazlar kayan pencere protokolünü kullanır. Alıcı genellikle aldığı her iki segment kesiminden sonra bir bildirim gönderir. Kabul edilmeden önce alınan segmentlerin sayısı değişebilir. Kayar pencerelerin avantajı, alıcının önceki segmentleri onayladığı sürece göndericinin segmentleri sürekli olarak iletmesine izin vermesidir.



### TCP Akış Kontrolü - Maksimum Kesim Boyutu (MSS)

Örneğin, kaynak her TCP segmenti içinde 1.460 bayt veri iletiyor. Genellikle hedef aygıtın alabileceği en büyük kesim boyutu (MSS) budur. MSS, bir aygıtın tek bir TCP segmentinde alabileceği en büyük veri miktarını bayt cinsinden belirten TCP başlığındaki seçenekler alanının bir parçasıdır. MSS boyutu TCP üstbilgisini içermez. MSS genellikle üç yönlü el sıkışma sırasında dahil edilir.

IPv4 kullanılırken ortak MSS 1.460 bayttır. Bir bilgisayar, IP ve TCP başlıklarını Ethernet maksimum iletim biriminden (MTU) çıkararak MSS alanının değerini belirler. Ethernet arabiriminde varsayılan MTU 1500 bayttır. 20 baytlık IPv4 üstbilgisini ve 20 baytlık TCP üstbilgisini çıkarma, varsayılan MSS boyutu şekilde gösterildiği gibi 1460 bayt olacaktır.



### TCP Akış Kontrolü - Maksimum Kesim Boyutu (MSS)

Ağda tıkanıklık oluştuğunda, paketlerin aşırı yüklenen yönlendirici tarafından atılmasına neden olur. TCP kesimleri içeren paketler hedeflerine ulaşmadığında, bunlar onaylanmamış kalır. Kaynak, TCP segmentlerinin gönderildiği ancak onaylanmadığı hızı belirleyerek, belirli bir ağ tıkanıklığı düzeyini varsayabilir. 

Tıkanıklık olduğunda, kayıp TCP segmentlerinin kaynaktan yeniden iletimi gerçekleşir. Yeniden iletim düzgün kontrol edilmezse, TCP segmentlerinin ek yeniden iletimi tıkanıklığı daha da kötüleştirebilir. Ağa yalnızca TCP segmentlerine sahip yeni paketler dahil edilmekle kalmaz, aynı zamanda kaybolan yeniden iletilen TCP segmentlerinin geri bildirim etkisi de tıkanıklığı artıracaktır. Sıkışıklığı önlemek ve kontrol etmek için, TCP çeşitli tıkanıklık işleme mekanizmaları, zamanlayıcılar ve algoritmalar kullanır.



### UDP Veri Birimi Yeniden Birleştirme

TCP'li segmentler gibi, UDP datagramları bir hedefe gönderildiğinde, genellikle farklı yollar kullanırlar ve yanlış sırada ulaşırlar. UDP sıra numaralarını TCP'nin yaptığı gibi izlemez. Bu yüzden, UDP sadece veriyi alındığı sırada yeniden birleştirip uygulamaya iletir.



### UDP İstemci İşlemleri

UDP istemci işlemi, port numaraları aralığından dinamik olarak bir port numarası seçer ve bunu konuşma için kaynak port olarak kullanır. Hedef port, genellikle sunucu işlemine atanmış iyi bilinen veya kayıtlı port numarasıdır. Bir müşteri kaynak ve hedef bağlantı noktalarını seçtikten sonra, işlemdeki tüm datagramların başlığında aynı port çifti kullanılır. Veri birimi başlığındaki kaynak ve hedef port numaraları, sunucudan istemciye dönen veriler için tersine çevrilir.

UDP'DE Three-Way Handshake YOK!!




# Modül 15

### Uygulama Katmanı

Uygulama katmanı son kullanıcıya en yakın katmandır. İletişim için kullanılan uygulamalar ile mesajların iletildiği temel ağ arasındaki arayüzü sağlayan katmandır. Uygulama katmanı protokolleri, kaynak ve hedef hostlarda çalışan programlar arasında veri alıp göndermek için kullanılır.

Birçok uygulama katmanı protokolü vardır ve her zaman yeni protokoller geliştirilmektedir. En çok bilinen bazı uygulama katmanı protokolleri şunlardır: HTTP, FTP, TFTP, IMAP ve DNS.



### Sunum ve Oturum Katmanı
##### Sunum Katmanı

Sunum katmanının üç ana işlevi bulunmaktadır:
- Kaynak cihazdaki verileri, hedef aygıt tarafından alınacak şekilde uyumlu bir biçimde biçimlendirme veya sunma.
- Verileri, hedef cihaz tarafından açılabilecek şekilde sıkıştırma.
- Verilerin iletim için şifrelenmesi ve alındıktan sonra verilerin şifresinin çözülmesi.

Sunum katmanı, verileri uygulama katmanı için biçimlendirir ve dosya biçimleri için standartları belirler.

Video için iyi bilinen bazı standartlar arasında MKV, MPG ve MOV bulunur. Bazı iyi bilinen grafik görüntü formatları GIF, JPG ve PNG biçimidir.

##### Oturum Katmanı

Oturum katmanındaki işlevler kaynak ve hedef uygulama arasındaki diyaloğu oluşturur ve sürdürür. Oturum katmanı diyalogların başlatılması ve etkin tutulmasının yanı sıra, kesintiye uğrayan veya uzun süre boşta kalan oturumların başlatılması için gereken bilgi alışverişini gerçekleştirir.



### TCP/IP Uygulama Katmanı Protokolleri

**DNS - Domain Name System (or Service)**
- TCP, UDP istemcisi 53
- cisco.com gibi alan adlarını IP adreslerine çevirir.

**BOOTP - Bootstrap Protocol**
- UDP istemcisi 68, sunucu 67
- Disksiz bir iş istasyonunun kendi IP adresini, ağdaki bir BOOTP sunucusunun IP adresini ve makineyi yüklemek için belleğe yüklenecek bir dosyayı bulmasına olanak tanır
- BOOTP'nin yerini DHCP almaktadır

**DHCP - Dynamic Host Configuration Protocol**
- UDP istemcisi 68, sunucu 67
- Artık ihtiyaç duyulmadığında IP adreslerini yeniden kullanılacak şekilde dinamik olarak atar.

**SMTP - Simple Mail Transfer Protocol**
- TCP 25
- İstemcilerin bir posta sunucusuna e-posta göndermesini sağlar
- Sunucuların diğer sunuculara e-posta göndermesini sağlar

**POP3 - Post Office Protocol**
- TCP 110
- İstemcilerin bir posta sunucusundan e-posta almasını sağlar  
    E-postayı istemcinin yerel posta uygulamasına* yükler

**IMAP - Internet Message Access Protocol**
- TCP 143
- İstemcilerin bir posta sunucusunda depolanan e-postaya erişmesini sağlar
- E-postayı sunucuda tutar

**FTP - File Transfer Protocol**
- TCP 20 - 21
- Bir hosttaki kullanıcının başka bir hosttaki dosyalara ağ üzerinden erişmesini veya aktarıp almasını sağlayan kuralları belirler
- FTP, güvenilir, bağlantı odaklı ve onaylanmış bir dosya dağıtım protokolüdür

**TFTP - Trivial File Transfer Protocol**
- UDP istemcisi 69
- En iyi çabayla, onaylanmamış dosya teslimi ile basit, bağlantısız dosya aktarım protokolü
- FTP'den daha az ek yük kullanır

**HTTP - Hypertext Transfer Protocol**
- TCP 80, 8080
- World Wide Web'de metin, grafik görüntü, ses, video ve diğer multimedya dosyalarını alıp vermek için bir dizi kural

**HTTPS - HTTP Secure**
- TCP, UDP 443
- Tarayıcı HTTP iletişimini güvence altına almak için şifreleme kullanır. 
- Tarayıcınızı bağladığınız web sitesinin kimliğini doğrular.



### İstemci-Sunucu Modeli

İstemci, kişilerin sunucuda depolanan kaynaklara doğrudan erişmek için kullandıkları bir donanım/yazılım birleşimidir.

İstemci ve sunucu işlemlerinin uygulama katmanında olduğu kabul edilir. İstemci, sunucudan veriler isteyerek alışverişi başlatır ve sunucu, istemciye bir veya daha fazla kesintisiz veri yayını göndererek yanıt verir. Uygulama katmanı protokolleri, istemciler ve sunucular arasındaki istek ve yanıtların biçimini tanımlar. Veri aktarımının kendisine ek olarak, bu alışveriş kullanıcı kimliğinin doğrulanmasını ve aktarılacak veri dosyasının tanımlanmasını gerektirebilir.

İstemci / sunucu ağının bir örneği, e-posta göndermek, almak ve saklamak için bir ISS'nin e-posta hizmetini kullanmaktır. Ev bilgisayarındaki e-posta istemcisi, okunmamış postalar için ISS'nin e-posta sunucusuna bir istek gönderir. Sunucu istenen e-postayı istemciye göndererek yanıt verir. İstemciden sunucuya veri aktarımı yükleme ve sunucudan istemciye veri indirme olarak adlandırılır.



### Eşler Arası Ağ

Eşler arası (P2P) ağ modelinde verilere özel bir sunucu kullanılmadan bir eş cihazdan erişilir.

P2P ağ modelinde iki bölüm bulunur: P2P ağları ve P2P uygulamaları. İki bölüm de benzer özelliklere sahip olmakla birlikte uygulamada farklı şekilde çalışır.

Bir P2P ağında iki veya daha fazla bilgisayar bir ağ üzerinden birbirine bağlanır ve özel bir sunucu olmaksızın kaynakları paylaşabilirler. Bağlı olan her bir cihaz (eş olarak bilinir), hem sunucu hem de istemci işlevini görebilir. Bir bilgisayar bir işlem için sunucu rolünü üstlenirken aynı anda bir başka bilgisayar için istemci görevi görüyor olabilir. İstemci ve sunucu rolleri, her istek için ayrı ayrı belirlenir.

Dosya paylaşımına ek olarak, bunun gibi bir ağ, kullanıcıların ağa bağlı oyunları etkinleştirmesine veya bir internet bağlantısını paylaşmasına izin verecektir.

Eşler arası bir alışverişte, iki cihaz da iletişim sürecinde eşit olarak ele alınır. Eş 1, Eş 2 ile paylaşılan dosyalara sahiptir ve dosyaları yazdırmak için Eş 2'ye doğrudan bağlı olan paylaşılan yazıcıya erişebilir.



### Peer-to-Peer Applications

Bir P2P uygulaması, bir cihazın aynı iletişim içinde hem istemci hem de sunucu olarak hareket etmesini sağlar. Bu modelde her istemci bir sunucudur ve her sunucu bir istemcidir. P2P uygulamaları, her uç cihazın bir kullanıcı arayüzü sağlamasını ve bir arka plan hizmeti çalıştırmasını gerektirir.

Bazı P2P uygulamaları kaynak paylaşımının dağıtıldığı hibrit bir sistem kullanırlar, fakat kaynak konumlarına işaret eden dizinler merkezi bir dizinde depolanır. Hibrit bir sistemde her eş, başka bir eşte depolanan kaynağın konumunu öğrenmek için bir dizin sunucusuna erişir.

Yaygın P2P uygulamaları:
- BitTorrent
- Direct Connect
- eDonkey
- Freenet

Bazı P2P uygulamaları, her kullanıcının tüm dosyaları diğer kullanıcılarla paylaştığı Gnutella protokolüne dayanır. Gnutella uyumlu istemci yazılımı, kullanıcıların internet üzerinden Gnutella servislerine bağlanmasına ve diğer Gnutella eşleri tarafından paylaşılan kaynakların yerini bulmasına ve bunlara erişmesine izin verir. μTorrent, BitComet, DC++, Deluge ve emule dahil olmak üzere birçok Gnutella istemci uygulaması mevcuttur.

Birçok P2P uygulaması, kullanıcıların birçok dosyanın parçasını aynı anda birbirleriyle paylaşmasına izin verir. İstemciler, ihtiyaç duydukları parçalara sahip diğer kullanıcıları bulmak için bir torrent dosyası kullanırlar, böylece doğrudan onlara bağlanabilirler. Bu dosya, belirli kullanıcıların belirli dosya parçalarına sahip olduğunu izleyen izleyici bilgisayarlar hakkında da bilgi içerir. Müşteriler aynı anda birden fazla kullanıcıdan parça ister. Bu swarm(sürü) olarak bilinir ve teknolojiye BitTorrent denir. BitTorrent'in kendi istemcisi vardır. Ancak başka birçok BitTorrent istemcisi de vardır.

**Not:** Herhangi bir dosya türü kullanıcılar arasında paylaşılabilir. Bu dosyaların çoğunun telif hakkı vardır, yani yalnızca içerik oluşturucusunun bunları kullanma ve dağıtma hakkı vardır. Telif hakkı olan dosyaları telif hakkı sahibinin izni olmadan indirmek veya dağıtmak yasalara aykırıdır. Telif hakkı ihlali cezai suçlamalara ve sivil davalara neden olabilir.



### Hypertext Transfer Protocol ve Hypertext Markup Language

Bir web adresi veya Tekdüzen Kaynak Konum Belirleyicisi(Uniform Resource Locator - URL) bir web tarayıcısına yazıldığında, web tarayıcısı web hizmetiyle bağlantı kurar. Web hizmeti HTTP protokolünü kullanan sunucuda çalışır. URL'ler ve Uniform Resource Identifiers (URI'ler) pek çok kişinin web adresleriyle ilişkilendirdiği adlardır.



### HTTP ve HTTPS

HTTP, bir istek/yanıt protokolüdür. Genellikle bir web tarayıcısı olan bir istemci bir web sunucusuna istek gönderdiğinde, HTTP bu iletişim için kullanılan ileti türlerini belirtir. Üç yaygın mesaj türü GET, POST ve PUT'tur:

- **GET** - Bu, veri için bir istemci isteğidir. Bir istemci (web tarayıcısı) HTML sayfalarını istemek için web sunucusuna GET mesajını gönderir.
- **POST** - Bu, web sunucusuna form verileri gibi veri dosyaları yükler.
- **PUT** - Bu, web sunucusuna görüntü gibi kaynakları veya içeriği yükler.

HTTP ciddi ölçüde esnek olmakla beraber, güvenli bir protokol değildir. İstek iletileri, sunucuya ele geçirilebilen ve okunabilen düz metin olarak bilgi gönderir. Sunucu yanıtları, genellikle HTML sayfaları da şifrelenmez.

İnternet üzerinden güvenli iletişim için, HTTP Güvenli(HTTPS) protokolü kullanılır. HTTPS, istemci ve sunucu arasında dolaşırken verileri korumak için kimlik doğrulama ve şifreleme kullanır. HTTPS, HTTP ile aynı istemci isteği-sunucu yanıtı sürecini kullanır, fakat kesintisiz veri yayını ağ üzerinden taşınmadan önce Güvenli Yuva Katmanı(SSL) ile şifrelenir.



### E-posta Protokolleri

İSS'nin sunduğu ana hizmetlerden biri e-posta barındırmadır. Bir uç cihazda çalışmak için e-posta, birkaç uygulama ve hizmet gerektirir. E-posta, elektronik mesajların bir ağ üzerinde gönderilmesi, depolanması ve alınması için depola ve ilet yöntemini kullanır. E-posta mesajları posta sunucularındaki veri tabanlarında depolanır.

E-posta istemcileri, e-posta almak ve göndermek için posta sunucularıyla iletişim kurar. Posta sunucuları mesajları bir alandan diğerine taşımak için diğer posta sunucularıyla iletişim kurar. E-posta gönderilirken, e-posta istemcisi doğrudan başka bir e-posta istemcisiyle iletişim kurmaz. Bunun yerine iki istemci de mesajların taşınması için posta sunucusunu kullanır.

E-posta, işlem için üç ayrı protokolü destekler: Basit Posta Aktarım Protokolü (SMTP), Postane Protokolü(POP) ve IMAP. Posta gönderen uygulama katmanı işlemi SMTP kullanır. İstemci, iki uygulama katmanı protokollerinden birini kullanarak e-posta alır: POP veya IMAP.

##### **SMTP**

SMTP mesaj biçimleri için bir mesaj başlığı ve mesaj gövdesi gerekir. İleti gövdesi herhangi bir miktarda metin içerebiliyor olsa da, ileti üstbilgisinin düzgün biçimlendirilmiş bir alıcı e-posta adresine ve bir gönderen adresine sahip olması gerekir.

Bir istemci e-posta gönderdiğinde iyi bilinen port 25'te bir sunucu SMTP süreciyle bağlantı kurar. Bağlantı kurulduktan sonra istemci, bağlantı üzerinden e-postayı gönderme girişiminde bulunur. Sunucu iletiyi aldığında, alıcı yerelse iletiyi yerel bir hesaba yerleştirir veya iletiyi teslim edilmek üzere başka bir posta sunucusuna iletir.

Hedef e-posta sunucusu, e-posta mesajları gönderildiğinde çevrim içi olmayabilir veya meşgul olabilir. Bu nedenle SMTP, mesajları daha sonra göndermek üzere kuyruğa alır. Sunucu, düzenli aralıklarla kuyruğu mesajlar için kontrol eder ve bunları yeniden göndermeyi dener. Mesaj önceden belirlenen bir geçerlilik süresinden sonra hala teslim edilmediyse, teslim edilemedi olarak gönderene iade edilir.

##### **POP**

POP, bir posta sunucusundan posta almak için bir uygulama tarafından kullanılır. POP'ta posta sunucudan istemciye indirilir, daha sonra da sunucudan silinir. Bu, POP'un varsayılan işlemidir.

Sunucu, istemci bağlantı istekleri için TCP port 110'u pasif olarak dinleyerek POP hizmetini başlatır. Bir istemci, hizmeti kullanmak istediğinde, sunucuyla bir TCP bağlantısı kurma isteği gönderir. Bağlantı kurulduğunda, POP sunucusu bir karşılama mesajı gönderir. Ardından istemci ve POP sunucusu bağlantı kapatılana veya iptal edilene kadar komut ve yanıt değişimi gerçekleştirir.

POP ile e-posta mesajları istemciye indirilir ve sunucudan kaldırılır, bu nedenle e-posta mesajlarının tutulduğu merkezi bir konum yoktur. POP iletileri depolanmadığından, merkezi bir yedekleme çözümüne ihtiyaç duyan küçük bir işletme için önerilmez.

POP3 en sık kullanılan sürümüdür.

##### **IMAP**

IMAP, e-posta mesajlarını alma yöntemini tanımlayan başka bir protokoldür. POP'un aksine, kullanıcı IMAP özellikli bir sunucuya bağlandığında, mesajların kopyaları istemci uygulamasına indirilir. Orijinal mesajlar el ile silinene kadar sunucuda tutulur. Kullanıcılar mesajların kopyalarını e-posta istemci yazılımlarında görüntüler.

Kullanıcılar postaları düzenlemek ve depolamak için sunucuda bir dosya hiyerarşisi oluşturabilir. Bu dosya yapısı e-posta istemcisine de kopyalanır. Kullanıcı bir mesajı silmeye karar verdiğinde, sunucu bu eylemi eşitler ve mesajı sunucudan siler.



### Etki Alanı Adı Hizmeti

Veri ağlarında, cihazlar ağlar üzerinden veri gönderebilmek ve alabilmek için sayısal IP adresleriyle etiketlenir. Etki alanı adları, sayısal adresi basit, tanınabilir adlara dönüştürmek için oluşturulmuştur.

İnternette, `http://www.example.com` gibi tam nitelikli alan adları (FQDN'ler), insanların hatırlaması için 198.133.219.25'ten daha kolaydır.

DNS protokolü, kaynak adları gerekli sayısal ağ adresleriyle eşleyen otomatik bir hizmet tanımlar. Sorguların, yanıtların ve verilerin biçimini içerir. DNS protokolü iletişimleri mesaj(ileti) olarak adlandırılan tek bir formatı kullanır. İstemci sorgularının ve sunucu yanıtlarının, hata mesajlarının ve kaynak kayıt bilgilerinin sunucular arasında aktarımının tüm türleri için bu mesaj formatı kullanılır.

__1. Adım:__ Kullanıcı, bir tarayıcı uygulaması Adres alanına bir FQDN yazar.
__2. Adım:__ DNS sorgusu, istemci bilgisayar için belirlenen DNS sunucusuna gönderilir.
__3. Adım:__ DNS sunucusu, FQDN'yi IP adresiyle eşleştirir.
__4. Adım:__ DNS sorgusu yanıtı, FQDN için IP adresiyle istemciye geri gönderilir.
__5. Adım:__ İstemci bilgisayar, sunucudan istekte bulunmak için IP adresini kullanır.



### DNS Mesaj Formatı

DNS sunucusu, adları çözümlemek için kullanılan farklı türde kaynak kayıtları saklar. Bu kayıtlar adı, adresi ve kayıt türünü içerir. Bu kayıt türlerinden bazıları aşağıdaki gibidir:
- **A** - Bir uç aygıt IPv4 adresi
- **NS -** Yetkili bir ad sunucusu
- **AAAA** - Bir uç aygıtı IPv6 adresi
- **MX** - Bir posta değişim kaydı

İstemci bir sorgu yaptığında, sunucu DNS işlemi önce adı çözümlemek için kendi kayıtlarına bakar. Depoladığı kayıtları kullanarak adı çözemezse, adı çözümlemek için diğer sunucularla iletişim kurar. Bir eşleşme bulunup orijinal istekte bulunan sunucuya geri döndükten sonra, sunucu aynı adın yeniden istenmesi durumunda numaralandırılmış adresi geçici olarak saklar.

Windows bilgisayarlardaki DNS istemci hizmeti de önceden çözümlenmiş adları bellekte depolar. **ipconfig /displaydns** komutu, önbelleğe alınan tüm DNS girdilerini görüntüler.

| **DNS mesajı bölümü** | **Açıklama**                               |
| --------------------- | ------------------------------------------ |
| Soru                  | Ad sunucusu için soru                      |
| Cevapla               | Kaynak Kayıtları soruyu yanıtlıyor         |
| Yetki                 | Kaynak Kayıtları bir yetkiye işaret ediyor |
| Ek                    | Kaynak Kayıtları ek bilgiler içeriyor      |



### DNS Hiyerarşisi

DNS protokolü, ad çözümlemesi sağlamak için bir veritabanı oluştururken hiyerarşik bir sistem kullanır. DNS hiyerarşiyi şekillendirirken etki alanı adlarını kullanır.

Adlandırma yapısı küçük, yönetilebilir bölgelere ayrılmıştır. Her DNS sunucusu belirli bir veritabanı dosyasını tutar ve yalnızca tüm DNS yapısının bu küçük kısmı için ad-IP eşlemelerini yönetmekten sorumludur. Bir DNS sunucusu, ad dönüştürme için kendi DNS bölgesinin içerisinde olmayan bir istek aldığında, DNS sunucusu isteği dönüştürme için uygun bölge içerisindeki bir başka DNS sunucusuna iletir. Ana bilgisayar adı çözümlemesi birden çok sunucuya yayıldığı için DNS ölçeklenebilirdir.

Farklı üst düzey etki alanları ya kuruluş türünü ya da kaynak ülkeyi temsil eder. Üst düzey alanlara örnekler şunlardır:
- **com** - işletme veya endüstri
- **org** - kar amacı gütmeyen kuruluş
- **au-** Avustralya
- **co** Kolombiya



### Nslookup Komutu

Bir ağ aygıtını yapılandırırken, DNS istemcisinin ad çözümlemesi için kullanabileceği bir veya daha fazla DNS Sunucusu adresi sağlanır. Genellikle ISS, DNS sunucuları için kullanılacak adresleri sağlar. Bir kullanıcı uygulaması uzak bir aygıta adıyla bağlanmak istediğinde, istekte bulunan DNS istemcisi, adı sayısal bir adrese çözümlemek için ad sunucusunu sorgular.

Bilgisayar işletim sistemlerinde ayrıca, kullanıcının belirli bir ana bilgisayar adını çözümlemek için ad sunucularını el ile sorgulamasını sağlayan `nslookup` adlı bir yardımcı programı vardır. Bu yardımcı program, ad çözümleme sorunlarını gidermek ve ad sunucularının güncel durumlarını doğrulamak için de kullanılabilir.



### Dinamik Host Yapılandırma Protokolü

IPv4 hizmeti için Dinamik Host Yapılandırma Protokolü (DHCP), IPv4 adreslerinin, alt ağ maskelerinin, ağ geçitlerinin ve diğer IPv4 ağ parametrelerinin atanmasını otomatikleştirir. Bu dinamik adresleme olarak adlandırılır. Dinamik adreslemeye alternatif olarak statik adresleme bulunmaktadır. Statik adresleme kullanırken, ağ yöneticisi hostara IP adresi bilgilerini manuel olarak girer.

Daha büyük ağlarda veya kullanıcı sayısının sık sık değiştiği yerlerde, adres ataması için DHCP tercih edilir. Yeni kullanıcılar gelebilir ve bağlantılara ihtiyaç duyabilir, diğerlerinin bağlanması gereken yeni bilgisayarlar olabilir. Her bağlantı için statik adresleme kullanmak yerine, IPCP4 adreslerinin DHCP kullanılarak otomatik olarak atanması daha verimlidir.

DHCP, IP adreslerini, kiralama süresi adı verilen yapılandırılabilir bir süre için tahsis edebilir. Kira süresi önemli bir DHCP ayarıdır. Kira süresi sona erdiğinde veya DHCP sunucusu bir DHCPRELEASE iletisi aldığında, adres yeniden kullanılmak üzere DHCP havuzuna döndürülür. Kullanıcılar konumdan konuma serbestçe hareket edebilir ve DHCP aracılığıyla ağ bağlantılarını kolayca yeniden kurabilir.

Çeşitli aygıt türleri DHCP sunucuları olabilir. Çoğu orta-büyük ağdaki DHCP sunucusu genellikle yerel, özel bir PC tabanlı sunucudur. Ev ağlarında DHCP sunucusu genellikle ev ağını İSS'ye bağlayan yerel yönlendiricide bulunur.

Pek çok ağ hem DHCP, hem de statik adresleme kullanmaktadır. DHCP, son kullanıcı aygıtları gibi genel amaçlı ana bilgisayarlar için kullanılır. Statik adresleme ağ geçidi yönlendiricileri, anahtarlar, sunucular ve yazıcılar gibi ağ aygıtları için kullanılır.

IPv6 için DHCP (DHCPv6), IPv6 istemcileri için benzer hizmetler sağlar. Önemli bir fark, DHCPv6'nın varsayılan bir ağ geçidi adresi sağlamamasıdır. Bu, yalnızca yönlendiricinin Yönlendirici Reklamı mesajından dinamik olarak elde edilebilir.



### DHCP İşlemi

IPv4, DHCP yapılandırmalı bir aygıt önyükleme yaptığında veya ağa bağlandığında, istemci ağdaki kullanılabilir DHCP sunucularını tanımlamak için bir DHCP bulma (DHCPDISCOVER) iletisi yayınlar. DHCP sunucusu, istemciye bir kontrat sunan, DHCP teklif (DHCPOFFER) mesajı ile yanıt verir. Teklif iletisi, atanacak IPv4 adresini ve alt ağ maskesini, DNS sunucusunun IPv4 adresini ve varsayılan ağ geçidinin IPv4 adresini içerir. Kontrat teklifi, kontrat süresini de içerir.

Yerel ağda birden fazla DHCP sunucusu varsa, istemci birden çok DHCPOFFER iletisi alabilir. Bu nedenle, aralarında seçim yapmalı ve istemcinin kabul ettiği açık sunucuyu ve kiralama teklifini tanımlayan bir DHCP isteği (DHCPREQUEST) iletisi gönderir. İstemci sunucu tarafından daha önce atanmış bir adres için istekte bulunmayı seçebilir.

İstemci tarafından istenen veya sunucu tarafından sunulan IPv4 adresinin hala kullanılabilir olduğu varsayılarak, sunucu istemciye kiralama işleminin sonlandırıldığını onaylayan bir DHCP onayı (DHCPACK) iletisi döndürür. Teklif artık geçerli değilse, seçilen sunucu DHCP negatif alındı bildirimi (DHCPNAK) iletisiyle yanıt verir. DHCPNAK mesajı döndürülürse, seçim işlemi yeni bir DHCPDISCOVER mesajının iletilmesiyle yeniden başlamalıdır. İstemci kontratı aldıktan sonra, kontratın bitiş zamanından önce başka bir DHCPREQUEST mesajı ile kontrat yenilenmelidir.

DHCP sunucusu tüm IP adreslerinin benzersiz olmasını sağlar (aynı IP adresi, aynı anda iki farklı ağ cihazına atanamaz). Çoğu ISS, müşterilerine adresleri ayırmak için DHCP kullanır.

DHCPv6, DHCPv4'tekine benzer bir dizi iletiye sahiptir. DHCPv6 iletileri SOLICIT, REPORTISE, INFORMATION REQUEST ve REPLY'dir.



### Dosya Transfer Protokolü

Her iki cihaz da bir dosya aktarım protokolü (FTP) kullanıyorsa, istemci bir sunucuya veri yükleyebilir ve sunucudan veri indirebilir. HTTP, e-posta ve adresleme protokolleri gibi FTP yaygın olarak kullanılan uygulama katmanı protokolüdür. 

FTP, istemci ve sunucu arasında veri aktarımlarını sağlamak için geliştirilmiştir. FTP istemcisi, bir FTP sunucusundan veri göndermek ve almak için kullanılan bir bilgisayarda çalışan bir uygulamadır.

İstemci, TCP bağlantı noktası 21'i kullanarak denetim trafiği için sunucuya ilk bağlantıyı kurar. Trafik, istemci komutları ve sunucu yanıtlarından oluşur.

İstemci, TCP bağlantı noktası 20'yi kullanarak gerçek veri aktarımı için sunucuya ikinci bağlantıyı kurar. Bu bağlantı aktarılacak verinin olduğu tüm zamanlarda oluşturulur.

İki yöne de veri aktarımı yapılabilir. İstemci, verileri sunucudan indirebilir veya istemci, sunucuya veri yükleyebilir.



### Sunucu Mesajı Bloğu

Sunucu İleti Bloğu (SMB), dizinler, dosyalar, yazıcılar ve seri bağlantı noktaları gibi paylaşılan ağ kaynaklarının yapısını açıklayan bir istemci/sunucu dosya paylaşım protokolüdür. Bu bir istek-yanıt protokolüdür. Tüm SMB mesajları ortak bir formatı paylaşır. Bu format sabit boyutlu bir başlık, onu takip eden değişken boyutlu parametre ve veri bileşenini kullanır.

İşte SMB mesajlarının üç işlevi:
- Oturumları başlatma, kimlik doğrulaması yapma ve sonlandırma.
- Dosya ve yazıcı erişimini kontrol etme.
- Bir uygulamanın başka bir cihaza/cihazdan mesaj göndermesine veya cihazdan mesaj almasına izin verme.

SMB dosya paylaşımı ve yazıcı hizmetleri Microsoft ağ hizmetlerinin başlıca dayanağı olmuştur. Windows 2000 yazılım serisinin sunulmasıyla, Microsoft, SMB kullanımı için temel yapıyı değiştirmiştir. Microsoft ürünlerinin önceki sürümlerinde, SMB hizmetleri ad çözümlemesi için TCP/IP olmayan bir protokol kullanmıştır. Windows 2000'den başlayarak, sonraki tüm Microsoft ürünleri, şekilde gösterildiği gibi TCP / IP protokollerinin SMB kaynak paylaşımını doğrudan desteklemesine izin veren DNS adlandırma kullanır.



# Modül 16

### Tehdit Türleri

Bireyler ve kuruluşlar bilgisayarlarına ve ağlarına bağlıdır. Yetkisiz kişilerin izinsiz girişleri maliyeti yüksek ağ kesintilerine ve iş kaybına yol açabilir. Bir ağa yapılan saldırılar yıkıcı olabilir ve hasar, önemli bilgi ve varlıkların çalınması nedeniyle zaman ve para kaybına neden olabilir.

İzinsiz giriş yapanlar, yazılımdaki güvenlik açıklarını kullanarak, donanım saldırılarıyla veya kullanıcı adını ve parolayı tahmin ederek ağa erişim elde edebilirler. Yazılımı değiştirerek veya yazılım açıklarından yararlanarak erişim sağlayan davetsiz misafirlere tehdit oyuncuları(korsan) denir.

Tehdit oyuncusu ağa eriştikten sonra dört tür tehdit ortaya çıkabilir:

- **Bilgi Hırsızlığı**, gizli bilgileri elde etmek için bir bilgisayara girmektir. Bilgi çeşitli amaçlarla kullanılabilir veya satılabilir.
- **Veri Kaybı ve Manipülasyon**, veri kayıtlarını yok etmek veya değiştirmek için bilgisayara izinsiz erişimdir. 
- **Kimlik Hırsızlığı**, kişisel bilgilerin birinin kimliğini devralmak amacıyla çalındığı bir tür bilgi hırsızlığıdır. Bir tehdit oyuncusu bu bilgileri kullanarak yasal belgeler alabilir, kredi başvurusunda bulunabilir ve yetkisiz çevrim içi satın alma işlemleri gerçekleştirebilir. 
- **Hizmet Kesintisi**, meşru kullanıcıların hakları olan hizmetlere erişimini engelliyor. Örnekler: Sunucular, ağ aygıtları veya ağ iletişim bağlantılarına yönelik hizmet reddi(DoS) saldırıları.



### Güvenlik Açığı Türleri

Güvenlik açığı, bir ağdaki veya bir aygıttaki zayıflık derecesidir. Routerlar, Switchler, masaüstü bilgisayarları, sunucular ve hatta güvenlik cihazlarında bir dereceye kadar güvenlik açığı vardır. Saldırıya maruz kalan ağ cihazları genellikle sunucular ve masaüstü bilgisayarlar gibi uç noktalardır.

Üç temel güvenlik açığı veya zayıf yönü vardır: teknolojik, yapılandırma ve güvenlik politikası.

##### Teknolojik Güvenlik Açıkları

| Güvenlik Açığı             | Açıklama                                                                                                                                                                                                                                                                      |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| TCP/IP Protokolü Zayıflığı | - HTTP, FTP ve ICMP doğal olarak güvenli değildir. <br>- SNMP ve SMTP, TCP'nin tasarlandığı doğası gereği güvenli olmayan yapı ile ilgilidir.                                                                                                                                 |
| İşletim Sistemi Zayıflığı  | - Her işletim sisteminde ele alınması gereken güvenlik sorunları vardır. <br>- UNIX, Linux, Mac OS, Mac OS X, Windows Server 2012, Windows 7, Windows 8...<br>- Bunlar, Bilgisayar Acil Müdahale Ekibi'nde (CERT) belgelenmiştir.                                             |
| Ağ Ekipmanları Zayıflığı   | Routerlar, güvenlik duvarları ve Switchler gibi çeşitli ağ ekipmanı türlerinin tanınması ve korunması gereken güvenlik zayıflıkları vardır. Zayıf yönleri arasında parola koruması, kimlik doğrulama eksikliği, yönlendirme protokolleri ve güvenlik duvarı açıkları bulunur. |

##### Yapılandırma Güvenlik Açıkları

| Güvenlik açığı                                              | Açıklama                                                                                                                                                                                                                                                                                                                                                           |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Güvenli olmayan kullanıcı hesapları                         | Kullanıcı hesabı bilgileri ağ üzerinden güvenli olmayan bir şekilde iletilebilir ve kullanıcı adları ve parolalar tehdit oyuncularına ifşa edilebilir.                                                                                                                                                                                                             |
| Kolayca tahmin edilebilen parolalara sahip sistem hesapları | Bu yaygın sorun, kötü oluşturulan kullanıcı parolalarının sonucudur.                                                                                                                                                                                                                                                                                               |
| Yanlış yapılandırılmış internet hizmetleri                  | Web tarayıcılarında JavaScript'i açmak, güvenilmeyen sitelere erişirken tehdit oyuncuları tarafından kontrol edilen JavaScript yoluyla saldırılara olanak tanır. Diğer olası zayıflık kaynakları arasında yanlış yapılandırılmış terminal hizmetleri, FTP veya web sunucuları (örn. Microsoft Internet Information Services (IIS) ve Apache HTTP Sunucusu bulunur. |
| Ürünler içinde güvenli olmayan varsayılan ayarlar           | Birçok üründe, güvenlik açısından boşluklar oluşturan veya etkinleştiren varsayılan ayarlar vardır.                                                                                                                                                                                                                                                                |
| Yanlış yapılandırılmış ağ ekipmanı                          | Ekipmanın kendisinin yanlış yapılandırılması önemli güvenlik sorunlarına neden olabilir.                                                                                                                                                                                                                                                                           |


##### Politika Güvenlik Açıkları

| Güvenlik Açığı                                                      | Açıklama                                                                                                                                                                                                                                                                                                                |
| ------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Yazılı güvenlik politikası eksikliği                                | Bir güvenlik politikası, yazılı değilse tutarlı bir şekilde uygulanamaz veya uygulamaya zorlanamaz.                                                                                                                                                                                                                     |
| Politika                                                            | Siyasi savaşlar ve bölge savaşları, tutarlı bir güvenlik politikası uygulanmasını zorlaştırabilir.                                                                                                                                                                                                                      |
| Kimlik doğrulama sürekliliği eksikliği                              | Kötü seçilmiş, kolayca kırılan veya varsayılan parolalar ağa yetkisiz erişime izin verebilir.                                                                                                                                                                                                                           |
| Mantıksal erişim kontrolleri uygulanmıyor                           | Yetersiz izleme ve denetim, saldırıların ve yetkisiz kullanımın devam etmesini sağlayarak şirket kaynaklarını boşa harcar. Bu, BT teknisyenlerine, BT yönetimine ve hatta güvenli olmayan koşulların devam etmesine izin veren şirket liderliğine karşı yasal işlem uygulama veya sözleşmenin feshi ile sonuçlanabilir. |
| Yazılım ve donanım yükleme ve değişiklikleri politikayı izlemezler. | Ağ topolojisindeki izinsiz değişiklikler veya onaylanmamış uygulamaların yüklenmesi, güvenlikte delikler oluşturur veya etkinleştirir.                                                                                                                                                                                  |
| Olağanüstü durum kurtarma planı mevcut değil                        | Olağanüstü durum kurtarma planının olmaması, doğal bir felaket meydana geldiğinde veya bir tehdit oyuncusu işletmeye saldırdığında kaos, panik ve karışıklığın ortaya çıkmasına yol açar.                                                                                                                               |



### Fiziksel Güvenlik

Ağın aynı derecede önemli bir savunmasız alanı, cihazların fiziksel güvenliğidir. Ağ kaynakları fiziksel olarak tehlikeye atılabiliyorsa, bir tehdit aktörü ağ kaynaklarının kullanımını engelleyebilir.

Dört fiziksel tehdit sınıfı aşağıdaki gibidir:
- **Donanım tehditleri:** Sunuculara, routerlara, switchlere, kablo tesisine ve iş istasyonlarına fiziksel hasarı içerir.
- **Çevresel tehditler:** Aşırı sıcaklıkları (çok sıcak veya çok soğuk) veya aşırı nemliliği (çok ıslak veya çok kuru) içerir.
- **Elektriksel tehditler:** Voltaj yükselmelerini, yetersiz besleme voltajını (tıkaçlar), koşulsuz güç (gürültü) ve toplam güç kaybını içerir.
- **Bakım tehditleri:** Önemli elektrikli bileşenlerin kötü kullanılmasını, kritik yedek parça eksikliğini, kötü kablolamayı ve kötü etiketlemeyi içerir.



### Ekipman Hasarını Sınırlamak için Fiziksel Güvenlik Planlama
- Güvenli bilgisayar odası.
- Ekipmanda hasarı sınırlamak için fiziksel güvenlik uygulayın.

**Adım 1.** Ekipmanı kilitleyin ve kapılardan, tavandan, yükseltilmiş kattan, pencerelerden, borulardan ve havalandırmadan izinsiz erişimi engelleyin.
**Adım 2.** Elektronik günlüklerle kabinet girişini izleyin ve kontrol edin.
**Adım 3.** Güvenlik kameraları kullanın.



### Zararlı Yazılım Türleri

Zararlı yazılımlar(malware), İngilizcedeki kötü amaçlı(malicious) yazılımın(software) kısaltmasıdır. Verilere, hostlara veya ağlara zarar vermek, bozmak, çalmak veya “kötü” ya da gayri meşru eyleme neden olmak için özel olarak tasarlanmış kod veya yazılımdır. Virüsler, solucanlar ve Truva atları kötü amaçlı yazılım türleridir.

**Virüsler**  
Bilgisayar virüsü, kendisinin kopyasını başka bir programa ekleyerek yayılan kötü amaçlı yazılımdır. Virüsler, hafif rahatsızlıklardan verilere zarar vermeye kadar farklı etkiler yaratabilir. Genelde bir yürütülebilir dosyaya eklenir ve kullanıcı bu dosyayı çalıştırana kadar etkinleşmez. Virüsler, ağ, disk veya e-posta gibi yollarla yayılır.

**Worms (Solucanlar)**  
Solucanlar, virüslere benzer fakat bağımsız olarak yayılır. Bir host programa ihtiyaç duymazlar ve sistemdeki güvenlik açıklarından yararlanarak ağda yayılırlar.

**Trojan Horses (Truva Atları)**  
Truva atları, kullanıcıyı kandırarak sistemlerine zararlı yazılımlar kuran meşru görünen zararlı yazılımlardır. Çoğalmazlar, ancak kullanıcı etkileşimi ile yayılırlar ve sisteme zarar verebilir veya kötü amaçlı yazılımlar bulaştırabilirler.



### Keşif Saldırıları

Ağ saldırıları üç ana kategoride sınıflandırılabilir:
- **Keşif saldırıları** - Sistemlerin, hizmetlerin veya güvenlik açıklarının keşfi ve haritalanması.
- **Erişim saldırıları** - Verilerin, sistem erişiminin veya kullanıcı ayrıcalıklarının yetkisiz olarak değiştirilmesi.
- **Hizmet reddi** - Ağların, sistemlerin veya hizmetlerin devre dışı bırakılması veya bozulması.

Keşif saldırılarında, dış tehdit oyuncuları, belirli bir kurumun IP adres alanını belirlemek için **nslookup** ve **whois** gibi internet araçlarını kullanabilir. IP adresi alanı belirlendikten sonra, etkin adresleri tespit etmek için halka açık IP adreslerine ping atılabilir. Bu adımı otomatikleştirmek için **fping** veya **gping** gibi araçlar kullanılabilir, bu araçlar belirli bir IP aralığındaki veya alt ağdaki tüm adresleri sistematik olarak pingler. Bu işlem, telefon rehberindeki tüm numaraları arayıp hangi numaranın yanıt verdiğini görmek gibidir.



### Erişim Saldırıları

Erişim saldırıları web hesaplarına, gizli veritabanlarına ve diğer hassas bilgilere erişim elde etmek için kimlik doğrulama hizmetleri, FTP hizmetleri ve web hizmetlerindeki bilinen güvenlik açıklarından yararlanır. Erişim saldırısı, bireylerin görüntüleme hakkı olmayan bilgilere yetkisiz erişim elde etmelerini sağlar. Erişim saldırıları dört türde sınıflandırılabilir: parola saldırıları, güven istismarı, port yeniden yönlendirme ve ortadaki adam.



### Hizmet Reddi Saldırıları

Hizmet reddi (DoS) saldırıları en yaygın saldırı türüdür ve ortadan kaldırılması en zor olanlarıdır. Bununla birlikte, uygulama kolaylığı ve potansiyel olarak önemli hasar nedeniyle, DoS saldırıları güvenlik yöneticilerinin özel ilgisini hak ediyor.

Sistem kaynaklarını tüketerek yetkili kişilerin bir hizmeti kullanmasını engellerler. DoS saldırılarını önlemeye yardımcı olmak için işletim sistemleri ve uygulamalar için en son güvenlik güncelleştirmelerinden haberdar olmak önemlidir.

**DoS Saldırısı**
DoS saldırıları, iletişimi kesintiye uğrattıkları, zaman ve para kaybına neden oldukları için büyük bir risk oluşturmaktadır. Bu saldırıların vasıfsız bir tehdit oyuncusu tarafından bile yapılması nispeten kolaydır.

**DDoS Saldırısı**
DDoS, DoS saldırısına benzer, ancak birden çok koordineli kaynaktan kaynaklanır. Örneğin, bir tehdit oyuncusu, zombi olarak bilinen virüslü hosttan oluşan bir ağ oluşturur. Zombi ağına botnet denir. Tehdit oyuncusu, zombilerin botnet'ine DDoS saldırısı gerçekleştirmesini bildirmek için bir komut ve kontrol (CnC) programı kullanır.



### Derinlemesine Savunma Yaklaşımı

Ağ saldırılarını azaltmak için öncelikle yönlendiriciler, switchler, sunucular ve hostlar gibi aygıtları güvence altına almalısınız. Çoğu kuruluş, güvenliğe derinlemesine bir savunma yaklaşımı (katmanlı yaklaşım) kullanır. Bu, ağ aygıtlarının ve hizmetlerinin birleşimini gerektirir.

- **VPN** - Güvenli şifreli tüneller kullanarak uzak kullanıcılar için kurumsal sitelerle güvenli VPN hizmetleri ve uzaktan erişim desteği sağlamak için bir router kullanılır.
- **ASA Güvenlik Duvarı** - Bu özel cihaz durum bilgisi güvenlik duvarı hizmetleri sağlar. Dahili trafiğin dışarı çıkmasını ve geri gelmesini sağlar, ancak harici trafik hostlara bağlantı başlatamaz.
- **IPS** - Saldırı önleme sistemi (IPS), gelen ve giden trafiği kötü amaçlı yazılım, ağ saldırısı imzaları ve daha fazlasını aramak için izler. Eğer bir tehdidi fark ederse, hemen durdurabilir.
- **ESA/WSA** - E-posta güvenlik cihazı (ESA), istenmeyen postaları(spam) ve şüpheli e-postaları filtreler. Web güvenlik cihazı (WSA) bilinen ve şüpheli internet kötü amaçlı yazılım sitelerini filtreler.
- **AAA Sunucusu** - Bu sunucu, ağ aygıtlarına erişme ve yönetme yetkisi olan güvenli bir veritabanı içerir. Ağ aygıtları, bu veri tabanını kullanarak yönetici kullanıcıların kimliklerini doğrular.



### Yedeklemeleri Saklama

Cihaz yapılandırmalarını ve verilerini yedeklemek, veri kaybına karşı korunmanın en etkili yollarından biridir. Veri yedekleme, bir bilgisayardaki bilgilerin bir kopyasını güvenli bir yerde tutulabilen çıkarılabilir yedekleme ortamına depolar. Altyapı aygıtlarının, bir FTP veya benzer dosya sunucusunda yapılandırma dosyalarının ve IOS görüntülerinin yedekleri olmalıdır. Bilgisayar veya router donanımı başarısız olursa, veri veya yapılandırma yedek kopya kullanılarak geri yüklenebilir.

Yedeklemeler, güvenlik ilkesinde tanımlandığı gibi düzenli olarak gerçekleştirilmelidir. Veri yedeklemeleri, ana tesise bir şey olursa yedekleme ortamını korumak için genellikle tesis dışında depolanır. Windows hostlarda, yedekleme ve geri yükleme yardımcı programı vardır. Kullanıcıların verilerini başka bir sürücüye veya bulut tabanlı depolama sağlayıcısına yedeklemeleri önemlidir.

| **Değerlendirme** | **Açıklama**                                                                                                                                                               |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Sıklık**        | <ul><li>Güvenlik ilkesinde tanımlandığı şekilde yedeklemeleri düzenli olarak gerçekleştirin.</li></ul>                                                                     |
| **Depolama**      | <ul><li>Verilerin bütünlüğünü sağlamak için daima yedeklemeleri doğrulayın ve dosya geri yükleme yordamlarını doğrulayın.</li></ul>                                        |
| **Güvenlik**      | <ul><li>Yedeklemeler, güvenlik politikasının gerektirdiği şekilde günlük, haftalık veya aylık rotasyonda onaylanmış bir tesis dışı depolama yerine taşınmalıdır.</li></ul> |
| **Doğrulama**     | <ul><li>Yedeklemeler güçlü parolalar kullanılarak korunmalıdır. Verileri geri yüklemek için parola gerekir.</li></ul>                                                      |



### Yükseltme, Güncelleme ve Yama Uygulama

Yükseltme, güncelleme ve yama uygulama, ağ güvenliğini artırmak için kritik öneme sahiptir. İşte bu sürecin temel adımları:

1. **Güncel Yazılım Takibi**: Yeni kötü amaçlı yazılımlar yayıldıkça, virüsten koruma yazılımlarının ve sistem yazılımlarının en son sürümleri güncel tutulmalıdır.
2. **Yama Uygulama**: Solucan saldırılarını en aza indirmek için güvenlik açığı olan sistemlere güvenlik yamaları uygulanmalıdır.
3. **Yazılım Görüntüleri Oluşturma**: Çok sayıda sistem yönetiliyorsa, her sisteme kurulacak standart bir yazılım görüntüsü oluşturulmalıdır.
4. **Otomatik Yama İndirme ve Yükleme**: Kritik güvenlik yamaları otomatik olarak indirilmeli ve yüklenmelidir. Bu, kullanıcı müdahalesine gerek kalmadan sistemin güvenliğini sağlar.

Bu adımlar, ağın güvenliğini korumak ve güncel tehditlere karşı savunmayı güçlendirmek için önemlidir.



### Kimlik Doğrulama, Yetkilendirme ve Hesap Tutma (AAA)

Tüm ağ aygıtlar, yalnızca yetkili kişilere erişim sağlayacak şekilde güvenli bir biçimde yapılandırılmalıdır. Kimlik doğrulama, yetkilendirme ve hesap tutma (AAA) ağ güvenlik hizmetleri, ağ aygıtlarında erişim denetimi kurmak için birincil çerçeve sağlar.

AAA, bir ağa kimin erişmesine izin verildiğini (kimlik doğrulama), ağa erişirken hangi eylemleri gerçekleştireceklerini (yetkilendirme) ve oradayken neler yapıldığını (hesap tutma) kaydetmenin bir yoludur.



### Güvenlik Duvarları

Güvenlik duvarı, kullanıcıları dış tehditlerden korumak için kullanılabilecek en etkili güvenlik araçlarından biridir. Güvenlik duvarı, istenmeyen trafiğin iç ağlara girmesini engelleyerek bilgisayarları ve ağları korur.

Ağ güvenlik duvarları iki veya daha fazla ağ arasında bulunur, aralarındaki trafiği kontrol eder ve yetkisiz erişimi önlemeye yardımcı olur. 

Güvenlik duvarı, dış kullanıcıların belirli hizmetlere erişimi denetlemesine izin verebilir. Örneğin, dış kullanıcılar tarafından erişilebilen sunucular, genellikle silahsızlandırılmış bölge (DMZ) olarak adlandırılan özel bir ağda bulunur. DMZ, bir ağ yöneticisinin söz konusu ağa bağlı hostlar için belirli ilkeler uygulamasını sağlar.



### Güvenlik Duvarı Türleri

Güvenlik duvarı ürünleri çeşitli şekillerde paketlenmiştir. Bu ürünler, bir ağa neyin erişmesine izin verileceğini veya neyin reddedileceğini belirlemek için farklı teknikler kullanır. Bunlar aşağıdakileri içerir:
- **Paket filtreleme** - IP veya MAC adreslerine dayalı erişimi önler veya izin verir.
- **Uygulama filtreleme** - Port numaralarına göre belirli uygulama türlerine erişimi engeller veya bunlara izin verir.
- **URL filtreleme** - Belirli URL'lere veya anahtar kelimelere göre web sitelerine erişimi engeller veya bunlara izin verir.
- **Durumsal paket incelemesi(SPI)** - Gelen paketler, dahili hostlardan çıkan isteklerin meşru yanıtları olmalıdır. Özel olarak izin verilmediği sürece, istenmeyen paketler engellenir. SPI ayrıca hizmet reddi (DoS) gibi belirli türde saldırıları algılama ve filtreleyerek önleme özelliğine sahiptir.



### Uç Nokta Güvenliği

Uç nokta cihazlarının güvenliğini sağlamak, insan faktörünü içerdiği için ağ yöneticisinin en zorlu görevlerinden biridir. Şirketlerin belgelerle desteklenen açık politikaları uygulamaya koyması ve çalışanların da bu kuralları bilmesi gerekir. Doğru ağ kullanımı konusunda çalışanlara eğitim verilmesi gerekir. Politikalar genellikle antivirüs yazılımlarının kullanımı ve hostlara izinsiz girişin önlenmesi konularını içerir. Daha kapsamlı uç noktası güvenlik çözümleri ise ağ erişim kontrolüne dayalıdır.



### Cisco Otomatik Güvenliği

Çoğu durumda, güvenlik düzeyi yetersizdir. Cisco AutoSecure özelliği, Cisco Routerlarda örnekte gösterildiği gibi sistemin güvenliğini sağlamak için kullanılabilir.

```
Router# auto secure
```

Ek olarak, çoğu işletim sistemi için uygulanması gereken bazı basit adımlar vardır:
- Varsayılan kullanıcı adları ve parolalar, hemen değiştirilmelidir.
- Sistem kaynaklarına erişim, bu kaynakları kullanma yetkisi olan kişilerle sınırlanmalıdır.
- Gereksiz hizmetler ve uygulamalar mümkün olduğunda kapatılmalı ve kaldırılmalıdır.

Üretici tarafından gönderilen cihazlar genellikle uzun bir süre depoda bekledikten sonra gönderildiği için bu cihazlarda güncel yamalar yüklü olmayabilir. Uygulamadan önce herhangi bir yazılımı güncellemek ve güvenlik yamalarını yüklemek önemlidir.



### Parolalar

Ağ cihazlarını korumak için güçlü parolalar kullanmak önemlidir. İzlenmesi gereken temel kurallar şunlardır:

1. **Uzunluk**: Parola en az 8, tercihen 10 veya daha fazla karakterden oluşmalıdır.
2. **Karmaşıklık**: Büyük/küçük harfler, rakamlar, semboller ve boşluk gibi karakter kombinasyonları kullanın.
3. **Kolay Tahmin Edilen Bilgilerden Kaçınma**: Yaygın sözcükler, kullanıcı adları, biyografik bilgiler ve numaralar gibi tahmin edilmesi kolay bilgilerle parola oluşturmayın.
4. **Yazım Hataları Kullanma**: Parolanızda bilinçli yazım hataları yapın (örneğin, "Security" yerine "5ecur1ty").
5. **Sık Değiştirme**: Parolaları düzenli aralıklarla değiştirin.
6. **Not Almak ve Görünür Yerlerde Bırakmamak**: Parolaları yazılı olarak kaydetmeyin ve kolayca bulunabilecek yerlere bırakmayın.

Cisco routerlarda, parolaların başındaki boşluklar dikkate alınmaz, ancak sonraki boşluklar geçerlidir. Güçlü bir parola oluşturmak için, boşluk karakteri kullanarak daha uzun ve tahmin edilmesi zor parola öbekleri (passphrase) oluşturabilirsiniz. Bu, parolanın hatırlanmasını kolaylaştırır ve güvenliğini artırır.



### Ek Parola Güvenliği

Ek parola güvenliğini sağlamak için Cisco router ve switch'lerde aşağıdaki adımlar izlenebilir:

1. **Düz Metin Parolalarını Şifreleme**: `service password-encryption` komutuyla, düz metin parolalarını şifreleyin. Bu, yetkisiz kişilerin yapılandırma dosyasındaki parolaları görmesini engeller.
2. **Minimum Parola Uzunluğunu Belirleme**: Parolaların güvenli olmasını sağlamak için `security passwords min-length <uzunluk>` komutuyla minimum parola uzunluğunu ayarlayın.
3. **Brute Force Saldırılarını Caydırma**: Deneme yanılma saldırılarına karşı `login block-for <süre> attempts <deneme_sayısı> within <zaman>` komutunu kullanarak, başarısız giriş denemeleri sonrasında IP adresinin bloklanmasını sağlayın. Örneğin, `login block-for 120 attempts 3 within 60` komutu, 60 saniye içinde 3 başarısız giriş denemesi yapılırsa 120 saniye boyunca girişleri engeller.
4. **Etkin Olmayan Oturumları Kapatma**: Kullanıcılar etkin olmadığında EXEC oturumlarının kapanması için `exec-timeout <dakika> <saniye>` komutunu kullanarak, zaman aşımını belirleyin. Bu, konsol, aux ve vty hatlarına uygulanabilir.

Bu adımlar, parolaların gizliliğini korur, brute force saldırılarına karşı koruma sağlar ve etkin olmayan oturumları güvenli bir şekilde kapatır.

```
R1(config)# service password-encryption 
R1(config)# security passwords min-length 8 
R1(config)# login block-for 120 attempts 3 within 60
R1(config)# line vty 0 4 
R1(config-line)# password cisco123 
R1(config-line)# exec-timeout 5 30 
R1(config-line)# transport input ssh 
R1(config-line)# end 
R1# 
R1# show running-config | section line vty
line vty 0 4 
password 7 094F471A1A0A 
exec-timeout 5 30 
login 
transport input ssh
R1#
```



### SSH'yi Etkinleştirme

Telnet uzaktan aygıt erişimini basitleştirir, ancak güvenli değildir. Bir Telnet paketinde bulunan veriler şifrelenmeden iletilir. Bu nedenle, güvenli uzaktan erişim için cihazlarda Güvenli Kabuk'un (SSH) etkinleştirilmesi önemle tavsiye edilir.

SSH'yi etkinleştirmek için Cisco cihazında şu adımları izleyin:

1. **Hostname Ayarlama**: Cihaza benzersiz bir host adı verin.
2. **IP Domain Name Yapılandırma**: `ip domain-name` komutuyla ağın IP etki alanını ayarlayın.
3. **Şifreleme Anahtarı Oluşturma**: `crypto key generate rsa general-keys modulus <bit_değeri>` komutuyla SSH trafiğini şifrelemek için anahtar oluşturun (1024 bit önerilir).
4. **Yerel Veritabanı Girişi Oluşturma**: `username <kullanıcı_adı> secret <şifre>` komutuyla yerel kullanıcı oluşturun.
5. **Kimlik Doğrulama Yapılandırma**: `login local` komutuyla yerel veritabanı kimlik doğrulaması ayarlayın.
6. **VTY SSH Oturumlarını Etkinleştirme**: `transport input ssh` komutuyla yalnızca SSH oturumlarına izin verin.

Bu adımlar, güvenli uzaktan erişim sağlamak için SSH'yi etkinleştirir.

```
Router# configure terminal
Router(config)# hostname R1
R1(config)# ip domain name span.com
R1(config)# crypto key generate rsa general-keys modulus 1024
The name for the keys will be: Rl.span.com 
% The key modulus size is 1024 bits
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
Dec 13 16:19:12.079: %SSH-5-ENABLED: SSH 1.99 has been enabled
R1(config)#
R1(config)# username Bob secret cisco
R1(config)# line vty 0 4
R1(config-line)# login local
R1(config-line)# transport input ssh
R1(config-line)# exit
R1(config)#
```



### Kullanılmayan Hizmetleri Devre Dışı Bırakma

Kullanılmayan hizmetleri devre dışı bırakmak için Cisco Router ve Switch'lerde şu adımları izleyin:
1. **Açık Portları Kontrol Etme**: Açık portları `show ip ports all` (IOS-XE) veya `show control-plane host open-ports` (eski IOS sürümleri) komutlarıyla kontrol edin.
2. **HTTP Sunucusunu Devre Dışı Bırakma**: HTTP sunucusunu devre dışı bırakmak için `no ip http server` komutunu kullanın.
3. **Telnet'i Devre Dışı Bırakma**: Telnet’i devre dışı bırakmak için `transport input ssh` komutuyla sadece SSH erişimine izin verin.

Bu adımlar, gereksiz hizmetlerin devre dışı bırakılmasına yardımcı olur, sistem kaynaklarını korur ve güvenliği artırır.



# Modül 17

### Küçük Ağ Topolojileri

Küçük ağlar, genellikle basit tasarımlara sahiptir ve az sayıda cihaz içerir. Bu tür ağlarda bir router, switch, kablosuz access point, IP telefon, yazıcı ve sunucu bulunur. İnternet bağlantısı genellikle DSL, kablo veya Ethernet üzerinden sağlanır.

Küçük ağların yönetimi, büyük ağlarla benzer beceriler gerektirir, ancak bu ağlar genellikle yerel bir BT teknisyeni veya sözleşmeli profesyoneller tarafından yönetilir.



### Küçük Ağ için Cihaz Seçimi

Küçük ağlar, büyük ağlar gibi dikkatlice planlanmalıdır. Aşağıdaki faktörler cihaz seçiminde önemlidir:

1. **Maliyet**: Cihazların maliyeti, kapasite, port sayısı ve türü gibi faktörlere göre değişir. Ağ yönetimi özellikleri ve güvenlik teknolojileri de maliyeti etkiler.
2. **Portların Hızı ve Türleri**: Router ve switch üzerindeki portların sayısı ve türü, ağın hız gereksinimlerine göre seçilmelidir. Yüksek hızları destekleyen cihazlar, ağın büyümesine olanak tanır.
3. **Genişletilebilirlik**: Modüler cihazlar, ağın büyüdükçe yeni modüller eklemeye olanak tanır. Sabit cihazlar ise eklemelere kapalıdır.
4. **İşletim Sistemi Özellikleri**: Ağ cihazları, 3 katman anahtarlaması, NAT, DHCP, güvenlik, QoS ve VoIP gibi hizmetleri desteklemelidir.



### Küçük Ağ için IP Adresleme

Bir ağda her cihazın benzersiz bir IP adresine sahip olması gerekir. IP adresleme şeması, cihaz türüne göre planlanmalı, belgelenmeli ve sürdürülmelidir. Bu, ağ trafiği sorunlarını çözmeyi kolaylaştırır.



### Küçük Ağda Yedeklilik

Yedeklilik, ağın güvenilirliğini artırmak için kritik öneme sahiptir. Ağda arıza durumunda, yedek ekipman ve ağ bağlantıları sayesinde hizmet kesintileri önlenebilir. Yedek servis sağlayıcıları kullanarak, router arızaları gibi durumlarda internet bağlantısının devamlılığı sağlanabilir.



### Trafik Yönetimi

Küçük ağlarda trafik yönetimi, çalışanların verimliliğini artırmak ve ağ kesintilerini en aza indirmek için önemlidir. Ağdaki ses ve video trafiği gibi gerçek zamanlı uygulamalar, QoS (Hizmet Kalitesi) kullanılarak önceliklendirilmelidir. Bu, ağ üzerindeki kritik trafiğin kesintisiz bir şekilde iletilmesini sağlar.



### Yaygın Uygulamalar

Bir ağın kurulumunun ardından, ağın doğru şekilde çalışabilmesi için çeşitli uygulamalar ve protokoller gereklidir. Bu uygulamalar ağın verimli ve güvenli çalışmasını sağlar.



#### Ağ Uygulamaları

Ağ uygulamaları, ağ üzerinden iletişim kurmak için kullanılan yazılım programlarıdır. Bu uygulamalar, ağda veri iletimi yapmak ve kullanıcıların ihtiyaçlarını karşılamak için ağ kaynaklarını kullanır. Örnekler:
- **E-posta istemcileri**: Kullanıcıların e-posta göndermesi ve alması için kullanılır.
- **Web tarayıcıları**: Web sitelerine erişim sağlar ve HTTP/HTTPS protokollerini kullanır.



#### Uygulama Katmanı Hizmetleri

Bazı programlar, ağ kaynaklarını kullanabilmek için uygulama katmanı hizmetlerinin yardımına ihtiyaç duyar. Bu hizmetler, veriyi ağ üzerinden iletim için hazırlamak ve düzenlemek için çalışır. Örneğin, dosya transferi veya ağ yazdırma hizmetleri.

Her ağ uygulaması veya hizmeti, iletişim için belirli protokoller kullanır. Protokoller, veri iletiminin düzgün ve güvenli bir şekilde yapılmasını sağlar.



### Yaygın Protokoller

Ağ protokolleri, ağın temel işlevlerini sağlamak için kullanılır. Bu protokoller, uygulamalar ve ağ hizmetleri arasında veri iletimini düzenler.

##### SSH ve Telnet
- **SSH** (Secure Shell) güvenli bir uzak erişim çözümüdür ve yönetici cihazlarına güvenli bağlantılar sağlar.
- **Telnet**, daha eski bir uzak erişim protokolüdür ve şifreli bir bağlantı sağlamaz, bu yüzden genellikle güvenli alternatif olan SSH kullanılır.

##### Web ve E-posta Sunucuları
- **HTTP/HTTPS**: Web sunucuları ile istemciler arasındaki veri iletimi için kullanılır. HTTPS, güvenli bağlantı sağlar.
- **SMTP**: E-posta göndermek için kullanılır.
- **POP3/IMAP**: E-posta almak için kullanılır.

##### FTP ve DHCP Sunucuları
- **FTP** (File Transfer Protocol) dosya aktarımını sağlar. Güvenli sürümleri olan **FTPS** ve **SFTP** ise şifreli bağlantılar sağlar.
- **DHCP** (Dynamic Host Configuration Protocol), ağdaki cihazlara otomatik IP adresi atamak için kullanılır.

##### DNS Sunucuları
- **DNS** (Domain Name System) etki alanı adlarını IP adreslerine çözer, bu sayede web siteleri ziyaret edilebilir.



### Ses ve Video Uygulamaları

Modern işletmeler, sesli ve görüntülü iletişim için IP tabanlı çözümler kullanmaktadır. Bu uygulamalar için ağ altyapısının doğru şekilde yapılandırılması gerekmektedir.

##### VoIP (Voice over IP)
- **VoIP**, sesli iletişimi dijital veriye dönüştürerek IP tabanlı ağlar üzerinden iletilmesini sağlar. Bu, analog telefon hatlarından daha ucuzdur.

##### IP Telefon
- **IP telefonlar**, analog telefon sinyallerini dijital IP paketlerine dönüştürerek çağrı yapar ve alır. İşletmeler için kurumsal VoIP çözümleri sunan birçok satıcı vardır.

##### Gerçek Zamanlı Uygulamalar
- Ağ, **gerçek zamanlı uygulamaların** (ses ve video) düzgün çalışabilmesi için düşük gecikme süresi sağlar. Bu amaçla, **Hizmet Kalitesi (QoS)** uygulamaları kullanılır ve **RTP** (Real-time Transport Protocol) ile **RTCP** (Real-time Transport Control Protocol) gibi protokollerle desteklenir.

Bu protokoller ve uygulamalar, ağların güvenli ve verimli bir şekilde çalışmasını sağlamak için kritik öneme sahiptir. Her bir ağ türü için farklı gereksinimler olabilir, ancak doğru yapılandırmalar ve protokollerle, ağlar etkili bir şekilde yönetilebilir.



### Küçük Ağ Büyümesi

Küçük bir ağ, işletme büyüdükçe doğal olarak genişlemelidir. Bu süreç ağın ölçeklenmesi olarak bilinir ve doğru planlama ile bu genişleme sorunsuz bir şekilde yapılabilir. Ağ yöneticisinin, ağ büyüdükçe bu büyümeyi destekleyecek kararlar alması önemlidir.



#### Ağı Ölçeklendirmek İçin Gerekenler:

1. **Ağ Belgeleme**: Fiziksel ve mantıksal topolojinin belgelenmesi, ağın nasıl yapılandığını ve büyüdüğünde nasıl genişleyeceğini anlamak için gereklidir.
2. **Cihaz Envanteri**: Ağdaki tüm cihazların listesi (sunucular, routerlar, anahtarlar, vb.) tutulmalıdır.
3. **Bütçe**: Yeni ekipman alımları ve ağ genişlemesi için ayrılacak mali kaynakların belirlenmesi gerekir.
4. **Trafik Analizi**: Hangi protokoller, uygulamalar ve hizmetlerin ağda kullanılacağı ve bunların trafik gereksinimlerinin belirlenmesi önemlidir.

Bu öğeler, ağın büyümesi sırasında doğru kararlar alabilmek için gerekli verilere sağlar.



### Protokol Analizi

Ağ büyüdükçe, ağ trafiğini yönetmek daha önemli hale gelir. Trafik akışlarının yönetilebilmesi için ağdaki veri akışını doğru bir şekilde analiz etmek gerekir. Bu, ağın verimli çalışmasını sağlamak için kritik bir adımdır.

#### Trafik Akışını Analiz Etmek İçin:

- En yoğun kullanım sırasında ağ trafiğini izlemek, ağın trafik profili hakkında fikir verir.
- Trafik akışını doğru bir şekilde analiz etmek için farklı ağ segmentlerinde izleme yapmak önemlidir.

Protokol analiz araçları (örneğin Wireshark), ağdaki trafiği takip eder ve trafiğin kaynağını, hedefine ve türünü analiz eder. Bu, ağ trafiğinin daha etkili yönetilmesine olanak tanır. Örneğin, gereksiz trafik azaltılarak ağ verimliliği artırılabilir veya sunucuların farklı ağ segmentlerine taşınarak performans iyileştirilebilir.



### Çalışan Ağı Kullanımı

Ağ yöneticisinin, ağ kullanımının nasıl değiştiğini takip etmesi önemlidir. Çalışanların ağ üzerindeki davranışlarını analiz etmek, ağın performansını optimize etmek için kritik bir adımdır.

##### Kullanılabilecek Araçlar:
- **Windows Görev Yöneticisi** ve **Olay Görüntüleyicisi**: Ağda çalışan uygulamaların ve sistem kaynaklarının kullanımını gösterir.
- **Veri Kullanımı Aracı**: Hangi uygulamaların ağ kaynaklarını kullandığını izlemek için kullanılabilir. Bu araç, belirli bir zaman diliminde ağdaki veri kullanımını takip etmenizi sağlar.

Örneğin, **Windows 10 Veri Kullanımı** aracı, ağ hizmetlerini kullanan uygulamaların belirlenmesinde özellikle yararlıdır. Kullanıcılar, **Ayarlar > Ağ ve İnternet > Veri kullanımı > ağ arabirimi** üzerinden 30 günlük veri kullanımı bilgilerine erişebilir.

Bu araçlar, ağ yöneticilerinin değişen trafik akışlarını ve protokol gereksinimlerini anlamalarına yardımcı olur, böylece ağ kaynaklarını daha verimli şekilde yönetebilirler.



### Ping ile Bağlanabilirliği Doğrulama

Ağınızın doğru şekilde bağlandığından emin olmak için **ping** komutu etkili bir araçtır. **Ping**, kaynak ve hedef IP adresi arasındaki 3. katman bağlantısını test eder ve gidiş-dönüş süresi gibi istatistikleri gösterir. ICMP (Internet Control Message Protocol) protokolü kullanarak yankı (ICMP Type 8) ve yankı yanıtı (ICMP Type 0) mesajları gönderir.

**Windows 10**'da **ping** komutu, hedefe dört ardışık ICMP yankı mesajı gönderir ve her birinden yanıt bekler.

Örnek: PC A'nın PC B'yi pinglediğini düşünelim. PC A, PC B'ye dört ICMP yankı mesajı gönderir (örneğin, hedef IP 10.1.1.10).



### IOS Ping Indicators
Ping komutunun çıktısında çeşitli semboller yer alır:

|**Eleman**|**Açıklama**|
|---|---|
|**!**|Yankı yanıtı alındığını gösterir. 3. katman bağlantısının doğru olduğunu doğrular.|
|**.**|Yanıt bekleme süresi dolmuş demektir. Bağlantı sorunlarına işaret edebilir.|
|**U**|Hedefe ulaşılamıyor, bu da bir routerın ICMP Tip 3 cevabını gönderdiğini gösterir. Hedef ağ veya ana bilgisayar bulunamamış olabilir.|



### Genişletilmiş Ping

Standart **ping** komutu, hedefe en yakın ağ arabirimine bağlı IP adresini kullanarak işlem yapar. Ancak, Cisco IOS'un genişletilmiş **ping** komutu, komut parametrelerini ayarlayarak özel pingler oluşturmanıza olanak tanır.

Örneğin, R1 üzerindeki **ping 10.1.1.10** komutu, G0/0/0 arabirimine (IP 209.165.200.225) bağlı bir kaynağı kullanır. Genişletilmiş ping, farklı bir kaynak IP adresi belirtilerek yapılandırılabilir. Örneğin, R1'deki genişletilmiş **ping** komutu, 192.168.10.1 IP adresini kaynak olarak kullanabilir.



### Traceroute ile Bağlantıyı Test Etme

**Ping** komutu, bağlantı sorunu olup olmadığını hızlıca test edebilir, ancak hangi noktada bir sorun olduğunu belirlemez. **Traceroute** komutu, bir paket ağ üzerinden yönlendirilirken geçtiği noktaları gösterir ve 3. katman sorunlarını tespit etmeye yardımcı olur.

**Traceroute** komutunun sözdizimi, işletim sistemine göre değişir. Windows'ta **tracert**, Cisco IOS'ta ise **traceroute** komutları kullanılır.



### Genişletilmiş Traceroute

Genişletilmiş **traceroute** komutu, daha fazla parametre ile özelleştirilebilir. Bu, yönlendirme döngülerinde sorun giderirken, bir sonraki router'ın belirlenmesinde ya da paketlerin düşürülüp reddedildiği noktaların tespit edilmesinde yardımcı olur.

Windows'ta **tracert** komutu, çeşitli seçenekler sunar. Cisco IOS'ta ise genişletilmiş **traceroute**, kullanıcıya parametreleri ayarlama imkânı verir.

Örneğin, R1'deki genişletilmiş **traceroute**, farklı bir kaynak adresi belirleyerek test yapılmasını sağlar.



### Ağ Temeli (Baseline)

Ağ temeli, ağ performansını izlemek ve sorunları tespit etmek için önemli bir araçtır. Ağ temeli oluşturulurken, ağda yapılan **ping**, **trace** komutlarından elde edilen veriler kullanılabilir. Bu veriler, zaman damgası eklenerek bir metin dosyasına kaydedilebilir ve ağın performansının düzenli olarak izlenmesine yardımcı olabilir.

Yanıt sürelerindeki artışlar, ağ gecikmesi gibi sorunları tespit etmek için önemlidir. Bu tür verilerin düzenli olarak kaydedilmesi, ağ performansını zaman içinde gözlemlemeyi sağlar ve sorunların hızlıca çözülmesini sağlar.



### Windows Hostta IP Yapılandırması

Bağlanabilirliği doğrulamak ve IP yapılandırmalarını kontrol etmek için Windows 10'da **Network and Sharing Center** aracını kullanabilirsiniz. Bu araç, IP adresi, alt ağ maskesi, ağ geçidi ve DNS gibi dört önemli ayar hakkında bilgi sağlar. Bu bilgiler, ağ bağlantılarının doğru yapılandırılıp yapılandırılmadığını kontrol etmenize yardımcı olur.

### Linux Hostta IP Yapılandırması

Linux sistemlerde IP ayarlarını kontrol etmek için GUI aracını kullanabilirsiniz. Örneğin, **Connection Information** iletişim kutusu, Gnome masaüstü ortamında Ubuntu dağıtımı üzerinde IP yapılandırması hakkında bilgi sağlar. Ayrıca, **ip address** komutu ile terminal üzerinden IP adreslerini ve ağ arayüzlerini görüntüleyebilirsiniz.

**Not:** Çıktılar, kullanılan Linux dağıtımına ve masaüstü ortamına bağlı olarak farklılık gösterebilir.

### macOS Hostta IP Yapılandırması

macOS sistemlerinde, IP yapılandırması **Network Preferences > Advanced** bölümünde görüntülenebilir. Ayrıca, terminal üzerinden **ifconfig** komutu ile ağ arayüzlerinin IP yapılandırması doğrulanabilir. Diğer yararlı komutlar arasında `networksetup -listallnetworkservices` ve `networksetup -getinfo <network service>` bulunmaktadır.

### arp Komutu

**arp** komutu, ağdaki cihazların IP adresleri ile MAC adreslerinin eşleşmelerini gösterir. **arp -a** komutu ile ARP önbelleğindeki tüm IP-MAC eşleşmeleri listelenebilir. ARP önbelleği yalnızca yakın zamanda erişilen cihazları içerir, bu yüzden bir cihaza **ping** atarak önbelleğin güncellenmesini sağlayabilirsiniz.

**Not:** ARP önbelleğini temizlemek için **netsh interface ip delete arpcache** komutunu kullanabilirsiniz, ancak bu komutun çalışabilmesi için yönetici erişimine ihtiyaç vardır.

### Yaygın show Komutlarını Gözden Geçirme

Ağ cihazlarının IP yapılandırmalarını doğrulamak ve ağ arayüzlerini kontrol etmek için **show** komutları yaygın olarak kullanılır. Cisco IOS CLI üzerinde kullanılan bazı önemli **show** komutları şunlardır:

|**Komut**|**Kullanım amacı**|
|---|---|
|**show running-config**|Geçerli yapılandırmayı görüntüler|
|**show interfaces**|Arayüz durumunu ve hata mesajlarını görüntüler|
|**show ip interface**|3. katman IP bilgilerini doğrular|
|**show arp**|Yerel Ethernet LAN'da bilinen hostların listesini görüntüler|
|**show ip route**|3. katman yönlendirme bilgisini görüntüler|
|**show protocols**|Çalışan protokolleri doğrular|
|**show version**|Cihazın lisans, arayüz ve bellek bilgilerini görüntüler|

**show running-config** komutu, cihazın mevcut yapılandırmasını görüntülemek için kullanılırken, **show interfaces** komutu, arayüzlerin durumunu ve olası hata mesajlarını kontrol eder. **show ip interface** komutu ise 3. katman bilgilerini doğrular.

### show cdp neighbors Komutu

Cisco Discovery Protocol (CDP), Cisco cihazları arasında veri bağlantı katmanında çalışan bir protokoldür. **show cdp neighbors** komutu, komşu Cisco cihazları hakkında bilgi sağlar. Bu komut cihazlar arasındaki bağlantıyı kontrol etmek için kullanışlıdır ve genellikle bağlantı sorunlarını tespit etmekte yardımcı olur.

**show cdp neighbors detail** komutu, daha ayrıntılı bilgi sunar, örneğin komşu cihazın IP adresi gibi bilgiler sağlar. Ancak, güvenlik nedeniyle, CDP'nin yalnızca Cisco cihazlarına bağlanan arayüzlerde etkinleştirilmesi önerilir.

CDP'yi devre dışı bırakmak için **no cdp run** komutu global yapılandırma modunda, veya **no cdp enable** komutu bir arayüzde kullanılır.

### show ip interface brief Komutu

**show ip interface brief** komutu, **show ip interface** komutundan daha kısa bir çıktı sağlar ve router'daki tüm ağ arayüzlerinin durumunu özetler. Bu komut, her bir arayüzün IP adresi ve çalışma durumunu görüntüler. Ayrıca, switch arayüzlerinin durumunu doğrulamak için de kullanılabilir.

Örneğin, **show ip interface brief** komutu, aktif olan bir VLAN1 arayüzünün ve kapalı olan bir FastEthernet0/1 arayüzünün durumunu gösterir.

Bu komut, ağ arayüzlerinin durumunu hızlıca kontrol etmek ve arayüzler arasında herhangi bir sorun olup olmadığını tespit etmek için kullanılır.


### Temel Sorun Giderme Yaklaşımları

Ağ sorunlarını gidermenin birkaç yolu vardır ve doğru metodoloji, ağ yöneticisinin etkinliğini artırır. Yaygın bir yaklaşım, bilimsel yöntemi temel alır ve aşağıdaki adımları içerir:

|**Adım**|**Açıklama**|
|---|---|
|**1. Sorunu Tanımlama**|Sorunun tanımlanması, kullanıcıdan alınan bilgi ile başlar.|
|**2. Olası Sebepler Teorisi Oluşturma**|Sorunun nedenlerini analiz edip teori oluşturulur.|
|**3. Teoriyi Test Etme**|Testlerle teoriler doğrulanır. Sorun çözülmezse, derinlemesine araştırma yapılır.|
|**4. Eylem Planı Oluşturma ve Çözüm Uygulama**|Sorunun kesin nedeni belirlendikten sonra çözüm uygulanır.|
|**5. Çözümü Doğrulama ve Önleyici Önlemler**|Çözüm doğrulanır ve gelecekte tekrarını engellemek için önlemler alınır.|
|**6. Bulguları Belgeleme**|Sorun giderme süreci belgelenir. Bu, gelecekteki referanslar için önemlidir.|

Sorunu değerlendirirken, ağdaki hangi cihazların etkilendiğini belirlemek önemlidir. Tek bir cihazda sorun varsa, o cihazda sorun giderme başlatılır. Tüm ağda sorun varsa, ağdaki merkez cihazda işlem yapılır.

### Çözüm mü, Bildirim mi?

Bazı durumlarda, sorun anında çözülemez ve üst yönetime bildirilmesi gerekir. Örneğin, bir donanım parçasının değiştirilmesi gerektiğinde yönetici onayı gerekebilir. Şirket politikası, ne zaman ve nasıl bildirim yapılacağına dair yönergeler sunmalıdır.

### Debug Komutu

Cisco IOS'ta **debug** komutu, gerçek zamanlı hata ayıklama için kullanılır. Bu komut, sadece ilgili özelliği izlemenize olanak tanır ve işlemciyi aşırı yüklememek için dikkatli kullanılmalıdır. Debug komutları **privileged EXEC mode**'da çalıştırılır. Debug çıktısını devre dışı bırakmak için **no debug** veya **undebug all** komutları kullanılır.

### Terminal Monitor Komutu

**debug** çıktılarının konsol üzerinden görünmesini sağlar. Uzaktan bağlantılar için, **terminal monitor** komutu ile çıktılar görülebilir. Gereksiz debug işlemleri, sistem kaynaklarını tüketmemek için sonlandırılmalıdır.



### Dupleks Çalışma ve Uyuşmazlık Sorunları

Dupleks, iki cihaz arasındaki veri iletiminin yönünü ifade eder. İki tür dupleks iletişim vardır:

- **Half dupleks**: Veri bir yönde, bir seferde bir kez iletilir.
- **Full dupleks**: Veri aynı anda iki yönde iletilir.

Ethernet bağlantılarında her iki cihazın aynı dupleks modda çalışması önemlidir. Eğer bir cihaz full dupleks, diğer cihaz ise half dupleks modda çalışıyorsa, **dupleks uyumsuzluğu** yaşanır ve bağlantı performansı düşer. Bu sorun genellikle yanlış yapılandırma veya başarısız otomatik anlaşma nedeniyle oluşur.

### IP Adresleme Sorunları

Yanlış IP adresi atamaları, ağ cihazlarının iletişimini engelleyebilir. IP adresleri hiyerarşiktir, bu yüzden her cihazın IP adresi doğru ağ aralığında olmalıdır. **El ile IP ataması** hataları veya **DHCP sorunları** yaygın nedenlerdir. IOS cihazlarda **show ip interface brief** komutuyla IP adreslerini doğrulamak mümkündür.

### Kullanıcı Cihazlarında IP Adresleme Sorunları

Windows cihazları, DHCP sunucusuyla iletişim kuramazsa **169.254.0.0/16** aralığından bir IP alır (APIPA). Bu adresle, cihaz ağdaki diğer cihazlarla iletişim kuramaz. **ipconfig** komutuyla IP adresi kontrol edilebilir.

### Varsayılan Ağ Geçidi Sorunları

Varsayılan ağ geçidi, trafiği yönlendiren en yakın ağ aygıtıdır. Yanlış yapılandırılmış bir ağ geçidi, uzak ağlarla iletişimi engeller. Bu sorun **manuel yapılandırma** veya **DHCP sorunları** nedeniyle oluşabilir. **ipconfig** komutu, Windows cihazlarındaki ağ geçidini kontrol etmek için kullanılabilir. **show ip route** komutuyla router'daki varsayılan ağ geçidi doğrulanabilir.

### DNS Sorunlarını Giderme

DNS, alan adlarını IP adreslerine çeviren bir hizmettir. DNS sorunları, web sitesi erişimi gibi son kullanıcı deneyimlerini etkileyebilir. DNS sunucuları manuel veya otomatik olarak atanabilir. **nslookup** komutu, DNS sorunlarını tespit etmek ve çözümlemek için kullanılabilir. Örneğin, **8.8.8.8** (Google DNS) genel bir test DNS sunucusudur.

Bu sorunları çözmek için, ilgili ağ yapılandırmalarını kontrol etmek ve uygun komutları kullanmak önemlidir.