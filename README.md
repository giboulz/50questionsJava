# 50questionsJava

1) What are the four principles of Object Oriented Languages like Java?
Encapsulation : encapsulation is the hiding of data implementation by restricting access to accessors and mutators
Abstraction : 
Inheritance
Polymorphism

2) Is Java a pure object oriented language?
Java is not a pure Object oriented language, but so called a "Hybrid" language.
For any language to be pure object oriented it must follow these 6 points strictly...
a) It must have full support for Encapsulation and Abstraction
b) It must support Inheritance
c) It must support Polymorphism
d) All predefined types must be Objects <- NOT TRUE -> non primitive object
e) All user defined types must be Objects
f) Lastly, all operations performed on objects must be only through methods exposed at the objects. <- NOT TRUE : In java we can have communicate with objects without  calling their methods for e.g. using arithmetic operators. String s1 = "ABC" + "A" ;

3) How will you write an immutable class?
Declare the class as final so it can’t be extended.
Make all fields private so that direct access is not allowed.
Don’t provide setter methods for variables
Make all mutable fields final so that it’s value can be assigned only once.
Initialize all the fields via a constructor performing deep copy.
Perform cloning of objects in the getter methods to return a copy rather than returning the actual object reference.


4) What is the difference between Comparable and Comparator interfaces in Java?
Comparable interface: Class whose objects to be sorted must implement this interface.In this,we have to implement compareTo(Object) method.
Comparator interface: Class whose objects to be sorted do not need to implement this interface.Some third class can implement this interface to sort.e.g.CountrySortByIdComparator class can implement Comparator interface to sort collection of country object by id.

5) What is hashcode and equals contract?
The contract between equals() and hashCode() is:
a) If two objects are equal, then they must have the same hash code.
b) If two objects have the same hash code, they may or may not be equal.
import java.util.HashMap;
 
public class Apple {
	private String color;
 
	public Apple(String color) {
		this.color = color;
	}
 
	public boolean equals(Object obj) {
		if(obj==null) return false;
		if (!(obj instanceof Apple))
			return false;	
		if (obj == this)
			return true;
		return this.color.equals(((Apple) obj).color);
	}
 
	public static void main(String[] args) {
		Apple a1 = new Apple("green");
		Apple a2 = new Apple("red");
 
		//hashMap stores apple type and its quantity
		HashMap<Apple, Integer> m = new HashMap<Apple, Integer>();
		m.put(a1, 10);
		m.put(a2, 20);
		System.out.println(m.get(new Apple("green")));
	}
}

you MUST implement hashcode for this to work : 
public int hashCode(){
	return this.color.hashCode();	
}

sinon java va d'abord vérifier que le hashcode est différents (ou pas) puis ensuite invoker equals. 

6) What is the difference between == and equals method?
Vérification par pointeur et par valeur.  

7) What are wrapper classes? Why they are declared as final classes?
wrapper des types primitif par exemple (Integer pour int par exemple). Final pour des raison de sécurité. Si on pouvait redéfinir String, cela pourrait être dangereux avec l'utilisation d'une api externe. On pourrait redéfinir les string en tant qu'objet non "immutable". 

8) What is the difference between String and StringBuffer classes?
Création de string plus rapide si nous ajoutons successivement des bout a un stringBuffer. 

9) What is the way to store the integer value in a string object to an integer variable?
String.valueOf(intVarable)
or
Integer.toString(intVarable)

10) What are sorted collections in Java?
Des collections, qui de part la structure de leur implémentation maintiennent un ordre dans la liste. Ce qui n'est pas le cas avec une liste<> = new ArrayList<>() par exemple, qui ne sera trié lors de l'invocation de Collection.sort(...).  

11) Why is String class declared as final class?
CF raison de sécurité dans le 7). 

12) What is JDBC API?
api de connectinon au BDD. 

13) What are checked and unchecked exceptions?
Check, c'est les exception "communes" que nous déclarons, attrappons (try-catch) ...
les unchecked, ce sont les runtimes exception. 

14) What is final, finally and finalize?
final s'applique sur methode -class pour empecher la rédifinition dans des classes filles. 
sur un champs pour empecher sa modif. 
finally est le dernier block d'un try catch qui s'exécutera forcement. 
finallize est la méthode qui est appellé lors qu'un objet va se faire manger par le GC. Peut être appeller manuellement sans aucune garantie qu'elle va s'exécuter immediatement. 

15) What is weakhashmap?
As others have already pointed out they provide a means for using an object as a key without creating a strong reference to it. This is useful in situations where you don't want to impair the JVM's ability to garbage collect the object but yet still want the ability to track some aspect of the object, this makes them ideal for caching or storing metadata about the object.

16) What is the purpose of reflection API?
c'est l'appel des methods d'un objet sans les connaitre à 'lavance : 
type : Method method = foo.getClass().getMethod("doSomething", null);
method.invoke(foo, null);

One very common use case in Java is the usage with annotations. JUnit 4, for example, will use reflection to look through your classes for methods tagged with the @Test annotation, and will then call them when running the unit test.

ou un pattern proxy qui va redéfinir des choses qui sont a faire à chaque fois qu'on appel une méthod. 

17) What is serialVersionUID?
un identifiant qui va être vérifié durant les process de sérialisation /désérialisation. En cas  de serialVersionUID différents lors d'une désérialisation, on aurant un InvalidClassExceptions.

18) What is bucketing in Java?


19) How does Java manages the threads?
avec des pool de threads.

20) What are memory leaks and how to detect/avoid them?
Performance: usually associated with excessive object creation and deletion, long delays in garbage collection, excessive operating system page swapping, and more.
Resource constraints: occurs when there’s either to little memory available or your memory is too fragmented to allocate a large object—this can be native or, more commonly, Java heap-related.
Java heap leaks: the classic memory leak, in which Java objects are continuously created without being released. This is usually caused by latent object references.
Native memory leaks: associated with any continuously growing memory utilization that is outside the Java heap, such as allocations made by JNI code, drivers or even JVM allocations.


21) What is the order of execution of blocks in a Java program?
package pack2;

public class Parent {    
    public Parent() {
        System.out.println("Parent Constructor");
    }    
    static {
        System.out.println("Parent static block");    
    }    
    {
        System.out.println("Parent initialisation  block");
    }
}

package pack2;

public class Child extends Parent {    
    {
        System.out.println("Child initialisation block");
    }
    static {
        System.out.println("Child static block");
    }

    public Child() {
        System.out.println("Child Constructor");
    }    
    public static void main(String[] args) {
        new Child();    
    }
}

The output of the above code will be

Parent static block
Child static block
Parent initialization  block
Parent Constructor
Child initialization block
Child Constructor


There are several rules in play
static blocks are always ran before the object is created, so that's why you see print message from both parents and childs static blocks
now, when you are calling constructor of the subclass (child), then this constructor implicitly calls super(); before executing it's own constructor. Initialization block comes into play even before the constructor call, so that's why is called first. So now your parent is created, program can continue creating child class which will undergo the same process.

22) What are the exception related rules in overloading and overriding?
overloading : 
on doit changer la signature. le changement de signature n'est pas impacté par le champs de retour ou les exception levées par la fonction. 

overriding : 
a) The method argument list in overridden and overriding methods must be exactly same If they don’t match, you will end up with an overloaded method.
b) The return type of overriding method can be child class of return type declared in overridden method.
public class SuperClass {
    //Overriden method
    public Number sum(Integer a, Integer b) {
        return a + b;
    }
}
class SubClass extends SuperClass {
    //Overriding method
    @Override
    public Integer sum(Integer a, Integer b) {      //Integer extends Number; so it's valid
        return a + b;
    }
}
c) Above all rules, private, static and final methods can not be overridden in java in any way. As simple as that !!
d) Overriding method can not throw checked Exception higher in hierarchy than thrown by overridden method. Let’s say for example overridden method in parent class throws FileNotFoundException, the overriding method in child class can throw FileNotFoundException; but it is not allowed to throw IOException or Exception, because IOException or Exception are higher in hierarchy i.e. super classes of FileNotFoundException.
e)Also note that overriding method can not reduce the access scope of overridden method. Put in simple words, if overridden method in parent class is protected, then overriding method in child class can not be private. It must be either protected (same access) or public (wider access).



23) What are the different ways of creating a thread?
There are two different ways to specify which code to run in that Thread:
Implement the interface java.lang.Runnable and pass an instance of the class implementing it to the Thread constructor.
Extend Thread itself and override its run() method.

24) What is inter thread communication?
wait, notify, notifyall

class Chat {
    boolean flag = false;

    public synchronized void Question(String msg) {
        if (flag) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println(msg);
        flag = true;
        notify();
    }

    public synchronized void Answer(String msg) {
        if (!flag) {
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        System.out.println(msg);
        flag = false;
        notify();
    }
}

class T1 implements Runnable {
    Chat m;
    String[] s1 = { "Hi", "How are you ?", "I am also doing fine!" };

    public T1(Chat m1) {
        this.m = m1;
        new Thread(this, "Question").start();
    }

    public void run() {
        for (int i = 0; i < s1.length; i++) {
            m.Question(s1[i]);
        }
    }
}

class T2 implements Runnable {
    Chat m;
    String[] s2 = { "Hi", "I am good, what about you?", "Great!" };

    public T2(Chat m2) {
        this.m = m2;
        new Thread(this, "Answer").start();
    }

    public void run() {
        for (int i = 0; i < s2.length; i++) {
            m.Answer(s2[i]);
        }
    }
}
public class TestThread {
    public static void main(String[] args) {
        Chat m = new Chat();
        new T1(m);
        new T2(m);
    }
}
When above program is complied and executed, it produces following result:

Hi
Hi
How are you ?
I am good, what about you?
I am also doing fine!
Great!

25) What are instance and class level locks? What is synchronization?
des verrous. 

26) What is the difference between IS-A and HAS-A relationship?
inheritance (IS-A relationship), or object composition (HAS-A relationship)

27) What is cloning and CloneNotSupported Exception?
Un pattern pour créer des objets. 

28) What is the difference between JAR, WAR and EAR files?
Les .war (web archive) sont utilisés pour les applications web (jsp et servlet)
Les Jar (java archive)pour des packages Java et les EJBs
Les Ear (enterprise archive) contiennent des fichiers jar et war

29) What are the coding standards for naming variables, constants, methods and classes?
voir la JLS 
Package Names : Acceptable: com.iwombat.ui  Unacceptable:com.iwombat.user_interface
Class and Interface Names : Acceptable: class FoodItem  interface Digestable  Unacceptable: class fooditem  class Crackers  interface Eat
http://www.iwombat.com/standards/JavaStyleGuide.html


30) What is a literal and what is special about String literals?


31) What is flyweight design pattern?
cf pattern

32) How will you create a Singleton class? Is it thread safe?
- création -> voir singleton
- si l'implémentation l'a supporté, c'est thread safe. 
public static Singleton getInstance()
	{	
		if (INSTANCE == null)
		{ 	
			synchronized(Singleton.class)
			{
				if (INSTANCE == null)
				{	INSTANCE = new Singleton();
				}
			}
		}
		return INSTANCE;
	}


33) What is the difference between throw and throws clause?
throw envoi une exception
throws signale que la fonction peut envoyer les exceptions suivantes. 

34) Can one access the private members of class using reflection API?
oui

35) What is the purpose of instanceof operator?
vérifier la nature d'un objet. 

36) Under what circumstances, the finally block in a program may not run?
If System.exit(0) is called; or,
if an Exception is thrown all the way up to the JVM and the default behavior occurs (i.e., printStackTrace() and exit)


37) What is the difference between private, protected, default and public access specifiers?
privé, pour mes enfants, pour les gens de mon pacakge, pour tout le monde

38) What are this and super keywords?
this refère à l'objet courant (permet d'invoquer d'autre fonction ou des constructeur. 
super refère à l'objet dont l'objet courant hérite. 

39) What are transient and volatile keywords?
- transient  : champs qui ne devront pas être géré lors de la sérialisation. 
- volatile : variable qui ne devra être acces comme il faut dans un environnement multi thread. 

40) How many package statements can a program have?


41) Are all the classes specified in import statement actually imported?
In Java, import is simply used by the compiler to let you name your classes by their unqualified name, let's say String instead of java.lang.String. You don't really need to import java.lang.* because the compiler does it by default. However this mechanism is just to save you some typing. Types in Java are fully qualified class names, so a String is really a java.lang.String object when the code is run. Packages are intended to prevent name clashes and allow two classes to have the same simple name, instead of relying on the old C convention of prefixing types like this. java_lang_String. This is called namespacing.


42) Can an inheritance relationship exist between two interfaces?
nope

43) What is Unicode?
format d'encodage des caractères. 

44) What are annotations in Java?

45) What are the various forms of polymorphism?
overloading & overriding.(mais y'en a 4 en fait ) : 
There are FOUR kinds of polymorphism
1.  Overloading  (Same function names, different parameter types.  This includes operator overloading and is done at compile time)

1.  Parametric polymorphism  (These are like templates in C++)  Compile time

3.  Subtype polymorphism (if a function has a parameter with a subtype, for example Car->Honda, f(Car), then function f will accept f(Honda) as well.)  Runtime

4.  Parameter coercion  (This is an implicit type conversion.  For example, a function might require a double/real/float, but will accept an int and will implicitly upcast the parameter)
Compile time


46) What is the meaning of various keywords specified in creating the main method?


47) What is the purpose of ^ operator?
ou exclusif 

48) What is the difference between pass by value and pass by reference?
copie par valeur & copie par pointeur. 

49) What are the various types of inner classes?
inner class : ce sont les classes définit dans une classe (en tant que champs quoi). 
1. Static Nested Classes

class Outer {
	static class Inner {
		void go() {
			System.out.println("Inner class reference is: " + this);
		}
	}
}
 
public class Test {
	public static void main(String[] args) {
		Outer.Inner n = new Outer.Inner();
		n.go();
	}
}
Inner class reference is: Outer$Inner@19e7ce87

2. Member Inner Class

Member class is instance-specific. It has access to all methods, fields, and the Outer's this reference.

public class Outer {
    private int x = 100;
 
    public void makeInner(){
        Inner in = new Inner();
        in.seeOuter();
    }
 
    class Inner{
        public void seeOuter(){
            System.out.println("Outer x is " + x);
            System.out.println("Inner class reference is " + this);
            System.out.println("Outer class reference is " + Outer.this);
        }
    }
 
    public static void main(String [] args){
    	Outer o = new Outer();
        Inner i = o.new Inner();
        i.seeOuter();
    }
}
Outer x is 100
Inner class reference is Outer$Inner@4dfd9726
Outer class reference is Outer@43ce67ca

3. Method-Local Inner Classes

public class Outer {
	private String x = "outer";
 
	public void doStuff() {
		class MyInner {
			public void seeOuter() {
				System.out.println("x is " + x);
			}
		}
 
		MyInner i = new MyInner();
		i.seeOuter();
	}
 
	public static void main(String[] args) {
		Outer o = new Outer();
		o.doStuff();
	}
}
x is outer
public class Outer {
	private static String x = "static outer";
 
	public static void doStuff() {
		class MyInner {
			public void seeOuter() {
				System.out.println("x is " + x);
			}
		}
 
		MyInner i = new MyInner();
		i.seeOuter();
	}
 
	public static void main(String[] args) {
		Outer.doStuff();
	}
}
x is static outer

4. Anonymous Inner Classes

This is frequently used when you add an action listener to a widget in a GUI application.

button.addActionListener(new ActionListener(){
     public void actionPerformed(ActionEvent e){
         comp.setText("Button has been clicked");
     }
});



50) What are the various memory areas in JVM and what kind of information is stored in each of them?

