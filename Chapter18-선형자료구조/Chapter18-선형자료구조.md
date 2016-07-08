# Chapter18 선형자료구조 #
## JOSEPHUS ##
교재는 C++ 이터레이터를 이용합니다. 자바의 이터레이터는 C++의 이터레이터와 약간 달라 예제를 바로 적용할 수 없고 수정이 필요합니다.

자바는 linked list 자료구조를 LinkedList 컬렉션 형태로 제공합니다.

우선 LinkedList에 익숙해집시다.

### 자바 LinkedList 연습 ###

	package Chapter18;
	
	/**
	 * Created by yunskim on 2016-07-08.
	 */
	import java.util.LinkedList;
	
	public class Josephus {
	
	    public static void main(String[] args) {
	        josephus(6, 3);
	    }
	    static void josephus(int n, int k) {
	        LinkedList<Integer> survivors = new LinkedList<Integer>();
	        for(int i = 1; i <= n; ++i) {
	            survivors.add(i - 1, i);;
	        }
	
	        for(int i = 0; i < n; ++i) {
	            System.out.println(survivors.get(i));
	        }
	    }
	}

	// 1 2 3 4 5 6이 출력됩니다.

`survivors.set()`을 바로 사용하니 에러가 날 수 밖에 없습니다.

사실 아래처럼 get()을 사용해도 충분합니다.

 	static void josephus(int n, int k) {
        LinkedList<Integer> survivors = new LinkedList<Integer>();
        for(int i = 1; i <= n; ++i) {
            survivors.add(i);;
        }

        for(int i = 0; i < n; ++i) {
            System.out.println(survivors.get(i));
        }
    }

### 자바 이터레이터 연습 ###
이터레이터를 이용해 내용을 채워봅시다.

혹시 아래와 같이 사용하면 `illegalstateexception`이 나옵니다. 자세한 내용은 [stackoveflow](http://stackoverflow.com/questions/22361194/iterator-remove-illegalstateexception)을 보세요.

	ListIterator<Integer> it = survivors.listIterator();
        if(it.hasNext()) {
            it.next();
            it.remove();
        }

