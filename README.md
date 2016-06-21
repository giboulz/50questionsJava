# 50questionsJava

1) What are the four principles of Object Oriented Languages like Java?

- Encapsulation : encapsulation is the hiding of data implementation by restricting access to accessors and mutators
- Abstraction : 
- Inheritance
- Polymorphism

2) Is Java a pure object oriented language?

Java is not a pure Object oriented language, but so called a "Hybrid" language.
For any language to be pure object oriented it must follow these 6 points strictly...
- a) It must have full support for Encapsulation and Abstraction
- b) It must support Inheritance
- c) It must support Polymorphism
- d) All predefined types must be Objects <- NOT TRUE -> non primitive object
- e) All user defined types must be Objects
- f) Lastly, all operations performed on objects must be only through methods exposed at the objects. <- NOT TRUE : In java we can have communicate with objects without  calling their methods for e.g. using arithmetic operators. String s1 = "ABC" + "A" ;

3) How will you write an immutable class?

- Declare the class as final so it can’t be extended.
- Make all fields private so that direct access is not allowed.
- Don’t provide setter methods for variables
- Make all mutable fields final so that it’s value can be assigned only once.
- Initialize all the fields via a constructor performing deep copy.
- Perform cloning of objects in the getter methods to return a copy rather than returning the actual object reference.


4) What is the difference between Comparable and Comparator interfaces in Java?

- Comparable interface: Class whose objects to be sorted must implement this interface.In this,we have to implement compareTo(Object) method.
- Comparator interface: Class whose objects to be sorted do not need to implement this interface.Some third class can implement this interface to sort.e.g.CountrySortByIdComparator class can implement Comparator interface to sort collection of country object by id.


5) What is hashcode and equals contract?

The contract between equals() and hashCode() is:
- a) If two objects are equal, then they must have the same hash code.
- b) If two objects have the same hash code, they may or may not be equal.


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

- String.valueOf(intVarable)
- Integer.toString(intVarable). 

10) What are sorted collections in Java?

Des collections, qui de part la structure de leur implémentation maintiennent un ordre dans la liste. Ce qui n'est pas le cas avec une

	liste<> = new ArrayList<>() 
	
par exemple, qui ne sera trié lors de l'invocation de 

	Collection.sort(...).  

11) Why is String class declared as final class?

CF raison de sécurité dans le 7). 

12) What is JDBC API?

api de connectinon au BDD. 

13) What are checked and unchecked exceptions?

- Check, c'est les exception "communes" que nous déclarons, attrappons (try-catch) ...
- les unchecked, ce sont les runtimes exception. 

14) What is final, finally and finalize?

- final s'applique sur methode -class pour empecher la rédifinition dans des classes filles. sur un champs pour empecher sa modif. 
- finally est le dernier block d'un try catch qui s'exécutera forcement. 
- finallize est la méthode qui est appellé lors qu'un objet va se faire manger par le GC. Peut être appeller manuellement sans aucune garantie qu'elle va s'exécuter immediatement. 

15) What is weakhashmap?
As others have already pointed out they provide a means for using an object as a key without creating a strong reference to it. This is useful in situations where you don't want to impair the JVM's ability to garbage collect the object but yet still want the ability to track some aspect of the object, this makes them ideal for caching or storing metadata about the object.

16) What is the purpose of reflection API?

c'est l'appel des methods d'un objet sans les connaitre à 'lavance : 
type : 

	Method method = foo.getClass().getMethod("doSomething", null);
	method.invoke(foo, null);

One very common use case in Java is the usage with annotations. JUnit 4, for example, will use reflection to look through your classes for methods tagged with the @Test annotation, and will then call them when running the unit test.

ou un pattern proxy qui va redéfinir des choses qui sont a faire à chaque fois qu'on appel une méthod. 

17) What is serialVersionUID?

un identifiant qui va être vérifié durant les process de sérialisation /désérialisation. En cas  de serialVersionUID différents lors d'une désérialisation, on aurant un InvalidClassExceptions.

18) What is bucketing in Java?


19) How does Java manages the threads?

avec des pool de threads.

20) What are memory leaks and how to detect/avoid them?

- Performance: usually associated with excessive object creation and deletion, long delays in garbage collection, excessive operating system page swapping, and more.
- Resource constraints: occurs when there’s either to little memory available or your memory is too fragmented to allocate a large object—this can be native or, more commonly, Java heap-related.
- Java heap leaks: the classic memory leak, in which Java objects are continuously created without being released. This is usually caused by latent object references.
- Native memory leaks: associated with any continuously growing memory utilization that is outside the Java heap, such as allocations made by JNI code, drivers or even JVM allocations.


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

Parent initialization block

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
- a) The method argument list in overridden and overriding methods must be exactly same If they don’t match, you will end up with an overloaded method.
- b) The return type of overriding method can be child class of return type declared in overridden method.

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
	
- c) Above all rules, private, static and final methods can not be overridden in java in any way. As simple as that !!
- d) Overriding method can not throw checked Exception higher in hierarchy than thrown by overridden method. Let’s say for example overridden method in parent class throws FileNotFoundException, the overriding method in child class can throw FileNotFoundException; but it is not allowed to throw IOException or Exception, because IOException or Exception are higher in hierarchy i.e. super classes of FileNotFoundException.
- e)Also note that overriding method can not reduce the access scope of overridden method. Put in simple words, if overridden method in parent class is protected, then overriding method in child class can not be private. It must be either protected (same access) or public (wider access).


23) What are the different ways of creating a thread?

There are two different ways to specify which code to run in that Thread:
- Implement the interface java.lang.Runnable and pass an instance of the class implementing it to the Thread constructor.
- Extend Thread itself and override its run() method.

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

- inheritance (IS-A relationship)
- object composition (HAS-A relationship)

27) What is cloning and CloneNotSupported Exception?
Un pattern pour créer des objets. 

28) What is the difference between JAR, WAR and EAR files?

- Les .war (web archive) sont utilisés pour les applications web (jsp et servlet)
- Les Jar (java archive)pour des packages Java et les EJBs
- Les Ear (enterprise archive) contiennent des fichiers jar et war


29) What are the coding standards for naming variables, constants, methods and classes?

voir la JLS 

Package Names : Acceptable: com.iwombat.ui  Unacceptable:com.iwombat.user_interface

Class and Interface Names : Acceptable: class FoodItem  interface Digestable  Unacceptable: class fooditem  class Crackers  interface Eat

http://www.iwombat.com/standards/JavaStyleGuide.html

outil pour vérifier sous eclipse : PMD & Checkstyle.


30) What is a literal and what is special about String literals?


31) What is flyweight design pattern?

cf pattern -> https://github.com/giboulz/DesignPatternJava/blob/master/README.md


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

- throw envoi une exception
- throws signale que la fonction peut envoyer les exceptions suivantes. 

34) Can one access the private members of class using reflection API?

oui

35) What is the purpose of instanceof operator?

vérifier la nature d'un objet. 

36) Under what circumstances, the finally block in a program may not run?

- If System.exit(0) is called; or,
- if an Exception is thrown all the way up to the JVM and the default behavior occurs (i.e., printStackTrace() and exit)


37) What is the difference between private, protected, default and public access specifiers?

- private : privé
- protected : pour mes enfants
- rien : pour les gens de mon pacakge
- public : pour tout le monde

38) What are this and super keywords?

- this refère à l'objet courant (permet d'invoquer d'autre fonction ou des constructeur). 
- super refère à l'objet dont l'objet courant hérite. 

39) What are transient and volatile keywords?

- transient  : champs qui ne devront pas être géré lors de la sérialisation. 
- volatile : variable qui ne devra être acces comme il faut dans un environnement multi thread. 

40) How many package statements can a program have?

package - only one

imports - unlimited. You can have as many import statements in your class as you want.

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
- 1.  Overloading  (Same function names, different parameter types.  This includes operator overloading and is done at compile time)
- 2.  Parametric polymorphism  (These are like templates in C++)  Compile time
- 3.  Subtype polymorphism (if a function has a parameter with a subtype, for example Car->Honda, f(Car), then function f will accept f(Honda) as well.)  Runtime
- 4.  Parameter coercion  (This is an implicit type conversion.  For example, a function might require a double/real/float, but will accept an int and will implicitly upcast the parameter) Compile time

46) What is the meaning of various keywords specified in creating the main method?


47) What is the purpose of ^ operator?

ou exclusif 

48) What is the difference between pass by value and pass by reference?

copie par valeur & copie par pointeur. 

49) What are the various types of inner classes?

inner class : ce sont les classes définit dans une classe (en tant que champs quoi). 
- 1. Static Nested Classes

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

- 2. Member Inner Class

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

- 3. Method-Local Inner Classes

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

- 4. Anonymous Inner Classes

This is frequently used when you add an action listener to a widget in a GUI application.

	button.addActionListener(new ActionListener(){
	     public void actionPerformed(ActionEvent e){
	         comp.setText("Button has been clicked");
	     }
	});



50) What are the various memory areas in JVM and what kind of information is stored in each of them?


Personal question : 

1) StringBuilder vs StringBuffer : 

- StringBuilder est plus rapide
- StringBuffer est synchronized. 

2) HashMap vs HashTable

Hashtable is synchronized, whereas HashMap is not. This makes HashMap better for non-threaded applications, as unsynchronized Objects typically perform better than synchronized ones.



Préambule :

•	pour cet entretien, toutes les questions concernent Java, spring, hibernate, xml
•	toutes les questions sont « à tiroir » et donnent lieu éventuellement à d'autres questions en fonction des réponses apportées par le candidat.

Liste des questions :

•	Citez les différents types de visibilité en java (private, public, package, default = package level)

•	Qu’est-ce qui différencient ces visibilités ?

•	Si on ne met rien comme visibilité devant un attribut ou une méthode dans une classe, quelle est sa visibilité ? (package level)

•	Même question pour une interface ? 


•	Quelles sont les nouveautés apportées par java 1.5 ? (enums, generiques, autoboxing / unboxing, for with iterator, variable arguments, adnotations)

•	Quel est l’intérêt des types génériques ? (type checking at compilation, desavantage un hierarchy des classes a creer si elle n'existe pas deja

•	Qu’est-ce que l’auto-boxing ? (les types primitives des donnes <int> sont convertis automatiquement au classes meres <Integer>)

•	Est-ce que l’inverse est possible ? Comment ça s’appelle ? (Oui - unboxing)


•	Qu’est-ce qu’une interface ? A quoi cela sert-il ?


•	Qu’est-ce qu’une classe abstraite ?

•	Une classe abstraite peut-elle avoir un constructeur ?

•	Quelle est la différence avec une interface ? Quel est l’intérêt de l’une par rapport à l’autre ?

•	Comment accède-t-on aux méthodes / attributs d’une classe mère ?

•	Est-ce qu’on peut appeler le constructeur de la classe mère n’importe où dans son propre constructeur ?

•	Comment fait-on pour qu’un attribut ne soit pas modifiable ?

•	Est-ce qu’on peut utiliser ce modificateur (final) à d’autres endroits que sur des attributs ? Qu’est-ce qu’il se passe alors ?

•	Est-ce que le paramètre d’une méthode (de type int par exemple) est modifié à la sortie de la méthode si la méthode le modifie ?

•	Pouvez-vous nous citer les différents types de collections en java ? (List, Set, Map, Queue)

•	Donnez nous un exemple d’implémentation de List ? (ArrayList)

•	Y a-t-il d’autres implémentations ? (LinkedList)

•	Quelle est la différence entre les deux ? Quand utiliser l’une ou l’autre ?


•	Qu’est ce que la sérialisation ? A quoi ça sert ?

•	Comment sérialise-t-on un objet ? (implémenter Serializable)

•	Il y a une autre interface qui permet de sérialiser un objet, laquelle ?

•	A quoi sert le mot clé transient ?


•	A quoi sert le mot clé volatile ?


•	Comment met-on en place le multi threading ? (la classe hérite de Thread)

•	Il y a une autre façon de le faire, laquelle ? (implémenter Runnable)

•	Pourquoi cette solution est-elle moins utilisée ?

•	Comment « sécuriser » un donnée partagée dans un environnement multi threadé ?

•	A quel niveau est positionné le lock quand on utilise « synchronized » ?

•	Si on met un « synchronized » sur une  méthode statique, où est placé le lock ?


•	Comment sont gérées les exceptions en java ?

•	Quelles sont les différentes méthodes pour lever une exception ?

•	Est-ce qu’on peut gérer les exceptions sans block try catch ?

•	Quand est-ce qu'on passe par le block finally ?
Example  - qu'est-ce qu'on imprime si le throw est commenté ou pas ?
        public static void main(String argv[]) {
             try{
                    System.out.println("A");
                    //throw new Exception("Help me !");
             } catch(Exception e) {
                    System.out.println("B");
             } finally {
                    System.out.println("C");
             }

•	Quand on ne gère pas un type d’exception et qu’elle est levée, qu’est-ce qu’il se passe ?


•	Citez-moi des design pattern que vous connaissez

•	Pouvez-vous m’écrire une implémentation du pattern singleton ?
•	Quels sont les 3 types pricipales des design patterns ? C'est quoi le but de chaque group ? Donnez un example pour chaqun.

•	Qu’est-ce que spring ? Comment cela fonctionne-t-il ? A quoi ça sert ?

•	Quel design pattern est utilisé par spring ? Inversion of control and dependency injection (http://martinfowler.com/articles/injection.html) Pourquoi cela ?


•	Qu’est-ce qu’hibernate ? Comment cela fonctionne-t-il ? A quoi ça sert ?

•	Qu’est-ce que le lazy-loading ?

•	Quel design pattern est utilisé dans ce cas ?


•	Avec quoi lisez-vous un fichier xml en java ? (SAX ou DOM)

•	Quelle est la différence entre ces parseurs ? Quel est le plus rapide

•	Si j’ai un gros fichier xml (en taille et profondeur de tags), quel parseur je choisis pour le lire ? Pourquoi ? si je veux avoir la valeur d'une donne quel parseur je l'utiliser ?

•	Difference entre Hashtable (all methods synchronized, early collection, pretty slow), Hashmap (nothing synchronized, fast), ConcurrentHashMap (synchronized for fast throughoutput)

•	Comme est-ce qu'un iterateur modifie l'acces a une collection quand on est en train d'inserer et d'effacer des ellements ? Pour un HashSet (pas synchronizé), pour un qui est synchronizé ?
	Example : 
      public class Test {
         private synchronized Set<Integer> set = new HashSet<Integer>();
         
         public synchronized void insert(Integer value) { 
             set.add(value);
         }

          public synchronized void delete(Integer value) { 
             set.remove(value);
         }

         public void list(Integer value) { 
              for(Integer it : set) {
                  System.out.print(it + " ");
              }              
         }
    Quel est le probleme ? (l'iterateur va proteger contre les insertion, mais s'il y a un autre thread qui efface un element on aura une exception ConcurrentModificationException; la classe n'est pas completement thread safe).


•	Principe de rangement des éléments dans une hashMap (Détails des fonctions hashcode et equals, pourquoi reimplementer un demand la reimplementation de l'autre); comme est-ce qu'un hashmap est implementé dpdv algorithmique? (liste chainé avec des listes des elements pour chaque hashcode) Mais pour le Hashmap (listes chaines pour les elements du meme hashcode, pointer vers le tete des liste pour chaque hashcode - quel est l'avantage de ca ?)


•	JAVA - questions generales 

- Types primitifs et leurs tailles en mémoire (ca depend de la machine virtuelle et du SO)
- Fonction et utilisation du mot clef static
- fonction et utilisation du mot clef synchronized
- Les différents types de Collections (List, Set, Map,Vector etc)
- Qu'est-ce que l'EDT
- Swing Worker
- Exemple de Designe Patterns avec description de 2 d'entre eux 
- Le designe pattern PROXY
- Les options de lancement -xms et -xmx et/ou autres exemples d'options au lancement d'une application JAVA
- Apports de Java 5, 6 et 7.
- Différence entre héritage et composition ? Donnez un exemple concret. Avantage / Inconvénient.
- Polymorphisme, qu’est-ce que ?
- Comment fonctionne le class loader ?
- Avantage / inconvénient du java et du C# ?
- Le garbage collector - comme est-ce qu'il fonctionne ? Est-ce qu'il peut donner des fuites de memoire ? (Oui, quand ?) Est-ce qu'il y a de maniere d'interactionner avec lui (oui, meme en code System.gc() ? - qu'est ce que ca fait, quand est ce que ca pourrait etre utile ? )
-Qu’est-ce que javax ? C’est le package qui contient les composants graphiques.

2) J2EE
- Que permet Spring
- Qu'est-ce que l'injection de dépendance
- JNDI
- JNI
- JMX 

3) SQL
- Tables à créer pour modéliser l'emprunt de livres par des membres d'une librairie 
- Type de clé primaire pour la table livre
- Méthode d'optimisation de requêtes
- Les différents types d'index
- Les informations dans un plan d'exécution


•	Quelle est la syntaxe pour créer une table en SQL
•	Connaissez-vous la notion de For Enquy ?
•	Left / Right (internal / external) join ?
•	Avez-vous optimiser des recherches ? Index ? Pouvez-vous mettre un index sur toute les colonnes ?
•	1 table avec les « personnes », une autre avec trois couleurs de cheveux, pouvez-vous mettre un index sur toute les colonnes ?
•	Jointure, a quoi cela sert ?
•	Ecrire la requête des personnes qui ont la couleur de cheveux bruns et blonds.

4) UML
- Agrégation
- Composition
- Représentation graphique sur un schéma UML

5) Finance
- Swap de taux; vanilla, position, quels sont les attributes d'une position, qu'est ce que ca veut dire FO, BO, MO, Risk, Var, strategie, jambe, In the money, out of money, coupon et zero coupon,
Advanced : fat-tail distribution, what are the greeks and which ones are there, deal blotter, courbe de taux et qu'est ce que ca veut dire de le calibrer, parametres de marches , BO to FO (why ?)

Quelques notions deja definis : 
La volatilité mesure l'importance des fluctuations de valeur d'un actif et donc son risque. Elle se calcule mathématiquement par l'écart type des rentabilités de l'actif.
  
La sensibilité d'une obligation mesure la variation de sa valeur en pourcentage induite par une variation donnée du taux d'intérêt. Mathématiquement, elle est égale à la valeur absolue de la dérivée de la valeur de l'obligation par rapport au taux d'intérêt, divisée par la valeur de l'obligation.
 
Un produit exotique désigne des produits financiers complexes et peu fréquent, principalement vendus à des investisseurs sophistiqués, qui peuvent permettre à un émetteur de trouver des fonds à un coût attractif mais souvent en contrepartie d'engagements qui peuvent se révéler coûteux à terme.
  
Le principe d'un swap de taux d'intérêt est de comparer un taux variable et un taux garanti et de se verser mutuellement les différentiels de taux d'intérêt sans échange en capital. Le swap de taux est particulièrement adapté à la gestion du risque de taux à long terme en entreprise. 
Le marché des swaps a connu un essor considérable et les banques occupent un rôle déterminant dans l'animation de ce marché. Les trésoriers d'entreprise apprécient la souplesse du swap qui leur permet de choisir la durée, le taux variable de référence et le notionnel. Le swap conclu entre une banque et une entreprise peut être liquidé à tout moment en calculant la valeur actuelle des flux fixes prévus au taux du marché et en la comparant au notionnel initial. L'utilisation du swap est également fréquente pour gérer le risque de taux sur des actifs à taux variable ou à taux fixe.
 
La value at risk (VAR) représente la perte potentielle maximale d'un Investisseur sur la Valeur d'un actif ou d'un portefeuille d'actifs financiers compte tenu d'un horizon de détention et d'un intervalle de confiance. Elle se calcule à partir d'un échantillon de données historiques ou se déduit des lois statistiques habituelles.


-----------------------------------------------------------

Personal question 2 : 
1) 
Soap vs Rest : 
Soap est plus carré car le contract est définie, mais plus lourd, car les devs dependent de la WSDL.
Soap est forcement en XML, le xml est plus lourd que le json (balise fermante obligatoire). le Soap est transactionnel, en REST il faut gérer la transaction soit meme. et les transactions peuvent être distribution (1 avec un noeud, la requete suivante avec un autre noeud ...). 

2) TU 
princiape du AAA : Arrange-Act-Assert (AAA) Pattern ( http://www.typemock.com/unit-test-patterns-for-net )
il y a d'autre pattern pour les TU, mais dans le AAA, on pose l'objet a tester (Arrange), on l'exécute (Act), et on test (Assert). 

3) BDD - propriété ACID. 
atomicité : La propriété d'atomicité assure qu'une transaction se fait au complet ou pas du tout : si une partie d'une transaction ne peut être faite, il faut effacer toute trace de la transaction et remettre les données dans l'état où elles étaient avant la transaction. L'atomicité doit être respectée dans toutes situations, comme une panne d'électricité, une défaillance de l'ordinateur, ou une panne d'un disque magnétique.

cohérence : 
La propriété de cohérence assure que chaque transaction amènera le système d'un état valide à un autre état valide. Tout changement à la base de données doit être valide selon toutes les règles définies, incluant mais non limitées aux contraintes d'intégrité, aux rollbacks en cascade, aux déclencheurs de base de données, et à toutes combinaisons d'évènements.

isolation : Toute transaction doit s'exécuter comme si elle était la seule sur le système. Aucune dépendance possible entre les transactions. La propriété d'isolation assure que l'exécution simultanée de transactions produit le même état que celui qui serait obtenu par l'exécution en série des transactions. Chaque transaction doit s'exécuter en isolation totale : si T1 et T2 s'exécutent simultanément, alors chacune doit demeurer indépendante de l'autre.

durabilité : La propriété de durabilité assure que lorsqu'une transaction a été confirmée, elle demeure enregistrée même à la suite d'une panne d'électricité, d'une panne de l'ordinateur ou d'un autre problème. Par exemple, dans une base de données relationnelle, lorsqu'un groupe d'énoncés SQL ont été exécutés, les résultats doivent être enregistrés de façon permanente, même dans le cas d'une panne immédiatement après l'exécution des énoncés.

4) principal avantage de Spring (IOC)
changement de l'impléméntation d'un service facilement. 

5) HIBERNATE : N+1 select
Let’s assume that you’re writing code that’d track the price of mobile phones. Now, let’s say you have a collection of objects representing different Mobile phone vendors (MobileVendor), and each vendor has a collection of objects representing the PhoneModels they offer.

To put it simple, there’s exists a one-to-many relationship between MobileVendor:PhoneModel.

MobileVendor Class

Class MobileVendor{
        long vendor_id;
        PhoneModel[] phoneModels;
        ...
 }
Okay, so you want to print out all the details of phone models. A naive O/R implementation would SELECT all mobile vendors and then do N additional SELECTs for getting the information of PhoneModel for each vendor.

-- Get all Mobile Vendors
 SELECT * FROM MobileVendor;
-- For each MobileVendor, get PhoneModel details
 SELECT * FROM PhoneModel WHERE MobileVendor.vendorId=?
As you see, the N+1 problem can happen if the first query populates the primary object and the second query populates all the child objects for each of the unique primary objects returned.

Resolve N+1 SELECTs problem

(i) HQL fetch join

"from MobileVendor mobileVendor join fetch mobileVendor.phoneModel PhoneModels"
Corresponding SQL would be (assuming tables as follows: t_mobile_vendor for MobileVendor and t_phone_model for PhoneModel)

SELECT * FROM t_mobile_vendor vendor LEFT OUTER JOIN t_phone_model model ON model.vendor_id=vendor.vendor_id
(ii) Criteria query

Criteria criteria = session.createCriteria(MobileVendor.class);
criteria.setFetchMode("phoneModels", FetchMode.EAGER);
In both cases, our query returns a list of MobileVendor objects with the phoneModels initialized. Only one query needs to be run to return all the PhoneModel and MobileVendor information required

6 ) HIBERANTE : stratégie de chargement

Lazy Loading : 

Le chargement à la demande (ou " lazy loading ") est la stratégie native mise en œuvre par Hibernate. Elle consiste à ne charger que le minimum de données, puis de générer une nouvelle requête SQL pour récupérer les données supplémentaires lorsque celles-ci seront demandées par le programme.

Dans la pratique, Hibernate charge tous champs de la table principale, et les clés étrangères sont stockées sous forme simplifiée (seul l'ID est renseigné), ce que l'on nomme un proxy. Lorsque le programme essaiera d'accéder aux membres de ce proxy, Hibernate générera une requête SQL et récupérera les données nécessaires afin de le remplir…

Avantage : Le principal avantage du lazy-loading est bien entendu sa transparence. Le programme utilise les objets du Domaine en toute simplicité, et seules les données vraiment nécessaires sont chargées.

Inconvénients : Si séduisante qu'elle puisse paraître, cette approche présente les défauts suivants :

Absence de maîtrise du chargement : chaque appel peut potentiellement provoquer une requête en base de données, et ce, de manière complètement transparente et incontrôlable. On peut ainsi assister à des effondrements spectaculaires des performances de l'application, dus à un excès de requêtes SQL. De plus, l'utilisation de certaines librairies, basées notamment sur l'introspection _ telles que JAXB ou Dozer_ provoque le chargement de la totalité du graphe d'objet lors de la sérialization du graphe d'objets
N+1 select : corollaire du point précédent, le chargement de collections se traduit par un nombre rédhibitoire de requêtes. Regardons l'exemple suivant :
 Sélectionnez
List<Address> addressList = person.getAddressList() ; (1)
for (Address address : addressList)
{
    displayCity(address.getCity(), address.getZipCode()) ; (2)
…
}
L'appel (1) génère une requête SQL remontant la liste des identifiants des adresses composant la liste, et que chaque appel (2) génère une autre requête SQL afin de remplir la totalité de l'objet Address, soit au total N+1 requêtes (N étant la cardinalité de la liste).

Seule solution, non pas pour maîtriser mais pour diagnostiquer ce genre de problématique : passer le paramètre de configuration " show_sql " à 'true' afin de visualiser en développement les requêtes générées automatiquement par les proxies Hibernate.

Eager fetching

Solution diamétralement opposée au chargement à la demande, le " eager-fetching " consiste à systématiquement charger l'intégralité du graphe d'objets associés.

Techniquement, il suffit de d'ajouter l'attribut " lazy " à 'false' (ou fetch='join') pour toutes les associations (one-to-one, many-to-one, one-to-many) et le tour est joué.

Avantage : 

Tout comme le chargement à la demande, cette stratégie de chargement présente l'avantage de la simplicité. De plus, une fois la donnée chargée, cette dernière est entièrement et immédiatement disponible, sans génération de requête SQL supplémentaire.

Inconvénient : 

Vous l'aurez deviné, là où le bât blesse, c'est justement au temps de chargement de votre graphe d'objets, qui peut vite s'avérer rédhibitoire avec un schéma classique (quelques associations suffisent à faire exploser les performances).

Si on y ajoute une grande propension au syndrome du produit cartésien (cf. encadré), vous comprendrez bien que cette approche est à bannir de toute application manipulant un graphe d'objet un tant soit peu conséquent.

Règle n°1 : pas plus d'une collection à la fois

Le chargement des collections (association many-to-one, coté " one ") est un point délicat de la programmation avec Hibernate.

En effet le chargement de plusieurs collections en parallèle risque de fortement dégrader les performances de votre application car une telle requête remonte en général trop de données (produit cartésien). En effet, le résultat de la requête SQL est composé de la combinaison des résultats possibles sur chaque jointure, comportant une multitude de doublons qu'Hibernate devra éliminer.

A titre d'exemple, une requête renvoyant 100 éléments, chacun associé à deux collections de 10 éléments chacun, serait constituée de (100*10*10) = 10 000 lignes de résultats.

Outre le temps de traitement par Hibernate, le temps de transfert d'un tel volume de résultat achèvera de réduire les performances de votre application à néant !


7) CleanCode : SOLID
https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)

The principles, when applied together, intend to make it more likely that a programmer will create a system that is easy to maintain and extend over time. 

single responsibility : a class should have only a single responsibility (i.e. only one potential change in the software's specification should be able to affect the specification of the class)
Martin defines a responsibility as a reason to change, and concludes that a class or module should have one, and only one, reason to change. As an example, consider a module that compiles and prints a report. Imagine such a module can be changed for two reasons. First, the content of the report could change. Second, the format of the report could change. These two things change for very different causes; one substantive, and one cosmetic. The single responsibility principle says that these two aspects of the problem are really two separate responsibilities, and should therefore be in separate classes or modules. It would be a bad design to couple two things that change for different reasons at different times.


open-closed : “software entities … should be open for extension, but closed for modification.”
Bertrand Meyer is generally credited for having originated the term open/closed principle, which appeared in his 1988 book Object Oriented Software Construction.

A module will be said to be open if it is still available for extension. For example, it should be possible to add fields to the data structures it contains, or new elements to the set of functions it performs.
A module will be said to be closed if it is available for use by other modules. This assumes that the module has been given a well-defined, stable description (the interface in the sense of information hiding).

Liskov substitution : “objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program.” See also design by contract.
Liskov's notion of a behavioral subtype defines a notion of substitutability for mutable objects; that is, if S is a subtype of T, then objects of type T in a program may be replaced with objects of type S without altering any of the desirable properties of that program (e.g. correctness).

interface segregation : “many client-specific interfaces are better than one general-purpose interface.”
Within object-oriented design, interfaces provide layers of abstraction that facilitate conceptual explanation of the code and create a barrier preventing dependencies.

According to many software experts who have signed the Manifesto for Software Craftsmanship, writing well-crafted and self-explanatory software is almost as important as writing working software.[4] Using interfaces to further describe the intent of the software is often a good idea.

A system may become so coupled at multiple levels that it is no longer possible to make a change in one place without necessitating many additional changes.[1] Using an interface or an abstract class can prevent this side effect.


dependency inversion :  one should “Depend upon Abstractions. Do not depend upon concretions.”
https://en.wikipedia.org/wiki/Dependency_inversion_principle

8) SOA
Service oriented Architecture.

