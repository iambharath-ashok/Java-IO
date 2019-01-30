# Serialization Int Questions

--------------------------------------------------------------------

## What is Serialization in Java

-	Object Serialization in Java is a process used to convert Object into a binary format 
-	which can be persisted into disk or sent over network to any other running Java virtual machine; 
-	the reverse process of creating object from binary stream is called deserialization in Java. 
-	Java provides Serialization API for serializing and deserializing object which includes java.io.Serializable, java.io.Externalizable, ObjectInputStream and ObjectOutputStream etc

--------------------------------------------------------------------

## What is the need of Serialization?

-	The serialization is used :-

	-	To send state of one or more object’s state over the network through a socket
	-	To save the state of an object in a file
	-	An object’s state needs to be manipulated as a stream of bytes

--------------------------------------------------------------------

## How to make a Java class Serializable?

-	Making a class Serializable in Java is very easy,Java class just needs to implements java.io.Serializable interface 
-	And JVM will take care of serializing object in default format

--------------------------------------------------------------------

## What is the difference between Serializable and Externalizable interface in Java?

-	Externalizable provides us writeExternal() and readExternal() method 
-	Which gives us flexibility to control java serialization mechanism instead of relying on Java's default serialization
-	Correct implementation of Externalizable interface can improve performance of application drastically
-	Data Integrity and Security issues can be solved

--------------------------------------------------------------------

## How many methods Serializable has? If no method then what is the purpose of Serializable interface?

-	Serializable interface doesn't have any method and also called Marker Interface in Java
-	When Class Implements java.io.Serializable interface it becomes Serializable in Java 
-	And gives compiler an indication that use Java Serialization mechanism to serialize this object

--------------------------------------------------------------------

## What is serialVersionUID? What would happen if you don't define this?

-	serialVersionUID is a static fianl field that is stamped to Serializable Class
-	serialVersionUID is usually a hashcode of Object
-	serialVersionUID is used for version control of object
-	Java serialization process relies on correct serialVersionUID 
	-	For recovering state of serialized object and throws java.io.InvalidClassException in case of serialVersionUID mismatc
--------------------------------------------------------------------
	
## While serializing you want some of the members not to serialize? How do you achieve it?

-	We need to make those fields as Transient or Static

--------------------------------------------------------------------

## What will happen if one of the members in the class doesn't implement Serializable interface?

-	If we try to serialize an object of a class which implements Serializable,
-	But the object includes a reference to an non- Serializable class 
-	Then a ‘NotSerializableException’ will be thrown at runtime

--------------------------------------------------------------------

## Can you Customize Serialization process or can you override default Serialization process in Java?

-	Yes we customize serialization process 
-	We can customize using ObjectOutputStream.writeObject (saveThisobject) and ObjectInputStream.readObject()
-	If we define these two methods in your class then JVM will invoke these two methods instead of applying default serialization mechanism
-	Making these methods private to avoid being inherited, overridden or overloaded

--------------------------------------------------------------------

## Which kind of variables is not serialized during Java Serialization?

-	Since static variables belong to the class and not to an object 
	-	They are not the part of the state of object so they are not saved during Java Serialization process
	-	As Java Serialization only persist state of object and not object itself

-	Transient variables are also not included in java serialization process 
	-	And are not the part of the object’s serialized state

--------------------------------------------------------------------

## Can we transfer a Serialized object vie network?

-	Yes we can transfer a Serialized object via network 
	-	Because Java serialized object remains in form of bytes which can be transmitter via network
	-	We can also store serialized object in Disk or database as Blob

--------------------------------------------------------------------

## What are the compatible changes and incompatible changes in Java Serialization Mechanism?

-	The real challenge lies with change in class structure by adding any field, method or removing any field or method is that with already serialized object
-	Adding any field or method comes under compatible change
-	Changing class hierarchy or UN-implementing Serializable interfaces some under non compatible changes

	-	List of changes which are compatible:
		-	Add fields
		-	Change a field from static to non-static
		-	Change a field from transient to non-transient
		-	Add classes to the object tree
		
	-	List of incompatible changes:
		-	Delete fields
		-	Change class hierarchy
		-	Change non-static to static
		-	Change non-transient to transient
		-	Change type of a primitive field

--------------------------------------------------------------------

## Suppose you have a class which you serialized it and stored in persistence and later modified that class to add a new field. What will happen if you deserialize the object already serialized?

-	It depends on whether class has its own serialVersionUID or not.
-	If we don't provide serialVersionUID in our code java compiler will generate it 
-	And normally it’s equal to hashCode of object
-	By adding any new field there is chance that new serialVersionUID generated for that class version is not the same of already serialized object
-	In this case Java Serialization API will throw java.io.InvalidClassException
-	Its recommended to have your own serialVersionUID in code and make sure to keep it same always for a single class

--------------------------------------------------------------------

## Which methods are used during Serialization and DeSerialization process in Java?

-	readObject(), writeObject, readExternal(), writeExternal() methods
-	Java Serialization is done by java.io.ObjectOutputStream class
-	ObjectOutputStream class is a filter stream which is wrapped around a lower-level byte stream 
-	To handle the serialization mechanism
-	To store any object via serialization mechanism 
-	We call ObjectOutputStream.writeObject(saveThisobject) 
-	And to deserialize that object we call ObjectInputStream.readObject() method
-	Call to writeObject() method trigger serialization process in java
-	readObject() method is used to read bytes from the persistence(Stream) and to create object from those bytes
-	And its return an Object which needs to be type cast to correct type
--------------------------------------------------------------------

## Other than Serialization what are the different approach to make object Serializable?

-	Three alternate approaches can serialize Java objects:

	###	1.	"roll-your-own" serialization approach
	-	We can write an object's content directly via either:
	-	The ObjectOutputStream or the DataOutputStream	
	-	This approach provides a performance advantage over Java serialization
	
	###	2.	XML serialization or JSON for data interchange	
	
	###	3.	Implement Externalizable Interface
	-	Which extends Serializable
	-	Developer is responsible for implementing the writeExternal() and readExternal() methods
	-	As a result, a developer has sole control over reading and writing the serialized objects
	
--------------------------------------------------------------------

## What happens if the object to be serialized includes the references to other serializable objects?


-	Then all those object’s state also will be saved as the part of the serialized state of the object
-	The whole object graph of the object to be serialized will be saved during serialization 
--------------------------------------------------------------------
## What will be the value of transient variable after de-serialization?

-	It’s default value. e.g. if the transient variable in question is an int, it’s value after deserialization will be zero

	Code Snippet:
	
		public class TestTransientVal implements Serializable { 
			  private static final long serialVersionUID =  -22L; 
			  private String name; 
			  transient private int age; 
			TestTransientVal(int age, String  name) { 
			  this.age = age; 
			  this.name = name; 
			} 
			public static void main(String [] args)  { 
			 TestTransientVal c = new TestTransientVal(1,"ONE"); 
			 System.out.println("Before serialization:" + c.name + " "+ c.age); 
			 try { 
			   FileOutputStream fs =new FileOutputStream("testTransient.ser");
			   ObjectOutputStream os = new ObjectOutputStream(fs); 
			   os.writeObject(c); 
			   os.close(); 
			 } catch (Exception e) {  e.printStackTrace(); } 
			   try { 
				 FileInputStream fis =new FileInputStream("testTransient.ser");
				 ObjectInputStream ois =new ObjectInputStream(fis); 
				 c = (TestTransientVal)  ois.readObject(); 
				 ois.close(); 
			   } catch (Exception e) {  e.printStackTrace(); } 
				System.out.println("After  de-serialization:" + c.name +" "+ c.age); 
			 } 
			}
--------------------------------------------------------------------
## Does the order in which the value of the transient variables and the state of the object using the defaultWriteObject() method are saved during serialization matter?


-	Yes order must be preserved for while restoring the object’s state
-	Yes, while restoring the object’s state transient variables and the serializable variables that are stored
-	Must be restored in the same order in which they were saved
--------------------------------------------------------------------

## How can one customize the Serialization process? or What is the purpose of implementing the writeObject() and readObject() method?

-	When we want to store the transient variables state as a part of the serialized object at the time of serialization 
-	Class must implement the following methods writeObject() and readObject()
 


	Code Snippet:
		
		private void wrtiteObject(ObjectOutputStream outStream) {
		  //code to save the transient variables state 
		  //as a part of serialized  object
		}
		private void readObject(ObjectInputStream inStream) {
		 //code to read the transient variables state 
		//and assign it to the  de-serialized object
		}

		public class TestCustomizedSerialization implements Serializable { 
		 private static final long serialVersionUID =-22L; 
		 private String noOfSerVar; 
		 transient private int noOfTranVar; 
		 TestCustomizedSerialization(int noOfTranVar, String  noOfSerVar) { 
			this.noOfTranVar = noOfTranVar; 
			this.noOfSerVar = noOfSerVar; 
		  } 
		  private void writeObject(ObjectOutputStream os) { 
			try { 
			  os.defaultWriteObject(); 
			  os.writeInt(noOfTranVar); 
			} catch (Exception e) {  e.printStackTrace(); } 
		  } 
		private void readObject(ObjectInputStream is) { 
		 try { 
		  is.defaultReadObject(); 
		   int noOfTransients =  (is.readInt()); 
		 } catch (Exception e) { 
		   e.printStackTrace(); } 
		 } 
		public int getNoOfTranVar() { 
		return noOfTranVar; 
		}
--------------------------------------------------------------------

## If a class is serializable but its superclass in not, what will be the state of the instance variables inherited from super class after deserialization?

	Code Snippet:
	
		public class SerializationInheritance {
	
			public static void main(String[] args) {
				
				Child c = new Child();
				FileOutputStream fos= null;
				ObjectOutputStream oos = null;
				try {
					 fos = new FileOutputStream(new File("Child.ser"));
					oos  = new ObjectOutputStream(fos);
					c.age = 15;
					System.out.println("Before Serialization: "+c.age);
					oos.writeObject(c);
					
				} catch (FileNotFoundException e) {
					e.printStackTrace();
				} catch (IOException e) {
					e.printStackTrace();
				}finally {
					try {
						fos.close();
						oos.close();
					} catch (IOException e) {
						e.printStackTrace();
					}
				}
				
				FileInputStream fis = null;
				ObjectInputStream ois = null;
				
				try {
					fis  = new FileInputStream(new File("Child.ser"));
					ois = new ObjectInputStream(fis);
					Child child =(Child) ois.readObject();
					System.out.println("After Serialization: "+child.age);
				} catch (FileNotFoundException e) {
					e.printStackTrace();
				} catch (IOException e) {
					e.printStackTrace();
				} catch (ClassNotFoundException e) {
					e.printStackTrace();
				}finally {
					try {
						fis.close();
						ois.close();
					} catch (IOException e) {
						e.printStackTrace();
					}
				}
			}
		}
		class  Child extends Parent implements Serializable {
			  private static final long serialVersionUID = 1L; 
			String color;
			public Child() {
				this.color = "blue";
				this.age = 13;
			}
		}
		class Parent{
			int age = 90;
			Parent() {
			}
		}


	Output:
	
		Before Serialization: 15
		After Serialization: 90

-	The instance variable "age" is inherited from superclass which is not serializable.
-	Therefore while restoring it the non-serializable superclass constructor runs
-	And its value is set to 90 and is not same as the value saved during serialization which is 15

--------------------------------------------------------------------

## If a class is Serializable but its super class in not, what will be the state of the instance variables inherited from super class after deserialization?

-	Java serialization process only continues in object hierarchy till the class is Serializable i.e. implements Serializable
-	Values of the instance variables inherited from super class will be initialized by calling constructor of Non-Serializable Super class during deserialization process
-	Once the constructor chaining gets started its not possible to stop that 
-	Hence even if classes higher in hierarchy implements Serializable interface , there constructor will be executed

--------------------------------------------------------------------

## Suppose super class of a new class implement Serializable interface, how can you avoid new class to being serialized?

-	To avoid Java serialization we need to implement writeObject() and readObject() method in your Class and need to throw NotSerializableException from those method


--------------------------------------------------------------------

## To serialize an array or a collection all the members of it must be serializable. True /False?

-	True

## Are the constructors in an object invoked when it is de-serialized?

-	No. When a class is De-serialized, initialization (constructor’s, initializer’s) does not take place
-	The state of the object is retained as it is

--------------------------------------------------------------------
## Which of the following interface would you implement for a serializable class?

-	Serializable
-	Externalizable
--------------------------------------------------------------------

## Which of the following is a correct difference between Serializable and Externalizable interface in Java?

-  	Serializable is a marker interface while Externalizable is not
-  	Externalizable provides readExternal() and writeExternal() to implement Serialization 
-	Whereas the Serializable doesn't expose any

--------------------------------------------------------------------
## Do you need to implement any method of Serializable interface to make an object serializable?
-	No. Serializable is a Marker Interface. It does not have any methods.

--------------------------------------------------------------------
## Which of the following is not a valid reason for static member variables being out of the java serialization process?

-	Serialization is applicable for objects or primitive data types only whereas static members are class level variables.
-	The static variable are allocated memory only once in a class area during class loading.
--------------------------------------------------------------------
## Please select when will you use the Serializable interface?

-	When you have small objects and we know that most or all attributes are required to be serialized

--------------------------------------------------------------------
## Please select when will you use the Externalizable interface?

-	When you have a big Java object with hundreds of attributes and we want to serialize only the dynamically selected attributes.

--------------------------------------------------------------------
## What are the different approaches for making an object Serializable other than Serializable interface?

-	Use Externalizable interface and implement the writeExternal() and readExternal() methods.
- 	Directly use the ObjectOutputStream or the DataOutputStream.
- 	Use the JSON Data Transfer.
--------------------------------------------------------------------
## What is the need of the Serialization Proxy Pattern?


-	Through a proxy, an attacker can not hit on the real object thus the object safety is appropriately imposed
--------------------------------------------------------------------
## Which of the following correctly defines the behaviour of a transient variable?

-	These variables don't convert to the byte stream and don't become the part of the object's serialized state.
--------------------------------------------------------------------
## What will be the value of a transient variable after deserialization?

-	It'll be set to default value after deserialization
-   For example, an integer variable will be set to '0'
--------------------------------------------------------------------
## Which of the following is the correct behaviour for static variables during serialization?

-	The static variables belong to the class are not the part of the state of the object so they are not saved as the part of serialized object.
--------------------------------------------------------------------
## What would happen with the superclass instance variables of a class if the class is serializable but its superclass is not?

-	They will initialize to the values given during the original construction of the object

--------------------------------------------------------------------
## Which of the following do you think is the purpose of the writeObject() and readObject() methods in Serialization process?

-	The value of transient variables are saved using the writeObject() and restored by the readObject() method
--------------------------------------------------------------------
## Which of the following is not the correct method to generate a  SerialVersionUID ?

-	Write a method to calculate the class hash value

--------------------------------------------------------------------
## How can a subclass prevent from Serialization if its superClass supports Serialization?

-	Override writeObject() and readObject()
-	Sub-class should have writeObject(), readObject() and readObjectNoData() methods which must throw NotSerializableException

--------------------------------------------------------------------
## Please select when to add readObjectNoData() during serialization?

-	When you implement a class with instance fields that is serializable and extendable

--------------------------------------------------------------------
## What happens if an object is serializable but it includes a reference to a non-serializable object?

-	‘NotSerializableException’ will be thrown at runtime.
--------------------------------------------------------------------
## What will be the values of int and Integer transient members of a class after the DeSerialization process?

-	 int=>0 and Integer=>null
--------------------------------------------------------------------
## What would happen if you try to alter any class variable without specifying the  serialVersionUID ?
-	java.io.InvalidClassException will be thrown.


--------------------------------------------------------------------













