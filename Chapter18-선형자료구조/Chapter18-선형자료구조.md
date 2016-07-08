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

자바 이터레이터를 이용하면서 어려움을 겪었던 이유가 있습니다. 커서는 항상 element사이에 위치합니다. next()나 previous()를 이용해 element를 지정해야 합니다.

> A ListIterator has no current element; its cursor position always lies between the element that would be returned by a call to previous() and the element that would be returned by a call to next(). An iterator for a list of length n has n+1 possible cursor positions, as illustrated by the carets (^) below:

> Note that the remove() and set(Object) methods are not defined in terms of the cursor position; they are defined to operate on the last element returned by a call to next() or previous().

remove()는 next()나 previous()에 의해 마지막으로 호출된 element를 제거합니다. 혹시 add()가 중간에 호출되는 경우 에러가 나옵니다.

	import java.util.LinkedList;
	import java.util.ListIterator;
	
	public class Josephus {
	
	    public static void main(String[] args) {
	        josephus(6, 3);
	    }
	    static void josephus(int n, int k) {
	        LinkedList<Integer> survivors = new LinkedList<Integer>();
	        for(int i = 1; i <= n; ++i) {
	            survivors.add(i);;
	        }
	        ListIterator<Integer> it = survivors.listIterator();
	
	        it.next();
	        it.remove();
	
	        it.next();
	        it.next();
	        it.remove();
	
	        for(int i = 0; i < survivors.size(); ++i) {
	            System.out.print(survivors.get(i) + " ");
	        }
	        System.out.println();
	
	    }
	}

위의 코드를 실행하면 `2 4 5 6`이 나옵니다. remove()를 실행하더라도 자동으로 다음 element로 넘어가지 않아요. 내가 next()를 실행해야 다음으로 넘어갑니다. 그래서 의도와 다른 결과가 나옵니다.

1 2 3 4 5 6


혹시 아래와 같이 사용하면 `illegalstateexception`이 나옵니다. 자세한 내용은 [stackoveflow](http://stackoverflow.com/questions/22361194/iterator-remove-illegalstateexception)을 보세요.

	ListIterator<Integer> it = survivors.listIterator();
        if(it.hasNext()) {
			it.remove();
        }

### 알고스팟 정리 ###
최종적으로 아래와 같은 코드를 얻었습니다. remove()는 항상 next()나 previous()를 실행해서 반환된 element를 제가한다는 사실 유념하세요.

아래의 코드는 온라인저지를 통과하기는 했지만 사실 sorting을 하지 않는 코드라 볼완전합니다. 아니면 sorting할 필요가 없다는 것을 증명해야 합니다.

	import java.util.LinkedList;
	import java.util.ListIterator;
	import java.util.Scanner;
	
	/**
	 * Created by yunskim on 2016-07-08.
	 */
	public class JosephusAlgoSpot {
	    public static void main(String[] args) {
	        Scanner sc = new Scanner(System.in);
	        int c = sc.nextInt();
	        sc.nextLine();
	
	        for (int i = 0; i < c; ++i) {
	            int n = sc.nextInt();
	            int k = sc.nextInt();
	            sc.nextLine();
	            josephus(n, k);
	        }
	    }
	
	    static void josephus(int n, int k) {
	        LinkedList<Integer> survivors = new LinkedList<Integer>();
	        for(int i = 1; i <= n; ++i) {
	            survivors.add(i);;
	        }
	        ListIterator<Integer> it = survivors.listIterator();
	        while(n > 2) {
	            it.next();
	            it.remove();
	            if(!it.hasNext()) {
	                it = survivors.listIterator();
	            }
	            --n;
	
	            for(int i = 0; i < k -1; ++i) {
	                it.next();
	                if(!it.hasNext()) {
	                    it = survivors.listIterator();
	                }
	            }
	
	        }
	
	        for(int i = 0; i < survivors.size(); ++i) {
	            System.out.print(survivors.get(i) + " ");
	        }
	        System.out.println();
	
	    }
	}

