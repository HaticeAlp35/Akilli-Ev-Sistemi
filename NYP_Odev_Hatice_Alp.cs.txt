using System;

namespace EvCihazYonetimi
{
    // cihaz türlerini burda tutuyoruz
    enum CihazTuru
    {
        Lamba,
        Klima,
        GuvenlikSistemi
    }

    class Cihaz
    {
        // cihazın özellikleri
        public int Kimlik { get; set; }
        public string Ad { get; set; }
        public CihazTuru Tur { get; set; }
        public bool AcikMi { get; set; }

        // kurucu metot, ilk oluştuğunda cihaz kapalı başlasın
        public Cihaz(int kimlik, string ad, CihazTuru tur)
        {
            Kimlik = kimlik;
            Ad = ad;
            Tur = tur;
            AcikMi = false;
        }

        // cihazı açma metodu
        public void Ac()
        {
            if (AcikMi)
            {
                Console.WriteLine("Bu cihaz zaten açık durumda.");
            }
            else
            {
                AcikMi = true;
                Console.WriteLine(Ad + " açıldı.");
            }
        }

        // cihazı kapatma metodu
        public void Kapat()
        {
            if (!AcikMi)
            {
                Console.WriteLine("Bu cihaz zaten kapalı durumda.");
            }
            else
            {
                AcikMi = false;
                Console.WriteLine(Ad + " kapatıldı.");
            }
        }

        // bilgileri ekrana yazdırmak için
        public void BilgileriGoster()
        {
            string durum = AcikMi ? "Açık" : "Kapalı";
            Console.WriteLine($"ID: {Kimlik} | Ad: {Ad} | Tür: {Tur} | Durum: {durum}");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // cihazları bu listede saklıcaz
            List<Cihaz> cihazlar = new List<Cihaz>();

            // başlangıçta birkaç cihaz olsun diye örnek ekledim
            cihazlar.Add(new Cihaz(1, "Salon Lambası", CihazTuru.Lamba));
            cihazlar.Add(new Cihaz(2, "Oturma Odası Kliması", CihazTuru.Klima));
            cihazlar.Add(new Cihaz(3, "Dış Kapı Kamerası", CihazTuru.GuvenlikSistemi));

            // kullanıcı çıkana kadar menü sürekli dönsün
            while (true)
            {
                Console.WriteLine("\n--- AKILLI EV CİHAZ YÖNETİMİ ---");
                Console.WriteLine("1 - Cihazları Listele");
                Console.WriteLine("2 - Yeni Cihaz Ekle");
                Console.WriteLine("3 - Cihaz Aç");
                Console.WriteLine("4 - Cihaz Kapat");
                Console.WriteLine("0 - Çıkış");
                Console.Write("Seçiminiz: ");
                string secim = Console.ReadLine();

                Console.WriteLine();

                if (secim == "1")
                {
                    Console.WriteLine("--- Kayıtlı Cihazlar ---");
                    foreach (var c in cihazlar)
                    {
                        c.BilgileriGoster();
                    }
                }
                else if (secim == "2")
                {
                    Console.Write("Cihaz Kimliği (Sayı): ");
                    int yeniId = int.Parse(Console.ReadLine());

                    // aynı id var mı diye bakıyoruz
                    bool idVarMi = false;
                    foreach (var c in cihazlar)
                    {
                        if (c.Kimlik == yeniId)
                        {
                            idVarMi = true;
                            break;
                        }
                    }

                    if (idVarMi)
                    {
                        Console.WriteLine("Hata: Bu kimliğe sahip bir cihaz zaten var! Eklenemedi.");
                    }
                    else
                    {
                        Console.Write("Cihaz Adı: ");
                        string yeniAd = Console.ReadLine();

                        Console.WriteLine("Cihaz Türü Seçin (0: Lamba, 1: Klima, 2: GuvenlikSistemi): ");
                        int turSecim = int.Parse(Console.ReadLine());
                        CihazTuru secilenTur = (CihazTuru)turSecim;

                        // yeni cihazı listeye ekle
                        Cihaz yeniCihaz = new Cihaz(yeniId, yeniAd, secilenTur);
                        cihazlar.Add(yeniCihaz);
                        Console.WriteLine("Yeni cihaz başarıyla eklendi.");
                    }
                }
                else if (secim == "3")
                {
                    Console.Write("Açmak istediğiniz cihazın kimliği: ");
                    int acilacakId = int.Parse(Console.ReadLine());

                    // girilen id'yi listede arıyoruz
                    Cihaz bulunan = null;
                    foreach (var c in cihazlar)
                    {
                        if (c.Kimlik == acilacakId)
                        {
                            bulunan = c;
                            break;
                        }
                    }

                    // cihaz yoksa hata ver varsa aç
                    if (bulunan == null)
                    {
                        Console.WriteLine("Hata: Böyle bir kimliğe sahip cihaz bulunamadı!");
                    }
                    else
                    {
                        bulunan.Ac();
                    }
                }
                else if (secim == "4")
                {
                    Console.Write("Kapatmak istediğiniz cihazın kimliği: ");
                    int kapatilacakId = int.Parse(Console.ReadLine());

                    Cihaz bulunan = null;
                    foreach (var c in cihazlar)
                    {
                        if (c.Kimlik == kapatilacakId)
                        {
                            bulunan = c;
                            break;
                        }
                    }

                    if (bulunan == null)
                    {
                        Console.WriteLine("Hata: Böyle bir kimliğe sahip cihaz bulunamadı!");
                    }
                    else
                    {
                        bulunan.Kapat();
                    }
                }
                else if (secim == "0")
                {
                    Console.WriteLine("Programdan çıkılıyor...");
                    break;
                }
                else
                {
                    Console.WriteLine("Geçersiz seçim yaptınız, tekrar deneyin.");
                }
            }
        }
    }
}