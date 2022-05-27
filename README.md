# Java 8 ile gelen yenilikler

## - Lambda Expression
### Genel olarak programlama dilinde Lambda ifadesi bir anonim fonksiyondur (İsmi ve tanımlayıcısı olmadan parametrelerden ve fonksiyon gövdesinden oluşan bir yapı.). Parametreler ve gövdeyi ayırmak için ok (->) kullanılır.
### Java 8 den önce Runnable new'lemek istediğimizde içindeki anonim run iç metodunu örnekte olduğu gibi yazabiliyorduk.

```java
Runnable runnable = new Runnable(){
    @Override
    public void run(){
        System.out.println("Hello World!");
    }
}
```
### Java 8 ile birlikte gelen Lambda Expressionlar ile birlikte aşağıdaki gibi yazabiliriz.
```java
Runnable runnable = () -> System.out.println("Hello world!")
```



