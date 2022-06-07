LSP nedir?
Yüksek düzeyde, LSP, nesne yönelimli bir programda, bir üst sınıf nesne referansını alt sınıflarından herhangi birinin bir nesnesiyle değiştirirsek, programın bozulmaması gerektiğini belirtir.

örnek:
class SomeClass {
  
  void aMethod(SuperClass superClassReference) {
    doSomething(superClassReference);
  }
  }

  Bu , kendisine iletilen her olası alt sınıf nesnesiSuperClass için beklendiği gibi çalışmalıdır . Bir üst sınıf nesnesini bir alt sınıf nesnesiyle değiştirmek, program davranışını beklenmedik şekillerde değiştirirse, LSP ihlal edilir.

  LSP, bir sınıfı genişleterek veya bir arabirim uygulayarak bir üst tip-alt tip kalıtım ilişkisi olduğunda uygulanabilir. **Üst tipte tanımlanan yöntemleri bir sözleşme tanımlama olarak düşünebiliriz. Her alt türün bu sözleşmeye bağlı kalması beklenir. Bir alt sınıf, üst sınıfın sözleşmesine uymuyorsa, LSP'yi ihlal ediyor demektir. **


  ir alt sınıftaki bir yöntem, bir üst sınıf yönteminin sözleşmesini nasıl bozabilir? Birkaç olası yol vardır:

Üst sınıf yöntemi tarafından döndürülen nesneyle uyumlu olmayan bir nesne döndürme.
Üst sınıf yöntemi tarafından atılmayan yeni bir istisna atmak.
Anlambilimin değiştirilmesi veya üst sınıfın sözleşmesinin parçası olmayan yan etkilerin tanıtılması.
ObjectJava ve diğer statik olarak yazılan diller , derleme zamanında işaretleyerek 1 ( gibi çok genel sınıflar kullanmadığımız sürece) ve 2'yi (kontrol edilen istisnalar için) engeller . Bu dillerde LSP'yi üçüncü yoldan ihlal etmek hala mümkündür.

LSP Neden Önemlidir?
LSP ihlalleri bir tasarım kokusudur. Bir kavramı erken genelleştirmiş ve hiçbirinin gerekli olmadığı bir üst sınıf oluşturmuş olabiliriz. Konsept için gelecekteki gereksinimler, yarattığımız sınıf hiyerarşisine uymayabilir.

instanceofİstemci kodu bir üst sınıf referansını bir alt sınıf nesnesiyle serbestçe değiştiremezse, kontroller yapmak ve bazı alt sınıfları özel olarak işlemek zorunda kalacaktır . Bu tür bir koşullu kod, kod tabanına yayılırsa, bakımı zor olacaktır.

Bir alt sınıfı her eklediğimizde veya değiştirdiğimizde, kod tabanını taramamız ve birden çok yeri değiştirmemiz gerekir . Bu zor ve hataya açık.

Ayrıca , programı geliştirmeyi kolaylaştırmak için ilk etapta üst tip soyutlamayı tanıtma amacını da ortadan kaldırır .

Tüm yerleri belirlemek ve değiştirmek bile mümkün olmayabilir - müşteri koduna sahip olmayabiliriz veya kontrol etmeyebiliriz. Örneğin, bir kütüphane olarak işlevselliğimizi geliştiriyor ve bunları harici kullanıcılara sağlıyor olabiliriz.

NASIL KULLANILIR

Tasarımın Sabitlenmesi
Tasarımı yeniden gözden geçirelim ve yalnızca gereksinim değişikliklerine esnek bir kod oluşturmaya yetecek kadar genel olduklarında üst tür soyutlamaları oluşturalım . Ayrıca aşağıdaki nesne yönelimli tasarım ilkelerini kullanacağız:

Arayüze program, uygulamaya değil
Değişenleri kapsülleyin
Kalıtım yerine kompozisyonu tercih edin
Başlangıç ​​olarak, uygulamamızın hem şimdi hem de gelecekte ödeme alması gerektiğinden emin olabiliriz. Topladığımız ödeme ayrıntılarını doğrulamak isteyebileceğimizi düşünmek de mantıklı. Neredeyse diğer her şey değişebilir. Şimdi aşağıdaki arayüzleri tanımlayalım:

interface IPaymentInstrument  {
  void validate() throws PaymentInstrumentInvalidException;
  PaymentResponse collectPayment() throws PaymentFailedException;
}
class PaymentResponse {
  String identifier;
}
PaymentResponsekapsüller a identifier- bu, kredi ve banka kartları için parmak izi veya ödül kartları için kart numarası olabilir. Gelecekte farklı bir ödeme aracı için başka bir şey olabilir. Kapsülleme IPaymentInstrument, gelecekteki ödeme araçlarının daha fazla veriye sahip olması durumunda değişmeden kalmasını sağlar.

PaymentProcessorsınıf şimdi şöyle görünür:

class PaymentProcessor {
  void process(
      OrderDetails orderDetails, 
      IPaymentInstrument paymentInstrument) {
    try {
      paymentInstrument.validate();
      PaymentResponse response = paymentInstrument.collectPayment();
      saveToDatabase(orderDetails, response.getIdentifier());
    } catch (...) {
      // exception handling
    }
  }

  void saveToDatabase(OrderDetails orderDetails, String identifier) {
    // save the identifier and order details in DB
  }
}
Artık çağrı yok - runFraudChecks()bunlar tüm ödeme araçlarına uygulanacak kadar genel değil .sendToPaymentGateway()PaymentProcessor

Sorun alanımızda yeterince genel görünen diğer kavramlar için birkaç arayüz daha ekleyelim:

interface IFraudChecker {
  void runChecks() throws FraudDetectedException;
}
interface IPaymentGatewayHandler {
  PaymentGatewayResponse handlePayment() throws PaymentFailedException;
}
interface IPaymentInstrumentValidator {
  void validate() throws PaymentInstrumentInvalidException;
}
class PaymentGatewayResponse {
  String fingerprint;
}
Ve işte uygulamalar:

class ThirdPartyFraudChecker implements IFraudChecker {
  // members omitted
  
  @Override
  void runChecks() throws FraudDetectedException {
    // external system call omitted
  }
}
class PaymentGatewayHandler implements IPaymentGatewayHandler {
  // members omitted
  
  @Override
  PaymentGatewayResponse handlePayment() throws PaymentFailedException {
    // send details to payment gateway (PG), set the fingerprint
    // received from PG on a PaymentGatewayResponse and return
  }
}
class BankCardBasicValidator implements IPaymentInstrumentValidator {
  // members like name, cardNumber etc. omitted

  @Override
  void validate() throws PaymentInstrumentInvalidException {
    // basic validation on name, expiryDate etc.
    if (name == null || name.isEmpty()) {
      throw new PaymentInstrumentInvalidException("Name is invalid");
    }
    // other basic validations
  }
}
Yukarıdaki yapı taşlarını farklı şekillerde bir araya getirerekCreditCard inşa ve DebitCardsoyutlamalar yapalım . Önce şunu uygulayan bir sınıf tanımlıyoruz :IPaymentInstrument

abstract class BaseBankCard implements IPaymentInstrument {
  // members like name, cardNumber etc. omitted
  // below dependencies will be injected at runtime
  IPaymentInstrumentValidator basicValidator;
  IFraudChecker fraudChecker;
  IPaymentGatewayHandler gatewayHandler;

  @Override
  void validate() throws PaymentInstrumentInvalidException {
    basicValidator.validate();
  }

  @Override
  PaymentResponse collectPayment() throws PaymentFailedException {
    PaymentResponse response = new PaymentResponse();
    try {
      fraudChecker.runChecks();
      PaymentGatewayResponse pgResponse = gatewayHandler.handlePayment();
      response.setIdentifier(pgResponse.getFingerprint());
    } catch (FraudDetectedException e) {
      // exception handling
    }
    return response;
  }
}
class CreditCard extends BaseBankCard {
  // constructor omitted
  
  @Override
  void validate() throws PaymentInstrumentInvalidException {
    basicValidator.validate();
    // additional validations for credit cards
  }
}
class DebitCard extends BaseBankCard {
  // constructor omitted
}
Bir sınıfı genişletmesine rağmen CreditCard, eskisi gibi değil. DebitCardKod tabanımızın diğer alanları artık yalnızca IPaymentInstrumentarayüze bağlıdır, BaseBankCard. Aşağıdaki pasaj, CreditCardnesne oluşturma ve işlemeyi gösterir:

IPaymentGatewayHandler gatewayHandler = 
  new PaymentGatewayHandler(name, cardNum, code, expiryDate);

IPaymentInstrumentValidator validator = 
  new BankCardBasicValidator(name, cardNum, code, expiryDate);

IFraudChecker fraudChecker = 
  new ThirdPartyFraudChecker(name, cardNum, code, expiryDate);

CreditCard card = 
  new CreditCard(
    name,
    cardNum,
    code,
    expiryDate,
    validator,
    fraudChecker,
    gatewayHandler);

paymentProcessor.process(order, card);
RewardsCardTasarımımız artık bir - zorlama yok ve koşullu kontroller eklememize izin verecek kadar esnek . Sadece yeni sınıfı ekliyoruz ve beklendiği gibi çalışıyor.

class RewardsCard implements IPaymentInstrument {
  String name;
  String cardNumber;

  @Override
  void validate() throws PaymentInstrumentInvalidException {
    // Rewards card related validations
  }

  @Override
  PaymentResponse collectPayment() throws PaymentFailedException {
    PaymentResponse response = new PaymentResponse();
    // Steps related to rewards card payment like getting current 
    // rewards balance, updating balance etc.
    response.setIdentifier(cardNumber);
    return response;
  }
}
Ve işte yeni kartı kullanan müşteri kodu:

RewardsCard card = new RewardsCard(name, cardNum);
paymentProcessor.process(order, card);



SP İhlalleri Nasıl Belirlenir?
LSP ihlallerini belirlemek için bazı iyi göstergeler şunlardır:

İstemci kodunda koşullu mantık ( instanceofoperatörü kullanarak veya gerçek alt sınıfı tanımlamak için)object.getClass().getName()
Alt sınıflarda bir veya daha fazla yöntemin boş, hiçbir şey yapma uygulamaları
UnsupportedOperationExceptionBir alt sınıf yönteminden beklenmeyen bir istisna veya başka bir istisna atmak
Yukarıdaki 3. nokta için, üst sınıfın sözleşme perspektifinden istisnanın beklenmeyen olması gerekir. Bu nedenle, üst sınıf yöntemimizin imzası, alt sınıfların veya uygulamaların UnsupportedOperationExceptionbir .

java.util.List<E>Arayüzün add(E e)yöntemini düşünün . java.util.Arrays.asList(T ...)Değiştirilemez bir liste döndürdüğünden, a öğesine bir öğe ekleyen istemci kodu, Listtarafından Listdöndürülürse bozulur Arrays.asList.

Bu bir LSP ihlali mi? Hayır - List.add(E e)yöntemin sözleşmesi, uygulamaların bir UnsupportedOperationException. İstemcilerin yöntemi kullanırken bunu işlemesi beklenir.

Çözüm
LSP, hem yeni bir uygulama geliştirirken hem de mevcut bir uygulamayı geliştirirken veya değiştirirken akılda tutulması gereken çok faydalı bir fikirdir.

Yeni bir uygulama için sınıf hiyerarşisini tasarlarken, LSP, problem alanımızdaki kavramları zamanından önce genelleştirmediğimizden emin olmamıza yardımcı olur.

Bir alt sınıf ekleyerek veya değiştirerek mevcut bir uygulamayı geliştirirken, LSP'ye dikkat etmek, değişikliklerimizin üst sınıfın sözleşmesiyle uyumlu olmasını ve müşteri kodunun beklentilerinin karşılanmaya devam etmesini sağlamaya yardımcı olur .