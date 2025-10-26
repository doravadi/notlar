# C# INTRODUCTION VE OOP KAPSAMLI SINAV DÖKÜMANTASYONU

## İÇİNDEKİLER
1. [C# TEMELLER](#1-c-temeller)
2. [VERİ TİPLERİ](#2-veri-tipleri)
3. [TİP DÖNÜŞÜMLERİ](#3-tip-dönüşümleri)
4. [OPERATÖRLER](#4-operatörler)
5. [KARAR YAPILARI](#5-karar-yapilari)
6. [DÖNGÜLER](#6-döngüler)
7. [HATA YÖNETİMİ](#7-hata-yönetimi)
8. [DİZİLER](#8-diziler)
9. [KOLEKSİYONLAR](#9-koleksiyonlar)
10. [METOTLAR](#10-metotlar)
11. [OOP - SINIFLAR](#11-oop---siniflar)
12. [OOP - KALITIM](#12-oop---kalitim-inheritance)
13. [OOP - ABSTRACT SINIFLAR](#13-oop---abstract-siniflar)
14. [OOP - INTERFACE](#14-oop---interface)
15. [OOP - GENERİC](#15-oop---generic)
16. [OOP İLİŞKİLER](#16-oop-ilişkiler)
17. [STRUCT VE ENUM](#17-struct-ve-enum)
18. [EXTENSION METHODS](#18-extension-methods)
19. [PARTIAL CLASS](#19-partial-class)
20. [STATIC](#20-static)

---

## 1. C# TEMELLER

### Değişken Tanımlama Kuralları

```csharp
// veri_tipi degisken_adi = deger;
int age = 40, score;
string name = "Fatih";
double price = 25.5;
bool isActive = true;
```

**Değişken İsimlendirme Kuralları:**
- Rakamla başlayamaz
- Boşluk içeremez
- C# anahtar kelimeleri kullanılamaz
- Case-sensitive (büyük-küçük harf duyarlı)
- camelCase veya PascalCase kullanılır

### Namespace ve Using

```csharp
using System;
using System.Collections.Generic;

namespace MyProject
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello, World!");
        }
    }
}
```

---

## 2. VERİ TİPLERİ

### Tam Sayılar (Integer Types)

| Tip | Min Değer | Max Değer | Boyut |
|-----|-----------|-----------|-------|
| **byte** | 0 | 255 | 1 byte |
| **sbyte** | -128 | 127 | 1 byte |
| **short** | -32,768 | 32,767 | 2 byte |
| **ushort** | 0 | 65,535 | 2 byte |
| **int** | -2,147,483,648 | 2,147,483,647 | 4 byte |
| **uint** | 0 | 4,294,967,295 | 4 byte |
| **long** | -9,223,372,036,854,775,808 | 9,223,372,036,854,775,807 | 8 byte |
| **ulong** | 0 | 18,446,744,073,709,551,615 | 8 byte |

### Ondalıklı Sayılar (Floating Point Types)

```csharp
float ondalik1 = 2.5F;    // 4 byte, 7 basamak hassasiyet
double ondalik2 = 2.5D;   // 8 byte, 15-16 basamak hassasiyet
decimal ondalik3 = 2.5M;  // 16 byte, 28-29 basamak hassasiyet
```

### Karakter ve String

```csharp
char karakter = 'A';        // Tek karakter
string metin = "Merhaba";   // Metin dizisi
```

### Boolean

```csharp
bool dogruMu = true;
bool yanlisMi = false;
```

### Gelişmiş Veri Tipleri

#### Object
```csharp
Object deger1 = 1;        // Boxing - değer tipini object'e atama
Object deger2 = "Merhaba";
Object deger3 = true;

int sayi = (int)deger1;   // Unboxing - object'ten değer tipine dönüştürme
```

#### Var
```csharp
// Derleme zamanında tip ataması yapılır
var degisken1 = 5;         // int olarak algılanır
var degisken2 = 5.4;       // double olarak algılanır
var degisken3 = "Merhaba"; // string olarak algılanır
```

#### Dynamic
```csharp
// Çalışma zamanında tip ataması yapılır
dynamic deneme1 = 5;
dynamic deneme2 = "Merhaba";
Console.WriteLine(deneme1 * 2); // Hata vermez
```

#### Nullable Tipler
```csharp
int? nullableSayi = null;  // null geçilebilir int
Console.WriteLine(nullableSayi.HasValue); // false

nullableSayi = 25;
Console.WriteLine(nullableSayi.HasValue); // true
Console.WriteLine(nullableSayi.Value);    // 25

// Null Coalescing Operator
int? puan = null;
int sonuc = puan ?? 50; // puan null ise 50 değerini al
```

---

## 3. TİP DÖNÜŞÜMLERİ

### Implicit (Kapalı) Dönüşüm
```csharp
byte data1 = 100;
int data2 = data1; // Otomatik dönüşür
```

### Explicit (Açık) Dönüşüm
```csharp
int data3 = 10000;
byte data4 = (byte)data3; // Manuel cast gerekir
```

### Convert Sınıfı
```csharp
string str = "123";
int number = Convert.ToInt32(str);
byte result = Convert.ToByte(123);
bool bool1 = Convert.ToBoolean("True");
```

### Parse Metodu
```csharp
int result1 = int.Parse("123");
double result2 = double.Parse("123.45");
bool result3 = bool.Parse("True");
```

### TryParse Metodu
```csharp
string input = "123a";
int number;
bool success = int.TryParse(input, out number);
if (success)
    Console.WriteLine($"Başarılı: {number}");
else
    Console.WriteLine("Başarısız dönüşüm");
```

### ToString Metodu
```csharp
int data = 123;
string text = data.ToString();
```

---

## 4. OPERATÖRLER

### Aritmetik Operatörler
```csharp
int a = 10, b = 3;
int toplam = a + b;    // 13
int fark = a - b;      // 7
int carpim = a * b;    // 30
int bolum = a / b;     // 3
int kalan = a % b;     // 1
```

### Atama Operatörleri
```csharp
int sayi = 10;
sayi += 5;  // sayi = sayi + 5  => 15
sayi -= 3;  // sayi = sayi - 3  => 12
sayi *= 2;  // sayi = sayi * 2  => 24
sayi /= 4;  // sayi = sayi / 4  => 6
sayi %= 4;  // sayi = sayi % 4  => 2
```

### Artırma/Azaltma Operatörleri
```csharp
int x = 5;
x++;        // Post-increment: önce kullan sonra artır
++x;        // Pre-increment: önce artır sonra kullan
x--;        // Post-decrement
--x;        // Pre-decrement

// Örnek
int i = 5;
int j = ++i + i++ + ++i + i;
// ++i => i=6, dönen=6
// i++ => i=7 (sonra), dönen=6
// ++i => i=8, dönen=8
// i   => dönen=8
// j = 6 + 6 + 8 + 8 = 28
```

### Karşılaştırma Operatörleri
```csharp
bool sonuc1 = (5 == 5);   // true
bool sonuc2 = (5 != 3);   // true
bool sonuc3 = (5 > 3);    // true
bool sonuc4 = (5 < 3);    // false
bool sonuc5 = (5 >= 5);   // true
bool sonuc6 = (5 <= 4);   // false
```

### Mantıksal Operatörler
```csharp
bool a = true, b = false;
bool sonuc1 = a && b;     // false (VE)
bool sonuc2 = a || b;     // true  (VEYA)
bool sonuc3 = !a;         // false (DEĞİL)
```

---

## 5. KARAR YAPILARI

### If-Else Yapısı
```csharp
int not = 75;
if (not > 100 || not < 0)
    Console.WriteLine("Geçersiz not");
else if (not >= 70)
    Console.WriteLine("Geçtiniz");
else
    Console.WriteLine("Kaldınız");
```

### Ternary If
```csharp
int not = 75;
string sonuc = not >= 70 ? "Geçti" : "Kaldı";

// İç içe ternary
int sayi = 0;
string durum = sayi > 0 ? "Pozitif" : (sayi < 0) ? "Negatif" : "Sıfır";
```

### Switch-Case
```csharp
int gun = 3;
switch (gun)
{
    case 1:
    case 2:
    case 3:
    case 4:
    case 5:
        Console.WriteLine("Hafta içi");
        break;
    case 6:
    case 7:
        Console.WriteLine("Hafta sonu");
        break;
    default:
        Console.WriteLine("Hatalı giriş");
        break;
}
```

### Switch Expression (C# 8.0+)
```csharp
char islem = '+';
int sayi1 = 5, sayi2 = 6;
double sonuc = islem switch
{
    '+' => sayi1 + sayi2,
    '-' => sayi1 - sayi2,
    '*' => sayi1 * sayi2,
    '/' => sayi1 / sayi2,
    _ => double.NaN
};
```

### Pattern Matching ile Switch
```csharp
object veri = 10;
switch (veri)
{
    case int sayi when sayi > 0:
        Console.WriteLine("Pozitif tam sayı");
        break;
    case int sayi when sayi < 0:
        Console.WriteLine("Negatif tam sayı");
        break;
    case string metin:
        Console.WriteLine($"String: {metin}");
        break;
    default:
        Console.WriteLine("Tanımlanamayan tür");
        break;
}
```

---

## 6. DÖNGÜLER

### For Döngüsü
```csharp
for (int i = 0; i < 5; i++)
{
    Console.WriteLine($"i = {i}");
}

// Sonsuz döngü
for (;;)
{
    Console.WriteLine("Sonsuz");
    break; // Çıkış için
}
```

### While Döngüsü
```csharp
int sayac = 0;
while (sayac < 5)
{
    Console.WriteLine(sayac);
    sayac++;
}
```

### Do-While Döngüsü
```csharp
string sifre;
do
{
    Console.Write("Şifre: ");
    sifre = Console.ReadLine();
} while (sifre != "1234");
```

### Foreach Döngüsü
```csharp
string[] meyveler = { "Elma", "Armut", "Portakal" };
foreach (string meyve in meyveler)
{
    Console.WriteLine(meyve);
}
```

### Jump İfadeleri
```csharp
// Break - döngüden çıkar
for (int i = 0; i < 10; i++)
{
    if (i == 5)
        break;
    Console.WriteLine(i);
}

// Continue - sonraki iterasyona geçer
for (int i = 0; i < 10; i++)
{
    if (i == 5)
        continue;
    Console.WriteLine(i);
}

// Goto - belirtilen etikete gider
int sayac = 0;
Start:
if (sayac < 5)
{
    Console.WriteLine($"Sayaç: {sayac}");
    sayac++;
    goto Start;
}
```

---

## 7. HATA YÖNETİMİ

### Try-Catch-Finally
```csharp
try
{
    // Hata gelmesi muhtemel kodlar
    int sayi = int.Parse("abc");
}
catch (FormatException ex)
{
    Console.WriteLine($"Format hatası: {ex.Message}");
}
catch (OverflowException ex)
{
    Console.WriteLine($"Taşma hatası: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Genel hata: {ex.Message}");
}
finally
{
    // Her durumda çalışır
    Console.WriteLine("İşlem tamamlandı");
}
```

### Throw ile Hata Fırlatma
```csharp
try
{
    int age = int.Parse(Console.ReadLine());
    if (age < 18)
        throw new ArgumentException("Yaş 18'den küçük olamaz!");
}
catch (ArgumentException ex)
{
    Console.WriteLine(ex.Message);
}
```

### When ile Filtreleme
```csharp
try
{
    int number = int.Parse("abc");
}
catch (FormatException ex) when (ex.Message.Contains("Input string"))
{
    Console.WriteLine("Özel format hatası");
}
catch (FormatException ex)
{
    Console.WriteLine("Genel format hatası");
}
```

### Özel Exception Sınıfı
```csharp
public class ValidationException : Exception
{
    public string PropertyName { get; private set; }
    
    public ValidationException(string message, string propertyName) 
        : base(message + " Date: " + DateTime.Now)
    {
        PropertyName = propertyName;
    }
}
```

---

## 8. DİZİLER

### Tek Boyutlu Diziler
```csharp
// Tanımlama yöntemleri
int[] numbers = new int[5];
int[] numbers2 = { 1, 2, 3, 4, 5 };
int[] numbers3 = new int[] { 1, 2, 3, 4, 5 };

// Değer atama ve okuma
numbers[0] = 10;
int deger = numbers[0];

// Döngü ile gezinme
for (int i = 0; i < numbers.Length; i++)
{
    Console.WriteLine(numbers[i]);
}

foreach (int num in numbers)
{
    Console.WriteLine(num);
}
```

### Çok Boyutlu Diziler
```csharp
int[,] matris = new int[3, 3]
{
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

Console.WriteLine(matris[1, 2]); // 6

// Matris yazdırma
for (int i = 0; i < 3; i++)
{
    for (int j = 0; j < 3; j++)
    {
        Console.Write(matris[i, j] + " ");
    }
    Console.WriteLine();
}
```

### Jagged Arrays (Düzensiz Diziler)
```csharp
int[][] duzensizDizi = new int[3][];
duzensizDizi[0] = new int[2] { 1, 2 };
duzensizDizi[1] = new int[4] { 3, 4, 5, 6 };
duzensizDizi[2] = new int[1] { 7 };

Console.WriteLine(duzensizDizi[1][2]); // 5
```

### Array Sınıfı Metotları
```csharp
string[] sehirler = { "Istanbul", "Ankara", "Izmir", "Bursa" };

// Sort - Sıralama
Array.Sort(sehirler);
Array.Sort(sehirler, 1, 2); // Belirli aralığı sırala

// Reverse - Ters çevir
Array.Reverse(sehirler);

// IndexOf - İlk indeksi bul
int index = Array.IndexOf(sehirler, "Ankara");

// LastIndexOf - Son indeksi bul
int lastIndex = Array.LastIndexOf(sehirler, "Ankara");

// Clear - Temizle
Array.Clear(sehirler, 0, sehirler.Length);

// Copy - Kopyala
string[] yeniSehirler = new string[4];
Array.Copy(sehirler, yeniSehirler, 2);

// Resize - Boyut değiştir
Array.Resize(ref sehirler, 10);
```

---

## 9. KOLEKSİYONLAR

### List<T>
```csharp
List<int> sayilar = new List<int>() { 10, 20, 5 };
sayilar.Add(7);
sayilar.AddRange(new[] { 8, 9, 10 });
sayilar.Remove(5);
sayilar.RemoveAt(1);
sayilar.Insert(2, 15);
sayilar.Clear();

// Arama ve kontrol
bool varMi = sayilar.Contains(10);
int index = sayilar.IndexOf(20);
int count = sayilar.Count;

// LINQ işlemleri
sayilar.ForEach(x => Console.WriteLine(x));
var sirali = sayilar.OrderBy(x => x).ToList();
var tersSirali = sayilar.OrderByDescending(x => x).ToList();
var enBuyuk = sayilar.Max();
var enKucuk = sayilar.Min();
var ortalama = sayilar.Average();
```

### Tuple
```csharp
// Tuple tanımlama
var person = (Id: 1, Name: "Fatih", IsActive: true);
Console.WriteLine(person.Name);

// Liste içinde tuple
List<(string Ad, string Sinif, int Not)> ogrenciler = new List<(string, string, int)>();
ogrenciler.Add(("Ali", "10-A", 85));

// Tuple döndüren metot
public (int toplam, double ortalama) Hesapla(List<int> notlar)
{
    return (notlar.Sum(), notlar.Average());
}
```

### Dictionary<TKey, TValue>
```csharp
Dictionary<int, string> ogrenciler = new Dictionary<int, string>();
ogrenciler.Add(101, "Ali");
ogrenciler.Add(102, "Ayşe");

// Değer okuma
string isim = ogrenciler[101];

// Güvenli okuma
string ad;
if (ogrenciler.TryGetValue(102, out ad))
{
    Console.WriteLine(ad);
}

// Kontrol
bool varMi = ogrenciler.ContainsKey(101);
bool degerVarMi = ogrenciler.ContainsValue("Ali");

// Güncelleme
ogrenciler[101] = "Mehmet";

// Silme
ogrenciler.Remove(101);

// Döngü
foreach (var item in ogrenciler)
{
    Console.WriteLine($"Key: {item.Key}, Value: {item.Value}");
}
```

### ArrayList (Non-Generic)
```csharp
ArrayList arrayList = new ArrayList() { "Selam", 112 };
arrayList.Add(10);
arrayList.Add(25.2);

foreach (var item in arrayList)
{
    if (item.GetType() == typeof(int))
    {
        int sonuc = (int)item * 2;
    }
}
```

### Stack<T> (LIFO - Son Giren İlk Çıkar)
```csharp
Stack<string> yigin = new Stack<string>();
yigin.Push("A");  // Ekleme
yigin.Push("B");
yigin.Push("C");

string ilkEleman = yigin.Pop();   // Çıkarma ve döndürme
string bas = yigin.Peek();        // İlk elemanı gösterme (çıkarmadan)
int adet = yigin.Count;
```

### Queue<T> (FIFO - İlk Giren İlk Çıkar)
```csharp
Queue<string> kuyruk = new Queue<string>();
kuyruk.Enqueue("A");  // Ekleme
kuyruk.Enqueue("B");
kuyruk.Enqueue("C");

string ilkEleman = kuyruk.Dequeue();  // Çıkarma ve döndürme
string bas = kuyruk.Peek();           // İlk elemanı gösterme
```

### SortedList<TKey, TValue>
```csharp
SortedList<int, string> sinif = new SortedList<int, string>();
sinif.Add(3, "Ali");
sinif.Add(1, "Veli");
sinif.Add(4, "Elif");
// Otomatik olarak key'e göre sıralanır
```

### HashSet<T> (Benzersiz Elemanlar)
```csharp
HashSet<string> emailSet = new HashSet<string>();
emailSet.Add("example1@example.com");
emailSet.Add("example2@example.com");
emailSet.Add("example2@example.com"); // Eklenmez, zaten var
```

---

## 10. METOTLAR

### Metot Tanımlama
```csharp
// [ErişimBelirleyici] [Niteleyici] [GeriDönüşTipi] [MetotAdi]([Parametreler])
public static int Topla(int sayi1, int sayi2)
{
    return sayi1 + sayi2;
}

private static void SelamVer(string ad)
{
    Console.WriteLine($"Merhaba {ad}");
}
```

### Varsayılan Parametreler
```csharp
public static void LogMessage(string message, string logLevel = "INFO", string status = "1")
{
    Console.WriteLine($"[{logLevel}] {message}");
}

// Kullanım
LogMessage("This is a log");
LogMessage("Error log", "ERROR");
LogMessage(status: "2", message: "Test"); // Named parameters
```

### Nullable Parametreler
```csharp
public static void DisplayUserInfo(string name, int? age = null)
{
    if (age.HasValue)
        Console.WriteLine($"{name} is {age} years old");
    else
        Console.WriteLine($"{name} did not provide an age");
}
```

### Method Overloading
```csharp
public static int Topla(int a, int b)
{
    return a + b;
}

public static int Topla(int a, int b, int c)
{
    return a + b + c;
}

public static double Topla(double a, double b)
{
    return a + b;
}
```

### Params Keyword
```csharp
public static int Topla(params int[] sayilar)
{
    int toplam = 0;
    foreach (var sayi in sayilar)
        toplam += sayi;
    return toplam;
}

// Kullanım
int sonuc = Topla(1, 2, 3, 4, 5, 6, 7, 8);
```

### Out Parametresi
```csharp
public static double Bolme(double bolunen, double bolen, out double kalan)
{
    kalan = bolunen % bolen;
    return bolunen / bolen;
}

// Kullanım
double sonuc, kalanDeger;
sonuc = Bolme(15, 2, out kalanDeger);
Console.WriteLine($"Sonuç: {sonuc}, Kalan: {kalanDeger}");
```

### Ref Parametresi
```csharp
public static void DegeriDegistir(ref int x)
{
    x = x + 10;
}

// Kullanım
int sayi = 5;
Console.WriteLine($"Önce: {sayi}");  // 5
DegeriDegistir(ref sayi);
Console.WriteLine($"Sonra: {sayi}"); // 15
```

### Local Functions
```csharp
static void Main(string[] args)
{
    int Hesapla(int x, int y)
    {
        return x + y;
    }
    
    int sonuc = Hesapla(5, 3);
}
```

### XML Documentation
```csharp
/// <summary>
/// N sayıda değişkeni toplayan metot
/// </summary>
/// <param name="sayilar">Birden fazla sayı girişi</param>
/// <returns>Sayıların toplam değeri</returns>
public static int Topla(params int[] sayilar)
{
    return sayilar.Sum();
}
```

---

## 11. OOP - SINIFLAR

### Class Tanımlama
```csharp
public class Car
{
    // Fields (Alanlar) - Private olmalı (Encapsulation)
    private string _brand;
    private string _model;
    private int _year;
    private double _fuel;
    
    // Constructor (Yapıcı Metot)
    public Car()
    {
        // Parametresiz constructor
    }
    
    public Car(string brand, double fuel)
    {
        _brand = brand;
        Fuel = fuel;  // Property üzerinden set
    }
    
    public Car(string brand, double fuel, string model) : this(brand, fuel)
    {
        Model = model;
    }
    
    // Properties (Özellikler)
    public string Brand
    {
        get { return _brand; }
        private set { _brand = value; }  // Sadece class içinden set edilebilir
    }
    
    // Expression-bodied property (Read-only)
    public string Brand2 => _brand;
    
    // Auto-implemented property
    public string Color { get; set; }
    
    // Property with validation
    public double Fuel
    {
        get { return _fuel; }
        set
        {
            if (value >= 0 && value <= 80)
                _fuel = value;
            else
                throw new Exception("Yakıt 0-80 litre arasında olmalı!");
        }
    }
    
    public int Year
    {
        get { return _year; }
        set
        {
            if (value >= DateTime.Now.Year)
                _year = value;
            else
                _year = DateTime.Now.Year;
        }
    }
    
    // Methods
    public void GetInformation()
    {
        Console.WriteLine($"Marka: {_brand}, Model: {_model}, Yıl: {_year}, Yakıt: {_fuel}");
    }
}
```

### Object Initializer
```csharp
Car car1 = new Car() 
{ 
    Model = "A6", 
    Year = 2025, 
    Fuel = 20, 
    Color = "Red" 
};

Car car2 = new Car("BMW", 40) 
{ 
    Year = 2025,
    Model = "X5" 
};
```

### Access Modifiers (Erişim Belirleyiciler)
```csharp
public class Person
{
    public string Name;           // Her yerden erişilebilir
    private int _age;             // Sadece class içinden
    protected string _id;         // Class ve türetilmiş classlardan
    internal string Company;      // Aynı assembly içinden
    protected internal string Dept; // Aynı assembly veya türetilmiş class
    private protected string Code;  // Aynı assembly içinde türetilmiş class
}
```

### Encapsulation Örneği
```csharp
public class Student
{
    // Private fields
    private int _id;
    private string _firstName;
    private string _lastName;
    private ICollection<int> _examScores;
    
    // Constructor
    public Student(int id, string firstName, string lastName)
    {
        Id = id;
        FirstName = firstName;
        LastName = lastName;
        _examScores = new List<int>();
    }
    
    // Read-only property
    public int Id
    {
        get { return _id; }
        private set { _id = value; }
    }
    
    // Computed property
    public string FullName => _firstName + " " + _lastName;
    
    // Average score property
    public double AverageScore => _examScores.Count == 0 ? 0 : _examScores.Average();
    
    // Method with validation
    public void AddExamScore(int score)
    {
        if (score < 0 || score > 100)
            throw new ArgumentException("Not 0-100 arasında olmalı!");
        
        _examScores.Add(score);
    }
    
    // Return read-only collection
    public IReadOnlyList<int> GetExamScores()
    {
        return _examScores.ToList().AsReadOnly();
    }
}
```

### Product Class Örneği
```csharp
public class Product
{
    private double _price;
    
    public Product(string name, double price)
    {
        Name = name;
        Price = price;
    }
    
    public Product(string name, double price, string category) 
        : this(name, price)
    {
        Category = category;
    }
    
    public string Name { get; private set; }
    
    public double Price
    {
        get => _price;
        private set
        {
            if (value < 0)
                throw new ArgumentException("Fiyat negatif olamaz!");
            _price = value;
        }
    }
    
    public string Category { get; set; }
    
    public string DisplayInfo()
    {
        return $"Ürün: {Name} | Kategori: {Category} | Fiyat: {Price:C2}";
    }
}
```

### Order Class Örneği
```csharp
public class Order
{
    private ICollection<Product> _products;
    
    public Order(int orderId)
    {
        OrderId = orderId;
        OrderDate = DateTime.Now;
        _products = new HashSet<Product>();
    }
    
    public int OrderId { get; private set; }
    public DateTime OrderDate { get; private set; }
    
    // Read-only collection property
    public IReadOnlyList<Product> Products => _products.ToList().AsReadOnly();
    
    public void AddProduct(Product product)
    {
        _products.Add(product);
    }
    
    public void RemoveProduct(Product product)
    {
        _products.Remove(product);
    }
    
    // LINQ kullanımı
    public double CalculateTotal() => _products.Sum(p => p.Price);
    
    public string DisplayOrderSummary()
    {
        string summary = $"Sipariş No: {OrderId} | Tarih: {OrderDate}\n";
        foreach (var item in _products)
        {
            summary += item.DisplayInfo() + "\n";
        }
        summary += $"Toplam: {CalculateTotal():C2}";
        return summary;
    }
}
```

---

## 12. OOP - KALITIM (INHERITANCE)

### Temel Kalıtım
```csharp
// Base Class (Parent/Super Class)
public class Phone
{
    private string _brand;
    protected string _connectionType = "Kablolu";
    
    public Phone() { }
    
    public Phone(string brand)
    {
        Brand = brand;
    }
    
    public string Brand
    {
        get { return _brand; }
        private set { _brand = value; }
    }
    
    public string ConnectionType
    {
        get { return _connectionType; }
    }
    
    // Virtual method - Override edilebilir
    public virtual string Call()
    {
        return "Arama yapılıyor... Did did did...";
    }
    
    public override string ToString()
    {
        return $"Marka: {Brand}, Bağlantı: {ConnectionType}";
    }
}

// Derived Class (Child/Sub Class)
public class MobilePhone : Phone
{
    public bool HasCamera { get; set; }
    public bool IsTouched { get; set; }
    
    public MobilePhone()
    {
        _connectionType = "3G"; // Protected field'a erişim
    }
    
    public MobilePhone(string brand) : base(brand) // Base constructor çağırma
    {
        _connectionType = "3G";
    }
    
    public string TakePhoto()
    {
        if (HasCamera)
            return "Fotoğraf çekebilir";
        else
            return "Fotoğraf çekemez";
    }
    
    // Method Override
    public override string Call()
    {
        return "Arama yapılıyor... Cendere cendere...";
    }
    
    public override string ToString()
    {
        return base.ToString() + $", Kamera: {HasCamera}, Dokunmatik: {IsTouched}";
    }
}

// Multilevel Inheritance
public class SmartPhone : MobilePhone
{
    public bool FrontCam { get; set; }
    
    public SmartPhone()
    {
        _connectionType = "5G";
    }
    
    public SmartPhone(string brand) : base(brand)
    {
        _connectionType = "5G";
    }
    
    public string DoVideoCall()
    {
        if (FrontCam)
            return "Görüntülü arama yapılabilir";
        else
            return "Görüntülü arama desteklenmiyor";
    }
    
    // Sealed method - Daha fazla override edilemez
    public sealed override string Call()
    {
        return "HD ses ile arama yapılıyor...";
    }
    
    // Method Hiding (new keyword)
    public new string TakePhoto()
    {
        return "AI destekli fotoğraf çekiliyor";
    }
}
```

### Polymorphism (Çok Biçimlilik)
```csharp
// Base referans ile türetilmiş nesneleri tutma
Phone phone1 = new Phone("AEG");
Phone phone2 = new MobilePhone("Nokia");
Phone phone3 = new SmartPhone("Apple");

// Her biri kendi Call metodunu çağırır
Console.WriteLine(phone1.Call()); // Phone.Call()
Console.WriteLine(phone2.Call()); // MobilePhone.Call()
Console.WriteLine(phone3.Call()); // SmartPhone.Call()

// Liste ile polymorphism
List<Phone> phones = new List<Phone>
{
    new Phone("AEG"),
    new MobilePhone("Nokia"),
    new SmartPhone("Apple")
};

foreach (Phone phone in phones)
{
    Console.WriteLine(phone.Call()); // Runtime'da doğru metot çağrılır
}
```

### Kalıtım Örneği - Üyelik Sistemi
```csharp
// Base Member Class
public class BaseMember
{
    private string _firstName;
    private string _lastName;
    private DateTime _joinedAt;
    protected decimal _price = 5000;
    
    protected BaseMember(string firstName, string lastName, DateTime joinedAt)
    {
        FirstName = firstName;
        LastName = lastName;
        JoinedAt = joinedAt;
    }
    
    public string FirstName
    {
        get { return _firstName; }
        set
        {
            if (string.IsNullOrEmpty(value))
                throw new ValidationException("İsim boş olamaz");
            _firstName = value;
        }
    }
    
    public string LastName
    {
        get { return _lastName; }
        set
        {
            if (string.IsNullOrEmpty(value))
                throw new ValidationException("Soyisim boş olamaz");
            _lastName = value;
        }
    }
    
    public DateTime JoinedAt
    {
        get { return _joinedAt; }
        set
        {
            if (value > DateTime.Now)
                _joinedAt = value;
            else
                throw new ValidationException("Geçersiz tarih");
        }
    }
    
    public string FullName => $"{_firstName} {_lastName.ToUpper()}";
    
    public virtual decimal MembershipFee(int month)
    {
        return _price * month;
    }
    
    public override string ToString()
    {
        return $"Üye: {FullName}, Kayıt: {JoinedAt:dd.MM.yyyy}";
    }
}

// VIP Member
public class VipMember : BaseMember
{
    public string Coach { get; private set; }
    
    public VipMember(string firstName, string lastName, DateTime joinedAt, string coach)
        : base(firstName, lastName, joinedAt)
    {
        Coach = coach;
        _price = 7500; // Base fiyatı değiştir
    }
    
    public override decimal MembershipFee(int month)
    {
        return base.MembershipFee(month) + 10000; // Ekstra ücret
    }
    
    public override string ToString()
    {
        return base.ToString() + $", Koç: {Coach}, Ücret: {MembershipFee(12):C2}";
    }
}

// Standard Member
public class StandardMember : BaseMember
{
    public bool Kit { get; set; }
    
    public StandardMember(string firstName, string lastName, DateTime joinedAt)
        : base(firstName, lastName, joinedAt)
    {
    }
    
    public override decimal MembershipFee(int month)
    {
        var price = base.MembershipFee(month);
        return Kit ? price + 3000 : price;
    }
    
    public override string ToString()
    {
        return base.ToString() + $", Kit: {(Kit ? "Var" : "Yok")}, Ücret: {MembershipFee(12):C2}";
    }
}
```

### Okul Yönetim Sistemi Örneği
```csharp
public class Person
{
    private static int _studentCounter = 100;
    private static int _teacherCounter = 300;
    private static int _adminCounter = 600;
    
    protected Person(string firstName, string lastName, string email)
    {
        FirstName = firstName;
        LastName = lastName;
        Email = email;
        Id = GetNextId(this);
    }
    
    public int Id { get; private set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
    
    protected static int GetNextId(Person person)
    {
        return person switch
        {
            Student => _studentCounter++,
            Teacher => _teacherCounter++,
            Administrator => _adminCounter++,
            _ => 0
        };
    }
    
    public override string ToString()
    {
        return $"{Id} - {FirstName} {LastName} ({Email})";
    }
}

public class Student : Person
{
    public IList<int> Grades { get; private set; } = new List<int>();
    public string Number { get; set; } = Guid.NewGuid().ToString().Substring(0, 4);
    
    public Student(string firstName, string lastName, string email)
        : base(firstName, lastName, email)
    {
    }
    
    public void AddGrade(int score)
    {
        if (score >= 0 && score <= 100)
            Grades.Add(score);
        else
            throw new Exception("Not 0-100 arasında olmalı!");
    }
    
    public double CalculateAverage()
    {
        return Grades.Count == 0 ? 0 : Grades.Average();
    }
    
    public override string ToString()
    {
        return base.ToString() + $" | Ortalama: {CalculateAverage():F2}";
    }
}

public class Teacher : Person
{
    public string Branch { get; set; }
    public int ExperienceYear { get; set; }
    
    public Teacher(string firstName, string lastName, string email, string branch, int experienceYear)
        : base(firstName, lastName, email)
    {
        Branch = branch;
        ExperienceYear = experienceYear;
    }
    
    public override string ToString()
    {
        return base.ToString() + $" | Branş: {Branch}, Deneyim: {ExperienceYear} yıl";
    }
}

public class Administrator : Person
{
    public string Role { get; set; }
    public List<Person> ManagedPeople { get; private set; } = new();
    
    public Administrator(string firstName, string lastName, string email, string role)
        : base(firstName, lastName, email)
    {
        Role = role;
    }
    
    public void AddPerson(Person p)
    {
        if (p is not Student)
            ManagedPeople.Add(p);
        else
            throw new Exception("Öğrenci yönetilemez!");
    }
    
    public override string ToString()
    {
        return base.ToString() + $" | Rol: {Role}, Yönetilen: {ManagedPeople.Count} kişi";
    }
}
```

---

## 13. OOP - ABSTRACT SINIFLAR

### Abstract Class Tanımlama
```csharp
// Abstract class - Instance alınamaz, sadece base class olarak kullanılır
public abstract class MuzikAleti
{
    private string _marka;
    private string _aciklama;
    
    // Abstract class constructor'ı olabilir
    protected MuzikAleti(string marka, string aciklama)
    {
        Marka = marka;
        Aciklama = aciklama;
    }
    
    public string Marka
    {
        get { return _marka; }
        set { _marka = value; }
    }
    
    public string Aciklama
    {
        get { return _aciklama; }
        set { _aciklama = value; }
    }
    
    // Concrete method - Gövdesi olan metot
    public string BilgiVer()
    {
        return $"Marka: {Marka} - Açıklama: {Aciklama}";
    }
    
    // Abstract method - Türetilen sınıfta override edilmek ZORUNDA
    public abstract string Cal();
    
    // Abstract metot kuralları:
    // 1. Private olamaz
    // 2. Sadece abstract class içinde tanımlanabilir
    // 3. Virtual olamaz (zaten doğal virtual)
    // 4. Static olamaz
    // 5. Gövdesi olamaz
}

// Concrete class - Abstract class'tan türetilmiş
public class Gitar : MuzikAleti
{
    public Gitar(string marka, string aciklama) : base(marka, aciklama)
    {
    }
    
    // Abstract metodu override etmek ZORUNLU
    public override string Cal()
    {
        return "Gitar sesi... 🎸";
    }
}

public class Bateri : MuzikAleti
{
    public Bateri(string marka, string aciklama) : base(marka, aciklama)
    {
    }
    
    public override string Cal()
    {
        return "Bateri sesi... 🥁";
    }
}

public class Flut : MuzikAleti
{
    public Flut(string marka, string aciklama) : base(marka, aciklama)
    {
    }
    
    public override string Cal()
    {
        return "Flüt sesi... 🎵";
    }
}
```

### Abstract Class Kullanımı
```csharp
// Abstract class'tan instance alınamaz
// MuzikAleti muzikAleti = new MuzikAleti(); // HATA!

// Ama referans olarak kullanılabilir (Polymorphism)
MuzikAleti gitar = new Gitar("Yamaha", "Profesyonel");
MuzikAleti bateri = new Bateri("Marshall", "Tam profesyonel");

Console.WriteLine(gitar.BilgiVer());
Console.WriteLine(gitar.Cal());

// Liste ile kullanım
List<MuzikAleti> enstrumanlar = new List<MuzikAleti>
{
    new Gitar("Fender", "Elektro"),
    new Bateri("Pearl", "Akustik"),
    new Flut("Yamaha", "Ahşap")
};

foreach (var enstruman in enstrumanlar)
{
    Console.WriteLine(enstruman.BilgiVer());
    Console.WriteLine(enstruman.Cal());
}
```

### Banka Hesap Sistemi Örneği
```csharp
public abstract class BankAccount
{
    protected BankAccount(string accountNumber, string accountName)
    {
        AccountNumber = accountNumber;
        AccountName = accountName;
        Balance = 0;
    }
    
    public string AccountNumber { get; private set; }
    public string AccountName { get; private set; }
    public double Balance { get; protected set; }
    
    // Abstract methods - Her hesap türü kendi mantığını uygular
    public abstract void Deposit(double amount);
    public abstract void Withdraw(double amount);
    
    public override string ToString()
    {
        return $"Hesap No: {AccountNumber}, Sahip: {AccountName}, Bakiye: {Balance:C2}";
    }
}

// Tasarruf Hesabı
public class SavingsAccount : BankAccount
{
    private double _interestRate = 0.03; // %3 faiz
    
    public SavingsAccount(string accountNumber, string accountName)
        : base(accountNumber, accountName)
    {
    }
    
    public override void Deposit(double amount)
    {
        Balance += amount;
        Console.WriteLine($"{amount:C2} yatırıldı. Yeni bakiye: {Balance:C2}");
        AddInterest(); // Para yatırıldığında faiz ekle
    }
    
    public override void Withdraw(double amount)
    {
        if (Balance - amount >= 0)
        {
            Balance -= amount;
            Console.WriteLine($"{amount:C2} çekildi. Yeni bakiye: {Balance:C2}");
        }
        else
        {
            Console.WriteLine("Yetersiz bakiye!");
        }
    }
    
    private void AddInterest()
    {
        Balance += Balance * _interestRate;
        Console.WriteLine($"Faiz eklendi. Yeni bakiye: {Balance:C2}");
    }
}

// Vadesiz Hesap
public class CurrentAccount : BankAccount
{
    private double _overdraftLimit = 5000; // Eksi bakiye limiti
    
    public CurrentAccount(string accountNumber, string accountName)
        : base(accountNumber, accountName)
    {
    }
    
    public override void Deposit(double amount)
    {
        Balance += amount;
        Console.WriteLine($"{amount:C2} yatırıldı. Yeni bakiye: {Balance:C2}");
    }
    
    public override void Withdraw(double amount)
    {
        if (Balance - amount >= -_overdraftLimit)
        {
            Balance -= amount;
            Console.WriteLine($"{amount:C2} çekildi. Yeni bakiye: {Balance:C2}");
            
            if (Balance < 0)
                Console.WriteLine($"DİKKAT: Eksi bakiyedesiniz!");
        }
        else
        {
            Console.WriteLine($"İşlem limiti aşıyor! Max çekilebilir: {Balance + _overdraftLimit:C2}");
        }
    }
}
```

---

## 14. OOP - INTERFACE

### Interface Tanımlama
```csharp
// Interface - Sadece sözleşme, implementasyon yok
public interface IFutbolcu
{
    // Properties
    string Name { get; set; }
    int Numarasi { get; set; }
    int PasGucu { get; set; }
    int SutGucu { get; set; }
    int KosuGucu { get; set; }
    
    // Methods - Gövde yok, sadece imza
    void PasAt();
    void SutCek();
    void Kos();
}

// İkinci interface
public interface IKaleci
{
    void TopKurtar();
}

// Interface implementation
public class Forvet : IFutbolcu
{
    // Interface'deki TÜM üyeleri implement etmek ZORUNLU
    public string Name { get; set; }
    public int Numarasi { get; set; }
    public int PasGucu { get; set; }
    public int SutGucu { get; set; }
    public int KosuGucu { get; set; }
    
    public void Kos()
    {
        Console.WriteLine("Forvet koşuyor...");
    }
    
    public void PasAt()
    {
        Console.WriteLine("Forvet pas atıyor...");
    }
    
    public void SutCek()
    {
        Console.WriteLine("Forvet şut çekiyor... GOL!");
    }
}

// Multiple Interface Implementation
public class Kaleci : IFutbolcu, IKaleci
{
    public string Name { get; set; }
    public int Numarasi { get; set; }
    public int PasGucu { get; set; }
    public int SutGucu { get; set; }
    public int KosuGucu { get; set; }
    
    public void Kos()
    {
        Console.WriteLine("Kaleci kaleye doğru koşuyor...");
    }
    
    public void PasAt()
    {
        Console.WriteLine("Kaleci uzun pas atıyor...");
    }
    
    public void SutCek()
    {
        Console.WriteLine("Kaleci degaj yapıyor...");
    }
    
    // IKaleci interface'inden
    public void TopKurtar()
    {
        Console.WriteLine("Kaleci topu kurtardı!");
    }
}
```

### Interface ile Polymorphism
```csharp
// Interface referansı ile farklı implementasyonları tutma
List<IFutbolcu> takim = new List<IFutbolcu>
{
    new Forvet { Name = "Osimhen", Numarasi = 45 },
    new Defans { Name = "Sanchez", Numarasi = 5 },
    new Kaleci { Name = "Uğurcan", Numarasi = 1 }
};

foreach (IFutbolcu oyuncu in takim)
{
    Console.WriteLine($"{oyuncu.Name} ({oyuncu.Numarasi})");
    oyuncu.PasAt();
    oyuncu.SutCek();
}
```

### Database Interface Örneği
```csharp
// Database interface - Farklı veritabanları için ortak sözleşme
public interface IDatabase
{
    void Create(string name, decimal price, int stock);
    void Update(int id, string name, decimal price);
    void Delete(int id);
    Product GetById(int id);
    List<Product> GetAll();
}

// SQL Server Implementation
public class SqlDatabase : IDatabase
{
    public void Create(string name, decimal price, int stock)
    {
        Console.WriteLine($"SQL Server'a eklendi: {name}");
        // SQL Server specific code
    }
    
    public void Delete(int id)
    {
        Console.WriteLine($"SQL Server'dan silindi: {id}");
    }
    
    public Product GetById(int id)
    {
        // SQL Server query
        return new Product();
    }
    
    public List<Product> GetAll()
    {
        // SQL Server query
        return new List<Product>();
    }
    
    public void Update(int id, string name, decimal price)
    {
        Console.WriteLine($"SQL Server'da güncellendi: {id}");
    }
}

// MySQL Implementation
public class MySqlDatabase : IDatabase
{
    public void Create(string name, decimal price, int stock)
    {
        Console.WriteLine($"MySQL'e eklendi: {name}");
        // MySQL specific code
    }
    
    public void Delete(int id)
    {
        Console.WriteLine($"MySQL'den silindi: {id}");
    }
    
    public Product GetById(int id)
    {
        // MySQL query
        return new Product();
    }
    
    public List<Product> GetAll()
    {
        // MySQL query
        return new List<Product>();
    }
    
    public void Update(int id, string name, decimal price)
    {
        Console.WriteLine($"MySQL'de güncellendi: {id}");
    }
}

// Kullanım - Dependency Injection mantığı
public class ProductService
{
    private readonly IDatabase _database;
    
    public ProductService(IDatabase database)
    {
        _database = database;
    }
    
    public void AddProduct(string name, decimal price, int stock)
    {
        _database.Create(name, price, stock);
    }
}

// Test
IDatabase database = new MySqlDatabase(); // İstediğimiz zaman değiştirebiliriz
ProductService service = new ProductService(database);
service.AddProduct("Laptop", 15000, 10);
```

### Ödeme Sistemi Örneği
```csharp
// Payment interface
public interface IPayment
{
    void MakePayment();
    void CancelPayment();
}

// Base abstract class
public abstract class BasePayment : IPayment
{
    private decimal _amount;
    
    protected BasePayment(decimal amount)
    {
        Amount = amount;
    }
    
    public decimal Amount
    {
        get { return _amount; }
        set
        {
            if (value <= 0)
                throw new ArgumentException("Miktar 0'dan büyük olmalı!");
            _amount = value;
        }
    }
    
    public abstract void CancelPayment();
    public abstract void MakePayment();
}

// Credit Card Payment
public class CreditCardPayment : BasePayment
{
    public string CardNumber { get; set; }
    public string CVV { get; set; }
    
    public CreditCardPayment(decimal amount, string cardNumber, string cvv)
        : base(amount)
    {
        CardNumber = cardNumber;
        CVV = cvv;
    }
    
    public override void MakePayment()
    {
        Console.WriteLine($"Kredi kartı ile {Amount:C2} ödeme yapıldı.");
        Console.WriteLine($"Kart: {CardNumber.Substring(0, 4)}****");
    }
    
    public override void CancelPayment()
    {
        Console.WriteLine($"Kredi kartı ödemesi iptal edildi: {Amount:C2}");
    }
}

// Cash Payment
public class CashPayment : BasePayment
{
    public CashPayment(decimal amount) : base(amount)
    {
    }
    
    public override void MakePayment()
    {
        Console.WriteLine($"Nakit {Amount:C2} ödeme alındı.");
    }
    
    public override void CancelPayment()
    {
        Console.WriteLine($"Nakit ödeme iadesi: {Amount:C2}");
    }
}
```

### Abstract Class vs Interface Karşılaştırma

| Özellik | Abstract Class | Interface |
|---------|---------------|-----------|
| **Çoklu Kalıtım** | Desteklemez (tek class) | Destekler (çoklu interface) |
| **Constructor** | Olabilir | Olamaz |
| **Field** | Olabilir | Olamaz (C# 8'e kadar) |
| **Access Modifier** | Her türlü olabilir | Public (default) |
| **Implementasyon** | Concrete ve abstract metotlar | Sadece imza (C# 8'e kadar) |
| **Ne zaman kullanılır** | IS-A ilişkisi, ortak kod | CAN-DO ilişkisi, sözleşme |

---

## 15. OOP - GENERIC

### Generic Class
```csharp
// Generic class tanımlama
public class Box<T>
{
    private T item;
    
    public void AddItem(T value)
    {
        item = value;
    }
    
    public T GetItem()
    {
        return item;
    }
}

// Kullanım
Box<int> intBox = new Box<int>();
intBox.AddItem(42);
int value = intBox.GetItem();

Box<string> stringBox = new Box<string>();
stringBox.AddItem("Hello");
string text = stringBox.GetItem();

Box<Product> productBox = new Box<Product>();
productBox.AddItem(new Product("Laptop", 15000));
Product product = productBox.GetItem();
```

### Generic Constraints (Kısıtlamalar)
```csharp
// where T : class -> T referans tip olmalı
public class Repository<T> where T : class
{
    private List<T> items = new List<T>();
    
    public void Add(T item)
    {
        items.Add(item);
    }
}

// where T : struct -> T value tip olmalı
public class ValueContainer<T> where T : struct
{
    public T Value { get; set; }
}

// where T : new() -> T parametresiz constructor'a sahip olmalı
public class Factory<T> where T : new()
{
    public T CreateInstance()
    {
        return new T();
    }
}

// where T : BaseClass -> T, BaseClass'tan türemeli
public class AnimalContainer<T> where T : Animal
{
    public void Feed(T animal)
    {
        animal.Eat();
    }
}

// where T : IInterface -> T, IInterface'i implement etmeli
public class Processor<T> where T : IProcessable
{
    public void Process(T item)
    {
        item.Process();
    }
}

// Birden fazla constraint
public class ComplexContainer<T> where T : class, IComparable<T>, new()
{
    // T hem class olmalı, hem IComparable implement etmeli, hem de new() olmalı
}
```

### Generic Interface
```csharp
public interface IRepository<T> where T : class
{
    void Add(T entity);
    void Update(T entity);
    void Delete(int id);
    T GetById(int id);
    List<T> GetAll();
}

// Implementation
public class ProductRepository : IRepository<Product>
{
    private List<Product> products = new List<Product>();
    
    public void Add(Product entity)
    {
        products.Add(entity);
    }
    
    public void Delete(int id)
    {
        products.RemoveAll(p => p.Id == id);
    }
    
    public Product GetById(int id)
    {
        return products.FirstOrDefault(p => p.Id == id);
    }
    
    public List<Product> GetAll()
    {
        return products;
    }
    
    public void Update(Product entity)
    {
        var existing = GetById(entity.Id);
        if (existing != null)
        {
            existing.Name = entity.Name;
            existing.Price = entity.Price;
        }
    }
}
```

### Inventory Management Örneği
```csharp
// Base Product class
public abstract class Product
{
    public Product(string name, decimal price, int quantity)
    {
        Name = name;
        Price = price;
        Quantity = quantity;
    }
    
    public string Name { get; set; }
    public decimal Price { get; set; }
    public int Quantity { get; set; }
    
    public virtual void IncreaseQuantity(int amount)
    {
        Quantity += amount;
        Console.WriteLine($"Stok güncellendi: {Quantity} adet");
    }
    
    public virtual void DecreaseQuantity(int amount)
    {
        Quantity -= amount;
        Console.WriteLine($"Stok güncellendi: {Quantity} adet");
    }
    
    public override string ToString()
    {
        return $"Ürün: {Name}, Fiyat: {Price:C2}, Stok: {Quantity}";
    }
}

// Derived classes
public class ElectronicProduct : Product
{
    public int WarrantyPeriod { get; set; }
    
    public ElectronicProduct(string name, decimal price, int quantity, int warrantyPeriod)
        : base(name, price, quantity)
    {
        WarrantyPeriod = warrantyPeriod;
    }
    
    public override void DecreaseQuantity(int amount)
    {
        if (Quantity - amount >= 0)
            base.DecreaseQuantity(amount);
        else
            Console.WriteLine("Yetersiz stok!");
    }
}

public class FoodProduct : Product
{
    public DateTime ExpiryDate { get; set; }
    
    public FoodProduct(string name, decimal price, int quantity)
        : base(name, price, quantity)
    {
        ExpiryDate = DateTime.Now.AddDays(30);
    }
}

// Generic Inventory Management Interface
public interface IInventoryManagement<T> where T : Product
{
    void Add(T item);
    void Remove(T item);
    List<T> GetAll();
    void Decrease(T item, int amount);
    void Increase(T item, int amount);
}

// Implementation
public class InventoryManagement<T> : IInventoryManagement<T> where T : Product
{
    private List<T> products = new List<T>();
    
    public void Add(T item)
    {
        products.Add(item);
        Console.WriteLine($"{item.Name} envantere eklendi.");
    }
    
    public void Remove(T item)
    {
        products.Remove(item);
        Console.WriteLine($"{item.Name} envanterden çıkarıldı.");
    }
    
    public List<T> GetAll()
    {
        return products;
    }
    
    public void Decrease(T item, int amount)
    {
        item.DecreaseQuantity(amount);
    }
    
    public void Increase(T item, int amount)
    {
        item.IncreaseQuantity(amount);
    }
}

// Kullanım
IInventoryManagement<ElectronicProduct> electronics = 
    new InventoryManagement<ElectronicProduct>();

electronics.Add(new ElectronicProduct("Laptop", 50000, 100, 2));
electronics.Add(new ElectronicProduct("Phone", 15000, 150, 1));

IInventoryManagement<FoodProduct> foods = 
    new InventoryManagement<FoodProduct>();

foods.Add(new FoodProduct("Apple", 50, 1000));
foods.Add(new FoodProduct("Bread", 10, 500));
```

### Generic Methods
```csharp
public class Utility
{
    // Generic method
    public T GetMax<T>(T item1, T item2) where T : IComparable<T>
    {
        return item1.CompareTo(item2) > 0 ? item1 : item2;
    }
    
    // Generic swap method
    public void Swap<T>(ref T item1, ref T item2)
    {
        T temp = item1;
        item1 = item2;
        item2 = temp;
    }
    
    // Generic list converter
    public List<TOutput> ConvertList<TInput, TOutput>(
        List<TInput> inputList, 
        Func<TInput, TOutput> converter)
    {
        List<TOutput> outputList = new List<TOutput>();
        foreach (TInput item in inputList)
        {
            outputList.Add(converter(item));
        }
        return outputList;
    }
}
```

---

## 16. OOP İLİŞKİLER

### 1. HAS-A (Sahiplik İlişkisi) - Composition & Aggregation

#### Aggregation (Gevşek Sahiplik)
```csharp
// Library HAS Books (Library yok olsa da Books var olabilir)
public class Author
{
    public Author(string name, string nationality)
    {
        Name = name;
        Nationality = nationality;
    }
    
    public string Name { get; set; }
    public string Nationality { get; set; }
    public List<Book> Books { get; set; } = new List<Book>();
}

public class Book
{
    public Book(string title, Author author)
    {
        Title = title;
        Author = author;
    }
    
    public string Title { get; set; }
    public Author Author { get; set; }
    public Library Library { get; set; }
}

public class Library
{
    public string Name { get; set; }
    public List<Book> Books { get; set; }
    
    public Library(string name)
    {
        Name = name;
        Books = new List<Book>();
    }
}
```

#### Composition (Sıkı Sahiplik)
```csharp
// Car HAS Engine (Car yok olursa Engine de yok olur)
public class Engine
{
    public Engine(string model, int horsePower)
    {
        Model = model;
        HorsePower = horsePower;
    }
    
    public string Model { get; set; }
    public int HorsePower { get; set; }
    
    public void Start()
    {
        Console.WriteLine($"Motor çalışıyor: {Model} ({HorsePower} HP)");
    }
}

public class MusicSystem
{
    public MusicSystem(string brand)
    {
        Brand = brand;
    }
    
    public string Brand { get; set; }
    
    public void PlayMusic()
    {
        Console.WriteLine($"{Brand} müzik sistemi çalıyor.");
    }
}

public class Car
{
    public Car(string brand, Engine engine)
    {
        Brand = brand;
        Engine = engine; // Composition - Zorunlu
    }
    
    public string Brand { get; set; }
    public Engine Engine { get; set; }           // Composition (Zorunlu)
    public MusicSystem MusicSystem { get; set; } // Aggregation (Opsiyonel)
    
    public void StartCar()
    {
        Console.WriteLine($"{Brand} çalıştırılıyor...");
        Engine.Start();
        MusicSystem?.PlayMusic(); // Null-safe
    }
}
```

### 2. IS-A (Kalıtım İlişkisi)
```csharp
// Developer IS-A Employee, Manager IS-A Employee
public class Employee
{
    public Employee(string name, decimal salary)
    {
        Name = name;
        Salary = salary;
    }
    
    public string Name { get; set; }
    public decimal Salary { get; set; }
    
    public override string ToString()
    {
        return $"Çalışan: {Name}, Maaş: {Salary:C2}";
    }
}

public class Developer : Employee // IS-A
{
    public List<string> ProgrammingLanguages { get; set; }
    
    public Developer(string name, decimal salary, List<string> languages)
        : base(name, salary)
    {
        ProgrammingLanguages = languages;
    }
    
    public override string ToString()
    {
        return base.ToString() + 
               $"\nDiller: {string.Join(", ", ProgrammingLanguages)}";
    }
}

public class Manager : Employee // IS-A
{
    public List<Employee> Team { get; set; }
    
    public Manager(string name, decimal salary) : base(name, salary)
    {
        Team = new List<Employee>();
    }
    
    public void AddToTeam(Employee employee)
    {
        Team.Add(employee);
        Console.WriteLine($"{employee.Name} takıma eklendi.");
    }
    
    public override string ToString()
    {
        string members = "";
        foreach (Employee emp in Team)
            members += "\n  - " + emp.Name;
        
        return base.ToString() + 
               $"\nTakım ({Team.Count} kişi): {members}";
    }
}
```

### 3. USE-A (Kullanma İlişkisi) - Dependency
```csharp
// Order USES PaymentProcessor (Geçici ilişki)
public class Customer
{
    public Customer(string name, string email)
    {
        Name = name;
        Email = email;
    }
    
    public string Name { get; set; }
    public string Email { get; set; }
}

public class Order
{
    public Order(int orderId, Customer customer, decimal totalAmount)
    {
        OrderId = orderId;
        Customer = customer;
        TotalAmount = totalAmount;
    }
    
    public int OrderId { get; set; }
    public Customer Customer { get; set; }
    public decimal TotalAmount { get; set; }
    
    // USE-A relationship - PaymentProcessor'ı kullanıyor
    public void ProcessPayment(PaymentProcessor processor)
    {
        Console.WriteLine($"Sipariş #{OrderId} ödeme işlemi başlatılıyor...");
        processor.ProcessPayment(this);
    }
}

public class PaymentProcessor
{
    public string ProviderName { get; set; }
    
    public PaymentProcessor(string providerName)
    {
        ProviderName = providerName;
    }
    
    public void ProcessPayment(Order order)
    {
        Console.WriteLine($"{ProviderName} ile {order.TotalAmount:C2} ödeme alındı.");
        Console.WriteLine($"Müşteri: {order.Customer.Name}");
        // Email gönder
        SendConfirmationEmail(order.Customer.Email);
    }
    
    private void SendConfirmationEmail(string email)
    {
        Console.WriteLine($"Onay e-postası gönderildi: {email}");
    }
}
```

### İlişki Türleri Özet

| İlişki Türü | Açıklama | Örnek | Bağımlılık |
|-------------|----------|-------|------------|
| **IS-A** | Kalıtım | Developer IS-A Employee | Güçlü |
| **HAS-A (Composition)** | Sahiplik (Zorunlu) | Car HAS Engine | Çok Güçlü |
| **HAS-A (Aggregation)** | Sahiplik (Opsiyonel) | Car HAS MusicSystem | Orta |
| **USE-A** | Kullanma | Order USES PaymentProcessor | Zayıf |

---

## 17. STRUCT VE ENUM

### Struct (Yapı)
```csharp
// Struct - Value Type (Değer tipi)
public struct Color
{
    public byte Red { get; set; }
    public byte Green { get; set; }
    public byte Blue { get; set; }
    
    // Struct'ta da constructor olabilir
    public Color(byte red, byte green, byte blue)
    {
        Red = red;
        Green = green;
        Blue = blue;
    }
    
    public void GetColor()
    {
        Console.WriteLine($"RGB({Red}, {Green}, {Blue})");
    }
}

// Currency struct örneği
public struct Currency
{
    public Currency(decimal amount, string symbol = "₺")
    {
        Amount = amount;
        Symbol = symbol;
    }
    
    public decimal Amount { get; set; }
    public string Symbol { get; set; }
    
    public string GetCurrency()
    {
        if (Symbol == "₺")
            return $"{Amount:F2}{Symbol}";
        else
            return $"{Symbol}{Amount:F2}";
    }
}

// Kullanım
Color color = new Color(255, 128, 64);
color.GetColor();

Currency price = new Currency(150.50m, "$");
Console.WriteLine(price.GetCurrency());
```

### Struct vs Class Karşılaştırma

| Özellik | Class | Struct |
|---------|-------|--------|
| **Tip** | Reference Type | Value Type |
| **Bellekte** | Heap | Stack |
| **Inheritance** | Destekler | Desteklemez |
| **Constructor** | Default + Custom | Custom only |
| **Null olabilir** | Evet | Hayır (nullable hariç) |
| **Performans** | Yavaş (heap allocation) | Hızlı (stack allocation) |

### Enum (Numaralandırma)
```csharp
// Basit enum
public enum OrderStatus
{
    Pending = 101,     // Manuel değer atama
    Processing,        // 102 (otomatik)
    Shipped = 200,     // Manuel değer
    Delivered,         // 201 (otomatik)
    Cancelled          // 202 (otomatik)
}

// Kullanım
OrderStatus status = OrderStatus.Processing;
Console.WriteLine(status);              // Processing
Console.WriteLine((int)status);        // 102
Console.WriteLine((OrderStatus)200);   // Shipped

// Order class'ta kullanım
public class Order
{
    public int OrderId { get; set; }
    public string Name { get; set; }
    public OrderStatus Status { get; set; }
    
    public void Detail()
    {
        Console.WriteLine($"Sipariş #{OrderId}");
        Console.WriteLine($"Müşteri: {Name}");
        Console.WriteLine($"Durum: {Status} ({(int)Status})");
    }
}
```

### Flags Enum
```csharp
[Flags]
public enum UserRole
{
    None = 0,
    Admin = 1 << 0,    // 1  (0001)
    Editor = 1 << 1,   // 2  (0010)
    Viewer = 1 << 2,   // 4  (0100)
    Manager = 1 << 3   // 8  (1000)
}

// Kullanım - Birden fazla rol atama
UserRole roles = UserRole.Admin | UserRole.Editor;  // 3 (0011)
Console.WriteLine(roles);  // Admin, Editor

// Rol kontrolü
if ((roles & UserRole.Admin) == UserRole.Admin)
{
    Console.WriteLine("Admin yetkisi var");
}

// Rol ekleme
roles |= UserRole.Manager;

// Rol çıkarma
roles &= ~UserRole.Editor;

// Tüm rolleri kontrol
if (roles.HasFlag(UserRole.Admin))
{
    Console.WriteLine("Admin rolü var");
}
```

### WorkDays Enum Örneği
```csharp
[Flags]
public enum WorkDays
{
    None = 0,
    Monday = 1,
    Tuesday = 2,
    Wednesday = 4,
    Thursday = 8,
    Friday = 16,
    Saturday = 32,
    Sunday = 64,
    Weekdays = Monday | Tuesday | Wednesday | Thursday | Friday,
    Weekend = Saturday | Sunday,
    AllDays = Weekdays | Weekend
}

public class Schedule
{
    public WorkDays WorkDays { get; set; }
    
    public void PrintSchedule()
    {
        Console.WriteLine($"Çalışma günleri: {WorkDays}");
        
        if ((WorkDays & WorkDays.Weekend) != 0)
            Console.WriteLine("Hafta sonu çalışıyor!");
    }
}
```

---

## 18. EXTENSION METHODS

### Extension Method Tanımlama
```csharp
// Extension method'lar static class içinde static method olmalı
public static class StringExtensions
{
    // this keyword'ü ile genişletilecek tipi belirtiyoruz
    public static string ReverseString(this string str)
    {
        char[] chars = str.ToCharArray();
        Array.Reverse(chars);
        return new string(chars);
    }
    
    public static string CapitalizeFirst(this string str)
    {
        if (string.IsNullOrEmpty(str))
            return str;
        
        return char.ToUpper(str[0]) + str.Substring(1).ToLower();
    }
    
    public static int WordCount(this string str)
    {
        return str.Split(' ', StringSplitOptions.RemoveEmptyEntries).Length;
    }
}

// Kullanım
string text = "hello world";
Console.WriteLine(text.ReverseString());    // dlrow olleh
Console.WriteLine(text.CapitalizeFirst());  // Hello world
Console.WriteLine(text.WordCount());        // 2
```

### Exception Extension
```csharp
public static class ExceptionExtensions
{
    public static string GetFriendlyMessage(this Exception ex)
    {
        return $"Hata: {ex.Message}\nTarih: {DateTime.Now:dd.MM.yyyy HH:mm}";
    }
    
    public static void LogToFile(this Exception ex, string filePath)
    {
        string log = $"[{DateTime.Now}] {ex.GetType().Name}: {ex.Message}\n";
        File.AppendAllText(filePath, log);
    }
}

// Kullanım
try
{
    int x = int.Parse("abc");
}
catch (Exception ex)
{
    Console.WriteLine(ex.GetFriendlyMessage());
    ex.LogToFile("errors.log");
}
```

### Order Extension Methods
```csharp
public static class OrderExtensions
{
    // Toplam tutarı formatla
    public static string GetFormattedTotal(this Order order)
    {
        return $"{order.CalculateTotal():C2} TL";
    }
    
    // En pahalı ürünü bul
    public static Product GetMostExpensiveProduct(this Order order)
    {
        return order.Products
            .OrderByDescending(p => p.Price)
            .FirstOrDefault();
    }
    
    // İndirim uygula
    public static void ApplyDiscount(this Order order, double percent)
    {
        if (percent < 0 || percent > 100)
            throw new ArgumentException("İndirim 0-100 arasında olmalı!");
        
        // Reflection ile private field'a erişim
        foreach (var product in order.Products)
        {
            typeof(Product)
                .GetProperty("Price")
                ?.SetValue(product, product.Price * (1 - percent / 100));
        }
        
        Console.WriteLine($"%{percent} indirim uygulandı.");
    }
}
```

### LINQ-Style Extension Methods
```csharp
public static class CollectionExtensions
{
    // Where benzeri filtreleme
    public static IEnumerable<T> Filter<T>(
        this IEnumerable<T> source, 
        Func<T, bool> predicate)
    {
        foreach (var item in source)
        {
            if (predicate(item))
                yield return item;
        }
    }
    
    // Select benzeri dönüştürme
    public static IEnumerable<TResult> Map<T, TResult>(
        this IEnumerable<T> source, 
        Func<T, TResult> selector)
    {
        foreach (var item in source)
        {
            yield return selector(item);
        }
    }
    
    // ForEach extension
    public static void ForEach<T>(
        this IEnumerable<T> source, 
        Action<T> action)
    {
        foreach (var item in source)
        {
            action(item);
        }
    }
}

// Kullanım
var numbers = new List<int> { 1, 2, 3, 4, 5 };

numbers
    .Filter(x => x > 2)          // 3, 4, 5
    .Map(x => x * 2)              // 6, 8, 10
    .ForEach(x => Console.WriteLine(x));
```

---

## 19. PARTIAL CLASS

### Partial Class Tanımlama
```csharp
// Employee.Personal.cs dosyası
public partial class Employee
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Address { get; set; }
    
    // Partial method declaration
    partial void OnNameChanged();
    
    public void SetName(string name)
    {
        Name = name;
        OnNameChanged(); // Eğer implement edilmişse çağrılır
    }
}

// Employee.Work.cs dosyası
public partial class Employee
{
    public string Position { get; set; }
    public decimal Salary { get; set; }
    
    public void DisplayWorkDetails()
    {
        Console.WriteLine($"Pozisyon: {Position}");
        Console.WriteLine($"Maaş: {Salary:C2}");
    }
    
    // Partial method implementation (opsiyonel)
    partial void OnNameChanged()
    {
        Console.WriteLine($"İsim değiştirildi: {Name}");
    }
}

// Program.cs
Employee employee = new Employee();
employee.Name = "Ali";          // Personal özellikler
employee.Position = "Developer"; // Work özellikler
employee.DisplayWorkDetails();
```

### Partial Class Kullanım Senaryoları

1. **Büyük Class'ları Bölme**: Farklı dosyalarda organize etme
2. **Code Generation**: Otomatik üretilen kod ile manuel kod ayrımı
3. **Team Development**: Farklı geliştiriciler farklı parçalar üzerinde çalışabilir
4. **Separation of Concerns**: İlgili özellikleri gruplama

---

## 20. STATIC

### Static Class ve Members
```csharp
// Static class - Instance alınamaz
public static class MathHelper
{
    // Static property
    public static double Pi => Math.PI;
    
    // Static method
    public static double CalculateCircleArea(double radius)
    {
        return Pi * radius * radius;
    }
    
    // Static constructor - Sadece bir kez çalışır
    static MathHelper()
    {
        Console.WriteLine("MathHelper initialized");
    }
}

// Kullanım - Instance olmadan
double area = MathHelper.CalculateCircleArea(5);
```

### Static Members in Non-Static Class
```csharp
public class User
{
    // Static field - Tüm instance'lar için ortak
    public static int TotalUsers = 0;
    
    // Instance fields
    public string Name { get; set; }
    public int Id { get; set; }
    
    public User()
    {
        TotalUsers++; // Her yeni user'da artır
        Id = TotalUsers;
    }
}

// Kullanım
User user1 = new User { Name = "Ali" };
Console.WriteLine(User.TotalUsers);  // 1

User user2 = new User { Name = "Veli" };
Console.WriteLine(User.TotalUsers);  // 2
```

### Encryption Helper Static Class
```csharp
public static class EncryptionHelper
{
    // Basit şifreleme (Caesar cipher)
    public static string Encrypt(string input)
    {
        char[] chars = input.ToCharArray();
        for (int i = 0; i < chars.Length; i++)
        {
            chars[i]++;
        }
        return new string(chars);
    }
    
    public static string Decrypt(string input)
    {
        char[] chars = input.ToCharArray();
        for (int i = 0; i < chars.Length; i++)
        {
            chars[i]--;
        }
        return new string(chars);
    }
}

// Kullanım
string original = "Hello";
string encrypted = EncryptionHelper.Encrypt(original);
string decrypted = EncryptionHelper.Decrypt(encrypted);
```

### Static Constructor
```csharp
public static class Configuration
{
    public static string ConnectionString;
    public static int Timeout;
    
    // Static constructor - Uygulama başında bir kez çalışır
    static Configuration()
    {
        Console.WriteLine("Configuration yükleniyor...");
        ConnectionString = "Server=localhost;Database=MyDb";
        Timeout = 30;
    }
}

// İlk kullanımda static constructor çalışır
Console.WriteLine(Configuration.ConnectionString);
```

---

## CASE ÖRNEKLER VE SENARYOLAR

### Case 1: Banka Hesap Yönetimi
```csharp
// Kullanıcı girişi ve hesap işlemleri
class BankSystem
{
    private static double bakiye = 1000;
    private static string kullaniciAdi = "admin";
    private static string sifre = "1234";
    
    public static void Run()
    {
        // Giriş kontrolü
        while (true)
        {
            try
            {
                Console.Write("Kullanıcı Adı: ");
                string girilenKullanici = Console.ReadLine();
                
                Console.Write("Şifre: ");
                string girilenSifre = Console.ReadLine();
                
                if (girilenKullanici == kullaniciAdi && girilenSifre == sifre)
                {
                    MenuGoster();
                    break;
                }
                else
                {
                    throw new Exception("Hatalı giriş!");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    }
    
    private static void MenuGoster()
    {
        while (true)
        {
            Console.WriteLine("\n1-Bakiye Sorgula");
            Console.WriteLine("2-Para Yatır");
            Console.WriteLine("3-Para Çek");
            Console.WriteLine("4-Çıkış");
            Console.Write("Seçim: ");
            
            string secim = Console.ReadLine();
            
            switch (secim)
            {
                case "1":
                    Console.WriteLine($"Bakiye: {bakiye:C2}");
                    break;
                    
                case "2":
                    Console.Write("Miktar: ");
                    if (double.TryParse(Console.ReadLine(), out double yatirilanMiktar))
                    {
                        bakiye += yatirilanMiktar;
                        Console.WriteLine($"Yeni bakiye: {bakiye:C2}");
                    }
                    break;
                    
                case "3":
                    Console.Write("Miktar: ");
                    if (double.TryParse(Console.ReadLine(), out double cekilenMiktar))
                    {
                        if (cekilenMiktar <= bakiye)
                        {
                            bakiye -= cekilenMiktar;
                            Console.WriteLine($"Yeni bakiye: {bakiye:C2}");
                        }
                        else
                        {
                            Console.WriteLine("Yetersiz bakiye!");
                        }
                    }
                    break;
                    
                case "4":
                    Console.WriteLine("Güle güle!");
                    return;
            }
        }
    }
}
```

### Case 2: E-Ticaret Sistemi
```csharp
// Ürün yönetimi ve sipariş sistemi
public interface IProduct
{
    int Id { get; set; }
    string Name { get; set; }
    decimal Price { get; set; }
    int Stock { get; set; }
    decimal CalculateDiscountedPrice(decimal discountRate);
}

public abstract class BaseProduct : IProduct
{
    private static int _idCounter = 1;
    
    public BaseProduct(string name, decimal price, int stock)
    {
        Id = _idCounter++;
        Name = name;
        Price = price;
        Stock = stock;
    }
    
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    public int Stock { get; set; }
    
    public virtual decimal CalculateDiscountedPrice(decimal discountRate)
    {
        return Price * (1 - discountRate / 100);
    }
    
    public abstract string GetProductType();
}

public class PhysicalProduct : BaseProduct
{
    public double Weight { get; set; }
    public string Dimensions { get; set; }
    
    public PhysicalProduct(string name, decimal price, int stock, double weight)
        : base(name, price, stock)
    {
        Weight = weight;
    }
    
    public override string GetProductType()
    {
        return "Fiziksel Ürün";
    }
    
    public decimal CalculateShippingCost()
    {
        return (decimal)(Weight * 5); // 5 TL per kg
    }
}

public class DigitalProduct : BaseProduct
{
    public string DownloadLink { get; set; }
    public long FileSizeInMB { get; set; }
    
    public DigitalProduct(string name, decimal price, int stock, long fileSize)
        : base(name, price, stock)
    {
        FileSizeInMB = fileSize;
    }
    
    public override string GetProductType()
    {
        return "Dijital Ürün";
    }
    
    public override decimal CalculateDiscountedPrice(decimal discountRate)
    {
        // Dijital ürünlerde ekstra %5 indirim
        return base.CalculateDiscountedPrice(discountRate + 5);
    }
}

// Sepet yönetimi
public class ShoppingCart
{
    private List<(IProduct product, int quantity)> items = new();
    
    public void AddProduct(IProduct product, int quantity)
    {
        if (product.Stock >= quantity)
        {
            items.Add((product, quantity));
            product.Stock -= quantity;
            Console.WriteLine($"{product.Name} sepete eklendi.");
        }
        else
        {
            Console.WriteLine("Yetersiz stok!");
        }
    }
    
    public decimal CalculateTotal(decimal discountRate = 0)
    {
        decimal total = 0;
        
        foreach (var item in items)
        {
            decimal price = item.product.CalculateDiscountedPrice(discountRate);
            total += price * item.quantity;
            
            // Fiziksel ürün ise kargo ekle
            if (item.product is PhysicalProduct physical)
            {
                total += physical.CalculateShippingCost();
            }
        }
        
        return total;
    }
    
    public void DisplayCart()
    {
        Console.WriteLine("\n--- SEPETİNİZ ---");
        foreach (var item in items)
        {
            Console.WriteLine($"{item.product.Name} x {item.quantity} = " +
                            $"{item.product.Price * item.quantity:C2}");
        }
        Console.WriteLine($"Toplam: {CalculateTotal():C2}");
    }
}
```

### Case 3: Okul Not Sistemi
```csharp
public class StudentManagementSystem
{
    private List<Student> students = new List<Student>();
    
    public void AddStudent(string name, string surname)
    {
        var student = new Student(students.Count + 100, name, surname);
        students.Add(student);
    }
    
    public void AddGrade(int studentId, string course, int grade)
    {
        var student = students.FirstOrDefault(s => s.Id == studentId);
        if (student != null)
        {
            student.AddGrade(course, grade);
        }
    }
    
    public void GenerateReport()
    {
        var report = students
            .OrderByDescending(s => s.GetAverage())
            .Select((s, index) => new
            {
                Rank = index + 1,
                Name = s.FullName,
                Average = s.GetAverage(),
                Status = s.GetAverage() >= 50 ? "Geçti" : "Kaldı"
            });
        
        Console.WriteLine("SINIF RAPORU");
        Console.WriteLine("Sıra | Öğrenci | Ortalama | Durum");
        Console.WriteLine(new string('-', 40));
        
        foreach (var item in report)
        {
            Console.WriteLine($"{item.Rank,3} | {item.Name,-20} | " +
                            $"{item.Average,5:F2} | {item.Status}");
        }
    }
}

public class Student
{
    private Dictionary<string, List<int>> grades = new();
    
    public Student(int id, string firstName, string lastName)
    {
        Id = id;
        FirstName = firstName;
        LastName = lastName;
    }
    
    public int Id { get; private set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string FullName => $"{FirstName} {LastName}";
    
    public void AddGrade(string course, int grade)
    {
        if (!grades.ContainsKey(course))
            grades[course] = new List<int>();
        
        grades[course].Add(grade);
    }
    
    public double GetAverage()
    {
        if (grades.Count == 0) return 0;
        
        return grades.Values
            .SelectMany(g => g)
            .Average();
    }
    
    public double GetCourseAverage(string course)
    {
        if (!grades.ContainsKey(course)) return 0;
        return grades[course].Average();
    }
}
```

---

## SONUÇ

Bu dökümantasyon, C# Introduction ve OOP konularının tamamını kapsamaktadır. Sınavda karşınıza çıkabilecek tüm konular örneklerle açıklanmıştır. 

**Önemli İpuçları:**
1. Constructor'lar ve property'lerin doğru kullanımına dikkat edin
2. Access modifier'ların ne zaman kullanılacağını bilin
3. Abstract class vs Interface ayrımını yapabilin
4. Inheritance ve Polymorphism'i uygulayabilin
5. Generic yapıları constraint'lerle kullanabilin
6. Collection'ları ve LINQ'yu etkin kullanın
7. Exception handling'i doğru yapın
8. Static ve instance member'ları ayırt edin

**Başarılar!**
