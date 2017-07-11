# Java ve OOP
Bu başlık altında temel düzeyde **OOP(Object Oriented Programming)** notları ve Java ile kullanılım örnekleri yer almaktadır.

### OOP
OOP'nin anlamı nesneye dayalı programlamadır. Her şey nesneler üzerine kurulmuştur. Sınıfları ve nesneleri kullanarak programlar geliştirmemizi sağlayan bir metodolojidir. Bu yöntemi kullanarak uygulama geliştirmek; geliştirme ve bakım işlemlerini kolaylaştırır.

OOP'nin temel kavramlarına bakacak olursak;
* **Inheritance (Kalıtım)**
* **Polymorphism (Çok Biçimlilik)**
* **Encapsulation (Sarmalama)**

### Inheritance 
Bir sınıfın başka bir sınıfın tüm özelliklerini almasıdır. Bu özellik kodun tekrar kullanılabilirliğini ve kalıtım alınan sınıf fonksiyonlarının yeniden yazılmasını **(overriding)** sağlar. **Çalışma zamanında polymorphism** işlemlerinin gerçekleşmesini sağlar.

Java da kalıtım **IS-A** ilişkisine dayalı olmalıdır. Örneğin **kedi bir hayvandır** gibi. Kedi sınıfı hayvan sınıfının özelliklerini kalıtım alır. Java’da kalıtım **extends** kelimesiyle sağlanır.

```java
class A{}

class B extends A{}
```

Java **multiple extends desteklemez** bu **diamond problemine** yol acar. Yani iki sınıfta aynı isme, parametrelere ve dönüş tipine sahip bir fonksiyonumuz olsun. Kalıtım alan sınıftan bu fonksiyona ulaşmak istediğimizde hangi fonksiyonunun çalıştırılacağına karar verilemez.

```java
class A{
    public void run(){}
}

class B{
    public void run(){}
}

class C extends A,B{}
```

Çoklu kalıtım interfaceler yardımıyla sağlanabilir.

Kalıtıma benzer bir yaklaşımda **HAS-A** ilişkisidir. Yani ev sınıfı adres nesnesine sahiptir. Bu sayede adres nesnesinin özelliklerine ev sınıfı içerisinden ulaşılabilir. **IS-A** ilişkisinin kurulamadığı durumlarda kullanılması gerekir. Bu da kodun yeniden kullanılabilirliğini sağlar.

```java
class A{}

class B{
    A a;
}
```

### Polymorphism 
Bir görevin çalışma zamanında farklı yöntemlerle tamamlanabilmesini sağlar. **Upcasting(tip değiştirme)** kullanarak ve **method overriding** ya da **method overloading** yöntemleri ile sağlanır. Çok biçimlilik derleme zamanında ya da çalışma zamanında sağlanabilir. 

#### Çalışma Zamanı Polymorphism 
Method overriding ile sağlanır.

```java

class A {
    public void run() {
        System.out.println("Class A");
    }
}

class B extends A {
    @Override
    public void run() {
        System.out.println("Class B");
    }
}

class Test {
    public static void main(String args[]) {
        
        ...

        A polymorphismObject;
        switch (userInput) {
            case "classA":
                polymorphismObject = new A();
                break;
            case "classB":
                polymorphismObject = new B();//Upcasting
                break;
        }
        //Kullanıcı girişine göre A sınıf tipinde olan polymorphismObject referansı A nesnesini ya da B nesnesini işaret edebiliyor.
        //B nesnesini işaret edebilmesi kalıtım sayesinde mümkündür ve işleme Upcasting denir. 
        polymorphismObject.run();
    }
}
```

Çalışma zamanı polymorphism sınıf değişkenlerine etki etmez. Yani kalıtım alınan sınıfdaki değişken değeri neyse upcasting işlemi sonucunda da aynı değeri elde ederiz.
```java

class A {
    public int testVariable = 10;
}

class B extends A {
    public int testVariable = 50;
}

class Test {
    public static void main(String args[]) {

        A polymorphismObject = new B();//Upcasting
        //Burada A referansı B nesnesini gösterse bile testVariable değeri 10 olarak kalır.
        System.out.println(polymorphismObject.testVariable);
    }
}
```

#### Derleme Zamanı Polymorphism
Method overloading ile sağlanır.

```java

class A {
    public void run(int arg1) {
        System.out.println(arg1);
    }

    public void run(int arg1, int arg2) {
        System.out.println(arg1 + " " + arg2);
    }
}


class Test {
    public static void main(String args[]) {

        int[] userInputs;
        A testObject = new A();
        ...

        //Method Overloading ile run fonksiyonu değişik biçimlerde kullanılabilir. Bu derleme zamanı polymorphism dir.
        testObject.run(userInputs[0]);
        testObject.run(userInputs[0], userInputs[1]);
    }
}
```



### Encapsulation
Bir değişkeni ya da bir metodu sarmalayarak erişim kısıtı sağlanabilir. Erişim belirteçleri kullanılarak bir sınıfın **read only** ya da **write only** olması sağlanabilir. Verinin yönetimini sağlar.

```java
//Read only class
class A {
    private int arg1 = 5;
    private int arg2 = 20;
    private int arg3 = 30;

    public int getArg1() {
        return arg1;
    }

    public int getArg2() {
        return arg2;
    }

    public int getArg3() {
        return arg3;
    }
}


class Test {
    public static void main(String args[]) {
        A testObject = new A();
        testObject.getArg1();
        testObject.getArg2();
        testObject.getArg3();
    }
}
```

Java’da erişim belirteçleri yardımıyla da verilere erişimi kısıtlayabiliriz. Java da paketler sınıfları kategorilemeyi sağlar, isim çakışmalarını önler.

Java erişim belirteçleri:

* **private**: Metot ya da değişkenlere sadece aynı sınıf içerisinden erişime izin verir. Sınıf ve interfaceler private olamaz.

* **default**: Varsayılan erişim belirtecidir. Sınıf, metot ya da değişkenin sadece paket içerisinde erişim sağlanabilir.

* **protected**: Aynı paket içerisinden ve bu sınıftan türetilmiş alt sınıflar tarafından erişilmeyi sağlayan erişim belirleyicisi.

* **public**: Her yerden erişim sağlanabilir.
