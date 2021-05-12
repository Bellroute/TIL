# 프록시 패턴(Proxy Pattern)

실제 기능을 수행하는 객체(Real Object) 대신 가상의 객체(Proxy Object)를 사용해 로직의 흐름을 제어하는 패턴

![img](https://blog.kakaocdn.net/dn/bmqJ69/btqCcBX3pkU/eSI7crhGbhZvu1Ea0C3aCk/img.png)

- Proxy는 '대리인'이라는 뜻. 사장님한테 사소한 질문을 하기 보다는 비서한테 먼저 물어보는 개념
- 어떤 객체를 사용하고자 할때, 객체를 직접적으로 참조하는 것이 아니라, 해당 객체를 대행하는 객체를 통해 대상객체에 접근하는 방식을 사용 
- 해당 객체가 메모리에 존재하지 않아도 기본적인 정보를 참조하거나 설정 가능
- 실제 객체의 기능이 반드시 필요한 시점까지 객체의 생성을 미룰 수 있음

<br>

### 프록시가 사용되는 대표적인 3가지 경우

#### 가상 프록시

꼭 필요로 하는 시점까지 객체의 생성을 연기하고, 해당 객체가 생성된 것처럼 동작하도록 만들고 싶을때 사용하는 패턴

- 프록시 클래스에서 자잘한 작업들을 처리하고 리소스가 많이 요구되는 작업들이 필요할 때에만 주체 클래스를 사용하도록 구현
- 이미지와 텍스트가 포함된 문서를 열 때, 로딩이 느린 이미지 처리와 비교적 로딩이 빠른 텍스트 처리 작업을 분산하여, 먼저 로딩된 텍스트를 먼저 보여줌

#### 원격 프록시

로컬 환경에 존재하면서 원격 객체에 대한 대변자 역할을 하는 객체를 만들어 서로 다른 주소 공간에 있는 객체에 대해 마치 같은 주소 공간에 있는 것처럼 동작하게 만드는 패턴

- Google Docs - 브라우저는 브라우저대로 필요한 자원을 로컬에 가지고, 또다른 자원은 Google 서버에 있는 형태

![img](http://wiki.gurubee.net/download/attachments/1507415/method_call.jpg)

1. 클라이언트 객체에서 클라이언트 보조객체의 메소드를 호출한다.
2. 클라이언트 보조객체에서는 메소드 호출에 대한 정보(인자, 메소드이름 등)를 잘 포장해서 네트워크를 통해 서비스 보조객체한테 전달한다.
3. 서비스 보조객체에서는 클라이언트 보조객체로 부터 받은 정보를 해석하여 어떤 객체의 어떤 메소드를 호출할지 알아낸 다음 진짜 서비스객체의 '진짜 메소드'를 호출한다.
4. 서비스객체의 메소드가 호출되고, 메소드 실행이 끝나면 서비스 보조객체에 결과를 리턴해준다.
5. 호출결과로 리턴된 정보를 포장해서 서비스 보조객체에서 클라이언트 보조객체한테 전달한다.
6. 클라이언트 보조객체에서는 리턴된 값을 해석하여 클라이언트 객체한테 리턴한다.

#### 보호 프록시

주체 클래스에 대한 접근을 제어하기 위한 경우에 객체에 대한 접근 권한을 제어하거나 객체마다 접근 권한을 달리하고 싶을때 사용하는 패턴

- 프록시 클래스에서 클라이언트가 주체 클래스에 대한 접근을 허용할지 말지 결정하도록 할 수 있음.

<br>

### 프록시 패턴 예제

```java
public interface Image {
   void displayImage();
}
```

```java
public class Real_Image implements Image {

    private String fileName;
    
    public Real_Image(String fileName) {
        this.fileName = fileName;
        loadFromDisk(fileName);
    }
    
    private void loadFromDisk(String fileName) {
        System.out.println("Loading " + fileName);
    }
    
    @Override
    public void displayImage() {
        System.out.println("Displaying " + fileName);
    }
}
```

```java
public class Proxy_Image implements Image {
    private Real_Image realImage;
    private String fileName;
    
    public Proxy_Image(String fileName) {
        this.fileName = fileName;
    }
    
    @Override
    public void displayImage() {
        if (realImage == null) {
            realImage = new Real_Image(fileName);
        }
        realImage.displayImage();
    }
}
```

```java
public class Proxy_Main {
    public static void main(String[] args) {
        Image image1 = new Proxy_Image("test1.png");
        Image image2 = new Proxy_Image("test2.png");
        
        image1.displayImage();
        System.out.println();
        image2.displayImage();
    }
}
```

<br>

### 프록시 패턴의 장단점

#### 장점

- 사이즈가 큰 객체(ex : 이미지)가 로딩되기 전에도 프록시를 통해 참조를 할 수 있다.

- 실제 객체의 public, protected 메소드들을 숨기고 인터페이스를 통해 노출시킬 수 있다. 

- 로컬에 있지 않고 떨어져 있는 객체를 사용할 수 있다. 

- 원래 객체의 접근에 대해서 사전처리를 할 수 있다.

#### 단점

- 객체를 생성할때 한단계를 거치게 되므로, 빈번한 객체 생성이 필요한 경우 성능이 저하될 수 있다.

- 프록시 내부에서 객체 생성을 위해 스레드가 생성, 동기화가 구현되야 하는 경우 성능이 저하될 수 있다.

- 로직이 난해해져 가독성이 떨어질 수 있다.

<br>

#### [참고]

> - https://limkydev.tistory.com/79
> - https://jdm.kr/blog/235
> - https://coding-factory.tistory.com/711