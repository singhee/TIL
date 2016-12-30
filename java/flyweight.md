# Flyweignt Pattern (경량 패턴) - 속성이 동일한 객체는 공유하자

* 구현하는 방법 2가지 
1) factory class
2) static factory method


### 1. Factory Class
* Image 를 생성하기 위한 클래스
=> 하나만 존재하면 된다. '싱글톤' 인 경우가 많다.

```
class Image {
    private String url;
    public Image(String url) {
        this.url = url;

        System.out.println("Downloading Image");
        try {
            TimeUnit.SECONDS.sleep(2);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public void draw() {
        System.out.println("Draw Image from " + url);
    }
}
```

*Factory
- 장점 : 객체 생성에 관련된 로직을 한곳에 모아서 관리할 수 있다.
- 단점 : 싱글톤의 문제점.  => 강한 결합. 모듈화 X

```
class ImageFactory {
    private Map<String, Image> cache = new WeakHashMap<>();
    // 자바에서 Cache 를 구현할 때는 WeakHashMap 을 많이 사용한다.
    //  => 메모리가 부족해서 GC가 발생되면 캐시의 객체가 수거된다.

    // 1. 오직 하나의 객체를 생성
    private static ImageFactory INSTANCE = new ImageFactory();

    // 2. 언제 어디서든 동일한 방법으로 얻을 수 있는 메소드
    public static ImageFactory getInstance() {
        return INSTANCE;
    }

    // 3. private 생성
    private ImageFactory() {

    }

    public Image create(String url) {
        // return new Image(url);
        if (!cache.containsKey(url))
            cache.put(url, new Image(url));

        return cache.get(url);
    }
}


public class Flyweight {
    public static void main(String[] args) {

        ImageFactory factory = ImageFactory.getInstance();

        Image image1 = factory.create("http://www.foxtail.cc/image1.jpg");
        Image image2 = factory.create("http://www.foxtail.cc/image1.jpg");

        image1.draw();
        image2.draw();
    }
}
```


### 2. static factory method
```
class Image {
    private String url;

    private Image(String url) {
        this.url = url;

        System.out.println("Downloading Image");
        try {
            TimeUnit.SECONDS.sleep(2);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public void draw() {
        System.out.println("Draw Image from " + url);
    }

    private static Map<String, Image> cache = new HashMap<>();

    // Static Factory Method
    // => Effective Java: 정적 팩토리 메소드
    // => iOS, 편의 생성자
    public static Image imageWithURL(String url) {
        if (!cache.containsKey(url))
            cache.put(url, new Image(url));
        return cache.get(url);
    }
}

public class Flyweight {
    public static void main(String[] args) {
        Image image1 = Image.imageWithURL("http://www.foxtail.cc/image1.jpg");
        Image image2 = Image.imageWithURL("http://www.foxtail.cc/image1.jpg");

        image1.draw();
        image2.draw();
    }
}

```
### Flyweight Pattern 고려해야 하는 사항들
1. Immutable Object
2. [COW(Copy On Write)](https://github.com/singhee/TIL/blob/master/etc/cow.md)

