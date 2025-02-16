[Görsel kayıt burada]( https://drive.google.com/file/d/1vbOHdEOnAxqS53GEToBmgoPkyUs_0Lyo). 1 saat 24 dakika.  

Hızlıca [on birinci ders](ders11.md) notlarına baktık. *Temel* adlı sınıfın, `public, protected, private, operator, friend` anahtar sözcüklerinin, yapıcı (`Tür(...)`) yöntemlerin ve yıkıcı (`~Tür()`) yöntemin üzerinden geçtik. Bilhassa `operator=, operator<< ve operator>>` ile *=, << ve >>* işlemcilerine yeni beceriler kazandırmanın faydalarını gördük.

*Eşlemlere* (`std::map`) derin bakış 
--

Pekiştirmek için dizilerden farklı davranan ama çok faydalı veri yapılarından biri olan **eşlem** kalıbına yani `std::map<Tür1, Tür2>` kullanan örnekler yazdık. [Kodu burada](https://www.onlinegdb.com/57Qmx1v00). Arkadaşlarınız güzel sorular sordular ve güzel gözlemler yaptılar. Bilhassa `std::vector<Tür>` kalıbıyla karşılaştırmak öğretici oldu. **Anahtar** da denilen **Tür1** değerlerinin kendiliğinden sıralandığını gözledik. Eşlemleri daha genel bir küme olarak düşünürsek, üyelerinin birer çift yani `std::pair<Tür1, Tür2>` olduğunu gördük. Derste görmediğimiz bir bağlantı daha kuralım: Daha önce kısaca gördüğümüz `std::set<Tür>` yani matematiksel kümelere çok benzeyen *küme* kalıbını anımsayalım. Eşlemleri kümelerin genellemesi olarak görebiliriz. Küme gereken her yerde kullanılabiliriz eşlemleri. Örneğin bir sayı kümemiz olsun: 
```c++ 
set<int> küme {1, 4, 6, 8, 9, 10, 12};
``` 
Buna eşdeğer bir eşlem şu olur: 
```c++ 
map<int, int> eşlem {
  {1, 1}, {4, 3}, {6, 4}, {8, 4},
  {9, 3}, {10, 4}, {12, 6}
};
```
Elbette sadece eşdeğer değil, fazladan bazı bilgiler de içeriyor. Sanırım *Tür2* değerlerinin ne anlama geldiğini tahmin etmişsinizdir. Hatta, daha da açık olsun istersek daha da ilginç bir örnek yazabiliriz:
```c++ 
map<int, set<int>> eşlem2 {
    { 1, {1} }, 
    { 4, {1, 2, 4} }, 
    { 9, {1, 3, 9} } 
    // ...
};
```
Bu gelişmiş kalıplarla biraz daha alıştırma yapmak ister misiniz? Faydalı olur. Örneğin, [buradaki kodu](https://onlinegdb.com/_CroqvtI4r) çatallayıp birden yüze kadar bütün sayıların çarpanlarını bu *eşlem2* içine ekleyebiliriz. Tabii elle değil, daha önce yazdığımız çarpan bulma kodunu kullanarak 😉. Onu da [burada bulabilirsiniz](https://onlinegdb.com/jw9PvWZ-G).


Şimdilik bu kadar. Vakit buldukça aşağıdaki iki konu için de notlar yazacağım: 

İşlev kalıplarına (*function templates*) giriş 
--
İlk defa kalıp anlamına gelen `template` anahtar sözcüğünü kullandık ve sade bir işlev yerine, bir *işlev kalıbı* tanımladık:
```c++
template<typename Tür>
Tür ekle(Tür a, Tür b) {
  return a + b;
}
```
Programımız bu sayede hem tam sayılar, hem de kesirli sayılar için doğru çalıştı:
```c++
cout << ekle(3, 3)              // 6 yazdı
     << endl << ekle(3.5, 3.5); // 7 yazdı
```
İşin güzel tarafı, bu kalıp sadece sayılar için değil, başka türler için de kullanılabilir. Örneğin, iki yazıyı birbirine ekleyebiliriz:
```c++
cout << ekle("ab", "bc"); // bu hata veriyor. Neden?
cout << ekle(string("ab"), string("bc\n")); // Bu çalışır.
```
Hatayı daha iyi anlamak için şu kısacık kodu çalıştırıverin: [tek kalıplı kod](https://onlinegdb.com/SMAOywzm7X).   
Hata mesajına bakalım. Derleyici önce kalıbı somutlaştırmaya çalıştığını yazar arkadan da şu hatayı verir:
```c++
error: invalid operands of types ‘const char*’ and ‘const char*’ to binary ‘operator+’
    9 |         return a + b;
```

[Derste yazığımız kod burada](https://www.onlinegdb.com/IibF-74Br).  

Dersten sonra bir kaç ekleme yaptım. En önemlisi, ikinci bir işlev kalıbı ekledim. Onu da şurda bulabilirsiniz: [iki kalıplı](https://onlinegdb.com/inzq7Z7O3)   

Arkası yarın!

Tür kalıplarına (*class templates*) giriş 
--
[Kodu burada](https://www.onlinegdb.com/nRozqW61O).

Sevgiler, başarılar.  
