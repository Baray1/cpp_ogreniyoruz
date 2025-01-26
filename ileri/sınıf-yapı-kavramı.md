Nesne yönelimi de ne demek?
====

C++'daki sınıf (`class`) kavramı ve anahtar sözcüğü, nesne yönelimli programlamanın temel yapı taşıdır. *OOP* diye duymuş olabilirsiniz: *object oriented programming*. C++'ın çok başarılı olmuş ve hala çok kullanılan atası *C* diline eklediği en önemli iki beceriden birincisi olan sınıfları, gerçek hayattan bir örnek üzerinden anlamaya çalışalım. *C*'de olmayan öbür büyük *C++* becerisi de **tür kalıplarıdır (class templates)**. Ona da aşağıda değineceğiz.

Bir "Araba" sınıfı oluşturarak başlayalım:

```cpp
class Araba {
private:
    // Kapsama (Encapsulation) örneği: Bu özelliklere dışarıdan doğrudan erişilemez
    string marka;
    string model;
    int hız;

public:
    // Sınıfın yapıcı yöntemi 
    Araba(string _marka, string _model) {
        marka = _marka;
        model = _model;
        hız = 0;
    }

    // istersek birden çok yapıcı da ekleyebiliriz. 
    // Ayrıca ilk değerleri en başta, gövdeye girmeden önce belirleyebiliyoruz
    Araba():marka("Murat", "Şahin", 50) {};

    // yıkıcı yönteme gerek yok! Neden?

    // Soyutlama (Abstraction) örneği: Karmaşık işlemler basit yöntemlerle gizlenir
    void hızlan() {
        if (hız < 180) {
            hız += 10;
            cout << "Araba hızlandı. Şu anki hız: " << hız << " km/s" << endl;
        }
    }

    void yavaşla() {
        if (hız > 0) {
            hız -= 10;
            cout << "Araba yavaşladı. Şu anki hız: " << hız << " km/s" << endl;
        }
    }

    // Bilgi alma metodları
    string markaModel() {
        return marka + " " + model;
    }
};
```

Kalıtım (Inheritance) örneği için ise:

```cpp
// Temel Araba sınıfından türeyen Spor Araba sınıfı
class SporAraba : public Araba {
private:
    int motorGücü;

public:
    SporAraba(string _marka, string _model, int _motorGücü) 
        : Araba(_marka, _model) {
        motorGücü = _motorGücü;
    }

    void hızlıGit() {
        // Ana sınıftan gelen özellikleri kullanarak yeni bir yöntem 
        for(int i = 0; i < 5; ++i) {
            hızlan();
        }
    }
};
```

Bu örnekte:

1. **Kapsama (Encapsulation)**: Araba'nın iç özellikleri (`marka`, `model`, `hız`) `private` olarak tanımlanmış. Bunlara doğrudan erişilemiyor, sadece sınıfın yöntemleri (metod da denir) aracılığıyla kontrollü bir şekilde değiştirilebiliyor. Kapsarken koruyor.

2. **Soyutlama (Abstraction)**: `hızlan()` ve `yavaşla()` yöntemleri, hız değişiminin karmaşık mantığını basit, anlaşılır metodlarla gizliyor. Kullanıcı detayları bilmeden bu yöntemleri kullanabilir. Detaylar yerine önemli konulara odaklanmamıza yardım ediyor.

3. **Kalıtım (Inheritance)**: `SporAraba` sınıfı, `Araba` sınıfından tüm özellikleri ve metodları miras alıyor. DRY (don't repeat yourself) ilkesinde olduğu gibi, üst sınıfta bir kere yazıyor, alt sınıflarda hazır buluyor, kolaylıkla kullanabiliyoruz. Birden fazla alt sınıf olan durumlarda, ki yaygın bir durum, faydası artıyor. Örneğin `ElektrikliAraba`, `TekKişilik` vb eklenebilir. Üstelik alt sınıflar kendi özel yöntemlerini de (`hızlıGit()` örneğinde olduğu gibi) ekleyebiliyorlar. 

Kullanım örneği:

```cpp
int main() {
    SporAraba bmw("BMW", "M3", 450);
    
    bmw.hızlıGit();  // Hızlanma metodunu çağırır
    cout << bmw.markaModel() << endl;  // Marka ve modeli yazdırır

    return 0;
}
``` 

[Kodun hepsini onlinegdb.com'da çalıştırmak için buraya tıklayabilirsiniz](https://onlinegdb.com/dbFycFP09).  


Bu yaklaşımlar, yazılımların:
- Daha düzenli,
- Güvenli,
- Tekrar kullanılabilir ve
- Bakım ve değişiklik yapması daha kolay olmasına yardımcı olur.  

Ayrıca, sınıflar ve genelde yeni türler, karmaşık sistemleri küçük, yönetilebilir parçalara ayırarak programlarımızı daha anlaşılır kılar.

Sınıf ve tür eş anlamlı kavramlar
----
Bu örnekte arabayı yeni bir tür olarak da düşünebiliriz. Her sınıf ya da yapı (`struct`) yeni bir tür oluşturarak yazılıma gereken karmaşık nesneleri düzenler ve anlaşılır kılar.  

`class` ve `struct` anahtar sözcüklerinin tek farkı içindeki verilerin gizli ya da görünür olması. Şu erişim belirleyicilerle (access specifier) kendimiz düzenlemeler yapabiliriz: `public` (görünür), `private` (gizli) ve `protected` (korunur).  

`typedef` anahtar sözcüğü 
----
Daha önce `typedef` örnekleri görmüştük. Türkçeye tür tanımlama diye çevirebiliriz. Bu anahtar sözcüğü bazen türlere takma ad vermek için kullandık. Örneğin:    
```c++ 
typedef unsigned long int Sayı;
Sayı s{123456789101112l}; 
``` 

[Tam kod burada](https://onlinegdb.com/X5tlT-dRP).  

Bazen de *tür kalıpları (class templates)* kullanarak yepyeni türler tanımlamak için kullandık. Örnek olarak aşağıda 'Ay' türüne bakacağız. Ama önce temelde ne oluyor, daha iyi kavrayalım.

**Bileşim (composition)** kavramı
----
Bu yeni kavram, yukarıda bahsettiğimiz *kapsama* kavramıyla yakından ilişkili. Örneğimizde `Araba` türünü, marka, model ve hız bilgilerinin bir bileşkesi olarak tanımladık. `SporAraba` da bir tür araba, ama onun bileşiminde bir de motorGücü var. 

Altıncı dersimizde gördüğümüz `Kişi` türünü ad, soyad, yaş ve telefon bilgilerinin bileşkesi olarak tanımladık. 

[Kişi türü onlinegdb](https://onlinegdb.com/2LlCHEI1H).   
  
[Kişi türü sololearn](https://www.sololearn.com/en/compiler-playground/cVc74iqt2Ul0).  

Yedinci dersimizde `dörtgen` türü, en, boy ve konum bilgilerinin bileşkesiydi. `konum` türünü x ve y sayılarıyla tanımladık. Yani konum da x ve y koordinatlarının bileşkesi olarak düşünülebilir. Ayrıca `dörtgen`, a ve b sayılarının bileşkesi olan `aralık` türünü kullandı. Aynı `konum` gibi `aralık` da bir ikili ama verdiğimiz anlamlar farklı.  [dörtgen, konum, aralık türleri](https://www.onlinegdb.com/3fk-Akokh). 

Ve son dersimizde karmaşık sayıları yine iki sayının bileşkesi olarak yapılandırdık `struct` kullanarak.
[Yazılım burada](https://www.onlinegdb.com/SybVZuiJ_).   

Gördüğünüz gibi ikili bileşkeler o kadar faydalı ve yaygın ki, standart kütüphane bize `std::pair<tür1, tür2>` adlı tür kalıbını sunuyor. Bu en genel anlamda bir tür ikili: iki alt türün bileşkesi: `tür1` ve `tür2`. Bu bileşenler henüz birer değişken, yani bir türün adı. Onlara değer vererek bu kalıptan somut bir tür üretiveriyoruz: 
```c++
typedef std::pair<std::string, int> Ay;
std::list<Ay> aylar{
   Ay("Ocak", 31), 
   Ay("Şubat", 28), // ya da 29!?
   // ...
   Ay("Aralık", 31)
}
``` 

[Tam çalışan ikili kalıp örneği](https://onlinegdb.com/dvtEol7le).   

Yazılım mühendisliğine değişik bir bakış 
----
Programlamaya matematiksel, mantıksal, mühendislik ve yüksek teknolojik bazı problemlerin çözümü olarak bakarız genelde. Asal çarpanlar, Fibonacci, patika sayımı, veri tabanları, iş, müşteri ilişkiler idaresi, internet tarayıcılar, arama motorları, sosyal medya uygulamaları, tomografi yazılımı, mimari ve yonga tasarımı için CAD (computer aided design, yani bilgisayar destekli tasarım) programları, mobil oyunlar... 

Ama işin bir de sanat tarafı var. Elbette [Avatar: Suyun Yolu](https://en.m.wikipedia.org/wiki/Avatar:_The_Way_of_Water) ve [Kaptan Marvel](https://en.m.wikipedia.org/wiki/Captain_Marvel_(film)) gibi bilim kurgu filmlerinde gördüğümüz bilgisayar grafikleri de bir sanat ve pek çok kıymetli grafik sanatçısı, ressam emek veriyor. Ama onları bir tarafa bırakalım. Yazılımın kendisine bakalım. Bu ciddi mühendislik ve iş alanı sadece kuru mantık ve uygulamalı problem çözümü değil. Temelinde çok daha yaratıcı bir sanatçılığa uzanıyor: Güzel ve uyumlu çalışan çeşitli yeni yeni *türlere (classes/types)* ait *bireylerin* ya da *nesnelerin (objects)* birlikte yaşadığı ve güzelce geçindiği sanal bir evreni tasarlıyor, yaratıyor ve elimizdeki donanım üzerinde verimli çalışmasını gerçekleştiriyoruz. Yani tadını alan, severek gayret edenler için sadece bir katma değer üretme ve ekmek parası kazanma yolu değil, bir de epey keyifli ve tatmin edici bir sanatkâr emeği oluyor. Kullanıcılar da severek faydalanıyorlar bu sanatın ürünlerinden. Hadi siz de alın bir fırça elinize!  

Bülent hocanızdan 🧑🏻‍🦳 ve [Claude 3.5 Haiku](https://claude.ai) 🤖 yapay beyninden sevgilerle.  