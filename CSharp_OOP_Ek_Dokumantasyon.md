# C# ve OOP EK DÖKÜMANTASYON - KRİTİK KONULAR

## İÇİNDEKİLER
1. [LINQ - Language Integrated Query](#1-linq---language-integrated-query)
2. [DELEGATE, ACTION, FUNC, PREDICATE](#2-delegate-action-func-predicate)
3. [EVENTS (OLAYLAR)](#3-events-olaylar)
4. [ANONYMOUS TYPES VE METHODS](#4-anonymous-types-ve-methods)
5. [NULLABLE HANDLING TEKNİKLERİ](#5-nullable-handling-teknikleri)
6. [MEMORY MANAGEMENT - STACK VS HEAP](#6-memory-management---stack-vs-heap)
7. [BOXING VE UNBOXING DETAYLI](#7-boxing-ve-unboxing-detayli)
8. [INDEXERS](#8-indexers)
9. [OPERATOR OVERLOADING](#9-operator-overloading)
10. [IENUMERABLE VE IENUMERATOR](#10-ienumerable-ve-ienumerator)
11. [REFLECTION](#11-reflection)
12. [DESIGN PATTERNS](#12-design-patterns)
13. [SOLID PRENSİPLERİ DETAYLI](#13-solid-prensipleri-detayli)
14. [STRING İŞLEMLERİ DETAYLI](#14-string-işlemleri-detayli)
15. [DATETIME İŞLEMLERİ](#15-datetime-işlemleri)
16. [FILE I/O İŞLEMLERİ](#16-file-io-işlemleri)
17. [ASYNC/AWAIT TEMELLER](#17-asyncawait-temeller)
18. [BEST PRACTICES VE CODE SMELLS](#18-best-practices-ve-code-smells)
19. [PERFORMANCE TIPS](#19-performance-tips)
20. [UNIT TEST TEMELLER](#20-unit-test-temeller)

---

## 1. LINQ - Language Integrated Query

### LINQ Method Syntax (Hocanın kodlarında çok kullanılmış!)

```csharp
// WHERE - Filtreleme
List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
var evenNumbers = numbers.Where(x => x % 2 == 0);  // 2, 4, 6, 8, 10

// SELECT - Projeksiyon (Dönüştürme)
var squared = numbers.Select(x => x * x);  // 1, 4, 9, 16, 25...

// WHERE + SELECT Kombinasyonu
var result = numbers
    .Where(x => x > 5)           // 6, 7, 8, 9, 10
    .Select(x => x * 2);         // 12, 14, 16, 18, 20

// ORDERBY / ORDERBYDESCENDING - Sıralama
var sorted = numbers.OrderBy(x => x);
var sortedDesc = numbers.OrderByDescending(x => x);

// THENBY - İkinci sıralama kriteri
List<Student> students = GetStudents();
var sortedStudents = students
    .OrderBy(s => s.Grade)
    .ThenBy(s => s.Name);

// FIRST / FIRSTORDEFAULT
var first = numbers.First();                      // 1 (yoksa exception)
var firstOrDefault = numbers.FirstOrDefault();    // 1 (yoksa default)
var firstGt5 = numbers.First(x => x > 5);        // 6
var firstGt100 = numbers.FirstOrDefault(x => x > 100); // 0 (default)

// LAST / LASTORDEFAULT
var last = numbers.Last();                        // 10
var lastOrDefault = numbers.LastOrDefault();      // 10

// SINGLE / SINGLEORDEFAULT - Tek eleman (birden fazla varsa exception)
var single = numbers.Single(x => x == 5);         // 5
var singleOrDef = numbers.SingleOrDefault(x => x == 100); // 0

// ANY - En az bir eleman var mı?
bool hasAny = numbers.Any();                      // true
bool hasGt5 = numbers.Any(x => x > 5);           // true

// ALL - Tüm elemanlar şartı sağlıyor mu?
bool allPositive = numbers.All(x => x > 0);      // true
bool allGt5 = numbers.All(x => x > 5);           // false

// COUNT / LONGCOUNT
int count = numbers.Count();                      // 10
int countGt5 = numbers.Count(x => x > 5);        // 5

// SUM / AVERAGE / MIN / MAX
int sum = numbers.Sum();                          // 55
double average = numbers.Average();               // 5.5
int min = numbers.Min();                          // 1
int max = numbers.Max();                          // 10

// TAKE / SKIP - Sayfalama
var firstThree = numbers.Take(3);                 // 1, 2, 3
var skipThree = numbers.Skip(3);                  // 4, 5, 6, 7, 8, 9, 10
var page2 = numbers.Skip(3).Take(3);             // 4, 5, 6 (2. sayfa)

// TAKEWHILE / SKIPWHILE
var takeWhile = numbers.TakeWhile(x => x < 5);   // 1, 2, 3, 4
var skipWhile = numbers.SkipWhile(x => x < 5);   // 5, 6, 7, 8, 9, 10

// DISTINCT - Benzersiz elemanlar
var duplicates = new[] { 1, 2, 2, 3, 3, 3, 4 };
var unique = duplicates.Distinct();               // 1, 2, 3, 4

// GROUPBY - Gruplama
var groups = students.GroupBy(s => s.Grade);
foreach (var group in groups)
{
    Console.WriteLine($"Grade {group.Key}: {group.Count()} students");
    foreach (var student in group)
        Console.WriteLine($"  - {student.Name}");
}

// JOIN - İki listeyi birleştirme
var orders = GetOrders();
var customers = GetCustomers();
var joined = customers.Join(
    orders,
    c => c.Id,           // Customer key
    o => o.CustomerId,   // Order key
    (c, o) => new { CustomerName = c.Name, OrderId = o.Id }
);

// SELECTMANY - İç içe koleksiyonları düzleştirme
var allGrades = students.SelectMany(s => s.Grades);

// AGGREGATE - Özel birleştirme işlemi
string sentence = "the quick brown fox";
string[] words = sentence.Split(' ');
string reversed = words.Aggregate((a, b) => b + " " + a);

// ZIP - İki listeyi birleştir
var numbers1 = new[] { 1, 2, 3 };
var numbers2 = new[] { 10, 20, 30 };
var zipped = numbers1.Zip(numbers2, (a, b) => a + b);  // 11, 22, 33

// CONTAINS
bool contains5 = numbers.Contains(5);             // true

// UNION / INTERSECT / EXCEPT
var set1 = new[] { 1, 2, 3, 4, 5 };
var set2 = new[] { 4, 5, 6, 7, 8 };
var union = set1.Union(set2);                    // 1,2,3,4,5,6,7,8
var intersect = set1.Intersect(set2);            // 4,5
var except = set1.Except(set2);                  // 1,2,3

// CAST / OFTYPE
ArrayList mixedList = new ArrayList { 1, "two", 3, "four", 5 };
var ints = mixedList.OfType<int>();              // 1, 3, 5
var strings = mixedList.OfType<string>();        // "two", "four"

// TOLIST / TOARRAY / TODICTIONARY / TOLOOKUP
var list = numbers.Where(x => x > 5).ToList();
var array = numbers.Where(x => x > 5).ToArray();
var dict = students.ToDictionary(s => s.Id, s => s.Name);
var lookup = students.ToLookup(s => s.Grade);
```

### LINQ Query Syntax (SQL benzeri)

```csharp
// FROM - WHERE - SELECT
var query1 = from n in numbers
             where n > 5
             select n * 2;

// JOIN
var query2 = from c in customers
             join o in orders on c.Id equals o.CustomerId
             select new { c.Name, o.OrderId };

// GROUP BY
var query3 = from s in students
             group s by s.Grade into g
             select new { Grade = g.Key, Count = g.Count() };

// ORDER BY
var query4 = from s in students
             orderby s.Grade descending, s.Name
             select s;

// LET - Ara değişken tanımlama
var query5 = from s in students
             let average = s.Grades.Average()
             where average > 70
             orderby average descending
             select new { s.Name, Average = average };

// Multiple FROM (SelectMany equivalent)
var query6 = from s in students
             from g in s.Grades
             where g > 80
             select new { s.Name, Grade = g };
```

---

## 2. DELEGATE, ACTION, FUNC, PREDICATE

### Delegate (Temsilci)

```csharp
// Delegate tanımlama - Metot imzası belirtir
public delegate int MathOperation(int x, int y);
public delegate void Logger(string message);
public delegate bool Validator<T>(T item);

public class Calculator
{
    // Delegate kullanımı
    public int Calculate(int a, int b, MathOperation operation)
    {
        return operation(a, b);
    }
    
    // Delegate için metotlar
    public static int Add(int x, int y) => x + y;
    public static int Multiply(int x, int y) => x * y;
    public static int Subtract(int x, int y) => x - y;
}

// Kullanım
Calculator calc = new Calculator();
MathOperation op = Calculator.Add;
int result1 = calc.Calculate(5, 3, op);           // 8

// Multicast delegate
Logger logger = Console.WriteLine;
logger += (msg) => File.AppendAllText("log.txt", msg);
logger("Test message");  // Hem console hem dosyaya yazar

// Anonymous method ile
MathOperation op2 = delegate(int x, int y) { return x - y; };

// Lambda expression ile
MathOperation op3 = (x, y) => x * y;
```

### Action (Void dönen delegate)

```csharp
// Action - parametre almaz, void döner
Action action1 = () => Console.WriteLine("Hello");

// Action<T> - T tipinde parametre alır, void döner
Action<string> action2 = (msg) => Console.WriteLine(msg);
Action<int, int> action3 = (x, y) => Console.WriteLine($"{x} + {y} = {x + y}");

// Kullanım
action1();                    // Hello
action2("Test");             // Test
action3(5, 3);               // 5 + 3 = 8

// Method parameter olarak
public void ProcessList<T>(List<T> items, Action<T> process)
{
    foreach (var item in items)
        process(item);
}

// Kullanım
var numbers = new List<int> { 1, 2, 3, 4, 5 };
ProcessList(numbers, x => Console.WriteLine(x * 2));
```

### Func (Değer dönen delegate)

```csharp
// Func<TResult> - parametre almaz, TResult döner
Func<int> func1 = () => 42;

// Func<T, TResult> - T parametre alır, TResult döner
Func<int, int> func2 = x => x * x;
Func<int, int, int> func3 = (x, y) => x + y;
Func<string, bool> func4 = s => !string.IsNullOrEmpty(s);

// Kullanım
int result1 = func1();                    // 42
int result2 = func2(5);                   // 25
int result3 = func3(3, 4);                // 7
bool result4 = func4("test");             // true

// Method parameter olarak
public List<TResult> Transform<T, TResult>(List<T> items, Func<T, TResult> transformer)
{
    var results = new List<TResult>();
    foreach (var item in items)
        results.Add(transformer(item));
    return results;
}

// Kullanım
var numbers = new List<int> { 1, 2, 3 };
var strings = Transform(numbers, x => $"Number: {x}");
```

### Predicate (Bool dönen delegate)

```csharp
// Predicate<T> - T parametre alır, bool döner
Predicate<int> isEven = x => x % 2 == 0;
Predicate<string> isValid = s => !string.IsNullOrEmpty(s) && s.Length > 5;

// Kullanım
bool result1 = isEven(4);                 // true
bool result2 = isValid("Hello");          // false

// List metodlarında kullanım
var numbers = new List<int> { 1, 2, 3, 4, 5 };
var even = numbers.FindAll(isEven);      // 2, 4
var first = numbers.Find(x => x > 3);    // 4
bool exists = numbers.Exists(x => x > 10); // false
numbers.RemoveAll(x => x < 3);           // 1, 2 silindi
```

### Karşılaştırma Tablosu

| Tip | Açıklama | Örnek |
|-----|----------|-------|
| **Delegate** | Özel tanımlı metot imzası | `delegate int Math(int a, int b)` |
| **Action** | Void dönen, 0-16 parametre | `Action<string> print` |
| **Func** | Değer dönen, 0-16 parametre | `Func<int, int, int> add` |
| **Predicate** | Bool dönen, 1 parametre | `Predicate<int> isEven` |

---

## 3. EVENTS (OLAYLAR)

```csharp
// Event tanımlama
public class Button
{
    // Event delegate tanımı
    public delegate void ClickEventHandler(object sender, EventArgs e);
    
    // Event
    public event ClickEventHandler Click;
    
    // Event'i tetikleme
    protected virtual void OnClick()
    {
        Click?.Invoke(this, EventArgs.Empty);
    }
    
    public void PerformClick()
    {
        OnClick();
    }
}

// EventHandler kullanımı (built-in)
public class Product
{
    private decimal _price;
    
    // Standard EventHandler
    public event EventHandler PriceChanged;
    
    // Custom EventArgs ile
    public event EventHandler<PriceChangedEventArgs> PriceChangedDetailed;
    
    public decimal Price
    {
        get => _price;
        set
        {
            if (_price != value)
            {
                decimal oldPrice = _price;
                _price = value;
                
                // Events'leri tetikle
                PriceChanged?.Invoke(this, EventArgs.Empty);
                PriceChangedDetailed?.Invoke(this, 
                    new PriceChangedEventArgs(oldPrice, value));
            }
        }
    }
}

// Custom EventArgs
public class PriceChangedEventArgs : EventArgs
{
    public decimal OldPrice { get; }
    public decimal NewPrice { get; }
    public decimal Difference => NewPrice - OldPrice;
    
    public PriceChangedEventArgs(decimal oldPrice, decimal newPrice)
    {
        OldPrice = oldPrice;
        NewPrice = newPrice;
    }
}

// Event kullanımı
class Program
{
    static void Main()
    {
        var product = new Product();
        
        // Event'e subscribe
        product.PriceChanged += Product_PriceChanged;
        product.PriceChangedDetailed += Product_PriceChangedDetailed;
        
        // Lambda ile subscribe
        product.PriceChanged += (s, e) => Console.WriteLine("Price changed!");
        
        // Event tetikleme
        product.Price = 100;  // Event'ler tetiklenir
        
        // Unsubscribe
        product.PriceChanged -= Product_PriceChanged;
    }
    
    static void Product_PriceChanged(object sender, EventArgs e)
    {
        Console.WriteLine("Price has changed!");
    }
    
    static void Product_PriceChangedDetailed(object sender, PriceChangedEventArgs e)
    {
        Console.WriteLine($"Price changed from {e.OldPrice} to {e.NewPrice}");
        Console.WriteLine($"Difference: {e.Difference}");
    }
}
```

---

## 4. ANONYMOUS TYPES VE METHODS

### Anonymous Types

```csharp
// Anonymous type oluşturma
var person = new { Name = "Ali", Age = 25, City = "Istanbul" };
Console.WriteLine($"{person.Name} - {person.Age}");

// LINQ ile kullanım
var products = GetProducts();
var summary = products.Select(p => new 
{
    p.Name,
    p.Price,
    DiscountedPrice = p.Price * 0.9,
    Category = p.Category.ToUpper()
});

// Nested anonymous types
var complex = new 
{
    Id = 1,
    Details = new 
    {
        Name = "Test",
        Date = DateTime.Now
    },
    Items = new[] { 1, 2, 3 }
};

// Array of anonymous types
var people = new[]
{
    new { Name = "Ali", Age = 25 },
    new { Name = "Veli", Age = 30 },
    new { Name = "Ayşe", Age = 28 }
};

// Grouping ile
var grouped = products
    .GroupBy(p => p.Category)
    .Select(g => new 
    {
        Category = g.Key,
        Count = g.Count(),
        TotalPrice = g.Sum(p => p.Price),
        AveragePrice = g.Average(p => p.Price)
    });
```

### Anonymous Methods

```csharp
// Delegate ile anonymous method
Func<int, int> square = delegate(int x) { return x * x; };

// Event handler olarak
button.Click += delegate(object sender, EventArgs e)
{
    Console.WriteLine("Button clicked!");
};

// Lambda expression (modern yöntem)
Func<int, int> cube = x => x * x * x;

// Closure - Dış değişkene erişim
int multiplier = 10;
Func<int, int> multiply = x => x * multiplier;
Console.WriteLine(multiply(5));  // 50

multiplier = 20;
Console.WriteLine(multiply(5));  // 100
```

---

## 5. NULLABLE HANDLING TEKNİKLERİ

### Nullable Value Types

```csharp
// Nullable tanımlama
int? nullableInt = null;
double? nullableDouble = 3.14;
DateTime? nullableDate = null;
bool? nullableBool = true;

// Kontrol ve kullanım
if (nullableInt.HasValue)
{
    int value = nullableInt.Value;
    // veya
    int value2 = nullableInt.GetValueOrDefault();
}

// Default değer ile
int value3 = nullableInt.GetValueOrDefault(10);  // null ise 10

// Null-coalescing operator (??)
int value4 = nullableInt ?? 0;                    // null ise 0
string name = GetName() ?? "Unknown";

// Null-coalescing assignment (??=)
nullableInt ??= 5;  // Eğer null ise 5 ata

// Chaining
int? a = null;
int? b = null;
int? c = 5;
int result = a ?? b ?? c ?? 0;  // 5
```

### Nullable Reference Types (C# 8.0+)

```csharp
#nullable enable

public class Person
{
    public string Name { get; set; }        // Non-nullable
    public string? MiddleName { get; set; } // Nullable
    public string LastName { get; set; }    // Non-nullable
    
    public Person(string name, string lastName)
    {
        Name = name;
        LastName = lastName;
    }
}

// Null-forgiving operator (!)
string? nullable = GetNullableString();
string nonNullable = nullable!;  // Compiler'a null olmadığını garanti ediyoruz

// Null-conditional operator (?.)
Person? person = GetPerson();
int? nameLength = person?.Name?.Length;
string upperName = person?.Name?.ToUpper() ?? "UNKNOWN";

// Null-conditional with arrays
int?[] array = new int?[] { 1, 2, 3, null, 5 };
int? fourthElement = array?[3];  // null

// Method chaining
var result = person?.GetAddress()?.GetCity()?.ToUpper() ?? "NO CITY";

// Null-conditional with delegates
Action? action = GetAction();
action?.Invoke();  // Null değilse çağır

// Elvis operator kombinasyonu
public string GetDisplayName(Person? person)
{
    return person?.Name ?? person?.Email ?? "Unknown User";
}
```

### Pattern Matching ile Null Check

```csharp
// is pattern
if (person is not null)
{
    Console.WriteLine(person.Name);
}

// is pattern with property
if (person is { Name: not null, Age: > 18 })
{
    Console.WriteLine($"Adult: {person.Name}");
}

// Switch expression
string GetStatus(Person? person) => person switch
{
    null => "No person",
    { Age: < 18 } => "Minor",
    { Age: >= 18 and < 65 } => "Adult",
    _ => "Senior"
};

// Tuple pattern matching
var result = (person?.Name, person?.Age) switch
{
    (null, _) => "Name missing",
    (_, null) => "Age missing",
    var (name, age) => $"{name} is {age} years old"
};
```

---

## 6. MEMORY MANAGEMENT - STACK VS HEAP

### Stack (Yığın)

```csharp
// STACK'te saklanır:
// 1. Value types (int, double, bool, struct, enum)
// 2. References to objects (adresler)
// 3. Method parameters
// 4. Local variables

public void StackExample()
{
    int a = 5;        // Stack'te
    int b = 10;       // Stack'te
    int c = a + b;    // Stack'te
    
    // Struct - Stack'te
    Point p = new Point { X = 10, Y = 20 };
}

struct Point
{
    public int X;     // Stack'te
    public int Y;     // Stack'te
}
```

### Heap (Öbek)

```csharp
// HEAP'te saklanır:
// 1. Objects (class instances)
// 2. Arrays
// 3. Strings
// 4. Delegates

public void HeapExample()
{
    // person referansı Stack'te, object Heap'te
    Person person = new Person("Ali");
    
    // array referansı Stack'te, dizi Heap'te
    int[] numbers = new int[10];
    
    // string referansı Stack'te, metin Heap'te
    string text = "Hello";
}

class Person
{
    public string Name;  // Heap'te
    
    public Person(string name)
    {
        Name = name;     // name parametresi Stack'te, değer Heap'te
    }
}
```

### Stack vs Heap Karşılaştırma

| Özellik | Stack | Heap |
|---------|-------|------|
| **Hız** | Çok hızlı | Yavaş |
| **Boyut** | Sınırlı (genelde 1MB) | Büyük |
| **Tahsis** | Otomatik | Manuel/GC |
| **Temizleme** | Otomatik (scope bitince) | Garbage Collector |
| **Fragmentation** | Yok | Var |
| **Thread-safe** | Evet (thread başına) | Hayır |

### Memory Allocation Örnekleri

```csharp
public class MemoryDemo
{
    public void Example()
    {
        // Stack allocation
        int x = 10;              // Stack: x = 10
        int y = x;               // Stack: y = 10 (kopya)
        y = 20;                  // Stack: x = 10, y = 20
        
        // Heap allocation
        int[] arr1 = new int[3]; // Stack: arr1 ref, Heap: [0,0,0]
        int[] arr2 = arr1;       // Stack: arr2 ref, aynı Heap object
        arr2[0] = 5;             // arr1[0] da 5 olur (aynı object)
        
        // String (özel durum - immutable)
        string s1 = "Hello";      // Stack: s1 ref, Heap: "Hello"
        string s2 = s1;          // Stack: s2 ref, aynı Heap object
        s2 = "World";            // Stack: s2 ref, Yeni Heap: "World"
                                 // s1 hala "Hello"'yu gösterir
    }
}

// Struct vs Class memory farkı
struct PointStruct    // Stack'te saklanır
{
    public int X, Y;
}

class PointClass      // Heap'te saklanır
{
    public int X, Y;
}

public void MemoryComparison()
{
    // Struct - Stack'te, kopyalama yapılır
    PointStruct ps1 = new PointStruct { X = 10, Y = 20 };
    PointStruct ps2 = ps1;  // Değer kopyalanır
    ps2.X = 30;             // ps1.X hala 10
    
    // Class - Heap'te, referans kopyalanır
    PointClass pc1 = new PointClass { X = 10, Y = 20 };
    PointClass pc2 = pc1;   // Referans kopyalanır
    pc2.X = 30;             // pc1.X de 30 olur
}
```

---

## 7. BOXING VE UNBOXING DETAYLI

### Boxing (Kutulamak)

```csharp
// Boxing - Value type'ı object'e dönüştürme
int value = 123;                    // Stack'te
object boxed = value;                // Heap'te yeni object oluşur
                                    // value kopyalanır

// Implicit boxing örnekleri
ArrayList list = new ArrayList();
list.Add(5);                        // int boxed to object
list.Add(3.14);                     // double boxed to object

// String formatting'de boxing
int age = 25;
string text = string.Format("Age: {0}", age);  // age boxed

// Method çağrısında
void PrintObject(object obj) 
{
    Console.WriteLine(obj);
}
PrintObject(42);                    // 42 boxed to object

// Interface boxing
IComparable comp = 5;               // int boxed to IComparable

// Performance etkisi
DateTime start = DateTime.Now;
for (int i = 0; i < 1000000; i++)
{
    object o = i;                   // Her döngüde boxing = YAVAŞ
}
```

### Unboxing (Kutudan Çıkarmak)

```csharp
// Unboxing - object'i value type'a dönüştürme
object boxed = 123;
int unboxed = (int)boxed;          // Explicit cast gerekli

// Yanlış tip ile unboxing = InvalidCastException
object boxedInt = 123;
// double wrong = (double)boxedInt; // HATA!
// Doğrusu:
double correct = (double)(int)boxedInt;

// ArrayList'ten unboxing
ArrayList list = new ArrayList { 1, 2, 3 };
int sum = 0;
foreach (object item in list)
{
    sum += (int)item;              // Her elemanı unbox
}

// Null check gerekli
object maybeNull = GetValue();
if (maybeNull != null)
{
    int value = (int)maybeNull;
}

// Safe unboxing
object obj = 123;
if (obj is int intValue)          // Pattern matching
{
    Console.WriteLine(intValue * 2);
}

// as operator ile unboxing (nullable için)
object boxedNullable = 5;
int? nullableInt = boxedNullable as int?;
```

### Boxing/Unboxing Performans Karşılaştırması

```csharp
public void PerformanceTest()
{
    const int iterations = 10000000;
    
    // Generic List (boxing yok) - HIZLI
    var watch1 = Stopwatch.StartNew();
    List<int> genericList = new List<int>();
    for (int i = 0; i < iterations; i++)
    {
        genericList.Add(i);
    }
    watch1.Stop();
    
    // ArrayList (boxing var) - YAVAŞ
    var watch2 = Stopwatch.StartNew();
    ArrayList arrayList = new ArrayList();
    for (int i = 0; i < iterations; i++)
    {
        arrayList.Add(i);  // Boxing
    }
    watch2.Stop();
    
    Console.WriteLine($"Generic: {watch1.ElapsedMilliseconds}ms");
    Console.WriteLine($"ArrayList: {watch2.ElapsedMilliseconds}ms");
    // Generic yaklaşık 10x daha hızlı!
}

// Boxing'i önleme teknikleri
// 1. Generic kullan
List<int> list = new List<int>();  // Boxing yok

// 2. Value type comparison
int a = 5, b = 5;
bool equal = a.Equals(b);          // Boxing yok
// bool equal = object.Equals(a, b); // Boxing VAR!

// 3. ToString() kullan
int number = 42;
string str = number.ToString();    // Boxing yok
// string str = "" + number;       // Boxing VAR!
```

---

## 8. INDEXERS

### Indexer Tanımlama

```csharp
public class StringCollection
{
    private string[] items = new string[10];
    
    // Indexer tanımı
    public string this[int index]
    {
        get
        {
            if (index < 0 || index >= items.Length)
                throw new IndexOutOfRangeException();
            return items[index];
        }
        set
        {
            if (index < 0 || index >= items.Length)
                throw new IndexOutOfRangeException();
            items[index] = value;
        }
    }
    
    // Multiple parameter indexer
    public string this[int row, int col]
    {
        get => $"Cell[{row},{col}]";
        set => Console.WriteLine($"Set [{row},{col}] = {value}");
    }
    
    // String indexer
    private Dictionary<string, string> dict = new Dictionary<string, string>();
    
    public string this[string key]
    {
        get => dict.ContainsKey(key) ? dict[key] : null;
        set => dict[key] = value;
    }
}

// Kullanım
StringCollection collection = new StringCollection();
collection[0] = "First";            // Indexer set
string first = collection[0];       // Indexer get
collection["key"] = "value";        // String indexer
collection[1, 2] = "Cell value";    // Multi-parameter
```

### Generic Indexer

```csharp
public class DataStore<T>
{
    private T[] data = new T[100];
    private int count = 0;
    
    public T this[int index]
    {
        get
        {
            if (index >= count)
                throw new IndexOutOfRangeException();
            return data[index];
        }
        set
        {
            if (index >= data.Length)
                Array.Resize(ref data, data.Length * 2);
            
            data[index] = value;
            if (index >= count)
                count = index + 1;
        }
    }
    
    // Read-only indexer
    public T this[Index index] => data[index];
    
    // Range indexer (C# 8.0+)
    public T[] this[Range range] => data[range];
}

// Kullanım
DataStore<int> store = new DataStore<int>();
store[0] = 10;
store[1] = 20;
int last = store[^1];               // Son eleman
int[] subset = store[0..2];         // İlk 2 eleman
```

---

## 9. OPERATOR OVERLOADING

### Operator Overloading Örnekleri

```csharp
public class Vector2D
{
    public double X { get; set; }
    public double Y { get; set; }
    
    public Vector2D(double x, double y)
    {
        X = x;
        Y = y;
    }
    
    // Binary operators
    public static Vector2D operator +(Vector2D a, Vector2D b)
    {
        return new Vector2D(a.X + b.X, a.Y + b.Y);
    }
    
    public static Vector2D operator -(Vector2D a, Vector2D b)
    {
        return new Vector2D(a.X - b.X, a.Y - b.Y);
    }
    
    public static Vector2D operator *(Vector2D v, double scalar)
    {
        return new Vector2D(v.X * scalar, v.Y * scalar);
    }
    
    public static Vector2D operator *(double scalar, Vector2D v)
    {
        return v * scalar;
    }
    
    public static double operator *(Vector2D a, Vector2D b)
    {
        return a.X * b.X + a.Y * b.Y;  // Dot product
    }
    
    // Unary operators
    public static Vector2D operator -(Vector2D v)
    {
        return new Vector2D(-v.X, -v.Y);
    }
    
    public static Vector2D operator ++(Vector2D v)
    {
        v.X++;
        v.Y++;
        return v;
    }
    
    // Comparison operators
    public static bool operator ==(Vector2D a, Vector2D b)
    {
        if (ReferenceEquals(a, b)) return true;
        if (a is null || b is null) return false;
        return a.X == b.X && a.Y == b.Y;
    }
    
    public static bool operator !=(Vector2D a, Vector2D b)
    {
        return !(a == b);
    }
    
    public static bool operator <(Vector2D a, Vector2D b)
    {
        return a.Magnitude() < b.Magnitude();
    }
    
    public static bool operator >(Vector2D a, Vector2D b)
    {
        return a.Magnitude() > b.Magnitude();
    }
    
    // Conversion operators
    public static implicit operator Vector2D(double value)
    {
        return new Vector2D(value, value);
    }
    
    public static explicit operator double(Vector2D v)
    {
        return v.Magnitude();
    }
    
    // Helper methods
    public double Magnitude()
    {
        return Math.Sqrt(X * X + Y * Y);
    }
    
    public override bool Equals(object obj)
    {
        return obj is Vector2D vector && this == vector;
    }
    
    public override int GetHashCode()
    {
        return HashCode.Combine(X, Y);
    }
    
    public override string ToString()
    {
        return $"({X}, {Y})";
    }
}

// Kullanım
Vector2D v1 = new Vector2D(3, 4);
Vector2D v2 = new Vector2D(1, 2);

Vector2D sum = v1 + v2;             // (4, 6)
Vector2D diff = v1 - v2;            // (2, 2)
Vector2D scaled = v1 * 2;           // (6, 8)
double dot = v1 * v2;               // 11
Vector2D neg = -v1;                 // (-3, -4)
bool equal = v1 == v2;              // false
bool greater = v1 > v2;             // true

Vector2D v3 = 5.0;                  // Implicit conversion
double mag = (double)v1;            // Explicit conversion
```

---

## 10. IEnumerable VE IEnumerator

### IEnumerable & IEnumerator Implementation

```csharp
// Custom collection with IEnumerable
public class CustomList<T> : IEnumerable<T>
{
    private T[] items;
    private int count;
    
    public CustomList(int capacity = 4)
    {
        items = new T[capacity];
        count = 0;
    }
    
    public void Add(T item)
    {
        if (count == items.Length)
            Array.Resize(ref items, items.Length * 2);
        
        items[count++] = item;
    }
    
    // IEnumerable implementation
    public IEnumerator<T> GetEnumerator()
    {
        return new CustomEnumerator(this);
    }
    
    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
    
    // Nested Enumerator class
    private class CustomEnumerator : IEnumerator<T>
    {
        private CustomList<T> list;
        private int index = -1;
        
        public CustomEnumerator(CustomList<T> list)
        {
            this.list = list;
        }
        
        public T Current => list.items[index];
        
        object IEnumerator.Current => Current;
        
        public bool MoveNext()
        {
            index++;
            return index < list.count;
        }
        
        public void Reset()
        {
            index = -1;
        }
        
        public void Dispose() { }
    }
}

// Yield kullanarak IEnumerable
public class NumberGenerator : IEnumerable<int>
{
    private int start, end;
    
    public NumberGenerator(int start, int end)
    {
        this.start = start;
        this.end = end;
    }
    
    public IEnumerator<int> GetEnumerator()
    {
        for (int i = start; i <= end; i++)
        {
            yield return i;  // Iterator block
        }
    }
    
    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}

// Yield ile lazy evaluation
public static class EnumerableExtensions
{
    public static IEnumerable<T> Where<T>(this IEnumerable<T> source, Func<T, bool> predicate)
    {
        foreach (var item in source)
        {
            if (predicate(item))
                yield return item;  // Lazy evaluation
        }
    }
    
    public static IEnumerable<int> InfiniteSequence()
    {
        int i = 0;
        while (true)
        {
            yield return i++;  // Sonsuz dizi
        }
    }
    
    public static IEnumerable<int> Fibonacci()
    {
        int prev = 0, curr = 1;
        yield return prev;
        yield return curr;
        
        while (true)
        {
            int next = prev + curr;
            yield return next;
            prev = curr;
            curr = next;
        }
    }
}

// Kullanım
var numbers = new NumberGenerator(1, 10);
foreach (var num in numbers)
{
    Console.WriteLine(num);
}

// Infinite sequence with Take
var firstTen = EnumerableExtensions.InfiniteSequence().Take(10);

// Fibonacci
var fib = EnumerableExtensions.Fibonacci().Take(20);
```

---

## 11. REFLECTION

### Reflection Temelleri

```csharp
using System.Reflection;

public class ReflectionDemo
{
    // Type bilgisi alma
    public void GetTypeInfo()
    {
        // 3 farklı yol
        Type type1 = typeof(string);
        Type type2 = "Hello".GetType();
        Type type3 = Type.GetType("System.String");
        
        // Type bilgileri
        Console.WriteLine($"Name: {type1.Name}");
        Console.WriteLine($"FullName: {type1.FullName}");
        Console.WriteLine($"Namespace: {type1.Namespace}");
        Console.WriteLine($"IsClass: {type1.IsClass}");
        Console.WriteLine($"IsValueType: {type1.IsValueType}");
        Console.WriteLine($"IsPublic: {type1.IsPublic}");
        Console.WriteLine($"BaseType: {type1.BaseType}");
    }
    
    // Member bilgileri
    public void GetMembers()
    {
        Type type = typeof(Person);
        
        // Properties
        PropertyInfo[] properties = type.GetProperties();
        foreach (var prop in properties)
        {
            Console.WriteLine($"Property: {prop.Name} ({prop.PropertyType})");
        }
        
        // Methods
        MethodInfo[] methods = type.GetMethods();
        foreach (var method in methods)
        {
            Console.WriteLine($"Method: {method.Name}");
        }
        
        // Fields
        FieldInfo[] fields = type.GetFields(BindingFlags.NonPublic | BindingFlags.Instance);
        foreach (var field in fields)
        {
            Console.WriteLine($"Field: {field.Name}");
        }
        
        // Constructors
        ConstructorInfo[] constructors = type.GetConstructors();
        foreach (var ctor in constructors)
        {
            var parameters = ctor.GetParameters();
            Console.WriteLine($"Constructor with {parameters.Length} parameters");
        }
    }
    
    // Dynamic object creation
    public void CreateInstance()
    {
        Type type = typeof(Person);
        
        // Parametresiz constructor
        object instance1 = Activator.CreateInstance(type);
        
        // Parametreli constructor
        object instance2 = Activator.CreateInstance(type, "Ali", 25);
        
        // Generic type
        Type genericType = typeof(List<>).MakeGenericType(typeof(int));
        object list = Activator.CreateInstance(genericType);
    }
    
    // Property get/set
    public void AccessProperties()
    {
        Person person = new Person("Ali", 25);
        Type type = person.GetType();
        
        // Get property value
        PropertyInfo nameProp = type.GetProperty("Name");
        object nameValue = nameProp.GetValue(person);
        
        // Set property value
        PropertyInfo ageProp = type.GetProperty("Age");
        ageProp.SetValue(person, 30);
        
        // Private field access
        FieldInfo field = type.GetField("_id", BindingFlags.NonPublic | BindingFlags.Instance);
        field.SetValue(person, 123);
    }
    
    // Method invocation
    public void InvokeMethod()
    {
        Person person = new Person("Ali", 25);
        Type type = person.GetType();
        
        // Parametresiz method
        MethodInfo method1 = type.GetMethod("GetInfo");
        object result1 = method1.Invoke(person, null);
        
        // Parametreli method
        MethodInfo method2 = type.GetMethod("SetAge");
        method2.Invoke(person, new object[] { 30 });
        
        // Generic method
        MethodInfo genericMethod = type.GetMethod("GenericMethod");
        MethodInfo constructedMethod = genericMethod.MakeGenericMethod(typeof(string));
        constructedMethod.Invoke(person, new object[] { "test" });
    }
    
    // Attribute okuma
    public void ReadAttributes()
    {
        Type type = typeof(Person);
        
        // Class attributes
        var classAttrs = type.GetCustomAttributes(typeof(SerializableAttribute), false);
        
        // Property attributes
        PropertyInfo prop = type.GetProperty("Name");
        var propAttrs = prop.GetCustomAttributes(typeof(RequiredAttribute), false);
        
        // Method attributes
        MethodInfo method = type.GetMethod("Validate");
        var methodAttrs = method.GetCustomAttributes();
        
        foreach (var attr in methodAttrs)
        {
            if (attr is ObsoleteAttribute obsolete)
            {
                Console.WriteLine($"Obsolete: {obsolete.Message}");
            }
        }
    }
}

// Assembly yükleme ve tarama
public void AssemblyScanning()
{
    // Current assembly
    Assembly current = Assembly.GetExecutingAssembly();
    
    // Load assembly
    Assembly loaded = Assembly.Load("System.Data");
    
    // Get types
    Type[] types = current.GetTypes();
    
    // Find specific types
    var interfaces = types.Where(t => t.IsInterface);
    var classes = types.Where(t => t.IsClass && !t.IsAbstract);
    
    // Find types implementing interface
    var implementations = types
        .Where(t => t.GetInterfaces().Contains(typeof(IDisposable)));
    
    // Create instances of all types
    foreach (var type in classes)
    {
        if (type.GetConstructor(Type.EmptyTypes) != null)
        {
            object instance = Activator.CreateInstance(type);
        }
    }
}
```

---

## 12. DESIGN PATTERNS

### Singleton Pattern

```csharp
// Thread-safe Singleton
public sealed class DatabaseConnection
{
    private static readonly Lazy<DatabaseConnection> lazy = 
        new Lazy<DatabaseConnection>(() => new DatabaseConnection());
    
    public static DatabaseConnection Instance => lazy.Value;
    
    private DatabaseConnection()
    {
        // Private constructor
        ConnectionString = "Server=localhost;Database=MyDb";
    }
    
    public string ConnectionString { get; private set; }
    
    public void ExecuteQuery(string query)
    {
        Console.WriteLine($"Executing: {query}");
    }
}

// Kullanım
var db = DatabaseConnection.Instance;
db.ExecuteQuery("SELECT * FROM Users");
```

### Factory Pattern

```csharp
// Product interface
public interface IVehicle
{
    void Drive();
    string Type { get; }
}

// Concrete products
public class Car : IVehicle
{
    public string Type => "Car";
    public void Drive() => Console.WriteLine("Driving a car");
}

public class Motorcycle : IVehicle
{
    public string Type => "Motorcycle";
    public void Drive() => Console.WriteLine("Riding a motorcycle");
}

// Factory
public class VehicleFactory
{
    public static IVehicle CreateVehicle(string type)
    {
        return type.ToLower() switch
        {
            "car" => new Car(),
            "motorcycle" => new Motorcycle(),
            _ => throw new ArgumentException($"Unknown vehicle type: {type}")
        };
    }
}

// Abstract Factory Pattern
public interface IUIFactory
{
    IButton CreateButton();
    ITextBox CreateTextBox();
}

public class WindowsFactory : IUIFactory
{
    public IButton CreateButton() => new WindowsButton();
    public ITextBox CreateTextBox() => new WindowsTextBox();
}

public class MacFactory : IUIFactory
{
    public IButton CreateButton() => new MacButton();
    public ITextBox CreateTextBox() => new MacTextBox();
}
```

### Repository Pattern

```csharp
// Entity
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

// Repository Interface
public interface IRepository<T> where T : class
{
    T GetById(int id);
    IEnumerable<T> GetAll();
    void Add(T entity);
    void Update(T entity);
    void Delete(int id);
    void SaveChanges();
}

// Generic Repository Implementation
public class Repository<T> : IRepository<T> where T : class
{
    protected readonly DbContext context;
    protected readonly DbSet<T> dbSet;
    
    public Repository(DbContext context)
    {
        this.context = context;
        this.dbSet = context.Set<T>();
    }
    
    public virtual T GetById(int id)
    {
        return dbSet.Find(id);
    }
    
    public virtual IEnumerable<T> GetAll()
    {
        return dbSet.ToList();
    }
    
    public virtual void Add(T entity)
    {
        dbSet.Add(entity);
    }
    
    public virtual void Update(T entity)
    {
        dbSet.Attach(entity);
        context.Entry(entity).State = EntityState.Modified;
    }
    
    public virtual void Delete(int id)
    {
        var entity = GetById(id);
        if (entity != null)
            dbSet.Remove(entity);
    }
    
    public void SaveChanges()
    {
        context.SaveChanges();
    }
}

// Specific Repository
public interface IProductRepository : IRepository<Product>
{
    IEnumerable<Product> GetProductsByCategory(string category);
    IEnumerable<Product> GetTopSellingProducts(int count);
}

public class ProductRepository : Repository<Product>, IProductRepository
{
    public ProductRepository(DbContext context) : base(context) { }
    
    public IEnumerable<Product> GetProductsByCategory(string category)
    {
        return dbSet.Where(p => p.Category == category).ToList();
    }
    
    public IEnumerable<Product> GetTopSellingProducts(int count)
    {
        return dbSet.OrderByDescending(p => p.SalesCount)
                   .Take(count)
                   .ToList();
    }
}

// Unit of Work Pattern
public interface IUnitOfWork : IDisposable
{
    IProductRepository Products { get; }
    ICustomerRepository Customers { get; }
    int SaveChanges();
}

public class UnitOfWork : IUnitOfWork
{
    private readonly DbContext context;
    private IProductRepository products;
    private ICustomerRepository customers;
    
    public UnitOfWork(DbContext context)
    {
        this.context = context;
    }
    
    public IProductRepository Products => 
        products ??= new ProductRepository(context);
    
    public ICustomerRepository Customers => 
        customers ??= new CustomerRepository(context);
    
    public int SaveChanges()
    {
        return context.SaveChanges();
    }
    
    public void Dispose()
    {
        context?.Dispose();
    }
}
```

### Observer Pattern

```csharp
// Subject interface
public interface ISubject
{
    void Attach(IObserver observer);
    void Detach(IObserver observer);
    void Notify();
}

// Observer interface
public interface IObserver
{
    void Update(ISubject subject);
}

// Concrete Subject
public class Stock : ISubject
{
    private decimal _price;
    private List<IObserver> observers = new List<IObserver>();
    
    public string Symbol { get; set; }
    
    public decimal Price
    {
        get => _price;
        set
        {
            _price = value;
            Notify();
        }
    }
    
    public void Attach(IObserver observer)
    {
        observers.Add(observer);
    }
    
    public void Detach(IObserver observer)
    {
        observers.Remove(observer);
    }
    
    public void Notify()
    {
        foreach (var observer in observers)
        {
            observer.Update(this);
        }
    }
}

// Concrete Observers
public class StockDisplay : IObserver
{
    public void Update(ISubject subject)
    {
        if (subject is Stock stock)
        {
            Console.WriteLine($"Display: {stock.Symbol} = ${stock.Price}");
        }
    }
}

public class StockAlert : IObserver
{
    private decimal threshold;
    
    public StockAlert(decimal threshold)
    {
        this.threshold = threshold;
    }
    
    public void Update(ISubject subject)
    {
        if (subject is Stock stock && stock.Price > threshold)
        {
            Console.WriteLine($"ALERT: {stock.Symbol} exceeded ${threshold}!");
        }
    }
}
```

---

## 13. SOLID PRENSİPLERİ DETAYLI

### S - Single Responsibility Principle

```csharp
// YANLIŞ - Birden fazla sorumluluk
public class Employee_Wrong
{
    public string Name { get; set; }
    public decimal Salary { get; set; }
    
    public void SaveToDatabase()
    {
        // Database işlemi - YANLIŞ
    }
    
    public void SendEmail()
    {
        // Email işlemi - YANLIŞ
    }
    
    public void CalculateSalary()
    {
        // Maaş hesaplama - YANLIŞ
    }
}

// DOĞRU - Tek sorumluluk
public class Employee
{
    public string Name { get; set; }
    public decimal Salary { get; set; }
}

public class EmployeeRepository
{
    public void Save(Employee employee)
    {
        // Sadece database işlemi
    }
}

public class EmailService
{
    public void SendEmail(Employee employee)
    {
        // Sadece email işlemi
    }
}

public class SalaryCalculator
{
    public decimal Calculate(Employee employee)
    {
        // Sadece maaş hesaplama
        return employee.Salary;
    }
}
```

### O - Open/Closed Principle

```csharp
// YANLIŞ - Değişikliğe açık
public class AreaCalculator_Wrong
{
    public double CalculateArea(object shape)
    {
        if (shape is Rectangle r)
            return r.Width * r.Height;
        else if (shape is Circle c)
            return Math.PI * c.Radius * c.Radius;
        // Her yeni şekil için if eklemek gerekir - YANLIŞ
        return 0;
    }
}

// DOĞRU - Genişletmeye açık, değişikliğe kapalı
public abstract class Shape
{
    public abstract double CalculateArea();
}

public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }
    
    public override double CalculateArea()
    {
        return Width * Height;
    }
}

public class Circle : Shape
{
    public double Radius { get; set; }
    
    public override double CalculateArea()
    {
        return Math.PI * Radius * Radius;
    }
}

// Yeni şekil eklemek için sadece yeni class
public class Triangle : Shape
{
    public double Base { get; set; }
    public double Height { get; set; }
    
    public override double CalculateArea()
    {
        return 0.5 * Base * Height;
    }
}
```

### L - Liskov Substitution Principle

```csharp
// YANLIŞ - Alt sınıf üst sınıf yerine kullanılamıyor
public class Bird
{
    public virtual void Fly()
    {
        Console.WriteLine("Flying...");
    }
}

public class Penguin : Bird  // YANLIŞ
{
    public override void Fly()
    {
        throw new NotSupportedException("Penguins can't fly!");
    }
}

// DOĞRU - Alt sınıflar üst sınıf yerine kullanılabilir
public abstract class Bird
{
    public abstract void Move();
}

public interface IFlyable
{
    void Fly();
}

public class Eagle : Bird, IFlyable
{
    public override void Move()
    {
        Console.WriteLine("Eagle moving...");
    }
    
    public void Fly()
    {
        Console.WriteLine("Eagle flying...");
    }
}

public class Penguin : Bird
{
    public override void Move()
    {
        Console.WriteLine("Penguin swimming...");
    }
}
```

### I - Interface Segregation Principle

```csharp
// YANLIŞ - Büyük interface
public interface IWorker_Wrong
{
    void Work();
    void Eat();
    void Sleep();
    void GetPaid();
}

public class Robot : IWorker_Wrong  // YANLIŞ
{
    public void Work() { }
    public void Eat() 
    { 
        throw new NotImplementedException(); // Robot yemez!
    }
    public void Sleep() 
    { 
        throw new NotImplementedException(); // Robot uyumaz!
    }
    public void GetPaid() { }
}

// DOĞRU - Küçük, özel interface'ler
public interface IWorkable
{
    void Work();
}

public interface IEatable
{
    void Eat();
}

public interface ISleepable
{
    void Sleep();
}

public interface IPayable
{
    void GetPaid();
}

public class Human : IWorkable, IEatable, ISleepable, IPayable
{
    public void Work() { }
    public void Eat() { }
    public void Sleep() { }
    public void GetPaid() { }
}

public class Robot : IWorkable, IPayable
{
    public void Work() { }
    public void GetPaid() { }
}
```

### D - Dependency Inversion Principle

```csharp
// YANLIŞ - Üst seviye sınıf alt seviyeye bağımlı
public class EmailService_Wrong
{
    public void SendEmail(string message)
    {
        // Email gönderme
    }
}

public class NotificationService_Wrong
{
    private EmailService_Wrong emailService = new EmailService_Wrong();
    
    public void Send(string message)
    {
        emailService.SendEmail(message);  // Concrete class'a bağımlı
    }
}

// DOĞRU - Abstraction'a bağımlı
public interface IMessageService
{
    void SendMessage(string message);
}

public class EmailService : IMessageService
{
    public void SendMessage(string message)
    {
        Console.WriteLine($"Email: {message}");
    }
}

public class SmsService : IMessageService
{
    public void SendMessage(string message)
    {
        Console.WriteLine($"SMS: {message}");
    }
}

public class NotificationService
{
    private readonly IMessageService messageService;
    
    public NotificationService(IMessageService messageService)
    {
        this.messageService = messageService;  // Interface'e bağımlı
    }
    
    public void Send(string message)
    {
        messageService.SendMessage(message);
    }
}

// Dependency Injection Container kullanımı
public class DIContainer
{
    private Dictionary<Type, Type> mappings = new Dictionary<Type, Type>();
    
    public void Register<TInterface, TImplementation>()
    {
        mappings[typeof(TInterface)] = typeof(TImplementation);
    }
    
    public T Resolve<T>()
    {
        return (T)Resolve(typeof(T));
    }
    
    private object Resolve(Type type)
    {
        var implementationType = mappings[type];
        var constructor = implementationType.GetConstructors().First();
        var parameters = constructor.GetParameters()
            .Select(p => Resolve(p.ParameterType))
            .ToArray();
        
        return Activator.CreateInstance(implementationType, parameters);
    }
}
```

---

## 14. STRING İŞLEMLERİ DETAYLI

### String Metodları

```csharp
public class StringOperations
{
    public void BasicMethods()
    {
        string text = "  Hello World  ";
        
        // Trim operations
        string trimmed = text.Trim();           // "Hello World"
        string trimStart = text.TrimStart();    // "Hello World  "
        string trimEnd = text.TrimEnd();        // "  Hello World"
        
        // Case operations
        string upper = text.ToUpper();          // "  HELLO WORLD  "
        string lower = text.ToLower();          // "  hello world  "
        string title = CultureInfo.CurrentCulture.TextInfo.ToTitleCase(text.ToLower());
        
        // Substring
        string sub1 = text.Substring(2);        // "Hello World  "
        string sub2 = text.Substring(2, 5);     // "Hello"
        
        // Replace
        string replaced = text.Replace("World", "Universe");
        string replacedChar = text.Replace(' ', '_');
        
        // Split
        string csv = "apple,banana,orange";
        string[] fruits = csv.Split(',');
        string[] words = text.Split(new[] { ' ' }, StringSplitOptions.RemoveEmptyEntries);
        
        // Join
        string joined = string.Join(", ", fruits);
        
        // Contains, StartsWith, EndsWith
        bool contains = text.Contains("Hello");
        bool starts = text.StartsWith("  He");
        bool ends = text.EndsWith("ld  ");
        
        // IndexOf, LastIndexOf
        int firstIndex = text.IndexOf("o");     // 6
        int lastIndex = text.LastIndexOf("o");  // 9
        int notFound = text.IndexOf("z");       // -1
        
        // Insert, Remove
        string inserted = text.Insert(7, "Big ");
        string removed = text.Remove(7, 5);     // Remove "World"
        
        // PadLeft, PadRight
        string padded = "123".PadLeft(5, '0');  // "00123"
        string paddedR = "123".PadRight(5, '*'); // "123**"
    }
    
    // String formatting
    public void Formatting()
    {
        int number = 42;
        decimal price = 123.456m;
        DateTime date = DateTime.Now;
        
        // String interpolation
        string interp = $"Number: {number}, Price: {price:C2}, Date: {date:yyyy-MM-dd}";
        
        // String.Format
        string formatted = string.Format("Number: {0}, Price: {1:C2}", number, price);
        
        // Composite formatting
        string composite = string.Format("{0,-10} {1,10:C2}", "Product", price);
        
        // Custom formats
        string custom = number.ToString("D5");           // "00042"
        string percent = (0.123).ToString("P");         // "12.30%"
        string hex = number.ToString("X");              // "2A"
        
        // StringBuilder for performance
        var sb = new StringBuilder();
        for (int i = 0; i < 1000; i++)
        {
            sb.Append("Item ");
            sb.Append(i);
            sb.AppendLine();
        }
        string result = sb.ToString();
    }
    
    // String comparison
    public void Comparison()
    {
        string s1 = "Hello";
        string s2 = "hello";
        
        // Case-sensitive
        bool equal1 = s1 == s2;                         // false
        bool equal2 = s1.Equals(s2);                    // false
        
        // Case-insensitive
        bool equal3 = s1.Equals(s2, StringComparison.OrdinalIgnoreCase);
        bool equal4 = string.Compare(s1, s2, true) == 0;
        
        // Culture-specific
        bool equal5 = s1.Equals(s2, StringComparison.CurrentCulture);
        
        // Compare
        int comp1 = string.Compare(s1, s2);             // > 0
        int comp2 = string.CompareOrdinal(s1, s2);      // < 0
        
        // CompareTo
        int comp3 = s1.CompareTo(s2);                   // < 0
    }
}
```

### Regular Expressions

```csharp
using System.Text.RegularExpressions;

public class RegexExamples
{
    public void BasicRegex()
    {
        // Email validation
        string emailPattern = @"^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$";
        string email = "test@example.com";
        bool isValidEmail = Regex.IsMatch(email, emailPattern);
        
        // Phone number
        string phonePattern = @"^\+?(\d{1,3})?[-. ]?\(?(\d{3})\)?[-. ]?(\d{3})[-. ]?(\d{4})$";
        string phone = "+1 (555) 123-4567";
        bool isValidPhone = Regex.IsMatch(phone, phonePattern);
        
        // Extract numbers
        string text = "The price is $123.45 and quantity is 10";
        MatchCollection numbers = Regex.Matches(text, @"\d+\.?\d*");
        
        // Replace
        string cleaned = Regex.Replace(text, @"[^\w\s]", "");
        
        // Split
        string[] parts = Regex.Split(text, @"\s+");
        
        // Groups
        string pattern = @"(\w+)@(\w+)\.(\w+)";
        Match match = Regex.Match("user@domain.com", pattern);
        if (match.Success)
        {
            string username = match.Groups[1].Value;  // "user"
            string domain = match.Groups[2].Value;    // "domain"
            string tld = match.Groups[3].Value;       // "com"
        }
    }
}
```

---

## 15. DATETIME İŞLEMLERİ

```csharp
public class DateTimeOperations
{
    public void BasicOperations()
    {
        // Creating DateTime
        DateTime now = DateTime.Now;              // Local time
        DateTime utcNow = DateTime.UtcNow;       // UTC time
        DateTime today = DateTime.Today;         // Today at 00:00
        DateTime specific = new DateTime(2025, 10, 27, 14, 30, 0);
        
        // Properties
        int year = now.Year;
        int month = now.Month;
        int day = now.Day;
        int hour = now.Hour;
        int minute = now.Minute;
        int second = now.Second;
        int dayOfYear = now.DayOfYear;
        DayOfWeek dayOfWeek = now.DayOfWeek;
        
        // Add operations
        DateTime tomorrow = now.AddDays(1);
        DateTime nextWeek = now.AddDays(7);
        DateTime nextMonth = now.AddMonths(1);
        DateTime nextYear = now.AddYears(1);
        DateTime later = now.AddHours(2.5);
        
        // Subtract operations
        TimeSpan difference = tomorrow - now;
        DateTime yesterday = now.Subtract(TimeSpan.FromDays(1));
        
        // Comparison
        bool isBefore = yesterday < now;
        bool isAfter = tomorrow > now;
        bool isEqual = now == now;
        int comparison = DateTime.Compare(yesterday, tomorrow);
        
        // Formatting
        string format1 = now.ToString("yyyy-MM-dd");
        string format2 = now.ToString("dd/MM/yyyy HH:mm:ss");
        string format3 = now.ToString("MMMM dd, yyyy");
        string format4 = now.ToString("dddd, dd MMMM yyyy");
        string shortDate = now.ToShortDateString();
        string longDate = now.ToLongDateString();
        string shortTime = now.ToShortTimeString();
        string longTime = now.ToLongTimeString();
        
        // Parsing
        DateTime parsed1 = DateTime.Parse("2025-10-27");
        DateTime parsed2 = DateTime.ParseExact("27/10/2025", "dd/MM/yyyy", null);
        
        DateTime result;
        bool success = DateTime.TryParse("2025-10-27", out result);
    }
    
    // TimeSpan operations
    public void TimeSpanOperations()
    {
        TimeSpan span1 = new TimeSpan(2, 30, 0);        // 2 hours, 30 minutes
        TimeSpan span2 = TimeSpan.FromDays(1.5);
        TimeSpan span3 = TimeSpan.FromHours(2.5);
        TimeSpan span4 = TimeSpan.FromMinutes(90);
        
        // Properties
        double totalDays = span2.TotalDays;
        double totalHours = span2.TotalHours;
        double totalMinutes = span2.TotalMinutes;
        int days = span2.Days;
        int hours = span2.Hours;
        int minutes = span2.Minutes;
        
        // Operations
        TimeSpan sum = span1 + span2;
        TimeSpan diff = span2 - span1;
        TimeSpan doubled = span1.Multiply(2);
        TimeSpan halved = span1.Divide(2);
        
        // Age calculation
        DateTime birthDate = new DateTime(1990, 5, 15);
        int age = DateTime.Now.Year - birthDate.Year;
        if (DateTime.Now.DayOfYear < birthDate.DayOfYear)
            age--;
    }
    
    // DateOnly and TimeOnly (C# 10+)
    public void DateOnlyTimeOnly()
    {
        DateOnly date = DateOnly.FromDateTime(DateTime.Now);
        DateOnly specificDate = new DateOnly(2025, 10, 27);
        
        TimeOnly time = TimeOnly.FromDateTime(DateTime.Now);
        TimeOnly specificTime = new TimeOnly(14, 30, 0);
        
        // Combine
        DateTime combined = date.ToDateTime(time);
    }
}
```

---

## 16. FILE I/O İŞLEMLERİ

```csharp
using System.IO;

public class FileOperations
{
    // Text file operations
    public void TextFileOperations()
    {
        string filePath = @"C:\temp\test.txt";
        
        // Write text
        File.WriteAllText(filePath, "Hello World");
        
        // Append text
        File.AppendAllText(filePath, "\nNew line");
        
        // Write lines
        string[] lines = { "Line 1", "Line 2", "Line 3" };
        File.WriteAllLines(filePath, lines);
        
        // Read all text
        string content = File.ReadAllText(filePath);
        
        // Read all lines
        string[] allLines = File.ReadAllLines(filePath);
        
        // Read line by line (memory efficient)
        foreach (string line in File.ReadLines(filePath))
        {
            Console.WriteLine(line);
        }
        
        // Using StreamWriter
        using (StreamWriter writer = new StreamWriter(filePath))
        {
            writer.WriteLine("Line 1");
            writer.WriteLine("Line 2");
        }
        
        // Using StreamReader
        using (StreamReader reader = new StreamReader(filePath))
        {
            string line;
            while ((line = reader.ReadLine()) != null)
            {
                Console.WriteLine(line);
            }
        }
    }
    
    // Binary file operations
    public void BinaryFileOperations()
    {
        string filePath = @"C:\temp\data.bin";
        
        // Write binary
        using (BinaryWriter writer = new BinaryWriter(File.Open(filePath, FileMode.Create)))
        {
            writer.Write(123);        // int
            writer.Write(456.78);     // double
            writer.Write("Hello");    // string
            writer.Write(true);       // bool
        }
        
        // Read binary
        using (BinaryReader reader = new BinaryReader(File.Open(filePath, FileMode.Open)))
        {
            int intValue = reader.ReadInt32();
            double doubleValue = reader.ReadDouble();
            string stringValue = reader.ReadString();
            bool boolValue = reader.ReadBoolean();
        }
        
        // Read/Write bytes
        byte[] data = { 0x01, 0x02, 0x03, 0x04 };
        File.WriteAllBytes(filePath, data);
        byte[] readData = File.ReadAllBytes(filePath);
    }
    
    // File and Directory operations
    public void FileDirectoryOperations()
    {
        string filePath = @"C:\temp\test.txt";
        string dirPath = @"C:\temp\newdir";
        
        // File operations
        bool exists = File.Exists(filePath);
        File.Copy(filePath, @"C:\temp\copy.txt", overwrite: true);
        File.Move(filePath, @"C:\temp\moved.txt");
        File.Delete(filePath);
        
        // File info
        FileInfo fileInfo = new FileInfo(filePath);
        long size = fileInfo.Length;
        DateTime created = fileInfo.CreationTime;
        DateTime modified = fileInfo.LastWriteTime;
        string extension = fileInfo.Extension;
        string directory = fileInfo.DirectoryName;
        
        // Directory operations
        Directory.CreateDirectory(dirPath);
        bool dirExists = Directory.Exists(dirPath);
        Directory.Move(dirPath, @"C:\temp\moveddir");
        Directory.Delete(dirPath, recursive: true);
        
        // Get files and directories
        string[] files = Directory.GetFiles(@"C:\temp", "*.txt");
        string[] dirs = Directory.GetDirectories(@"C:\temp");
        string[] allFiles = Directory.GetFiles(@"C:\temp", "*.*", SearchOption.AllDirectories);
        
        // DirectoryInfo
        DirectoryInfo dirInfo = new DirectoryInfo(@"C:\temp");
        FileInfo[] fileInfos = dirInfo.GetFiles();
        DirectoryInfo[] subDirs = dirInfo.GetDirectories();
    }
    
    // Path operations
    public void PathOperations()
    {
        string path = @"C:\temp\subfolder\file.txt";
        
        string fileName = Path.GetFileName(path);           // "file.txt"
        string fileNameNoExt = Path.GetFileNameWithoutExtension(path); // "file"
        string extension = Path.GetExtension(path);         // ".txt"
        string directory = Path.GetDirectoryName(path);     // "C:\temp\subfolder"
        
        string combined = Path.Combine(@"C:\temp", "subfolder", "file.txt");
        string fullPath = Path.GetFullPath("file.txt");
        string tempPath = Path.GetTempPath();
        string tempFile = Path.GetTempFileName();
        
        char separator = Path.DirectorySeparatorChar;       // '\' on Windows
        char[] invalidChars = Path.GetInvalidFileNameChars();
    }
}
```

---

## 17. ASYNC/AWAIT TEMELLER

```csharp
public class AsyncProgramming
{
    // Basic async/await
    public async Task<string> GetDataAsync()
    {
        await Task.Delay(1000);  // Simulate async operation
        return "Data retrieved";
    }
    
    // Async void - sadece event handler'larda kullan
    private async void Button_Click(object sender, EventArgs e)
    {
        await DoWorkAsync();
    }
    
    // Multiple async operations
    public async Task MultipleOperationsAsync()
    {
        // Sequential execution
        var result1 = await GetDataAsync();
        var result2 = await GetDataAsync();
        
        // Parallel execution
        Task<string> task1 = GetDataAsync();
        Task<string> task2 = GetDataAsync();
        
        string[] results = await Task.WhenAll(task1, task2);
        
        // First completed
        Task<string> firstCompleted = await Task.WhenAny(task1, task2);
    }
    
    // File I/O async
    public async Task FileOperationsAsync()
    {
        string filePath = "test.txt";
        
        // Write async
        await File.WriteAllTextAsync(filePath, "Hello async");
        
        // Read async
        string content = await File.ReadAllTextAsync(filePath);
        
        // Stream async
        using (StreamWriter writer = new StreamWriter(filePath))
        {
            await writer.WriteLineAsync("Async line");
        }
    }
    
    // HttpClient example
    public async Task<string> DownloadAsync(string url)
    {
        using (HttpClient client = new HttpClient())
        {
            HttpResponseMessage response = await client.GetAsync(url);
            response.EnsureSuccessStatusCode();
            return await response.Content.ReadAsStringAsync();
        }
    }
    
    // Task.Run for CPU-bound operations
    public async Task<int> CalculateAsync()
    {
        return await Task.Run(() =>
        {
            // Heavy CPU work
            int result = 0;
            for (int i = 0; i < 1000000; i++)
            {
                result += i;
            }
            return result;
        });
    }
    
    // Cancellation token
    public async Task CancellableOperationAsync(CancellationToken token)
    {
        for (int i = 0; i < 10; i++)
        {
            token.ThrowIfCancellationRequested();
            await Task.Delay(1000, token);
            Console.WriteLine($"Step {i}");
        }
    }
    
    // Progress reporting
    public async Task DownloadWithProgressAsync(IProgress<int> progress)
    {
        for (int i = 0; i <= 100; i += 10)
        {
            await Task.Delay(100);
            progress?.Report(i);
        }
    }
}

// Usage example
public async Task MainAsync()
{
    var asyncOps = new AsyncProgramming();
    
    // Basic usage
    string data = await asyncOps.GetDataAsync();
    
    // With cancellation
    var cts = new CancellationTokenSource();
    cts.CancelAfter(5000);  // Cancel after 5 seconds
    
    try
    {
        await asyncOps.CancellableOperationAsync(cts.Token);
    }
    catch (OperationCanceledException)
    {
        Console.WriteLine("Operation cancelled");
    }
    
    // With progress
    var progress = new Progress<int>(percent =>
    {
        Console.WriteLine($"Progress: {percent}%");
    });
    
    await asyncOps.DownloadWithProgressAsync(progress);
}
```

---

## 18. BEST PRACTICES VE CODE SMELLS

### Best Practices

```csharp
// 1. NAMING CONVENTIONS
// PascalCase for classes, methods, properties
public class CustomerService
{
    public string FirstName { get; set; }
    public void ProcessOrder() { }
}

// camelCase for variables, parameters
public void CalculateTotal(decimal unitPrice, int quantity)
{
    decimal totalAmount = unitPrice * quantity;
}

// 2. NULL HANDLING
// Always check for null
public void ProcessData(string data)
{
    if (string.IsNullOrWhiteSpace(data))
        throw new ArgumentNullException(nameof(data));
    
    // or use null-conditional
    int? length = data?.Length;
}

// 3. USING STATEMENTS for IDisposable
// Always dispose resources
public void ReadFile(string path)
{
    using (var reader = new StreamReader(path))
    {
        // Use reader
    } // Automatically disposed
}

// 4. CONST vs READONLY
public class Constants
{
    // Compile-time constant
    public const double Pi = 3.14159;
    
    // Runtime constant
    public readonly DateTime CreatedAt = DateTime.Now;
}

// 5. AVOID MAGIC NUMBERS
// BAD
if (age > 18) { }

// GOOD
const int LegalAge = 18;
if (age > LegalAge) { }

// 6. GUARD CLAUSES
public void ProcessOrder(Order order)
{
    // Early returns
    if (order == null) return;
    if (order.Items.Count == 0) return;
    if (order.TotalAmount <= 0) return;
    
    // Main logic here
}

// 7. SINGLE RETURN POINT vs EARLY RETURNS
// Early returns (preferred for guard clauses)
public string GetStatus(int value)
{
    if (value < 0) return "Negative";
    if (value == 0) return "Zero";
    if (value > 100) return "Large";
    return "Normal";
}

// 8. AVOID DEEP NESTING
// BAD
if (condition1)
{
    if (condition2)
    {
        if (condition3)
        {
            // Deep nesting
        }
    }
}

// GOOD
if (!condition1) return;
if (!condition2) return;
if (!condition3) return;
// Main logic

// 9. PREFER COMPOSITION OVER INHERITANCE
public interface ILogger { void Log(string message); }
public interface IEmailer { void SendEmail(string to, string message); }

public class NotificationService
{
    private readonly ILogger logger;
    private readonly IEmailer emailer;
    
    public NotificationService(ILogger logger, IEmailer emailer)
    {
        this.logger = logger;
        this.emailer = emailer;
    }
}

// 10. IMMUTABILITY
public class ImmutablePerson
{
    public string Name { get; }
    public int Age { get; }
    
    public ImmutablePerson(string name, int age)
    {
        Name = name;
        Age = age;
    }
    
    // Return new instance for changes
    public ImmutablePerson WithAge(int newAge)
    {
        return new ImmutablePerson(Name, newAge);
    }
}
```

### Code Smells

```csharp
// 1. LONG METHOD - Kötü
public void ProcessOrderBad()
{
    // 100+ lines of code
    // Validate
    // Calculate
    // Save
    // Send email
    // Log
}

// İyi - Küçük metodlara böl
public void ProcessOrderGood()
{
    ValidateOrder();
    CalculateTotal();
    SaveOrder();
    SendConfirmationEmail();
    LogOrderProcessing();
}

// 2. LARGE CLASS - Kötü
public class GodClass
{
    // 50+ methods
    // 100+ properties
    // Multiple responsibilities
}

// 3. DUPLICATE CODE - Kötü
public decimal CalculateTax1(decimal amount)
{
    return amount * 0.18m;
}

public decimal CalculateTax2(decimal total)
{
    return total * 0.18m;  // Duplicate
}

// İyi - DRY principle
private const decimal TaxRate = 0.18m;
public decimal CalculateTax(decimal amount)
{
    return amount * TaxRate;
}

// 4. DEAD CODE - Kötü
public void Method()
{
    int unused = 10;  // Never used
    // return;  // Unreachable code below
    Console.WriteLine("Never executed");
}

// 5. FEATURE ENVY - Kötü
public class Order
{
    public Customer Customer { get; set; }
    
    public string GetCustomerInfo()
    {
        // Order class accessing too many Customer properties
        return $"{Customer.FirstName} {Customer.LastName} " +
               $"{Customer.Address.Street} {Customer.Address.City}";
    }
}

// İyi
public class Customer
{
    public string GetFullInfo()
    {
        return $"{FirstName} {LastName} {Address.GetFullAddress()}";
    }
}

// 6. INAPPROPRIATE INTIMACY - Kötü
public class ClassA
{
    private ClassB b;
    
    public void Method()
    {
        b._privateField = 10;  // Accessing private members
    }
}

// 7. PRIMITIVE OBSESSION - Kötü
public void CreateUser(string name, string email, string phone, 
                      string address, string city, string zip)
{
    // Too many primitive parameters
}

// İyi - Use objects
public class UserInfo
{
    public string Name { get; set; }
    public Email Email { get; set; }
    public PhoneNumber Phone { get; set; }
    public Address Address { get; set; }
}

// 8. SWITCH STATEMENTS - Kötü (çok tekrarlanıyorsa)
public double CalculateArea(string shapeType, double... parameters)
{
    switch (shapeType)
    {
        case "Circle": // ...
        case "Rectangle": // ...
        case "Triangle": // ...
    }
}

// İyi - Polymorphism kullan
public abstract class Shape
{
    public abstract double CalculateArea();
}
```

---

## 19. PERFORMANCE TIPS

### String Performance

```csharp
// 1. StringBuilder for concatenation
// KÖTÜ - O(n²) complexity
string result = "";
for (int i = 0; i < 10000; i++)
{
    result += i.ToString();  // Her seferinde yeni string
}

// İYİ - O(n) complexity
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 10000; i++)
{
    sb.Append(i);
}
string result = sb.ToString();

// 2. String.IsNullOrEmpty vs string.IsNullOrWhiteSpace
// Daha hızlı
if (string.IsNullOrEmpty(str)) { }

// Daha yavaş ama daha güvenli
if (string.IsNullOrWhiteSpace(str)) { }

// 3. StringComparison.Ordinal for performance
// YAVAŞ
bool equal = str1.ToLower() == str2.ToLower();

// HIZLI
bool equal = string.Equals(str1, str2, StringComparison.OrdinalIgnoreCase);
```

### Collection Performance

```csharp
// 1. Capacity belirleme
// KÖTÜ - Multiple resizing
List<int> list = new List<int>();
for (int i = 0; i < 10000; i++)
    list.Add(i);

// İYİ - No resizing
List<int> list = new List<int>(10000);
for (int i = 0; i < 10000; i++)
    list.Add(i);

// 2. Doğru collection seçimi
// Sık ekleme/silme: LinkedList
// Index erişimi: List
// Key-based erişim: Dictionary
// Unique items: HashSet
// Sorted items: SortedSet

// 3. LINQ performance
// YAVAŞ - Multiple iterations
var list = data.Where(x => x > 5).ToList();
var count = list.Count();
var sum = list.Sum();

// HIZLI - Single iteration
var result = data.Where(x => x > 5);
var materialized = result.ToList();
var count = materialized.Count;
var sum = materialized.Sum();

// 4. foreach vs for
// foreach - daha okunaklı
foreach (var item in collection)
{
    Process(item);
}

// for - array'lerde daha hızlı
for (int i = 0; i < array.Length; i++)
{
    Process(array[i]);
}
```

### Memory Performance

```csharp
// 1. Struct vs Class
// Struct - Stack allocation (hızlı)
public struct Point2D
{
    public double X, Y;
}

// Class - Heap allocation (yavaş)
public class Point2DClass
{
    public double X, Y;
}

// 2. Object pooling
public class ObjectPool<T> where T : new()
{
    private readonly Stack<T> pool = new Stack<T>();
    
    public T Rent()
    {
        return pool.Count > 0 ? pool.Pop() : new T();
    }
    
    public void Return(T item)
    {
        pool.Push(item);
    }
}

// 3. Span<T> for performance (C# 7.2+)
public void ProcessArray(Span<int> data)
{
    // No allocation, works on stack
    for (int i = 0; i < data.Length; i++)
    {
        data[i] *= 2;
    }
}

// 4. ArrayPool kullanımı
var pool = ArrayPool<byte>.Shared;
byte[] buffer = pool.Rent(1024);
try
{
    // Use buffer
}
finally
{
    pool.Return(buffer);
}
```

---

## 20. UNIT TEST TEMELLER

### Basic Unit Test

```csharp
using NUnit.Framework; // veya using Microsoft.VisualStudio.TestTools.UnitTesting;

[TestFixture] // [TestClass] for MSTest
public class CalculatorTests
{
    private Calculator calculator;
    
    [SetUp] // [TestInitialize] for MSTest
    public void Setup()
    {
        calculator = new Calculator();
    }
    
    [Test] // [TestMethod] for MSTest
    public void Add_TwoNumbers_ReturnsSum()
    {
        // Arrange
        int a = 5;
        int b = 3;
        int expected = 8;
        
        // Act
        int result = calculator.Add(a, b);
        
        // Assert
        Assert.AreEqual(expected, result);
    }
    
    [Test]
    [TestCase(1, 2, 3)]
    [TestCase(-1, -1, -2)]
    [TestCase(0, 0, 0)]
    public void Add_MultipleTestCases(int a, int b, int expected)
    {
        int result = calculator.Add(a, b);
        Assert.AreEqual(expected, result);
    }
    
    [Test]
    public void Divide_ByZero_ThrowsException()
    {
        Assert.Throws<DivideByZeroException>(() => 
        {
            calculator.Divide(10, 0);
        });
    }
    
    [TearDown] // [TestCleanup] for MSTest
    public void Cleanup()
    {
        calculator = null;
    }
}

// Test Doubles
public interface IEmailService
{
    void SendEmail(string to, string message);
}

public class OrderService
{
    private readonly IEmailService emailService;
    
    public OrderService(IEmailService emailService)
    {
        this.emailService = emailService;
    }
    
    public void ProcessOrder(Order order)
    {
        // Process order
        emailService.SendEmail(order.CustomerEmail, "Order confirmed");
    }
}

[TestFixture]
public class OrderServiceTests
{
    [Test]
    public void ProcessOrder_SendsEmail()
    {
        // Arrange
        var mockEmailService = new MockEmailService();
        var orderService = new OrderService(mockEmailService);
        var order = new Order { CustomerEmail = "test@test.com" };
        
        // Act
        orderService.ProcessOrder(order);
        
        // Assert
        Assert.IsTrue(mockEmailService.EmailSent);
        Assert.AreEqual("test@test.com", mockEmailService.LastRecipient);
    }
}

public class MockEmailService : IEmailService
{
    public bool EmailSent { get; private set; }
    public string LastRecipient { get; private set; }
    
    public void SendEmail(string to, string message)
    {
        EmailSent = true;
        LastRecipient = to;
    }
}

// AAA Pattern
[Test]
public void TestMethod()
{
    // Arrange - Hazırlık
    var input = "test";
    var expected = "TEST";
    
    // Act - Çalıştır
    var result = input.ToUpper();
    
    // Assert - Doğrula
    Assert.AreEqual(expected, result);
}
```

---

## SONUÇ - KRİTİK NOKTALAR

### Sınavda Dikkat Edilmesi Gerekenler

1. **LINQ kullanımı** - Where, Select, OrderBy, GroupBy
2. **Delegate/Action/Func** farkları
3. **Event tanımlama ve kullanma**
4. **Nullable handling** - ??, ?., !, ??=
5. **Stack vs Heap** - Value types vs Reference types
6. **Boxing/Unboxing** performans etkileri
7. **IEnumerable ve yield** kullanımı
8. **Reflection** ile runtime type manipulation
9. **SOLID prensipleri** - Özellikle Single Responsibility ve Dependency Inversion
10. **String operations** - StringBuilder ne zaman kullanılır
11. **DateTime formatting** ve operations
12. **File I/O** - using statement kullanımı
13. **async/await** basic patterns
14. **Best practices** - Naming, null checks, early returns
15. **Performance** - Collection seçimi, string concatenation

### Case Çözüm Stratejisi

1. **Requirement Analysis** - Ne isteniyor?
2. **Class Design** - Hangi class'lar gerekli?
3. **Relationships** - IS-A, HAS-A ilişkileri
4. **Interfaces/Abstract** - Ortak davranışlar
5. **Encapsulation** - Private fields, public properties
6. **Validation** - Input kontrolü
7. **Error Handling** - Try-catch kullanımı
8. **SOLID Principles** - Single responsibility
9. **Collections** - List, Dictionary kullanımı
10. **LINQ** - Data manipulation

**Başarılar! 🚀**
