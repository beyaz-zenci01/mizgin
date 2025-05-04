<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Burger King 100 Soruluk Test</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #f5f5f5;
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
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0,0,0,0.1);
            width: 100%;
            max-width: 800px;
        }
        .question-info {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            font-weight: bold;
        }
        #question {
            font-size: 20px;
            margin-bottom: 25px;
            color: #333;
        }
        .options {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 30px;
        }
        .option {
            padding: 15px;
            background: #f0f0f0;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
            text-align: left;
        }
        .option:hover {
            background: #e0e0e0;
        }
        .selected {
            background: #d4e6f1;
            border-left: 5px solid #3498db;
        }
        .correct {
            background: #d4edda;
            color: #155724;
        }
        .wrong {
            background: #f8d7da;
            color: #721c24;
        }
        #next-btn {
            display: block;
            width: 100%;
            padding: 15px;
            background: #3498db;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            cursor: pointer;
            margin-top: 20px;
        }
        #next-btn:disabled {
            background: #cccccc;
            cursor: not-allowed;
        }
        #result {
            margin-top: 20px;
            padding: 15px;
            border-radius: 8px;
            font-weight: bold;
        }
        .correct-answer {
            background: #d4edda;
            color: #155724;
        }
        .progress-container {
            width: 100%;
            background: #f0f0f0;
            border-radius: 5px;
            margin: 20px 0;
        }
        #progress-bar {
            height: 10px;
            background: #3498db;
            border-radius: 5px;
            width: 0%;
            transition: width 0.5s;
        }
    </style>
</head>
<body>
    <div class="quiz-container">
        <div class="question-info">
            <div id="counter">Soru 1/100</div>
            <div id="score">Puan: 0/100</div>
        </div>
        <div class="progress-container">
            <div id="progress-bar"></div>
        </div>
        <div id="question"></div>
        <div class="options" id="options"></div>
        <button id="next-btn" disabled>Sonraki Soru</button>
        <div id="result"></div>
    </div>

    <script>
        // 100 SORULUK VERİ SETİ
        const questionBank = [
            // 1. Soru
    {
        question: "Gıda kaynaklı hastalıkların yayılmasına engel olmak için aşağıdakilerden hangisine dikkat edilmelidir?",
        options: [
            "Tüm ekip olarak el yıkama kural ve sürelerine uyulması",
            "Sandviçlerin misafire sıcak servis edilmesi",
            "Promosyon ürünlerinin stoklarının kontrol edilmesi",
            "Ürün gramajlarına dikkat edilmesi"
        ],
        answer: 1,
        explanation: "El hijyeni gıda kaynaklı hastalıkların önlenmesinde en kritik faktördür."
    },
    // 2. Soru
    {
        question: "Aşağıdakilerden hangisi bakterilerin zararlı etkileri arasındadır?",
        options: [
            "Gıdayı bulaşmadan korur",
            "Bakterilerin üremesi yavaşlar",
            "Gıda çürümelerine neden olur",
            "Hepsi"
        ],
        answer: 2,
        explanation: "Bakteriler gıdaların bozulmasına ve çürümesine neden olur."
    },
    // 3. Soru
    {
        question: "Aşağıdakilerden hangisi soğutucu dolaplarda bulunan ürünler için risk oluşturur?",
        options: [
            "Ürünlerin aşırı derecede doldurulmaması",
            "Gıdanın paketli saklanması",
            "Tüm gıda kaplarının kapalı olması",
            "Sıcak ürün konulması"
        ],
        answer: 3,
        explanation: "Sıcak ürünler soğutucunun iç sıcaklığını yükselterek diğer gıdaların bozulmasına yol açar."
    },
    // 4. Soru
    {
        question: "Aşağıdakilerden hangisi personel kıyafet yönetmeliği arasında yer almaz?",
        options: [
            "Çalışma süresi boyunca mutlaka şapka takılmalıdır",
            "Personel üniforması ile işe gidip gelebilir",
            "Çalışma saatleri içinde restoranda kolye, saat, sallanan küpe gibi aksesuarlar takılmamalıdır",
            "Altı kaymaz, lastik, vinil deri ayakkabı giyilmelidir"
        ],
        answer: 1,
        explanation: "Personel üniformasıyla işe gidip gelmek hijyen kurallarına aykırıdır."
    },
    // 5. Soru
    {
        question: "Aşağıdakilerden hangisi gıdayı uzun süre muhafaza etme yöntemleri arasında bulunmaz?",
        options: [
            "Dondurarak kullanım",
            "Çözündürme",
            "Konserve",
            "Pastörizasyon"
        ],
        answer: 1,
        explanation: "Çözündürme bir muhafaza yöntemi değil, dondurulmuş gıdaların kullanıma hazır hale getirilmesidir."
    },
    // 6. Soru
    {
        question: "Kola kulesi nozzle ve difüzör (püskürtücü ve dağıtıcı) temizliklerinde hangi kimyasallar kullanılabilir?",
        options: [
            "D2-D1",
            "D44-D1",
            "D1-D10",
            "D3-D10"
        ],
        answer: 2,
        explanation: "Nozzle ve difüzör temizliğinde D1 ve D10 kimyasalları kullanılır."
    },
    // 7. Soru
    {
        question: "Sanitizer su ölçümü sırasında aşağıdaki adımlardan hangisi doğrudur?",
        options: [
            "PPM şeridi suyun içinde 5 saniye bekletilmeli",
            "PPM şeridi suyun içinde 10 saniye sabit bir şekilde bekletilmeli",
            "PPM şeridi yeşil rengi almış ise uygundur",
            "PPM şeridi suyun içinde 10 saniye sallanmalıdır"
        ],
        answer: 1,
        explanation: "PPM ölçümünde şerit 10 saniye sabit şekilde bekletilmelidir."
    },
    // 8. Soru
    {
        question: "Gün içinde çalışma istasyonlarının temizlik işlemi sırasında hangi kimyasal kullanılmalıdır?",
        options: [
            "D44",
            "D3",
            "D10",
            "D1"
        ],
        answer: 2,
        explanation: "Günlük istasyon temizliklerinde D10 kimyasalı kullanılır."
    },
    // 9. Soru
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
    // 10. Soru
    {
        question: "Multiplex hat temizliği yapılırken hangi kimyasal kullanılmalıdır?",
        options: [
            "D2",
            "D3",
            "Divomil RD",
            "R6"
        ],
        answer: 2,
        explanation: "Multiplex hat temizliğinde Divomil RD kullanılır."
    },
    // 11. Soru
    {
        question: "Soygun gibi acil durumda ne yapmalıyız?",
        options: [
            "Sakin kalıp, karşı çıkmadan kişinin dediğini yapmalı ve personel ile misafiri de bu doğrultuda yönlendirmeliyiz",
            "Hemen kaçmalıyız",
            "Karşı koyarak engellemeliyiz",
            "Olayı telefon kamerası ile kaydetmeliyiz"
        ],
        answer: 1,
        explanation: "Soygun durumunda öncelik sakin kalmak ve can güvenliğini sağlamaktır."
    },
    // 12. Soru
    {
        question: "Aşağıdakilerden hangisi aylık yapılan 'İş Sağlığı ve Güvenliği ile ilgili ISG Görsel Kontrol Çizelgeleri'nden değildir?",
        options: [
            "İlk Yardım Malzemeleri Kontrol Çizelgesi",
            "Yangın Söndürme Cihazı Görsel Kontrol Çizelgesi",
            "Uyarı İkaz ve Bilgilendirme Levhaları Kontrol Çizelgesi",
            "Temizlik Görsel Kontrol Çizelgesi"
        ],
        answer: 3,
        explanation: "Temizlik kontrol çizelgeleri ISG kapsamında değil, operasyonel kontrollerdendir."
    },
    // 13. Soru
    {
        question: "Restoran 'ACİL DURUM PLANI' dahilinde, personellerden ACİL DURUM EKİPLERİ oluşturulmuş olmalı ve kişilere görev bildirimi yapıp, o konuda eğitim verilmeli ve bu ekiplerin listesi İSG klasöründe muhafaza edilmelidir. Aşağıdakilerden hangisi kurulan ekipler arasında yer almaz?",
        options: [
            "İlk Yardım Ekibi",
            "Arama Kurtarma Ekibi",
            "Tahliye ve Haberleşme Ekibi",
            "Ürünleri Kurtarma Ekibi"
        ],
        answer: 3,
        explanation: "Acil durumlarda ürün kurtarma öncelikli bir ekip değildir."
    },
    // 14. Soru
    {
        question: "Aşağıdakilerden hangisi acil eylem durumları için önceden alınması gereken önlemler arasında yer almaz?",
        options: [
            "Acil durum kapıları asla kilitlenmemeli ve dışarıya doğru açılır olmalıdır",
            "Restoranda bulunan raflar duvara sabitlenmelidir",
            "Önceden bir acil eylem planı oluşturulmalıdır",
            "Restorandan çıkılmayıp lobi alanında beklenmelidir"
        ],
        answer: 3,
        explanation: "Acil durumlarda binanın tahliye edilmesi esastır, içeride beklenmez."
    },
    // 15. Soru
    {
        question: "Ansül sistemini elle devreye alma yetkisi kimde bulunmaktadır?",
        options: [
            "Vardiyada bulunan herkes devreye alabilir",
            "Vardiyada bulunan yetkili müdür devreye alabilir",
            "Vardiyada bulunan ekip üyesi devreye alabilir",
            "Kimsenin yetkisi bulunmamaktadır"
        ],
        answer: 1,
        explanation: "Ansül sistemini sadece yetkili müdür devreye alabilir."
    },
    // 16. Soru
    {
        question: "Aşağıdakilerden hangisi TAB Gıda değerleri arasında yer almaz?",
        options: [
            "Çeşitlilik",
            "Mükemmellik taahhüdü",
            "Şeffaf sorumluluklar",
            "Risk analizi"
        ],
        answer: 3,
        explanation: "Risk analizi TAB Gıda değerleri arasında değil, iş sağlığı ve güvenliği prosedürlerindendir."
    },
    // 17. Soru
    {
        question: "Operasyonel sorumluluğun tanımı nedir?",
        options: [
            "Pürüzsüz bir vardiyanın sağlanabilmesi ve operasyonun sürekliliği için düzenli yapılan ve takip edilen işlerdir",
            "Çalışanların operasyonel sorumluluklarını yerine getirebilmeleri için yöneticilerin sahip olması gereken yeteneklerdir",
            "Operasyonu aksatan unsurların tespit edilmesi ve ortadan kaldırılmasıdır",
            "Restorana gerçekleştirilen denetimlerden yüksek skor alabilmektir"
        ],
        answer: 1,
        explanation: "Operasyonel sorumluluk günlük rutin işlerin sorunsuz yürütülmesidir."
    },
    // 18. Soru
    {
        question: "Yönetimsel sorumluluğun tanımı nedir?",
        options: [
            "Etkili bir iletişim kurarak operasyonu sorunsuz bir şekilde yönetmektir",
            "Çalışanların operasyonel sorumluluklarını yerine getirebilmeleri için yöneticilerin sahip olması gereken yeteneklerdir",
            "Yöneticilerin KST yaparak restoranı kontrol altında tutabilmesidir",
            "Restorana gerçekleştirilen denetimlerde denetmenlere eşlik ederek vardiya turu yapılmasıdır"
        ],
        answer: 1,
        explanation: "Yönetimsel sorumluluk ekip yönetimi ve gelişimini kapsar."
    },
    // 19. Soru
    {
        question: "Aşağıdakilerden hangisi yönetimsel becerilerden değildir?",
        options: [
            "Çalışanların eğitim eksiklerini tespit edebilme",
            "Perşembe kalibrasyonlarını yapabilme",
            "Ekibi motive edebilme kabiliyetine sahip olma",
            "Çalışanlar, yöneticiler ve misafirlerle doğru iletişimi kurabilme"
        ],
        answer: 1,
        explanation: "Kalibrasyon yapmak operasyonel bir beceridir."
    },
    // 20. Soru
    {
        question: "Kasa bölgesinde bir ekip üyesinin gülümsemesini sağlamak nasıl bir sorumluluktur?",
        options: [
            "Yönetimsel",
            "Operasyonel",
            "Yöneticinin sorumlulukları arasında değildir",
            "Hepsi"
        ],
        answer: 1,
        explanation: "Ekip motivasyonu ve davranış yönetimi yönetimsel bir sorumluluktur."
    },
    // 21. Soru
    {
        question: "Aşağıdakilerden hangisi sorun çözme adımlarındandır?",
        options: [
            "Sorunu tanımlama",
            "Tüm muhtemel sebepleri belirle",
            "Muhtemel çözümleri belirle",
            "Hepsi"
        ],
        answer: 3,
        explanation: "Tüm bu adımlar sorun çözme sürecinin parçasıdır."
    },
    // 22. Soru
    {
        question: "Aşağıdakilerden hangisi beyin fırtınası adımlarından biri olarak değerlendirilmez?",
        options: [
            "Karşıdaki kişinin fikirleri eleştirilmemelidir",
            "Süre sınırlaması yoktur",
            "Niteliğe değil niceliğe bakılmalıdır",
            "Verilecek yanıtların amacı belirlenmelidir"
        ],
        answer: 1,
        explanation: "Beyin fırtınasında genellikle bir süre sınırlaması vardır."
    },
    // 23. Soru
    {
        question: "Aşağıdakilerden hangisi delegasyonu ifade eder?",
        options: [
            "Sizin sorumluluğunuzdaki bir işi yaptırmak üzere astınıza sormak ya da sizin adınıza hareket etmesi için başkasını tayin etmektir",
            "Delegasyon bir iletişim biçimidir",
            "Ekibe verilecek eğitimin organize edilmesidir",
            "Ekip motivasyonunu sağlayabilme yeteneğidir"
        ],
        answer: 1,
        explanation: "Delegasyon, yetki ve sorumluluğun devredilmesidir."
    },
    // 24. Soru
    {
        question: "Delege edilecek bir iş için kimse ilgi duymuyorsa ne yapılmalıdır?",
        options: [
            "İşi kendiniz yapın",
            "Durumu üst yöneticinize bildirin",
            "İş için ilgi uyandırın",
            "Çalışanların sorumluluk almasını bekleyin"
        ],
        answer: 2,
        explanation: "İşin önemini anlatarak ilgi uyandırmak en doğru yaklaşımdır."
    },
    // 25. Soru
    {
        question: "Bir iş delege edilirken kişinin yeteneğinde eksiklik gözlemleniyorsa nasıl bir yöntem uygulanmalıdır?",
        options: [
            "Eğitim vermeyi önerin",
            "İşi başkasına devredin",
            "Çalışana yeteneğini geliştirmesini söyleyin",
            "Herhangi bir şey yapılmasına gerek yoktur, iş direkt devredilebilir"
        ],
        answer: 1,
        explanation: "Yetkinlik eksikliğinde eğitim verilmelidir."
    },
    // 26. Soru
    {
        question: "Aşağıdakilerden hangisinde delegasyonun adımları doğru verilmiştir?",
        options: [
            "İşi seçin-Kişiyi seçin-Bir plan oluşturun-İşi delege edin-Süreci takip etmeyi önerin",
            "Bir plan oluşturun ve işi delege edin",
            "Delegasyonun herhangi bir adımı yoktur, iş direkt devredilebilir",
            "Vardiya planını kontrol edin ve işi delege edin"
        ],
        answer: 1,
        explanation: "Delegasyon bu beş adımda doğru şekilde yapılır."
    },
    // 27. Soru
    {
        question: "Zamanı daha iyi yönetebilmek için aşağıdaki davranışlardan hangisi uygulanmamalıdır?",
        options: [
            "İşe C önceliğinden değil A önceliğinden başlamak",
            "Aynı anda birden fazla işe başlamak",
            "Yapılacak işler listesi oluşturmak",
            "Yapılacak işleri küçük parçalar halinde uygulamak"
        ],
        answer: 1,
        explanation: "Multitasking verimliliği düşürür, tek bir işe odaklanmak daha etkilidir."
    },
    // 28. Soru
    {
        question: "Peynir tekniği nedir?",
        options: [
            "Peynir sıcaklıklarını düzenli olarak takip etme",
            "Yerleşim planını doğru şekilde uygulama",
            "A Önceliğindeki bir işiniz çok büyükse küçük işler halinde uygulamak",
            "Servis süre takip çizelgesini kontrol etme"
        ],
        answer: 2,
        explanation: "Peynir tekniği büyük işleri küçük parçalara bölerek yönetmektir."
    },
    // 29. Soru
    {
        question: "''C.R.I.S.P'' yöntemini aşağıdakilerden en iyi hangisi ifade eder?",
        options: [
            "İletişimi kolaylaştırır",
            "Vardiya kontrolünü sağlar",
            "Nasıl övgüde bulunulması gerektiğini açıklar",
            "Verimlilik hakkında bilgi edinmemizi sağlar"
        ],
        answer: 2,
        explanation: "CRISP yöntemi etkili övgü verme tekniğidir."
    },
    // 30. Soru
    {
        question: "Aşağıdakilerden hangisi C.R.I.S.P'in adımları arasında yer almaz?",
        options: [
            "Tutarlı",
            "Belirli",
            "Hemen",
            "Çeşitlilik"
        ],
        answer: 3,
        explanation: "CRISP'in açılımı: Ciddi, Rutin, İçten, Spesifik, Pozitiftir."
    },
    // 31. Soru
    {
        question: "Aşağıdakilerden hangisi Etkili İletişimde bulunması gereken özellikler arasında yer almaz?",
        options: [
            "Etkili Dinlemek",
            "Hoş görülü Olmak",
            "Empati Yapmak",
            "Eleştiriye Açık Olmamak"
        ],
        answer: 3,
        explanation: "Etkili iletişim için eleştiriye açık olmak gereklidir."
    },
    // 32. Soru
    {
        question: "İletişimde, alıcının mesajı alarak değerlendirdiğini ve yorumladığını gösterdikten sonra yanıt vermesine ne denir?",
        options: [
            "Kanal",
            "Çevre",
            "İletişim",
            "Geri Bildirim"
        ],
        answer: 3,
        explanation: "Bu tanım geri bildirimin tanımıdır."
    },
    // 33. Soru
    {
        question: "Aşağıdaki davranışlardan hangisi 'etkin dinleme' durumunda yapılmamalıdır?",
        options: [
            "Konuşanın gözlerine bakmak",
            "Ne söylediğini anlamaya çalışmak",
            "Başka şeyler düşünmemek",
            "Karşımızdaki kişiyi yargılamak"
        ],
        answer: 3,
        explanation: "Etkin dinlemede yargılama yapılmaz."
    },
    // 34. Soru
    {
        question: "Aşağıdakilerden hangisi iletişim sürecinin öğeleri arasında değildir?",
        options: [
            "Gönderici",
            "Alıcı",
            "Kanal",
            "İşletici"
        ],
        answer: 3,
        explanation: "İletişim sürecinde 'işletici' diye bir öğe yoktur."
    },
    // 35. Soru
    {
        question: "Ben mesajlarının ana bileşenleri aşağıdakilerden hangisinde doğru olarak belirtilmiştir?",
        options: [
            "Davranışlar-Duygular-Etkiler",
            "Tepki-Etki-Duygular",
            "Sesler-Davranışlar-Tepki",
            "Duygu-Gözlem-Tavır"
        ],
        answer: 1,
        explanation: "Ben mesajları bu üç bileşenden oluşur."
    },
    // 36. Soru
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
    // 37. Soru
    {
        question: "İşe yeni başlayan ekip üyesine ilk olarak hangi eğitim adımı uygulanmalıdır?",
        options: [
            "Performans değerlendirme",
            "Temel Eğitim Videosu",
            "Mutfak İstasyon Testleri",
            "İstasyon Değerlendirme"
        ],
        answer: 1,
        explanation: "Yeni çalışanlara önce temel eğitim videosu izletilir."
    },
    // 38. Soru
    {
        question: "4 Adımda Eğitim Metodunun 4.aşaması aşağıdakilerden hangisidir?",
        options: [
            "Takdir",
            "Takip",
            "Teşekkür",
            "Transfer"
        ],
        answer: 1,
        explanation: "4. adım takip aşamasıdır (Göster-Yaptır-Gözden Geçir-Takip Et)."
    },
    // 39. Soru
    {
        question: "4.Adımda Eğitim Metodunda 'Hazırlık Aşaması' ile ilgili aşağıdaki ifadelerden hangisi yanlıştır?",
        options: [
            "Eğitim verilecek saat önemlidir",
            "Mutfak bölgesi eğitimi servis bölgesinde verilmelidir",
            "Eğitim verecek kişi bilgilere hakim olmalıdır",
            "Eğitim ekipmanları tam ve çalışır durumda olmalıdır"
        ],
        answer: 1,
        explanation: "Eğitim ilgili bölgede verilmelidir."
    },
    // 40. Soru
    {
        question: "Vardiya yönetiminde 4 D kuralını aşağıdakilerden hangisi tanımlar?",
        options: [
            "Doğru sayı, Doğru Zaman, Doğru yeterlilik, Doğru yer",
            "Doğru Sipariş, Doğru servis, Doğru yeterlilik, Doğru karar",
            "Doğru iletişim, Doğru para kontrolü, Doğru zaman yönetimi, Doğru problem çözümü",
            "Hepsi"
        ],
        answer: 1,
        explanation: "4D kuralı: Doğru sayıda, Doğru zamanda, Doğru yeterlilikte, Doğru yerde personel."
    },
    // 41. Soru
    {
        question: "Çıtır soğan ile ilgili ifadelerden hangisi doğrudur?",
        options: [
            "Ambalajı yeni açılan çıtır soğan direkt kullanıma girer",
            "Ambalajı yeni açılan çıtır soğana 15 günlük HKA yazılır",
            "Açıldıktan sonra kalan miktar kuru depoya kaldırılır",
            "Hepsi"
        ],
        answer: 1,
        explanation: "Açılan çıtır soğana 15 günlük HKA (Hazırlama-Kullanma Aralığı) yazılır."
    },
    // 42. Soru
    {
        question: "Ekmek kalitesi ile ilgili belirtilen bilgilerden hangisi doğrudur?",
        options: [
            "Ekmekler kızartıldıktan sonra 30 sn içinde kullanılmalıdır",
            "Ekmekler açıldıktan sonra raf ömrü 10 gündür",
            "Ekmekler soğuk odada muhafaza edilir",
            "Ekmekler yerden en az 30 cm yukarıda olacak şekilde muhafaza edilmelidir"
        ],
        answer: 3,
        explanation: "Ekmekler hijyen kuralları gereği yerden yüksekte muhafaza edilmelidir."
    },
    // 43. Soru
    {
        question: "Aşağıdakilerin hangisinde açılmış olan füme etin HKA bilgileri doğru verilmiştir?",
        options: [
            "H:01.10.2021 Saat 12.00 - K:01.10.2021 Saat 14.00 - A:02.10.2021 Saat 10.00",
            "H:01.10.2021 Saat 12.00 - A:03.10.2021 Saat 12.00",
            "H:01.10.2021 Saat 12.00 - A:02.10.2021 Saat 22.00",
            "H:01.10.2021 Saat 12.00 - A:02.10.2021 Saat 12.00"
        ],
        answer: 3,
        explanation: "Füme et için HKA 24 saattir (Hazırlama + 1 gün)."
    },
    // 44. Soru
    {
        question: "Çikolata dolgulu kornet ürününe kaç gram sos kullanılmaktadır?",
        options: [
            "30 gr",
            "20 gr",
            "18 gr",
            "15 gr"
        ],
        answer: 2,
        explanation: "Çikolata dolgulu kornete 18 gram sos eklenir."
    },
    // 45. Soru
    {
        question: "Sundae dondurma hazırlarken bardağa kaç gram dondurma koyulur?",
        options: [
            "120 gr",
            "140 gr",
            "110 gr",
            "80 gr"
        ],
        answer: 1,
        explanation: "Sundae bardağına 120 gram dondurma konulur."
    },
    // 46. Soru
    {
        question: "Aynı anda kaç adet Çikolatalı Sufle mikrodalga fırında ısıtılır?",
        options: [
            "1 adet",
            "2 adet",
            "3 adet",
            "Farketmez"
        ],
        answer: 1,
        explanation: "Aynı anda maksimum 2 sufle ısıtılabilir."
    },
    // 47. Soru
    {
        question: "Aynı anda kaç adet Tavuk Burger sandviç hazırlanır?",
        options: [
            "1 adet",
            "2 adet",
            "3 adet",
            "Farketmez"
        ],
        answer: 1,
        explanation: "Aynı anda en fazla 2 tavuk burger hazırlanır."
    },
    // 48. Soru
    {
        question: "Aynı anda en fazla kaç adet Vişneli Tatlı kızartılır?",
        options: [
            "2 adet",
            "3 adet",
            "4 adet",
            "5 adet"
        ],
        answer: 2,
        explanation: "Aynı anda maksimum 4 vişneli tatlı kızartılabilir."
    },
    // 49. Soru
    {
        question: "Crispy Chicken sandviç çok mayonezli istenirse toplam kaç gram mayonez eklenmelidir?",
        options: [
            "21 gr",
            "20 gr",
            "30 gr",
            "11 gr"
        ],
        answer: 2,
        explanation: "Çok mayonezli Crispy Chicken'a toplam 30 gram mayonez eklenir."
    },
    // 50. Soru
    {
        question: "Mega Double Cheeseburger sandviç içerisine kaç gram ketçap konur?",
        options: [
            "20 gr",
            "21 gr",
            "7 gr",
            "14 gr"
        ],
        answer: 3,
        explanation: "Mega Double Cheeseburger'a 14 gram ketçap eklenir."
    }
	// 51. Soru
{
    question: "Yıldızlı ürünler hangileridir?",
    options: [
        "Soğan halkası, içecekler, dondurma, tatlılar",
        "Tüm menüler",
        "Sadece içecekler",
        "Tüm tatlılar"
    ],
    answer: 1,
    explanation: "Yıldızlı ürünler genellikle soğan halkası, içecekler, dondurma ve tatlıları içerir."
},
// 52. Soru
{
    question: "Hangi durumlar void işlemine örnek verilebilir?",
    options: [
        "Yanlış tuşlama, misafirin siparişi iptal etmesi, ürünün kalmaması",
        "Misafirin para vermemesi",
        "Ürünün yanlış yapılması",
        "Kasada para üstünün eksik verilmesi"
    ],
    answer: 1,
    explanation: "Void işlemleri genellikle yanlış girişler veya iptal edilen siparişlerde kullanılır."
},
// 53. Soru
{
    question: "Kasa bölgesinde 100 TL üzeri tüm işlemler ne yapılmalıdır?",
    options: [
        "Yönetici onayı alınmalıdır",
        "Direkt işlem yapılmalıdır",
        "Kameraya gösterilmelidir",
        "Yöneticiye teslim edilmelidir"
    ],
    answer: 1,
    explanation: "Yüksek meblağlı işlemlerde yöneticiden onay alınmalıdır."
},
// 54. Soru
{
    question: "Açık ürün girişi ne zaman yapılır?",
    options: [
        "Asla yapılmaz",
        "Sadece ürün kalmadığında",
        "Sadece yöneticinin bilgisi dahilinde",
        "Her zaman yapılabilir"
    ],
    answer: 2,
    explanation: "Açık ürün girişi sadece yönetici bilgisi ve onayı ile yapılabilir."
},
// 55. Soru
{
    question: "Kasada hangi paralar sayılarak verilmelidir?",
    options: [
        "Tüm paralar",
        "Sadece bozuk paralar",
        "50 TL ve üzeri banknotlar",
        "5 TL altı banknotlar"
    ],
    answer: 2,
    explanation: "Büyük meblağlar sayılarak verilmelidir, özellikle 50 TL ve üzeri banknotlar."
},
// 56. Soru
{
    question: "Void işlemi yapılmadan önce ne yapılmalıdır?",
    options: [
        "Yönetici bilgilendirilmelidir",
        "Direkt işlem yapılır",
        "Misafire ürün verilir",
        "Kasadan para alınır"
    ],
    answer: 1,
    explanation: "Void işlemi yapılmadan önce mutlaka yönetici bilgilendirilmelidir."
},
// 57. Soru
{
    question: "Kasa sorumlusu boş kasa çekmecesinde neyi kontrol eder?",
    options: [
        "Çekmecede para olup olmadığını",
        "Çekmecenin sağlamlığını",
        "Etiket ve sticker olup olmadığını",
        "Yedek slipleri"
    ],
    answer: 2,
    explanation: "Boş kasa çekmecesinde etiket ve sticker olup olmadığı kontrol edilir."
},
// 58. Soru
{
    question: "Kasa bölgesinde ne zaman eller yıkanmalıdır?",
    options: [
        "Yiyecek içecek hazırlığında",
        "Parayla temas sonrası",
        "Çöplerle temas sonrası",
        "Hepsi"
    ],
    answer: 3,
    explanation: "Hijyen için tüm bu durumlarda eller yıkanmalıdır."
},
// 59. Soru
{
    question: "Kasa sorumlusu günlük olarak neyi kontrol eder?",
    options: [
        "Kasa çekmecesini",
        "POS cihazını",
        "Slip ve zarfları",
        "Yönetici notlarını"
    ],
    answer: 2,
    explanation: "Slipler ve zarflar günlük olarak kontrol edilmelidir."
},
// 60. Soru
{
    question: "Kasadaki eksik- fazla durumlarında ne yapılmalıdır?",
    options: [
        "Yöneticiye bilgi verilmelidir",
        "Kamera kayıtları izlenmelidir",
        "Paralar sayılmalıdır",
        "Hiçbir şey yapılmaz"
    ],
    answer: 1,
    explanation: "Her türlü eksik/fazla durumda yönetici bilgilendirilmelidir."
},
// 61. Soru
{
    question: "Vardiya müdürü restoranında rutin kontrolleri gerçekleştiriyor. Bu sırada, kızartma makinesi yağ seviyelerinin azaldığını, sıcak suyun akmadığını, bir chute ampulunun arızalandığını ve serviste mini ketçap sos stoklarının bittiğini tespit etmiştir. Vardiya müdürü ilk olarak hangi konuyla ilgili aksiyon alınmasını sağlamalıdır?",
    options: [
        "Mini ketçap stoklarının tamamlanması",
        "Sıcak suların kontrolü",
        "Kızartma makinesi yağ seviyelerinin tamamlanması",
        "Chute ampulunun tamamlanması"
    ],
    answer: 1,
    explanation: "Sıcak suyun olmaması hijyen açısından kritik olduğundan öncelikli aksiyon alınması gereken konudur."
},
// 62. Soru
{
    question: "ÇÜTÜ de bulunan King Chicken eti en az kaç derece olmalıdır?",
    options: [
        "min 60°C",
        "min 65 °C",
        "min 68 °C",
        "min 74 °C"
    ],
    answer: 3,
    explanation: "King Chicken gibi ürünlerde güvenli tüketim için en az 74°C sıcaklık gereklidir."
},
// 63. Soru
{
    question: "Hamburger eti cook out sıcaklığı kaç olmalıdır?",
    options: [
        "min 68°C",
        "68 °C-79°C",
        "min 74 °C",
        "60-79 °C"
    ],
    answer: 2,
    explanation: "Hamburger eti pişirme sonrası en az 74°C olmalıdır."
},
// 64. Soru
{
    question: "Soğuk odada bulunan peynirin derecesi kaç olmalıdır?",
    options: [
        "1-4 °C",
        "1-6°C",
        "-18/-23 °C",
        "18/29 °C"
    ],
    answer: 1,
    explanation: "Soğuk saklama için ideal sıcaklık aralığı 1-4 °C’dir."
},
// 65. Soru
{
    question: "Aşağıdakilerden hangisi Kritik Gıda Güvenliği ifadesi arasında yer almaz?",
    options: [
        "ÇÜTÜ deki ürünlerin sıcaklığının min 60 °C olması",
        "Domatesin bekleme süresinin dolması",
        "Dondurma sütü sıcaklığının 1-4 °C arasında olması",
        "BİB Kola SKT nin geçmesi"
    ],
    answer: 1,
    explanation: "Domatesin bekleme süresi kritik gıda güvenliği kapsamında değerlendirilmez."
},
// 66. Soru
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
// 67. Soru
{
    question: "Ebro Cihazında polar madde ölçümünde görülen turuncu-sarı ışık neyi ifade eder?",
    options: [
        "Yağ Kirli",
        "Yağ Temiz",
        "Yağ yeteri derecede ısınmamış",
        "Yağ kritik seviyede her an atık olabilir"
    ],
    answer: 3,
    explanation: "Turuncu-sarı ışık, yağın kritik seviyeye yaklaştığını ve atık olabileceğini gösterir."
},
// 68. Soru
{
    question: "Multiplex hat sanitasyon temizliği (line temizliği) hangi sıklıkla yapılmalıdır?",
    options: [
        "3 ay",
        "1 ay",
        "6 ay",
        "45 gün"
    ],
    answer: 1,
    explanation: "Hat sanitasyon temizliği ayda bir kez yapılmalıdır."
},
// 69. Soru
{
    question: "Kızartma makinesinde yağ sıcaklığını ayarlamak için hangi kod kullanılmalıdır?",
    options: [
        "1650",
        "1658",
        "1652",
        "1653"
    ],
    answer: 2,
    explanation: "Yağ sıcaklığı ayar kodu 1652 olarak belirlenmiştir."
},
// 70. Soru
{
    question: "Paket servis ile ilgili bilgilerden hangisi yanlıştır?",
    options: [
        "Paket içerisinde sandviçler en fazla 2 adet üst üste konulabilir",
        "Patates kızartması paket içerisine yerleştirilirken dik olarak konulmalıdır",
        "İçecekler doldurulurken içecek bardağının 2 cm aşağısında kalacak şekilde doldurulur",
        "Paket serviste, sıcakla soğuk ürünler aynı torbaya konulmaz"
    ],
    answer: 2,
    explanation: "İçecekler bardak tam dolu olacak şekilde değil, kapatılabilecek güvenli seviyede doldurulmalıdır. Bu nedenle bu ifade yanlıştır."
},
// 71. Soru
{
    question: "Aşağıdaki servis tepsilerine ait bilgilerden hangisi doğrudur?",
    options: [
        "Servis Tepsileri silinerek temizlenmelidir",
        "Servis Tepsileri üçlü aşamada yıkanmalıdır",
        "Servis Tepsileri H500 ile temizlenmelidir",
        "Hiçbiri"
    ],
    answer: 1,
    explanation: "Servis tepsileri, her zaman üçlü aşamada yıkanmalıdır."
},
// 72. Soru
{
    question: "Sevkiyat prosedürle ilgili bilgilerden hangisi yanlıştır?",
    options: [
        "Ürünlerin son kullanım tarihleri kontrol edilmelidir",
        "Ürünlerin ambalaj kontrolleri, ezilme, çözünme, buzlanma, ıslanma vb. oluşabilecek problemler kontrol edilmelidir",
        "Sevkiyat öncesi depo düzenleri yapıldıktan sonra mutlaka soğutucu ve dondurucu odalarının sıcaklıklarının alınması gerekir",
        "Sevkiyat ile restorana teslim edilen ürünler uygun sıcaklık derecelerinde değil ise de ürünler teslim alınmalıdır"
    ],
    answer: 3,
    explanation: "Uygun sıcaklıkta olmayan ürünler teslim alınmamalıdır."
},
// 73. Soru
{
    question: "Burger King markamızda misafirlere verilmesi gereken servis süresi ne kadardır?",
    options: [
        "3.00 dakika",
        "2.45 dakika",
        "2.00 dakika",
        "2.15 dakika"
    ],
    answer: 1,
    explanation: "Misafirlere servis süresi 2.45 dakika olmalıdır."
},
// 74. Soru
{
    question: "Burger King markamızda kullanılan Guest Trac uygulaması nedir?",
    options: [
        "Misafir Memnuniyeti Değerlendirme Anketi",
        "Servis Süresi Değerlendirme Anketi",
        "Restoran Güvenliği Değerlendirme Anketi",
        "İş Güvenliği Değerlendirme Anketi"
    ],
    answer: 1,
    explanation: "Guest Trac, misafir memnuniyetini değerlendiren bir uygulamadır."
},
// 75. Soru
{
    question: "Aşağıdakilerden hangisi KST derece alımı işlemi için gerekli olan araç ve gereçlerden biri değildir?",
    options: [
        "Dondurma Temizlik Fırçası",
        "Yüzey uçlu termometre",
        "Probe mendili",
        "Sanitize edilmiş Çütü peni"
    ],
    answer: 1,
    explanation: "KST derece alımı için dondurma temizlik fırçası gerekli değildir."
},
// 76. Soru
{
    question: "Patates servis standartları ile ilgili bilgilerden hangisi yanlıştır?",
    options: [
        "Patates kızartma yağı kalitesi her gün kontrol edilmelidir",
        "Tuzlama işlemi patateslerden 30 cm yukarıda yapılmalıdır",
        "Patatesler tuzlandıktan sonra tuzun dağılması için karıştırılmalıdır",
        "Paketlenen patatesler 1 dk boyunca patates ünitesinde bekleyebilir"
    ],
    answer: 3,
    explanation: "Paketlenen patateslerin 1 dakika boyunca beklememesi gerekir."
},
// 77. Soru
{
    question: "Dondurma Makinesi ile ilgili bilgilerden hangisi yanlıştır?",
    options: [
        "Süt seviyesi her gün uygun şekilde ayarlanmalıdır",
        "Süt içerisinde siyah parçalar bulunmamalıdır",
        "Dondurma makinesi temizleme çizelgesine yıkama tarihi belirtilmelidir",
        "Dondurma makinesi çırpıcıları sadece kapanışta detaylı yıkanmalıdır"
    ],
    answer: 3,
    explanation: "Dondurma makinesi çırpıcıları kapanışta detaylı değil, günlük olarak temizlenmelidir."
},
// 78. Soru
{
    question: "Burger King servis standartlarına dair aşağıda verilen bilgilerden hangisi doğru belirtilmemiştir?",
    options: [
        "Tepsiye direkt tuz konulmaz misafir isteğine göre servis edilir",
        "Büyük veya Dublex seçim istendiğinde 1 büyük ayran servis edilir",
        "Menü satın alan müşterilerimizin tepsilerine 2 adet Ketçap direkt olarak konulmalıdır",
        "Misafirlerimiz yan ürün satın aldıysa ücretli soslardan ücretsiz verilebilir"
    ],
    answer: 3,
    explanation: "Yan ürün satın alan misafirlere ücretli soslar ücretsiz verilmemelidir."
},
// 79. Soru
{
    question: "Aşağıda çıtır peynir ile ilgili belirtilen bilgilerden hangisi yanlıştır?",
    options: [
        "Donuk çıtır peynirler spesiyal mobilde poşet içinde saklanabilir",
        "Çıtır Peynirler 4-6-9 adet olarak misafire servis edilir",
        "Çıtır peynir sıcaklığı 174+/- 3 derece olan vatta pişirilmelidir",
        "Çıtır peynir chutta 10 dakikalık time ile bekleyebilir"
    ],
    answer: 3,
    explanation: "Çıtır peynirler chutta 10 dakikadan fazla beklememelidir."
},
// 80. Soru
{
    question: "Aşağıdaki genel bilgilerden hangisi yanlıştır?",
    options: [
        "Çütü pen ve ızgaralarında çatlak olmamalı ve temiz olmalıdır",
        "Sanitize solüsyonlar gıdaya yakın saklanmamalıdır",
        "Kullanılan patates küreği sağlam olmalıdır",
        "Buz makinesinden kola kulesine buz tamamlanırken bardak, pen gibi malzemeler kullanılabilir"
    ],
    answer: 3,
    explanation: "Buz makinesinden kola kulesine buz aktarılırken bardak gibi malzemeler kullanılmamalıdır."
},
// 81. Soru
{
    question: "Bardağa koyulan Sprite içeceği bekleme süresi ne kadardır?",
    options: [
        "5 Dakika",
        "2 Dakika",
        "10 Dakika",
        "Beklemez"
    ],
    answer: 3,
    explanation: "Sprite içeceği, beklemeden hemen servis edilmelidir."
},
// 82. Soru
{
    question: "Kahve ürünleri paketi açıldıktan sonra paket ağzı kapatılarak ne kadar süre bekleyebilir?",
    options: [
        "5 Gün",
        "10 Gün",
        "15 Gün",
        "14 Gün"
    ],
    answer: 3,
    explanation: "Kahve ürünleri, paketi açıldıktan sonra 14 gün boyunca saklanabilir."
},
// 83. Soru
{
    question: "Hazırlanan ton balıklı salatalar servis soğutucuda ne kadar süre muhafaza edilmelidir?",
    options: [
        "2 Saat",
        "4 Saat",
        "6 Saat",
        "12 Saat"
    ],
    answer: 1,
    explanation: "Ton balıklı salatalar, servis soğutucuda en fazla 4 saat muhafaza edilmelidir."
},
// 84. Soru
{
    question: "Chicken Nugget ürünün Chute bekleme süresi ne kadardır?",
    options: [
        "5 Dakika",
        "10 Dakika",
        "20 Dakika",
        "Beklemez"
    ],
    answer: 1,
    explanation: "Chicken Nugget ürünleri Chute’de 10 dakika bekleyebilir."
},
// 85. Soru
{
    question: "Tavukburger ürününün Çoklu ürün tutma ünitesinde (ÇÜTÜ) tutma süresi ne kadardır?",
    options: [
        "30 Dakika",
        "40 Dakika",
        "45 Dakika",
        "1 Saat"
    ],
    answer: 2,
    explanation: "Tavukburger, Çoklu Ürün Tutma Ünitesinde (ÇÜTÜ) 45 dakika tutulabilir."
}
// 86. Soru
{
    question: "Hazırlanan Cheeseburger’ lerin Chute’te bekleme süresi kaç dakikadır?",
    options: [
        "5 Dakika",
        "10 Dakika",
        "20 Dakika",
        "Beklemez"
    ],
    answer: 1,
    explanation: "Cheeseburger'lerin Chute'teki bekleme süresi 10 dakikadır."
},
// 87. Soru
{
    question: "Gurme Tavuk etlerinin ÇÜTÜ’de bekleme süresi aşağıdakilerden hangisidir?",
    options: [
        "30 Dakika",
        "40 Dakika",
        "45 Dakika",
        "1 Saat"
    ],
    answer: 2,
    explanation: "Gurme Tavuk etlerinin ÇÜTÜ'deki bekleme süresi 45 dakikadır."
},
// 88. Soru
{
    question: "Sabah saat 10.00'da oda sıcaklığına prep çıkartılırken hangi satış aralığı baz alınmalıdır?",
    options: [
        "Saat 12.00-14.00 arası beklenen satış",
        "Saat 10.00-12.00 arası beklenen satış",
        "Saat 08.00-10.00 arası beklenen satış",
        "Saat 10.00 daki beklenen satış"
    ],
    answer: 1,
    explanation: "Oda sıcaklığına prep çıkartılırken saat 10.00-12.00 arası beklenen satış baz alınmalıdır."
},
// 89. Soru
{
    question: "Kova Big King Sos soğutucu dolapta açılmış ambalaj ömrü aşağıdakilerden hangisidir?",
    options: [
        "5 Gün",
        "7 Gün",
        "8 Gün",
        "10 Gün"
    ],
    answer: 1,
    explanation: "Kova Big King Sos'un soğutucu dolapta açılmış ambalaj ömrü 7 gündür."
},
// 90. Soru
{
    question: "Aşağıdaki bilgilerden hangisi yanlıştır?",
    options: [
        "Garlic sos kullanılmadan önce oda sıcaklığına 2 saat beklemelidir",
        "Çıtır Soğan soğuk odadan çıkarıldıktan sonra kullanılmadan önce oda sıcaklığında 2 saat beklemelidir",
        "Beyaz küp peynir soğuk depoda 7 gün bekleyebilir",
        "Dilimlenmiş salatalık soğuk depoda 1 gün bekleyebilir"
    ],
    answer: 2,
    explanation: "Beyaz küp peynir soğuk depoda 7 gün değil, daha kısa süre dayanabilir."
},
// 91. Soru
{
    question: "Aşağıdakilerden hangisi restoran karlılığını arttırmanın yollarındandır?",
    options: [
        "Satışları artırmak ve maliyeti kontrol etmek",
        "ISG kurallarına dikkat etmek",
        "Kişisel hijyen kurallarına uymak",
        "Sevkiyatta gelmeyen ürünler için tutanak tutmak"
    ],
    answer: 1,
    explanation: "Restoran karlılığını arttırmak için satışları artırmak ve maliyeti kontrol etmek önemlidir."
},
// 92. Soru
{
    question: "Aşağıdakilerden hangisi kontrol edilebilen giderlerden biridir?",
    options: [
        "Enerji",
        "Atık",
        "Yemek",
        "Hepsi"
    ],
    answer: 3,
    explanation: "Enerji, atık ve yemek giderleri kontrol edilebilen giderlerdir."
},
// 93. Soru
{
    question: "Aşağıdaki şıklardan hangisinin porsiyonlamalarla doğrudan ilgisi yoktur?",
    options: [
        "Soslama",
        "Kızarmış Ürünler",
        "Yağ seviyesi",
        "İçecekler"
    ],
    answer: 2,
    explanation: "Yağ seviyesi porsiyonlama ile doğrudan ilgili değildir."
},
// 94. Soru
{
    question: "Aşağıdakilerden hangisi atık çeşitleri arasında yer almaz?",
    options: [
        "Kabul edilebilir atıklar",
        "Düzensiz atıklar",
        "Kabul edilemez atıklar",
        "Kayıtsız atıklar"
    ],
    answer: 1,
    explanation: "Düzensiz atıklar atık çeşitleri arasında yer almaz."
},
// 95. Soru
{
    question: "Aşağıdakilerden hangisi kabul edilebilir atıklara örnek verilemez?",
    options: [
        "Dondurma makine temizliğinde atılan süt",
        "Hat sanitasyonunda atık olan şurup",
        "Yere dökülen domates prebi",
        "KST atıkları"
    ],
    answer: 2,
    explanation: "Yere dökülen domates prebi kabul edilebilir atıklara örnek verilemez."
},
// 96. Soru
{
    question: "Aşağıdaki seçeneklerin hangisinde yönetimdeki kontrol alanları yanlış belirtilmiştir?",
    options: [
        "Atık",
        "Kampanyalar",
        "Porsiyonlama",
        "Envanter"
    ],
    answer: 1,
    explanation: "Kampanyalar yönetimdeki kontrol alanı arasında yer almaz."
},
// 97. Soru
{
    question: "Envanter kaydı oluşturulurken doğru formül aşağıdakilerin hangisinde verilmiştir?",
    options: [
        "Başlangıç stoğu (Açılış)+ Satın alınan ürün - Satılan ürün - Personel Yemeği - Atık - Promosyonlar = Sayım Stoğu(Açılış)",
        "Başlangıç stoğu (Açılış)+ Satın alınan ürün - Satılan ürün - Personel Yemeği - Atık = Sayım Stoğu(Kapanış)",
        "Başlangıç stoğu (Açılış)+ Satın alınan ürün - Satılan ürün - Atık - Promosyonlar = Sayım Stoğu(Kapanış)",
        "Başlangıç stoğu (Açılış)+ Satın alınan ürün - Satılan ürün - Personel Yemeği - Atık - Promosyonlar = Sayım Stoğu(Kapanış)"
    ],
    answer: 3,
    explanation: "Doğru formül: Başlangıç stoğu (Açılış)+ Satın alınan ürün - Satılan ürün - Personel Yemeği - Atık - Promosyonlar = Sayım Stoğu(Kapanış)"
},
// 98. Soru
{
    question: "Ana kasa şifre değişikliği hangi sıklıkla yapılmalıdır?",
    options: [
        "3 ayda bir kere",
        "Restoran devirlerinde",
        "Ayda 1 kere ve gerektikçe",
        "Kapanış işlemleri sırasında"
    ],
    answer: 2,
    explanation: "Ana kasa şifre değişikliği ayda bir kere ve gerektikçe yapılmalıdır."
},
// 99. Soru
{
    question: "Kapanış vardiyalarında yazar kasa çekmeceleri neden açık bırakılmalıdır?",
    options: [
        "Olası bir hırsızlık ihtimaline karşılık çekmecelerin zorlanmaması için",
        "Dezenfekte etmek için",
        "Temizlik yapmak için",
        "Çekmeceler açık bırakılmaz"
    ],
    answer: 1,
    explanation: "Kapanış vardiyalarında yazar kasa çekmeceleri olası bir hırsızlık ihtimaline karşı açık bırakılmalıdır."
},
// 100. Soru
{
    question: "Bir vardiya kontrolünde aşağıdakilerden hangisi öncelikli değildir?",
    options: [
        "Ekipman",
        "Ürün",
        "Personel",
        "Yazar kasa çekmecesi"
    ],
    answer: 3,
    explanation: "Yazar kasa çekmecesi vardiya kontrolünde öncelikli bir konu değildir."
}
        ];

        // OYUN DEĞİŞKENLERİ
        let currentQuestionIndex = 0;
        let score = 0;
        let userAnswer = null;
        let questions = [...questionBank]; // Tüm 100 soruyu kopyala

        // ELEMANLARI SEÇ
        const questionElement = document.getElementById('question');
        const optionsElement = document.getElementById('options');
        const nextButton = document.getElementById('next-btn');
        const resultElement = document.getElementById('result');
        const counterElement = document.getElementById('counter');
        const scoreElement = document.getElementById('score');
        const progressBar = document.getElementById('progress-bar');

        // BAŞLANGIÇ
        function initQuiz() {
            showQuestion();
        }

        // SORU GÖSTER
        function showQuestion() {
            const question = questions[currentQuestionIndex];
            questionElement.textContent = question.question;
            counterElement.textContent = `Soru ${currentQuestionIndex + 1}/100`;
            scoreElement.textContent = `Puan: ${score}/100`;
            progressBar.style.width = `${((currentQuestionIndex + 1) / 100) * 100}%`;
            
            optionsElement.innerHTML = '';
            question.options.forEach((option, index) => {
                const button = document.createElement('button');
                button.className = 'option';
                button.textContent = option;
                button.onclick = () => selectOption(index);
                optionsElement.appendChild(button);
            });
            
            nextButton.disabled = true;
            resultElement.textContent = '';
            resultElement.className = '';
        }

        // CEVAP SEÇ
        function selectOption(index) {
            userAnswer = index;
            const buttons = document.querySelectorAll('.option');
            buttons.forEach(btn => btn.classList.remove('selected'));
            buttons[index].classList.add('selected');
            nextButton.disabled = false;
        }

        // SONRAKİ SORU
        nextButton.addEventListener('click', () => {
            const question = questions[currentQuestionIndex];
            const buttons = document.querySelectorAll('.option');
            
            // Cevap kontrolü
            buttons.forEach(btn => btn.disabled = true);
            if (userAnswer === question.answer) {
                score += 1; // Her doğru cevap 1 puan
                buttons[userAnswer].classList.add('correct');
                resultElement.textContent = `✔ Doğru! +1 Puan | ${question.explanation}`;
                resultElement.className = 'correct-answer';
            } else {
                buttons[userAnswer].classList.add('wrong');
                buttons[question.answer].classList.add('correct');
                resultElement.textContent = `✖ Yanlış! Doğru cevap: ${question.options[question.answer]} | ${question.explanation}`;
                resultElement.className = 'wrong-answer';
            }
            
            scoreElement.textContent = `Puan: ${score}/100`;
            
            // Sonraki soru veya bitiş
            setTimeout(() => {
                if (currentQuestionIndex < questions.length - 1) {
                    currentQuestionIndex++;
                    showQuestion();
                } else {
                    endQuiz();
                }
            }, 2000); // 2 saniye bekle
        });

        // OYUN SONU
        function endQuiz() {
            questionElement.textContent = `Test Tamamlandı! 🎉`;
            optionsElement.innerHTML = '';
            nextButton.style.display = 'none';
            resultElement.innerHTML = `
                <h3>Toplam Puan: ${score}/100</h3>
                <p>${getResultMessage(score)}</p>
                <button onclick="location.reload()" style="margin-top: 20px; padding: 10px 20px; background: #3498db; color: white; border: none; border-radius: 5px; cursor: pointer;">
                    Tekrar Dene
                </button>
            `;
        }

        // SONUÇ MESAJI
        function getResultMessage(score) {
            if (score >= 90) return "Mükemmel! Neredeyse tam puan!";
            if (score >= 70) return "İyi iş çıkardınız!";
            if (score >= 50) return "Orta seviye, biraz daha çalışmalısınız";
            return "Daha fazla çalışmaya ihtiyacınız var";
        }

        // UYGULAMAYI BAŞLAT
        window.onload = initQuiz;
    </script>
</body>
</html>
