# Chapter19 큐와 스택, 데크 #
## 자바 LinkedList ##
### stack ###
<- addLast()
-> removeLast()
..> getLast()

### queue ###

<- poll()      <- offer()
<.. peek() 

### deque ###
-> push()     <- offer()
<- pop()

### BRACKETS2 ###

	package Chapter19;
	
	/**
	 * Created by yunskim on 2016-07-08.
	 */
	import java.util.LinkedList;
	import java.util.Scanner;
	
	public class Bracket2AlgoSpot {
	    public static void main(String[] args) {
	        Scanner sc = new Scanner(System.in);
	        int c = sc.nextInt();
	        sc.nextLine();
	
	        for(int i = 0; i < c; ++i) {
	            String formula = sc.nextLine();
	            System.out.println(wellBalanced(formula) ? "YES" : "NO");
	        }
	    }
	
	    static boolean wellBalanced(String formula) {
	        LinkedList<Character> stack = new LinkedList<Character>();
	        String opening = "({[";
	        String closing = ")}]";
	
	        for(int i = 0; i < formula.length(); ++i) {
	            char ch = formula.charAt(i);
	            if(opening.indexOf(ch) != -1) {
	                stack.addLast(ch);
	            }
	            else {
	                if(stack.isEmpty()) {
	                    return false;
	                }
	                if(opening.indexOf(stack.getLast()) != closing.indexOf(ch)) {
	                    return false;
	                }
	                stack.removeLast();
	            }
	        }
	        return stack.isEmpty();
	    }
	}