# Singleton
Singleton 은 오직 한개의 객체를 생성하고, 언제 어디에서든 동일한 방법으로 접근 가능한 객체를 말한다.

//  전역 변수
//   void foo() {
//      a = 10;
//   }

//   void foo(int* a) {
//      *a = 10;
//   }

// 문제점 2가지
// 1. 위험하다.
//    Reflection, Serialization -> 객체가 2개 이상 생성될 위험성이 있다.
/*
enum Cursor {
    INSTANCE;
}
*/



// 2. JVM Cursor.class + Cursor 객체 생성
//    프로그램 시작에 악영향을 미칠 수 있다.
//  => Lazy Initialization(지연된 초기화)

class Cursor {
    // private static Cursor sInstance;
    private Cursor() {}

    // 문제점 1
    // => Thread safety 보장되지 않는다.
//    public synchronized Cursor getInstance() {
//        if (sInstance == null)
//            sInstance = new Cursor();
//        return sInstance;
//    }

    // 문제점 2
    // => 성능에 좋지 않다. (비효율적)
    // => DCLP(Double Check Locking Pattern)
//    public Cursor getInstance() {
//        if (sInstance == null) {
//            synchronized (Cursor.class) {
//                if (sInstance == null)
//                    sInstance = new Cursor();
//            }
//        }
//        return sInstance;
//    }

    // 문제점 3
    // => 선언적이지 않다.
    // => IODH(Initialization On Demand Holder)
    // => Design Pattern vs Idiom(관용구)
    // => JLS, 내부 정적 클래스의 정적 필드는 처음 접근시에 생성된다.
    private static class Singleton {
        private static final Cursor INSTANCE = new Cursor();
    }

    public Cursor getInstance() {
        return Singleton.INSTANCE;
    }
}




/*
class Cursor {
    private static final Cursor INSTANCE = new Cursor();
    public Cursor getInstance() {
        return INSTANCE;
    }

    private Cursor() {}
}
*/


public class Program {
    public static void main(String[] args) {

    }
}