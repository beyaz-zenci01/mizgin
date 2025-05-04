Ferhat Bey'in gönderdiği testlerin tamamı! 
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mizgin Müdür Olacak</title>
    <style>
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
        }
        .quiz-container {
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 600px;
            text-align: center;
            transition: all 0.3s ease;
        }
        h1 {
            color: #2c3e50;
            margin-bottom: 20px;
        }
        .question-counter {
            font-size: 14px;
            color: #7f8c8d;
            margin-bottom: 15px;
        }
        button.option {
            display: block;
            width: 100%;
            margin: 10px 0;
            padding: 12px;
            cursor: pointer;
            background: #ecf0f1;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            transition: all 0.2s;
        }
        button.option:hover {
            background: #d6eaf8;
            transform: translateY(-2px);
        }
        #next-btn {
            margin-top: 25px;
            padding: 12px 25px;
            background: #3498db;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            transition: all 0.3s;
        }
        #next-btn:hover {
            background: #2980b9;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(41, 128, 185, 0.4);
        }
        #next-btn:disabled {
            background: #bdc3c7;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        .selected {
            background: #a0d8ef !important;
            border-left: 5px solid #3498db !important;
        }
        .correct {
            background: #2ecc71 !important;
            color: white;
        }
        .wrong {
            background: #e74c3c !important;
            color: white;
        }
        #result {
            margin-top: 20px;
            font-size: 18px;
            font-weight: bold;
            min-height: 27px;
        }
        #final-result {
            animation: fadeIn 0.8s ease;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .progress-container {
            width: 100%;
            height: 8px;
            background: #ecf0f1;
            border-radius: 4px;
            margin: 20px 0;
            overflow: hidden;
        }
        #progress-bar {
            height: 100%;
            background: #3498db;
            width: 0%;
            transition: width 0.4s ease;
        }
        .timer {
            font-size: 18px;
            color: #e74c3c;
            font-weight: bold;
            margin-top: 10px;
        }
        .score-display {
            font-size: 20px;
            margin: 15px 0;
            color: #2c3e50;
        }
        #restart-btn {
            margin-top: 30px;
            padding: 12px 30px;
            font-size: 16px;
            background: #3498db;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
        }
        #restart-btn:hover {
            background: #2980b9;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(41, 128, 185, 0.4);
        }
    </style>
</head>
<body>
    <div class="quiz-container">
        <div class="question-counter" id="counter">Soru 1/<span id="total-questions">0</span></div>
        <div class="progress-container">
            <div id="progress-bar"></div>
        </div>
        <div class="score-display">Puan: <span id="score">0</span>/<span id="max-score">0</span></div>
        <h1 id="question">Soru Yükleniyor... Heyecanlısın mı?</h1>
        <div id="options"></div>
        <button id="next-btn" disabled>Sonraki Soru</button>
        <div id="result"></div>
        <div class="timer" id="timer" style="display: none;"></div>
    </div>

    <script>
        // Temel soru bankası
        let questionBank = [
            {
        question: "Gıda kaynaklı hastalıkların yayılmasına engel olmak için aşağıdakilerden hangisine dikkat edilmelidir?",
        options: [
            "Tüm ekip olarak el yıkama kural ve sürelerine uyulması",
            "Sandviçlerin misafire sıcak servis edilmesi",
            "Promosyon ürünlerinin stoklarının kontrol edilmesi",
            "Ürün gramajlarına dikkat edilmesi"
        ],
        answer: 0,
        explanation: "El yıkama, gıda güvenliğinin en temel kurallarındandır ve hastalık yayılımını önlemede en etkili yöntemlerden biridir."
    },
    {
        question: "Aşağıdakilerden hangisi bakterilerin zararlı etkileri arasındadır?",
        options: [
            "Gıdayı bulaşmadan korur",
            "Bakterilerin üremesi yavaşlar",
            "Gıda çürümelerine neden olur",
            "Hepsi"
        ],
        answer: 2,
        explanation: "Bakteriler gıdaların bozulmasına ve çürümesine neden olarak sağlık riski oluştururlar."
    },
    {
        question: "Aşağıdakilerden hangisi soğutucu dolaplarda bulunan ürünler için risk oluşturur?",
        options: [
            "Ürünlerin aşırı derecede doldurulmaması",
            "Gıdanın paketli saklanması",
            "Tüm gıda kaplarının kapalı olması",
            "Sıcak ürün konulması"
        ],
        answer: 3,
        explanation: "Sıcak ürünlerin soğutucuya konulması iç sıcaklığı artırarak diğer ürünlerin bozulmasına yol açabilir."
    },
    {
        question: "Aşağıdakilerden hangisi personel kıyafet yönetmeliği arasında yer almaz?",
        options: [
            "Çalışma süresi boyunca mutlaka şapka takılmalıdır",
            "Personel üniforması ile işe gidip gelebilir",
            "Çalışma saatleri içinde restoranda kolye, saat, sallanan küpe gibi aksesuarlar takılmamalıdır",
            "Altı kaymaz, lastik, vinil deri ayakkabı giyilmelidir"
        ],
        answer: 1,
        explanation: "Personel üniformaları sadece iş yerinde giyilmelidir, dışarıda giyilmesi hijyen kurallarına aykırıdır."
    },
    {
        question: "Aşağıdakilerden hangisi gıdayı uzun süre muhafaza etme yöntemleri arasında bulunmaz?",
        options: [
            "Dondurarak kullanım",
            "Çözündürme",
            "Konserve",
            "Pastörizasyon"
        ],
        answer: 1,
        explanation: "Çözündürme bir muhafaza yöntemi değil, dondurulmuş gıdaların kullanıma hazır hale getirilme işlemidir."
    },
    {
        question: "Kola kulesi nozzle ve difüzör (püskürtücü ve dağıtıcı) temizliklerinde hangi kimyasallar kullanılabilir?",
        options: [
            "D2-D1",
            "D44-D1",
            "D1-D10",
            "D3-D10"
        ],
        answer: 2,
        explanation: "Kola kulesi temizliğinde D1 ve D10 kimyasalları kullanılmalıdır."
    },
    {
        question: "Sanitizer su ölçümü sırasında aşağıdaki adımlardan hangisi doğrudur?",
        options: [
            "PPM şeridi suyun içinde 5 saniye bekletilmeli",
            "PPM şeridi suyun içinde 10 saniye sabit bir şekilde bekletilmeli",
            "PPM şeridi yeşil rengi almış ise uygundur",
            "PPM şeridi suyun içinde 10 saniye sallanmalıdır"
        ],
        answer: 1,
        explanation: "PPM şeridi 10 saniye boyunca sabit şekilde suda bekletilmeli ve renk değişimi bu şekilde doğru okunmalıdır."
    },
    {
        question: "Gün içinde çalışma istasyonlarının temizlik işlemi sırasında hangi kimyasal kullanılmalıdır?",
        options: [
            "D44",
            "D3",
            "D10",
            "D1"
        ],
        answer: 2,
        explanation: "D10 kimyasalı günlük temizliklerde kullanılan dezenfektandır."
    },
    {
        question: "Aşağıdakilerden hangisi buz makinesi temizliğinde kullanılan kimyasallardan biridir?",
        options: [
            "D3",
            "D44",
            "D2",
            "D52"
        ],
        answer: 3,
        explanation: "Buz makinesi temizliğinde D52 kimyasalı kullanılır."
    },
    {
        question: "Multiplex hat temizliği yapılırken hangi kimyasal kullanılmalıdır?",
        options: [
            "D2",
            "D3",
            "Divomil RD",
            "R6"
        ],
        answer: 2,
        explanation: "Multiplex hat temizliğinde Divomil RD kimyasalı kullanılmalıdır."
    },
    {
        question: "Soygun gibi acil durumda ne yapmalıyız?",
        options: [
            "Sakin kalıp, karşı çıkmadan kişinin dediğini yapmalı ve personel ile misafiri de bu doğrultuda yönlendirmeliyiz",
            "Hemen kaçmalıyız",
            "Karşı koyarak engellemeliyiz",
            "Olayı telefon kamerası ile kaydetmeliyiz"
        ],
        answer: 0,
        explanation: "Soygun durumunda sakin kalmak ve şiddetten kaçınmak en güvenli yoldur."
    },
    {
        question: "Aşağıdakilerden hangisi aylık yapılan 'İş Sağlığı ve Güvenliği ile ilgili ISG Görsel Kontrol Çizelgeleri'nden değildir?",
        options: [
            "İlk Yardım Malzemeleri Kontrol Çizelgesi",
            "Yangın Söndürme Cihazı Görsel Kontrol Çizelgesi",
            "Uyarı İkaz ve Bilgilendirme Levhaları Kontrol Çizelgesi",
            "Temizlik Görsel Kontrol Çizelgesi"
        ],
        answer: 3,
        explanation: "Temizlik kontrol çizelgesi ISG kapsamında değil, hijyen kontrol sürecinde yer alır."
    },
    {
        question: "Restoran 'ACİL DURUM PLANI' dahilinde, personellerden ACİL DURUM EKİPLERİ oluşturulmuş olmalı ve kişilere görev bildirimi yapıp, o konuda eğitim verilmeli ve bu ekiplerin listesi İSG klasöründe muhafaza edilmelidir. Aşağıdakilerden hangisi kurulan ekipler arasında yer almaz?",
        options: [
            "İlk Yardım Ekibi",
            "Arama Kurtarma Ekibi",
            "Tahliye ve Haberleşme Ekibi",
            "Ürünleri Kurtarma Ekibi"
        ],
        answer: 3,
        explanation: "Acil durum ekipleri arasında ürün kurtarma ekibi bulunmaz, öncelik can güvenliğidir."
    },
    {
        question: "Aşağıdakilerden hangisi acil eylem durumları için önceden alınması gereken önlemler arasında yer almaz?",
        options: [
            "Acil durum kapıları asla kilitlenmemeli ve dışarıya doğru açılır olmalıdır",
            "Restoranda bulunan raflar duvara sabitlenmelidir",
            "Önceden bir acil eylem planı oluşturulmalıdır",
            "Restorandan çıkılmayıp lobi alanında beklenmelidir"
        ],
        answer: 3,
        explanation: "Acil durumlarda binanın derhal tahliye edilmesi gerekir, içeride beklenmemelidir."
    },
    {
        question: "Ansül sistemini elle devreye alma yetkisi kimde bulunmaktadır?",
        options: [
            "Vardiyada bulunan herkes devreye alabilir",
            "Vardiyada bulunan yetkili müdür devreye alabilir",
            "Vardiyada bulunan ekip üyesi devreye alabilir",
            "Kimsenin yetkisi bulunmamaktadır"
        ],
        answer: 1,
        explanation: "Ansül sistemini devreye alma yetkisi sadece yetkili müdürde bulunur."
    },
    {
        question: "Aşağıdakilerden hangisi TAB Gıda değerleri arasında yer almaz?",
        options: [
            "Çeşitlilik",
            "Mükemmellik taahhüdü",
            "Şeffaf sorumluluklar",
            "Risk analizi"
        ],
        answer: 3,
        explanation: "Risk analizi TAB Gıda'nın temel değerleri arasında yer almaz, bu bir süreç yönetimi aracıdır."
    },
    {
        question: "Operasyonel sorumluluğun tanımı nedir?",
        options: [
            "Pürüzsüz bir vardiyanın sağlanabilmesi ve operasyonun sürekliliği için düzenli yapılan ve takip edilen işlerdir",
            "Çalışanların operasyonel sorumluluklarını yerine getirebilmeleri için yöneticilerin sahip olması gereken yeteneklerdir",
            "Operasyonu aksatan unsurların tespit edilmesi ve ortadan kaldırılmasıdır",
            "Restorana gerçekleştirilen denetimlerden yüksek skor alabilmektir"
        ],
        answer: 0,
        explanation: "Operasyonel sorumluluk, günlük işleyişin sorunsuz devam etmesini sağlamakla ilgilidir."
    },
    {
        question: "Yönetimsel sorumluluğun tanımı nedir?",
        options: [
            "Etkili bir iletişim kurarak operasyonu sorunsuz bir şekilde yönetmektir",
            "Çalışanların operasyonel sorumluluklarını yerine getirebilmeleri için yöneticilerin sahip olması gereken yeteneklerdir",
            "Yöneticilerin KST yaparak restoranı kontrol altında tutabilmesidir",
            "Restorana gerçekleştirilen denetimlerde denetmenlere eşlik ederek vardiya turu yapılmasıdır"
        ],
        answer: 1,
        explanation: "Yönetimsel sorumluluk, ekibin performansını artırmak için gerekli ortamı sağlamayı içerir."
    },
    {
        question: "Aşağıdakilerden hangisi yönetimsel becerilerden değildir?",
        options: [
            "Çalışanların eğitim eksiklerini tespit edebilme",
            "Perşembe kalibrasyonlarını yapabilme",
            "Ekibi motive edebilme kabiliyetine sahip olma",
            "Çalışanlar, yöneticiler ve misafirlerle doğru iletişimi kurabilme"
        ],
        answer: 1,
        explanation: "Perşembe kalibrasyonları operasyonel bir işlemdir, yönetimsel beceri değildir."
    },
    {
        question: "Kasa bölgesinde bir ekip üyesinin gülümsemesini sağlamak nasıl bir sorumluluktur?",
        options: [
            "Yönetimsel",
            "Operasyonel",
            "Yöneticinin sorumlulukları arasında değildir",
            "Hepsi"
        ],
        answer: 0,
        explanation: "Ekip motivasyonu ve davranışları yönetimsel sorumluluk kapsamındadır."
    },
    {
        question: "Aşağıdakilerden hangisi sorun çözme adımlarındandır?",
        options: [
            "Sorunu tanımlama",
            "Tüm muhtemel sebepleri belirle",
            "Muhtemel çözümleri belirle",
            "Hepsi"
        ],
        answer: 3,
        explanation: "Tüm bu adımlar sistematik sorun çözme sürecinin parçalarıdır."
    },
    {
        question: "Aşağıdakilerden hangisi beyin fırtınası adımlarından biri olarak değerlendirilmez?",
        options: [
            "Karşıdaki kişinin fikirleri eleştirilmemelidir",
            "Süre sınırlaması yoktur",
            "Niteliğe değil niceliğe bakılmalıdır",
            "Verilecek yanıtların amacı belirlenmelidir"
        ],
        answer: 1,
        explanation: "Beyin fırtınası genellikle belirli bir zaman limiti içinde yapılır."
    },
    {
        question: "Aşağıdakilerden hangisi delegasyonu ifade eder?",
        options: [
            "Sizin sorumluluğunuzdaki bir işi yaptırmak üzere astınıza sormak ya da sizin adınıza hareket etmesi için başkasını tayin etmektir",
            "Delegasyon bir iletişim biçimidir",
            "Ekibe verilecek eğitimin organize edilmesidir",
            "Ekip motivasyonunu sağlayabilme yeteneğidir"
        ],
        answer: 0,
        explanation: "Delegasyon, yetki ve sorumluluğun başkasına devredilmesi sürecidir."
    },
    {
        question: "Delege edilecek bir iş için kimse ilgi duymuyorsa ne yapılmalıdır?",
        options: [
            "İşi kendiniz yapın",
            "Durumu üst yöneticinize bildirin",
            "İş için ilgi uyandırın",
            "Çalışanların sorumluluk almasını bekleyin"
        ],
        answer: 2,
        explanation: "Yöneticinin görevlerinden biri de ekip üyelerinde görev isteği uyandırmaktır."
    },
    {
        question: "Bir iş delege edilirken kişinin yeteneğinde eksiklik gözlemleniyorsa nasıl bir yöntem uygulanmalıdır?",
        options: [
            "Eğitim vermeyi önerin",
            "İşi başkasına devredin",
            "Çalışana yeteneğini geliştirmesini söyleyin",
            "Herhangi bir şey yapılmasına gerek yoktur, iş direkt devredilebilir"
        ],
        answer: 0,
        explanation: "Delegasyon öncesinde kişinin gerekli yetkinliklere sahip olması için eğitim verilmelidir."
    },
    {
        question: "Aşağıdakilerden hangisinde delegasyonun adımları doğru verilmiştir?",
        options: [
            "İşi seçin-Kişiyi seçin-Bir plan oluşturun-İşi delege edin-Süreci takip etmeyi önerin",
            "Bir plan oluşturun ve işi delege edin",
            "Delegasyonun herhangi bir adımı yoktur, iş direkt devredilebilir",
            "Vardiya planını kontrol edin ve işi delege edin"
        ],
        answer: 0,
        explanation: "Etkili delegasyon için tüm bu adımların sistematik şekilde uygulanması gerekir."
    },
    {
        question: "Zamanı daha iyi yönetebilmek için aşağıdaki davranışlardan hangisi uygulanmamalıdır?",
        options: [
            "İşe C önceliğinden değil A önceliğinden başlamak",
            "Aynı anda birden fazla işe başlamak",
            "Yapılacak işler listesi oluşturmak",
            "Yapılacak işleri küçük parçalar halinde uygulamak"
        ],
        answer: 1,
        explanation: "Multitasking (aynı anda birden fazla iş yapma) zaman yönetiminde verimliliği düşürür, odaklanmayı zorlaştırır."
    },
    {
        question: "Peynir tekniği nedir?",
        options: [
            "Peynir sıcaklıklarını düzenli olarak takip etme",
            "Yerleşim planını doğru şekilde uygulama",
            "A Önceliğindeki bir işiniz çok büyükse küçük işler halinde uygulamak",
            "Servis süre takip çizelgesini kontrol etme"
        ],
        answer: 2,
        explanation: "Peynir tekniği, büyük işleri daha küçük ve yönetilebilir parçalara bölerek ilerleme sağlamaktır."
    },
    {
        question: "''C.R.I.S.P'' yöntemini aşağıdakilerden en iyi hangisi ifade eder?",
        options: [
            "İletişimi kolaylaştırır",
            "Vardiya kontrolünü sağlar",
            "Nasıl övgüde bulunulması gerektiğini açıklar",
            "Verimlilik hakkında bilgi edinmemizi sağlar"
        ],
        answer: 2,
        explanation: "CRISP yöntemi (Clear, Relevant, Immediate, Specific, Positive) etkili övgü verme tekniğidir."
    },
    {
        question: "Aşağıdakilerden hangisi C.R.I.S.P'in adımları arasında yer almaz?",
        options: [
            "Tutarlı",
            "Belirli",
            "Hemen",
            "Çeşitlilik"
        ],
        answer: 3,
        explanation: "CRISP'in açılımı: Clear (Açık), Relevant (İlgili), Immediate (Anında), Specific (Özel), Positive (Olumlu)"
    },
    {
        question: "Aşağıdakilerden hangisi Etkili İletişimde bulunması gereken özellikler arasında yer almaz?",
        options: [
            "Etkili Dinlemek",
            "Hoş görülü Olmak",
            "Empati Yapmak",
            "Eleştiriye Açık Olmamak"
        ],
        answer: 3,
        explanation: "Etkili iletişim için eleştiriye açık olmak önemli bir özelliktir."
    },
    {
        question: "İletişimde, alıcının mesajı alarak değerlendirdiğini ve yorumladığını gösterdikten sonra yanıt vermesine ne denir?",
        options: [
            "Kanal",
            "Çevre",
            "İletişim",
            "Geri Bildirim"
        ],
        answer: 3,
        explanation: "Geri bildirim, iletişim sürecinin tamamlandığını gösteren önemli bir aşamadır."
    },
    {
        question: "Aşağıdaki davranışlardan hangisi 'etkin dinleme' durumunda yapılmamalıdır?",
        options: [
            "Konuşanın gözlerine bakmak",
            "Ne söylediğini anlamaya çalışmak",
            "Başka şeyler düşünmemek",
            "Karşımızdaki kişiyi yargılamak"
        ],
        answer: 3,
        explanation: "Etkin dinlemede yargılama yapılmaz, anlamaya odaklanılır."
    },
    {
        question: "Aşağıdakilerden hangisi iletişim sürecinin öğeleri arasında değildir?",
        options: [
            "Gönderici",
            "Alıcı",
            "Kanal",
            "İşletici"
        ],
        answer: 3,
        explanation: "İletişimin temel öğeleri: Gönderici, alıcı, mesaj, kanal, geri bildirim ve bağlamdır."
    },
    {
        question: "Ben mesajlarının ana bileşenleri aşağıdakilerden hangisinde doğru olarak belirtilmiştir?",
        options: [
            "Davranışlar-Duygular-Etkiler",
            "Tepki-Etki-Duygular",
            "Sesler-Davranışlar-Tepki",
            "Duygu-Gözlem-Tavır"
        ],
        answer: 0,
        explanation: "Ben mesajı üç bölümden oluşur: 1) Somut davranış 2) Bu davranışın bizde yarattığı duygu 3) Davranışın somut etkisi"
    },
    {
        question: "Aşağıdakilerden hangisi 'Ben Mesajlarının' özelliklerinden birisi değildir?",
        options: [
            "Karşı tarafı dinlemeye ve anlamaya yöneliktir",
            "Bizde neden olduğu duyguları dile getiririz",
            "Karşı tarafın kişiliğine değil davranışa yöneliktir",
            "Saldırgan davranış modeline benzer"
        ],
        answer: 3,
        explanation: "Ben mesajları saldırgan değil, yapıcı iletişim tekniğidir."
    },
    {
        question: "İşe yeni başlayan ekip üyesine ilk olarak hangi eğitim adımı uygulanmalıdır?",
        options: [
            "Performans değerlendirme",
            "Temel Eğitim Videosu",
            "Mutfak İstasyon Testleri",
            "İstasyon Değerlendirme"
        ],
        answer: 1,
        explanation: "Yeni çalışanlara öncelikle temel oryantasyon eğitimleri verilmelidir."
    },
    {
        question: "4 Adımda Eğitim Metodunun 4.aşaması aşağıdakilerden hangisidir?",
        options: [
            "Takdir",
            "Takip",
            "Teşekkür",
            "Transfer"
        ],
        answer: 1,
        explanation: "4 Adım: 1) Hazırlık 2) Göster 3) Uygulat 4) Takip"
    },
    {
        question: "4.Adımda Eğitim Metodunda 'Hazırlık Aşaması' ile ilgili aşağıdaki ifadelerden hangisi yanlıştır?",
        options: [
            "Eğitim verilecek saat önemlidir",
            "Mutfak bölgesi eğitimi servis bölgesinde verilmelidir",
            "Eğitim verecek kişi bilgilere hakim olmalıdır",
            "Eğitim ekipmanları tam ve çalışır durumda olmalıdır"
        ],
        answer: 1,
        explanation: "Eğitimler ilgili çalışma alanında yapılmalıdır."
    },
    {
        question: "Vardiya yönetiminde 4 D kuralını aşağıdakilerden hangisi tanımlar?",
        options: [
            "Doğru sayı, Doğru Zaman, Doğru yeterlilik, Doğru yer",
            "Doğru Sipariş, Doğru servis, Doğru yeterlilik, Doğru karar",
            "Doğru iletişim, Doğru para kontrolü, Doğru zaman yönetimi, Doğru problem çözümü",
            "Hepsi"
        ],
        answer: 0,
        explanation: "4D Kuralı: Doğru sayıda personel, Doğru zamanda, Doğru yeterlilikte, Doğru yerde"
    },
    {
        question: "Çıtır soğan ile ilgili ifadelerden hangisi doğrudur?",
        options: [
            "Ambalajı yeni açılan çıtır soğan direkt kullanıma girer",
            "Ambalajı yeni açılan çıtır soğana 15 günlük HKA yazılır",
            "Açıldıktan sonra kalan miktar kuru depoya kaldırılır",
            "Hepsi"
        ],
        answer: 1,
        explanation: "Açılan çıtır soğan paketine 15 günlük HKA (Hazırlama-Kullanma Aralığı) yazılmalıdır."
    },
    {
        question: "Ekmek kalitesi ile ilgili belirtilen bilgilerden hangisi doğrudur?",
        options: [
            "Ekmekler kızartıldıktan sonra 30 sn içinde kullanılmalıdır",
            "Ekmekler açıldıktan sonra raf ömrü 10 gündür",
            "Ekmekler soğuk odada muhafaza edilir",
            "Ekmekler yerden en az 30 cm yukarıda olacak şekilde muhafaza edilmelidir"
        ],
        answer: 3,
        explanation: "Ekmekler her zaman yerden yüksekte ve uygun koşullarda muhafaza edilmelidir."
    },
    {
        question: "Aşağıdakilerin hangisinde açılmış olan füme etin HKA bilgileri doğru verilmiştir?",
        options: [
            "H:01.10.2021 Saat 12.00 - K:01.10.2021 Saat 14.00 - A:02.10.2021 Saat 10.00",
            "H:01.10.2021 Saat 12.00 - A:03.10.2021 Saat 12.00",
            "H:01.10.2021 Saat 12.00 - A:02.10.2021 Saat 22.00",
            "H:01.10.2021 Saat 12.00 - A:02.10.2021 Saat 12.00"
        ],
        answer: 3,
        explanation: "Açılan füme et 24 saat içinde tüketilmelidir (Hazırlama + 24 saat)."
    },
    {
        question: "Çikolata dolgulu kornet ürününe kaç gram sos kullanılmaktadır?",
        options: [
            "30 gr",
            "20 gr",
            "18 gr",
            "15 gr"
        ],
        answer: 2,
        explanation: "Çikolata dolgulu kornete 18 gram sos uygulanır."
    },
    {
        question: "Sundae dondurma hazırlarken bardağa kaç gram dondurma koyulur?",
        options: [
            "120 gr",
            "140 gr",
            "110 gr",
            "80 gr"
        ],
        answer: 0,
        explanation: "Standart sundae bardağına 120 gram dondurma konulmalıdır."
    },
    {
        question: "Aynı anda kaç adet Çikolatalı Sufle mikrodalga fırında ısıtılır?",
        options: [
            "1 adet",
            "2 adet",
            "3 adet",
            "Farketmez"
        ],
        answer: 1,
        explanation: "Mikrodalgada aynı anda maksimum 2 sufle ısıtılabilir."
    },
    {
        question: "Aynı anda kaç adet Tavuk Burger sandviç hazırlanır?",
        options: [
            "1 adet",
            "2 adet",
            "3 adet",
            "Farketmez"
        ],
        answer: 1,
        explanation: "Kalite standartları gereği aynı anda maksimum 2 tavuk burger hazırlanmalıdır."
    },
    {
        question: "Aynı anda en fazla kaç adet Vişneli Tatlı kızartılır?",
        options: [
            "2 adet",
            "3 adet",
            "4 adet",
            "5 adet"
        ],
        answer: 2,
        explanation: "Vişneli tatlılar aynı anda maksimum 4 adet olacak şekilde kızartılmalıdır."
    },
    {
        question: "Crispy Chicken sandviç çok mayonezli istenirse toplam kaç gram mayonez eklenmelidir?",
        options: [
            "21 gr",
            "20 gr",
            "30 gr",
            "11 gr"
        ],
        answer: 2,
        explanation: "Çok mayonezli Crispy Chicken sandviçte toplam 30 gram mayonez kullanılır."
    },
    {
        question: "Mega Double Cheeseburger sandviç içerisine kaç gram ketçap konur?",
        options: [
            "20 gr",
            "21 gr",
            "7 gr",
            "14 gr"
        ],
        answer: 3,
        explanation: "Mega Double Cheeseburger'e 14 gram ketçap konulmalıdır."
    },
    {
        question: "Açılmış olan A1 Steak sosun soğuk odada bekleme süresi ne kadardır?",
        options: [
            "3 gün",
            "7 gün",
            "10 gün",
            "14 gün"
        ],
        answer: 1,
        explanation: "Açılan A1 Steak sos soğuk odada maksimum 7 gün saklanabilir."
    },
    {
        question: "Whopper sandviç içerisine kaç gram ketçap konulmalıdır?",
        options: [
            "9 gram",
            "21 gram",
            "14 gram",
            "7 gram"
        ],
        answer: 2,
        explanation: "Whopper sandviçte 14 gram ketçap kullanılır."
    },
    {
        question: "Chicken Royal eti pişme süresi ne kadardır?",
        options: [
            "3.00 dakika",
            "3.15 dakika",
            "3.30 dakika",
            "3.45 dakika"
        ],
        answer: 2,
        explanation: "Chicken Royal eti tam 3 dakika 30 saniyede pişer."
    },
    {
        question: "Chicken Tenders eti ÇÜTÜ de en fazla kaç adet bekletilir?",
        options: [
            "12 adet",
            "24 adet",
            "20 adet",
            "18 adet"
        ],
        answer: 3,
        explanation: "Chicken Tenders eti Çıkmaz Ürün Tutucu Ünitesi'nde maksimum 18 adet bekletilebilir."
    },
    {
        question: "Onion Rings ÇÜTÜ de ne kadar süre bekler?",
        options: [
            "5 dakika",
            "10 dakika",
            "15 dakika",
            "Beklemez"
        ],
        answer: 1,
        explanation: "Onion Rings ÇÜTÜ'de maksimum 10 dakika bekletilebilir."
    },
    {
        question: "Gurme Tavuk ile ilgili aşağıdaki ifadelerden hangisi yanlıştır?",
        options: [
            "Cook Out sıcaklığı min 72 °C dir",
            "Probu, etin yanından diğer tarafını delmeyecek şekilde merkeze kadar ete boylamasına batırın",
            "Ürün derecesi vattan çıktığı andan itibaren ölçülmelidir",
            "Gurme tavuk eti 4:30 dk da pişer"
        ],
        answer: 2,
        explanation: "Gurme Tavuk etinin sıcaklığı vattan çıkar çıkmaz değil, 30 saniye bekletildikten sonra ölçülmelidir."
    },
    {
        question: "Aşağıdakilerden hangisi Restoranda Probe Vibes (Probe mendili) bitmesi veya kuruması durumunda yapılması gereken uygulamadır?",
        options: [
            "Mini üçlü sistem kullanılarak probe sanitize edilmelidir",
            "Probe sıcak suda minimum 2 dakika bekletildikten sonra kullanılabilir",
            "Probe mendili D10 ilave edilerek kullanılmalıdır",
            "Probe mendili H500 ilave edilerek kullanılmalıdır"
        ],
        answer: 0,
        explanation: "Probe mendili olmadığında mini üçlü sistem (D10+D1+su) kullanılarak probe sanitize edilmelidir."
    },
    {
        question: "Çalışma istasyonlarında kullanılan bezler nasıl saklanmalıdır?",
        options: [
            "Sanitizer suyun içine tamamen batırılmalıdır",
            "Bir yerde asılı olarak bırakılmalıdır",
            "Sanitizer kova içerisinde solüsyon eklemeden saklanmalıdır",
            "Çalışma alanında bırakılmalıdır"
        ],
        answer: 0,
        explanation: "Kullanılan bezler daima sanitizer solüsyonu içinde tamamen batırılmış halde saklanmalıdır."
    },
    {
        question: "Aşağıdakilerden hangisi 12 Kritik Gıda Güvenliği başlığından biri değildir?",
        options: [
            "Sağlık Kontrolleri",
            "Çapraz Bulaşma",
            "Gıda Kalitesi",
            "Sıcaklık Kontrolü"
        ],
        answer: 2,
        explanation: "Gıda kalitesi kritik güvenlik başlıkları arasında yer almaz, bu bir kalite kontrol konusudur."
    },
    {
        question: "Whopper etinin cook out sıcaklığı kaç derece olmalıdır?",
        options: [
            "min 68°C",
            "68 °C-79 °C",
            "min 74 °C",
            "60-79 °C"
        ],
        answer: 2,
        explanation: "Whopper etinin minimum çıkış sıcaklığı 74°C olmalıdır."
    },
    {
        question: "Vardiya müdürü restoranında rutin kontrolleri gerçekleştiriyor. Bu sırada, kızartma makinesi yağ seviyelerinin azaldığını, sıcak suyun akmadığını, bir chute ampulunun arızalandığını ve serviste mini ketçap sos stoklarının bittiğini tespit etmiştir. Vardiya müdürü ilk olarak hangi konuyla ilgili aksiyon alınmasını sağlamalıdır?",
        options: [
            "Mini ketçap stoklarının tamamlanması",
            "Sıcak suların kontrolü",
            "Kızartma makinesi yağ seviyelerinin tamamlanması",
            "Chute ampulunun tamamlanması"
        ],
        answer: 1,
        explanation: "Sıcak su olmadan temizlik ve hijyen işlemleri yapılamayacağı için bu en acil çözülmesi gereken sorundur."
    },
    {
        question: "ÇÜTÜ de bulunan King Chicken eti en az kaç derece olmalıdır?",
        options: [
            "min 60°C",
            "min 65 °C",
            "min 68 °C",
            "min 74 °C"
        ],
        answer: 3,
        explanation: "King Chicken etinin ÇÜTÜ'deki minimum sıcaklığı 74°C olmalıdır."
    },
    {
        question: "Hamburger eti cook out sıcaklığı kaç olmalıdır?",
        options: [
            "min 68°C",
            "68 °C-79°C",
            "min 74 °C",
            "60-79 °C"
        ],
        answer: 2,
        explanation: "Hamburger etlerinin minimum çıkış sıcaklığı 74°C olmalıdır."
    },
    {
        question: "Soğuk odada bulunan peynirin derecesi kaç olmalıdır?",
        options: [
            "1-4 °C",
            "1-6°C",
            "-18/-23 °C",
            "18/29 °C"
        ],
        answer: 0,
        explanation: "Peynirler soğuk odada 1-4°C arasında muhafaza edilmelidir."
    },
    {
        question: "Aşağıdakilerden hangisi Kritik Gıda Güvenliği ifadesi arasında yer almaz?",
        options: [
            "ÇÜTÜ deki ürünlerin sıcaklığının min 60 °C olması",
            "Domatesin bekleme süresinin dolması",
            "Dondurma sütü sıcaklığının 1-4 °C arasında olması",
            "BİB Kola SKT nin geçmesi"
        ],
        answer: 1,
        explanation: "Domatesin bekleme süresi kalite ile ilgilidir, kritik gıda güvenliği kapsamında değildir."
    },
    {
        question: "Fuse Tea'nin akış hızı nedir?",
        options: [
            "10 sn 5 oz",
            "Akış hızı yoktur",
            "4 sn 10 oz",
            "5 sn 4 oz"
        ],
        answer: 1,
        explanation: "Fuse Tea'nin belirli bir akış hızı standardı bulunmamaktadır."
    },
    {
        question: "Ebro Cihazında polar madde ölçümünde görülen turuncu-sarı ışık neyi ifade eder?",
        options: [
            "Yağ Kirli",
            "Yağ Temiz",
            "Yağ yeteri derecede ısınmamış",
            "Yağ kritik seviyede her an atık olabilir"
        ],
        answer: 3,
        explanation: "Turuncu-sarı ışık yağın kritik seviyede olduğunu ve değiştirilmesi gerektiğini gösterir."
    },
    {
        question: "Multiplex hat sanitasyon temizliği (line temizliği) hangi sıklıkla yapılmalıdır?",
        options: [
            "3 ay",
            "1 ay",
            "6 ay",
            "45 gün"
        ],
        answer: 1,
        explanation: "Multiplex hat temizliği ayda bir düzenli olarak yapılmalıdır."
    },
    {
        question: "Kızartma makinesinde yağ sıcaklığını ayarlamak için hangi kod kullanılmalıdır?",
        options: [
            "1650",
            "1658",
            "1652",
            "1653"
        ],
        answer: 2,
        explanation: "Kızartma makinesi yağ sıcaklığı ayarı için 1652 kodu kullanılır."
    },
    {
        question: "Paket servis ile ilgili bilgilerden hangisi yanlıştır?",
        options: [
            "Paket içerisinde sandviçler en fazla 2 adet üst üste konulabilir",
            "Patates kızartması paket içerisine yerleştirilirken dik olarak konulmalıdır",
            "İçecekler doldurulurken içecek bardağının 2 cm aşağısında kalacak şekilde doldurulur",
            "Paket serviste, sıcakla soğuk ürünler aynı torbaya konulmaz"
        ],
        answer: 2,
        explanation: "İçecekler taşmayı önlemek için 1 cm boşluk bırakılarak doldurulmalıdır, 2 cm değil."
    },
    {
        question: "Aşağıdaki servis tepsilerine ait bilgilerden hangisi doğrudur?",
        options: [
            "Servis Tepsileri silinerek temizlenmelidir",
            "Servis Tepsileri üçlü aşamada yıkanmalıdır",
            "Servis Tepsileri H500 ile temizlenmelidir",
            "Hiçbiri"
        ],
        answer: 1,
        explanation: "Servis tepsileri üçlü aşamada (ön durulama, deterjanlı yıkama, durulama) yıkanmalıdır."
    },
    {
        question: "Sevkiyat prosedürle ilgili bilgilerden hangisi yanlıştır?",
        options: [
            "Ürünlerin son kullanım tarihleri kontrol edilmelidir",
            "Ürünlerin ambalaj kontrolleri, ezilme, çözünme, buzlanma, ıslanma vb. oluşabilecek problemler kontrol edilmelidir",
            "Sevkiyat öncesi depo düzenleri yapıldıktan sonra mutlaka soğutucu ve dondurucu odalarının sıcaklıklarının alınması gerekir",
            "Sevkiyat ile restorana teslim edilen ürünler uygun sıcaklık derecelerinde değil ise de ürünler teslim alınmalıdır"
        ],
        answer: 3,
        explanation: "Uygun sıcaklıkta olmayan ürünler kesinlikle teslim alınmamalıdır."
    },
    {
        question: "Burger King markamızda misafirlere verilmesi gereken servis süresi ne kadardır?",
        options: [
            "3.00 dakika",
            "2.45 dakika",
            "2.00 dakika",
            "2.15 dakika"
        ],
        answer: 1,
        explanation: "Burger King servis standartlarına göre maksimum servis süresi 2 dakika 45 saniyedir."
    },
    {
        question: "Burger King markamızda kullanılan Guest Trac uygulaması nedir?",
        options: [
            "Misafir Memnuniyeti Değerlendirme Anketi",
            "Servis Süresi Değerlendirme Anketi",
            "Restoran Güvenliği Değerlendirme Anketi",
            "İş Güvenliği Değerlendirme Anketi"
        ],
        answer: 0,
        explanation: "Guest Trac, misafir memnuniyetini ölçmek için kullanılan bir anket sistemidir."
    },
    {
        question: "Aşağıdakilerden hangisi KST derece alımı işlemi için gerekli olan araç ve gereçlerden biri değildir?",
        options: [
            "Dondurma Temizlik Fırçası",
            "Yüzey uçlu termometre",
            "Probe mendili",
            "Sanitize edilmiş Çütü peni"
        ],
        answer: 0,
        explanation: "Dondurma temizlik fırçası KST ölçümünde kullanılmaz."
    },
    {
        question: "Patates servis standartları ile ilgili bilgilerden hangisi yanlıştır?",
        options: [
            "Patates kızartma yağı kalitesi her gün kontrol edilmelidir",
            "Tuzlama işlemi patateslerden 30 cm yukarıda yapılmalıdır",
            "Patatesler tuzlandıktan sonra tuzun dağılması için karıştırılmalıdır",
            "Paketlenen patatesler 1 dk boyunca patates ünitesinde bekleyebilir"
        ],
        answer: 3,
        explanation: "Paketlenen patatesler hemen servis edilmeli, bekletilmemelidir."
    },
    {
        question: "Dondurma Makinesi ile ilgili bilgilerden hangisi yanlıştır?",
        options: [
            "Süt seviyesi her gün uygun şekilde ayarlanmalıdır",
            "Süt içerisinde siyah parçalar bulunmamalıdır",
            "Dondurma makinesi temizleme çizelgesine yıkama tarihi belirtilmelidir",
            "Dondurma makinesi çırpıcıları sadece kapanışta detaylı yıkanmalıdır"
        ],
        answer: 3,
        explanation: "Dondurma makinesi çırpıcıları gün içinde de düzenli olarak temizlenmelidir."
    },
    {
        question: "Burger King servis standartlarına dair aşağıda verilen bilgilerden hangisi doğru belirtilmemiştir?",
        options: [
            "Tepsiye direkt tuz konulmaz misafir isteğine göre servis edilir",
            "Büyük veya Dublex seçim istendiğinde 1 büyük ayran servis edilir",
            "Menü satın alan müşterilerimizin tepsilerine 2 adet Ketçap direkt olarak konulmalıdır",
            "Misafirlerimiz yan ürün satın aldıysa ücretli soslardan ücretsiz verilebilir"
        ],
        answer: 3,
        explanation: "Ücretli soslar yan ürün alınsa bile ücretsiz verilmez, ayrıca satılır."
    },
    {
        question: "Aşağıda çıtır peynir ile ilgili belirtilen bilgilerden hangisi yanlıştır?",
        options: [
            "Donuk çıtır peynirler spesiyal mobilde poşet içinde saklanabilir",
            "Çıtır Peynirler 4-6-9 adet olarak misafire servis edilir",
            "Çıtır peynir sıcaklığı 174+/- 3 derece olan vatta pişirilmelidir",
            "Çıtır peynir chutta 10 dakikalık time ile bekleyebilir"
        ],
        answer: 3,
        explanation: "Çıtır peynir chutta maksimum 5 dakika bekleyebilir."
    },
    {
        question: "Aşağıdaki genel bilgilerinden hangisi yanlıştır?",
        options: [
            "Çütü pen ve ızgaralarında çatlak olmamalı ve temiz olmalıdır",
            "Sanitize solüsyonlar gıdaya yakın saklanmamalıdır",
            "Kullanılan patates küreği sağlam olmalıdır",
            "Buz makinesinden kola kulesine buz tamamlanırken bardak, pen gibi malzemeler kullanılabilir"
        ],
        answer: 3,
        explanation: "Buz transferi sadece temiz ve uygun kaplarla yapılmalıdır, bardak/pen kullanılmamalıdır."
    },
    {
        question: "Bardağa koyulan Sprite içeceği bekleme süresi ne kadardır?",
        options: [
            "5 Dakika",
            "2 Dakika",
            "10 Dakika",
            "Beklemez"
        ],
        answer: 3,
        explanation: "Sprite gibi gazlı içecekler hemen servis edilmeli, bekletilmemelidir."
    },
    {
        question: "Kahve ürünleri paketi açıldıktan sonra paket ağzı kapatılarak ne kadar süre bekleyebilir?",
        options: [
            "5 Gün",
            "10 Gün",
            "15 Gün",
            "14 Gün"
        ],
        answer: 3,
        explanation: "Açılan kahve ürünleri maksimum 14 gün saklanabilir."
    },
    {
        question: "Hazırlanan ton balıklı salatalar servis soğutucuda ne kadar süre muhafaza edilmelidir?",
        options: [
            "2 Saat",
            "4 Saat",
            "6 Saat",
            "12 Saat"
        ],
        answer: 1,
        explanation: "Ton balıklı salatalar servis soğutucuda maksimum 4 saat bekletilebilir."
    },
    {
        question: "Chicken Nugget ürünün Chute bekleme süresi ne kadardır?",
        options: [
            "5 Dakika",
            "10 Dakika",
            "20 Dakika",
            "Beklemez"
        ],
        answer: 1,
        explanation: "Chicken Nugget chute'da maksimum 10 dakika bekleyebilir."
    },
    {
        question: "Tavukburger ürününün Çoklu ürün tutma ünitesinde (ÇÜTÜ) tutma süresi ne kadardır?",
        options: [
            "30 Dakika",
            "40 Dakika",
            "45 Dakika",
            "1 Saat"
        ],
        answer: 2,
        explanation: "Tavukburger ürünü ÇÜTÜ'de maksimum 45 dakika bekletilebilir."
    },
    {
        question: "Hazırlanan Cheeseburger'lerin Chute'te bekleme süresi kaç dakikadır?",
        options: [
            "5 Dakika",
            "10 Dakika",
            "20 Dakika",
            "Beklemez"
        ],
        answer: 1,
        explanation: "Cheeseburger'ler chute'te maksimum 10 dakika bekleyebilir."
    },
    {
        question: "Gurme Tavuk etlerinin ÇÜTÜ'de bekleme süresi aşağıdakilerden hangisidir?",
        options: [
            "30 Dakika",
            "40 Dakika",
            "45 Dakika",
            "1 Saat"
        ],
        answer: 2,
        explanation: "Gurme Tavuk etleri ÇÜTÜ'de maksimum 45 dakika bekletilebilir."
    },
    {
        question: "Sabah saat 10.00'da oda sıcaklığına prep çıkartılırken hangi satış aralığı baz alınmalıdır?",
        options: [
            "Saat 12.00-14.00 arası beklenen satış",
            "Saat 10.00-12.00 arası beklenen satış",
            "Saat 08.00-10.00 arası beklenen satış",
            "Saat 10.00 daki beklenen satış"
        ],
        answer: 1,
        explanation: "Prep çıkartılırken bir sonraki 2 saatlik satış tahmini baz alınmalıdır."
    },
    {
        question: "Kova Big King Sos soğutucu dolapta açılmış ambalaj ömrü aşağıdakilerden hangisidir?",
        options: [
            "5 Gün",
            "7 Gün",
            "8 Gün",
            "10 Gün"
        ],
        answer: 1,
        explanation: "Açılan Big King Sos kovası soğutucuda maksimum 7 gün saklanabilir."
    },
    {
        question: "Aşağıdaki bilgilerden hangisi yanlıştır?",
        options: [
            "Garlic sos kullanılmadan önce oda sıcaklığına 2 saat beklemelidir",
            "Çıtır Soğan soğuk odadan çıkarıldıktan sonra kullanılmadan önce oda sıcaklığında 2 saat beklemelidir",
            "Beyaz küp peynir soğuk depoda 7 gün bekleyebilir",
            "Dilimlenmiş salatalık soğuk depoda 1 gün bekleyebilir"
        ],
        answer: 2,
        explanation: "Beyaz küp peynir soğuk depoda 5 gün bekleyebilir, 7 gün değil."
    },
    {
        question: "Aşağıdakilerden hangisi restoran karlılığını arttırmanın yollarındandır?",
        options: [
            "Satışları artırmak ve maliyeti kontrol etmek",
            "ISG kurallarına dikkat etmek",
            "Kişisel hijyen kurallarına uymak",
            "Sevkiyatta gelmeyen ürünler için tutanak tutmak"
        ],
        answer: 0,
        explanation: "Karlılık için temel prensip satış artışı ve maliyet kontrolüdür."
    },
    {
        question: "Aşağıdakilerden hangisi kontrol edilebilen giderlerden biridir?",
        options: [
            "Enerji",
            "Atık",
            "Yemek",
            "Hepsi"
        ],
        answer: 3,
        explanation: "Tüm bu giderler doğru uygulamalarla kontrol altına alınabilir."
    },
    {
        question: "Aşağıdaki şıklardan hangisinin porsiyonlamalarla doğrudan ilgisi yoktur?",
        options: [
            "Soslama",
            "Kızarmış Ürünler",
            "Yağ seviyesi",
            "İçecekler"
        ],
        answer: 2,
        explanation: "Yağ seviyesi porsiyonlama ile değil, makine bakımı ile ilgilidir."
    },
    {
        question: "Aşağıdakilerden hangisi atık çeşitleri arasında yer almaz?",
        options: [
            "Kabul edilebilir atıklar",
            "Düzensiz atıklar",
            "Kabul edilemez atıklar",
            "Kayıtsız atıklar"
        ],
        answer: 1,
        explanation: "Atık çeşitleri arasında 'düzensiz atıklar' tanımı bulunmamaktadır."
    },
    {
        question: "Aşağıdakilerden hangisi kabul edilebilir atıklara örnek verilemez?",
        options: [
            "Dondurma makine temizliğinde atılan süt",
            "Hat sanitasyonunda atık olan şurup",
            "Yere dökülen domates prebi",
            "KST atıkları"
        ],
        answer: 2,
        explanation: "Yere düşen gıdalar kabul edilemez atık sınıfına girer."
    },
    {
        question: "Aşağıdaki seçeneklerin hangisinde yönetimdeki kontrol alanları yanlış belirtilmiştir?",
        options: [
            "Atık",
            "Kampanyalar",
            "Porsiyonlama",
            "Envanter"
        ],
        answer: 1,
        explanation: "Kampanyalar yönetim kontrol alanı değil, pazarlama faaliyetidir."
    },
    {
        question: "Envanter kaydı oluşturulurken doğru formül aşağıdakilerin hangisinde verilmiştir?",
        options: [
            "Başlangıç stoğu (Açılış)+ Satın alınan ürün - Satılan ürün - Personel Yemeği- Atık-Promosyonlar= Sayım Stoğu(Açılış)",
            "Başlangıç stoğu (Açılış)+ Satın alınan ürün - Satılan ürün - Personel Yemeği- Atık-= Sayım Stoğu(Kapanış)",
            "Başlangıç stoğu (Açılış)+ Satın alınan ürün - Satılan ürün -Atık-Promosyonlar= Sayım Stoğu(Kapanış)",
            "Başlangıç stoğu (Açılış)+ Satın alınan ürün - Satılan ürün - Personel Yemeği- Atık-Promosyonlar= Sayım Stoğu(Kapanış)"
        ],
        answer: 3,
        explanation: "Envanter formülü tüm çıkış kalemlerini (satış, personel yemeği, atık, promosyon) içermelidir."
    },
    {
        question: "Ana kasa şifre değişikliği hangi sıklıkla yapılmalıdır?",
        options: [
            "3 ayda bir kere",
            "Restoran devirlerinde",
            "Ayda 1 kere ve gerektikçe",
            "Kapanış işlemleri sırasında"
        ],
        answer: 2,
        explanation: "Kasa şifreleri aylık periyotlarla ve ihtiyaç halinde değiştirilmelidir."
    },
    {
        question: "Kapanış vardiyalarında yazar kasa çekmeceleri neden açık bırakılmalıdır?",
        options: [
            "Olası bir hırsızlık ihtimaline karşılık çekmecelerin zorlanmaması için",
            "Dezenfekte etmek için",
            "Temizlik yapmak için",
            "Çekmeceler açık bırakılmaz"
        ],
        answer: 0,
        explanation: "Çekmecelerin açık bırakılması hırsızlık durumunda zorlanarak hasar görmesini engeller."
    },
    {
        question: "Bir vardiya kontrolünde aşağıdakilerden hangisi öncelikli değildir?",
        options: [
            "Ekipman",
            "Ürün",
            "Personel",
            "Yazar kasa çekmecesi"
        ],
        answer: 3,
        explanation: "Vardiya kontrolünde kasa çekmecesi diğerleri kadar öncelikli değildir."
    }
];

        // Oyun durumu
        let questions = [];
        let currentQuestion = 0;
        let score = 0;
        let userAnswer = null;
        let timerInterval;

        // Soruları karıştır
        function shuffleQuestions() {
            // Soruların bir kopyasını oluştur
            let shuffled = [...questionBank];
            
            // Soru sırasını karıştır
            shuffled = shuffled.sort(() => Math.random() - 0.5);
            
            // Her sorunun şıklarını karıştır (doğru cevabın indexini güncelle)
            shuffled.forEach(q => {
                const correctAnswer = q.options[q.answer];
                q.options = q.options.sort(() => Math.random() - 0.5);
                q.answer = q.options.indexOf(correctAnswer);
            });
            
            return shuffled;
        }

        // Oyunu başlat
        function startGame() {
            questions = shuffleQuestions();
            currentQuestion = 0;
            score = 0;
            
            document.getElementById('total-questions').textContent = questions.length;
            document.getElementById('max-score').textContent = questions.length * 1;
            document.getElementById('score').textContent = score;
            
            showQuestion();
        }

        function showQuestion() {
            clearInterval(timerInterval);
            document.getElementById('timer').style.display = 'none';
            
            const question = questions[currentQuestion];
            document.getElementById('question').textContent = question.question;
            document.getElementById('counter').textContent = `Soru ${currentQuestion + 1}/${questions.length}`;
            document.getElementById('progress-bar').style.width = `${((currentQuestion + 1) / questions.length) * 100}%`;
            
            const optionsDiv = document.getElementById('options');
            optionsDiv.innerHTML = '';
            
            question.options.forEach((option, index) => {
                const button = document.createElement('button');
                button.className = 'option';
                button.textContent = option;
                button.onclick = () => selectOption(index);
                optionsDiv.appendChild(button);
            });
            
            document.getElementById('next-btn').disabled = true;
            document.getElementById('result').textContent = '';
        }

        function selectOption(index) {
            userAnswer = index;
            const buttons = document.querySelectorAll('.option');
            buttons.forEach(btn => {
                btn.classList.remove('selected');
                btn.disabled = false;
            });
            buttons[index].classList.add('selected');
            document.getElementById('next-btn').disabled = false;
        }

        function startTimer(duration, display, callback) {
            let timer = duration;
            display.style.display = 'block';
            display.textContent = `Sonraki soruya geçiş: ${timer}s`;
            
            timerInterval = setInterval(function () {
                display.textContent = `Sonraki soruya geçiş: ${--timer}s`;
                
                if (timer <= 0) {
                    clearInterval(timerInterval);
                    display.style.display = 'none';
                    callback();
                }
            }, 1000);
        }

        document.getElementById('next-btn').addEventListener('click', () => {
            const question = questions[currentQuestion];
            const buttons = document.querySelectorAll('.option');
            
            buttons.forEach(btn => btn.disabled = true);
            
            if (userAnswer === question.answer) {
                score += 1; // Her doğru cevap 1 puan
                buttons[userAnswer].classList.add('correct');
                document.getElementById('result').innerHTML = `
                    ? İştee buu! Bebeğim Gibisin ?? Doğru yaptın. Sana 1 Puan ve çokça sevgiler <small style="display:block;font-weight:normal">${question.explanation}</small>
                `;
            } else {
                buttons[userAnswer].classList.add('wrong');
                buttons[question.answer].classList.add('correct');
                document.getElementById('result').innerHTML = `
                    ? Yanlış Yaptın Mizigo! Sıfffır Sıffır Sıffır!<small style="display:block;font-weight:normal">Doğrusu şu şekerim: ${question.options[question.answer]}<br>${question.explanation}</small>
                `;
            }
            
            document.getElementById('score').textContent = score;
            document.getElementById('next-btn').disabled = true;
            
            // 7 saniye timer başlat
            startTimer(7, document.getElementById('timer'), () => {
                if (currentQuestion < questions.length - 1) {
                    currentQuestion++;
                    showQuestion();
                } else {
                    showFinalResult();
                }
            });
        });

        function showFinalResult() {
            const totalPossibleScore = questions.length * 1;
            document.querySelector('.quiz-container').innerHTML = `
                <div id="final-result">
                    <h1>Test Tamamlandı! </h1>
                    <p style="font-size: 24px; margin: 30px 0;">Puan: <strong>${score}/${totalPossibleScore}</strong></p>
                    <p style="color: #7f8c8d;">${getResultMessage(score, totalPossibleScore)}</p>
                    <button id="restart-btn">Tekrar Başlat</button>
                </div>
            `;
            
            document.getElementById('restart-btn').addEventListener('click', startGame);
        }

        function getResultMessage(score, total) {
            const percentage = (score / total) * 100;
            if (percentage >= 90) return "Tam puan aldın Bebeğimm Gibisinn! Sen artık Müdürsün hayatooo??";
			if (percentage >= 80) return "Tam puan aldın Mizgin! Sınavı geçtin ama 1 soru daha yapmasan kalacaktın. Daha fazlasını alman gerek.?";
            if (percentage >= 70) return "Müdür olmak için yeterli puan mı sence?? 10 soru daha bilmen gerek! ??";
            if (percentage >= 50) return "Mizgin soruların yarısını çözdün! Daha fazla çözmen gerekiyor cano.";
			if (percentage >= 40) return "Mizo bu ne? Hazar daha fazla çözerdi!.";
            return "Daha fazla çalışmaya ihtiyacın var. Pes etme Mizgin!??";
        }

        // Oyunu ilk başlatma
        startGame();
    </script>
</body>
</html>
