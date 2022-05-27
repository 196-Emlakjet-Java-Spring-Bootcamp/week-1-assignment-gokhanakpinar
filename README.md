# Java 8 ile gelen yenilikler

##  Lambda Expression
Genel olarak programlama dilinde Lambda ifadesi bir anonim fonksiyondur (İsmi ve tanımlayıcısı olmadan parametrelerden ve fonksiyon gövdesinden oluşan bir yapı.). Parametreler ve gövdeyi ayırmak için ok (->) kullanılır.
Java 8 den önce Runnable new'lemek istediğimizde içindeki anonim run iç metodunu örnekte olduğu gibi yazabiliyorduk.

```java
Runnable runnable = new Runnable(){
    @Override
    public void run(){
        System.out.println("Hello World!");
    }
}
```
Java 8 ile birlikte gelen Lambda Expressionlar ile birlikte aşağıdaki gibi yazabiliriz.
```java
Runnable runnable = () -> System.out.println("Hello world!")
```

## Stream API

Java'da Stream bir kaynaktan gelen eleman dizisi olarak tanımlanabilir. Buradaki eleman dizisi Stream'e data sağlayan Collection veya Array olabilir.
**Stream :**
- Veri yapısı değildir.
- Lambda Expressionlar için tasarlanmıştır.
- İndex numaraları ile elemanlarına erişim sağlanmaz.
- Array ve listeler olarak kolayca kümelendirilebilirler.
- Lazy Access desteklerler.
- Stream içerisindeki metodlardan ardışık işletilmesi gerekmeyenler paralel bir şekilde çalıştırılabilir.

**Stream Oluşturma Örnekleri**

- Stream.of()

```java
Stream<Integer> stream = Stream.of(1,2,3,4,5,6,7);
stream.forEach(p -> System.out.println(p));
```

- Stream.of(array)

```java
Stream<Integer> stream = Stream.of(new Integer[]{1,2,3,4,5,6,7});
stream.forEach(p -> System.out.println(p));
```

- List.stream()

```java
List<Integer> list = new ArrayList<Integer>();
for(int i = 0; i < 10; i++){
    list.add(i);
}
Stream<Integer> stream = list.stream();
stream.forEach(p -> System.out.println(p));
```

**Bazı Stream API methodları**

Örneklendirmeler için bir List oluşturalım.

```java
List<String> memberNames = new ArrayList<>();
memberNames.add("Amitabh");
memberNames.add("Shekhar");
memberNames.add("Aman");
memberNames.add("Rahul");
memberNames.add("Shahrukh");
memberNames.add("Salman");
memberNames.add("Yana");
memberNames.add("Lokesh");
```

- Stream.filter()
methodu bir kosul kabul ederek Stream elemanlarini filtreler.

```java
memberNames.stream().filter((s) -> s.startsWith("A"))
                    .forEach(System.out::println);
```
**Output**
```
Amitabh
Aman
```

- Stream.map()
methodu her bir elemani verilen fonksiyona gore baska bir objeye donusturur.

```java
memberNames.stream().filter((s) -> s.startsWith("A"))
                    .map(String::toUpperCase)
                    .forEach(System.out::println);
```
**Output**
```
AMITABH
AMAN
```

- Stream.sorted()
methodu elemanlari siralayacaktir eger biz kendi duzenledigimiz bir Comparator verirsek istedigimiz sekilde siralama yaptirabiliriz.

```java
memberNames.stream().sorted()
                    .map(String::toUpperCase)
                    .forEach(System.out::println);
```
**Output**
```
AMAN
AMITABH
LOKESH
RAHUL
SALMAN
SHAHRUKH
SHEKHAR
YANA
```

- Stream.forEach()
methodu stream elemanlari uzerinde iterate etmemize yarar.
```java
memberNames.forEach(System.out::println);
```

- Stream.collect()
methodu Stream elemanlarini bir collection icine koymamiza yarar.

```java
List<String> memNamesInUppercase = memberNames.stream().sorted()
                            .map(String::toUpperCase)
                            .collect(Collectors.toList());

System.out.print(memNamesInUppercase);
```

**Output**
```java
[AMAN, AMITABH, LOKESH, RAHUL, SALMAN, SHAHRUKH, SHEKHAR, YANA]
```

- Stream.match()
methodu verilen kosul ile eslesen Stream elemani olup olmadigini kontrol eder ve boolean bir deger doner.
```java
boolean matchedResult = memberNames.stream()
        .anyMatch((s) -> s.startsWith("A"));
 
System.out.println(matchedResult);     //true
 
matchedResult = memberNames.stream()
        .allMatch((s) -> s.startsWith("A"));
 
System.out.println(matchedResult);     //false
 
matchedResult = memberNames.stream()
        .noneMatch((s) -> s.startsWith("A"));
 
System.out.println(matchedResult);     //false
```

- Stream.reduce()
methodu verilen fonksiyona gore stream elemanlarini donusturecektir. Sonuc olarak Optinal donecektir cunku reduce() fonksiyonu geriye bos donebilir.

**Ornekler**
```java
String[] array = { "Geeks", "for", "Geeks" };

Optional<String> String_combine = Arrays.stream(array)
                                           .reduce((str1, str2)
                                           -> str1 + "-" + str2);

if (String_combine.isPresent()) {
            System.out.println(String_combine.get());
        }
```

**Output**
```
Geeks-for-Geeks
```

```java
List<Integer> array = Arrays.asList(-2, 0, 4, 6, 8);

int sum = array.stream().reduce(0,
                (element1, element2) -> element1 + element2);

System.out.println("Elemanlarin toplami " + sum);
```

**Output**
```
Elemanlarin toplami 16
```

## Functional interfaceler ve default methodlar

**Functional Interface Nedir ?**
Java 8 de tanitilan Functional Interfaceler iclerinde sadece bir adet abstract methodlari bulunan interfacelere denir.Java 8 den once bu tarz durumlarda anonim ic class olusturmamiz veya bu interface'i implemente etmemiz gerekiyordu asagidaki ornek gibi

```java
new Thread(new Runnable() {
            @Override public void run()
            {
                System.out.println("New thread created");
            }
        }).start();
```

**Output**
```
New thread created
```

Java 8 den sonra Lambda Expressionlar ile birlikte ayni islemi daha basit bir sekilde asagidaki gibi yapabiliriz.

```java
new Thread(() -> {
            System.out.println("New thread created");
        }).start();
```

**Output**
```
New thread created
```

**Java 8 ile Birlikte gelen Default Methods**
Java 8 ile birlikte gelen default methodlar ile birlikte eger methodu override etmediysek varsayilan(default) olarak calisacak olan kod bloklaridir. Bir ornekle anlamaya calisalim.

```java
public interface Moveable {
    default void move(){
        System.out.println("I am moving");
    }
}
```

```java
public class Animal implements Moveable{
    public static void main(String[] args){
        Animal tiger = new Animal();
        tiger.move();
    }
}
```

**Output**
```
I am moving
```

```java
public class Animal implements Moveable{
     
    public void move(){
        System.out.println("I am running");
    }
     
    public static void main(String[] args){
        Animal tiger = new Animal();
        tiger.move();
    }
}
```

**Output**
```
I am running
```