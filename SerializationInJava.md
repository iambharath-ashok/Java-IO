# Serialization in Java – Java Serialization

-	Serialization in Java allows us to convert an Object to stream 
-	That we can send over the network or save it as file or store in DB for later usage
-	Deserialization is the process of converting Object stream to actual Java Object to be used in our program

-	If we want a class object to be SERIALIZABLE that class should implement the java.io.Serializable interface
-	Serialization in java is implemented by ObjectInputStream and ObjectOutputStream
-	If we want an object property to be not serialized to stream, we can use transient keyword 

--------------------------------------------------------------------

## ObjectOutputStream in Java – write Object to File

-	ObjectOutputStream in Java can be used to convert an object to OutputStream
-	The process of converting object to stream is called serialization in java

-	Once an object is converted to Output Stream:
	-	It can be saved to file or database, send over the network or used in socket connections
	-	So we can use FileOutputStream to write Object to file
	
-	When we create an instance of ObjectOutputStream, we have to provide the OutputStream to be used
-	This OutputStream is further used by ObjectOutputStream to channel the object stream to underlying output stream:
	-	For example FileOutputStream

-	The object that we want to serialize should implement java.io.Serializable interface
-	Serializable is just a marker interface and doesn’t have any abstract method
-	we will get java.io.NotSerializableException if the class doesn’t implement Serializable interface

	
### Java ObjectOutputStream Example to write object to file	

	Code Snippet:
	
		public class Employee implements Serializable {

			private static final long serialVersionUID = -299482035708790407L;

			private String name;
			private String gender;
			private int age;

			private String role;
			// private transient String role;
		}

	
-	It’s not a requirement to have getter/setter for all the properties
-	Or to have a non-argument constructor
	
	
		Code Snippet:
			
			public static void main(String[] args) {
				Employee emp = new Employee("bharath");

				emp.setAge(35);
				emp.setGender("Male");
				emp.setRole("CEO");
				System.out.println(emp);
				
				try {
					FileOutputStream fos = new FileOutputStream("EmployeeObject.ser");
					ObjectOutputStream oos = new ObjectOutputStream(fos);
					// write object to file
					oos.writeObject(emp);
					System.out.println("Done");
					// closing resources
					oos.close();
					fos.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
### ObjectOutputStream with transient
	
	
-	If we don’t want some object property to be converted to stream,
-	We have to use transient keyword for that. 
-	For example, the role property like below and it will not be saved

	Code snippet:
	
		private transient String role;
		
	
### ObjectOutputStream and serialVersionUID
	
-	serialVersionUID will be used by ObjectOutputStream and ObjectInputStream classes for write and read object operations
-	Although it’s not mandatory to have this field, but we should keep it.
-	Otherwise anytime if we change your class:
	
	-	Erlier serialized objects will not effect, it will start failing	
	
	Output:
			Exception in thread "main" java.io.InvalidClassException: serialization.Hosting; local class incompatible: stream classdesc serialVersionUID = -8922886121092364976, local class serialVersionUID = -3559576588472889564
		at java.io.ObjectStreamClass.initNonProxy(ObjectStreamClass.java:621)
		at java.io.ObjectInputStream.readNonProxyDesc(ObjectInputStream.java:1623)
		at java.io.ObjectInputStream.readClassDesc(ObjectInputStream.java:1518)
		at java.io.ObjectInputStream.readOrdinaryObject(ObjectInputStream.java:1774)
		at java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1351)
		at java.io.ObjectInputStream.readObject(ObjectInputStream.java:371)
		at java.util.ArrayList.readObject(ArrayList.java:791)
		at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
		at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
		at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
		at java.lang.reflect.Method.invoke(Method.java:497)
		at java.io.ObjectStreamClass.invokeReadObject(ObjectStreamClass.java:1017)
		at java.io.ObjectInputStream.readSerialData(ObjectInputStream.java:1900)
		at java.io.ObjectInputStream.readOrdinaryObject(ObjectInputStream.java:1801)
		at java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1351)
		at java.io.ObjectInputStream.readObject(ObjectInputStream.java:371)
		at serialization.SerializationEx.desializeObject(SerializationEx.java:38)
		at serialization.SerializationEx.main(SerializationEx.java:25)

-------------------------------------------------------	
## ObjectInputStream in Java – read Object from File

-	ObjectInputStream class is used to convert InputStream to Object
-	The process of converting input stream to object is called deserialization

-	ObjectInputStream constructor take InputStream as argument
-	Since we are reading Serialized Object from file:
	-	We will be using FileInputStream with our ObjectInputStream to read object from file
	
	
	
### Java ObjectInputStream Example	
	
-	java.io.ObjectInputStream readObject() is used to read input stream to Object
-	We have to do class casting to convert Object to actual class

	Code Snippet:
	
		public static void main(String[] args) {
			try {
				FileInputStream is = new FileInputStream("EmployeeObject.ser");
				ObjectInputStream ois = new ObjectInputStream(is);
				Employee emp = (Employee) ois.readObject();
				ois.close();
				is.close();
				System.out.println(emp.toString());
			} catch (ClassNotFoundException | IOException e) {
				e.printStackTrace();
			}
		}
	
-----------------------------------------------------------------------------

## Class Refactoring with Serialization and serialVersionUID

-	Serialization in java permits some changes in the java class 
-	And they can be ignored.
-	Some of the changes in class that will not affect the deserialization process are:

	-	Adding new variables to the class
	-	Changing the variables from transient to non-transient
		-	For serialization it’s like having a new field
	-	Changing the variable from static to non-static
		-	For serialization it’s like having a new field

-	But for all these changes to work, the java class should have serialVersionUID defined for the class
-	If we delete some of fields or variables after serialization then we will get java.io.InvalidClassException 
	
	Exception while deserialization:
	
		java.io.InvalidClassException: com.bharath.serialization.Employee; local class incompatible: stream classdesc serialVersionUID = -6470090944414208496, local class serialVersionUID = -6234198221249432383

-	Actually if the class doesn’t define serialVersionUID, it’s getting calculated automatically and assigned to the class
-	Java uses class variables, methods, class name, package etc to generate this unique long number		

-----------------------------------------------------------------------------

## Java Externalizable Interface

-	Used to add some data integrity validations while serializing and de-serializing
-	We can do this by implementing java.io.Externalizable interface 
-	And we need to provide implementation for writeExternal() and readExternal() methods 
-	writeExternal() and readExternal() methods will be used in serialization process
	
	
	Code Snippet of Externalizable:
	
		public class Person implements Externalizable{

			private int id;
			private String name;
			private String gender;
			
			@Override
			public void writeExternal(ObjectOutput out) throws IOException {
				out.writeInt(id);
				out.writeObject(name+"xyz");
				out.writeObject("abc"+gender);
			}

			@Override
			public void readExternal(ObjectInput in) throws IOException,
					ClassNotFoundException {
				id=in.readInt();
				//read in the same order as written
				name=(String) in.readObject();
				if(!name.endsWith("xyz")) throw new IOException("corrupted data");
				name=name.substring(0, name.length()-3);
				gender=(String) in.readObject();
				if(!gender.startsWith("abc")) throw new IOException("corrupted data");
				gender=gender.substring(3);
			}
			
			// Getters and Setters
			
		}
		
-	We can throw exception if after reading the stream data, the integrity checks fail
-	which one is better to be used for serialization in java? - it’s better to use Serializable interface 

-----------------------------------------------------------------------------

## Java Serialization Methods

-	By default serialization is automatic
-	If we want to change the way we are saving data
	-	For example we have some sensitive information in the object 
	-	And before saving/retrieving we want to encrypt/decrypt it
-	There are four methods that we can provide in the class to change the serialization behavior
-	If these methods are present in the class, they are used for serialization purposes	
	
-	readObject(ObjectInputStream ois): 
	-	If this method is present in the class
	-	ObjectInputStream readObject() method will use this method for reading the object from stream
	
-	writeObject(ObjectOutputStream oos): 
	-	If this method is present in the class
	-	ObjectOutputStream writeObject() method will use this method for writing the object to stream
	- 	We can use this to maintain data integrity using object variables
	
-	Object writeReplace():
	-	If this method is present
	-	Then after serialization process this method is called 
	-	And the object returned is serialized to the stream

-	Object readResolve(): 
	-	If this method is present
	-	Then after deserialization process, this method is called to return the final object to the caller program
	

-	Usually while implementing above methods, it’s kept as private so that subclasses can’t override them
-	They are meant for serialization purpose only and keeping them private avoids any security issue

-----------------------------------------------------------------------------

## Serialization with Inheritance

-	Serialization with Inheritance will not supported
-	Parent class fields will not be included in the Serialization and Deserialization
-	If we depend on the automatic serialization behavior and the superclass has some state
-	Then they will not be converted to stream and hence not retrieved later on
-	This is one place, where readObject() and writeObject() methods really help
-	By providing their implementation we can save the super class state to the stream and then retrieve it later on

	
	Code Snippet:
	
		public class SuperClass {

			private int id;
			private String value;
			
			public int getId() {
				return id;
			}
			public void setId(int id) {
				this.id = id;
			}
			public String getValue() {
				return value;
			}
			public void setValue(String value) {
				this.value = value;
			}	
		}
		
		public class SubClass extends SuperClass implements Serializable, ObjectInputValidation{

			private static final long serialVersionUID = -1322322139926390329L;

			private String name;

			public String getName() {
				return name;
			}

			public void setName(String name) {
				this.name = name;
			}
			
			@Override
			public String toString(){
				return "SubClass{id="+getId()+",value="+getValue()+",name="+getName()+"}";
			}
			
			//adding helper method for serialization to save/initialize super class state
			private void readObject(ObjectInputStream ois) throws ClassNotFoundException, IOException{
				ois.defaultReadObject();
				
				//notice the order of read and write should be same
				setId(ois.readInt());
				setValue((String) ois.readObject());	
			}
			
			private void writeObject(ObjectOutputStream oos) throws IOException{
				oos.defaultWriteObject();
				
				oos.writeInt(getId());
				oos.writeObject(getValue());
			}

			@Override
			public void validateObject() throws InvalidObjectException {
				//validate the object here
				if(name == null || "".equals(name)) throw new InvalidObjectException("name can't be null or empty");
				if(getId() <=0) throw new InvalidObjectException("ID can't be negative or zero");
			}	
		}

-   Order of writing and reading the extra data to the stream should be same
-	We can put some logic in reading and writing data to make it secure

-	The class is also implementing ObjectInputValidation interface
-	By implementing validateObject() method, we can put some business validations to make sure that the data integrity is not harmed
-	So in this way, we can serialize super class state even though it’s not implementing Serializable interface
-	This strategy comes handy when the super class is a third-party class that we can’t change

-----------------------------------------------------------------------------

## Serialization Proxy Pattern

-	We cant change the Class Structure after Serialization
-	Even though we dont need some variables we need to still keep for backward compatibility
-	Serialization causes huge security risks, an attacker can change the stream sequence and cause harm to the system
-	For example, user role is serialized and an attacker change the stream value to make it admin and run malicious code

-	Java Serialization Proxy pattern is a way to achieve greater security with Serialization
-	In this pattern, an inner private static class is used as a proxy class for serialization purpose
- 	This class is designed in the way to maintain the state of the main class
-	This pattern is implemented by implementing readResolve() and writeReplace() methods

	
	Code Snippet:
	
		public class Data implements Serializable{
			private static final long serialVersionUID = 2087368867376448459L;
			private String data;
			public Data(String d){
				this.data=d;
			}
			public String getData() {
				return data;
			}
			public void setData(String data) {
				this.data = data;
			}
			@Override
			public String toString(){
				return "Data{data="+data+"}";
			}
			
			//serialization proxy class
			private static class DataProxy implements Serializable{
			
				private static final long serialVersionUID = 8333905273185436744L;
				
				private String dataProxy;
				private static final String PREFIX = "ABC";
				private static final String SUFFIX = "DEFG";
				
				public DataProxy(Data d){
					//obscuring data for security
					this.dataProxy = PREFIX + d.data + SUFFIX;
				}
				
				private Object readResolve() throws InvalidObjectException {
					if(dataProxy.startsWith(PREFIX) && dataProxy.endsWith(SUFFIX)){
					return new Data(dataProxy.substring(3, dataProxy.length() -4));
					}else throw new InvalidObjectException("data corrupted");
				}
			}
			
			//replacing serialized object to DataProxy object
			private Object writeReplace(){
				return new DataProxy(this);
			}
			
			private void readObject(ObjectInputStream ois) throws InvalidObjectException{
				throw new InvalidObjectException("Proxy is not used, something fishy");
			}
		}

-	Both Data and DataProxy class should implement Serializable interface
-	DataProxy should be able to maintain the state of Data object
	-	DataProxy is inner private static class, so that other classes can’t access it
	-	DataProxy should have a single constructor that takes Data as argument

-	Data class should provide writeReplace() method returning DataProxy instance
	-	So when Data object is serialized, the returned stream is of DataProxy class 
	-	However DataProxy class is not visible outside, so it can’t be used directly

-	DataProxy class should implement readResolve() method returning Data object
	-	So when Data class is deserialized, internally DataProxy is deserialized 
	-	And when it’s readResolve() method is called, we get Data object

-	Finally implement readObject() method in Data class and throw InvalidObjectException to avoid hackers attack trying to fabricate Data object stream and parse it

---------------------------------------------------------------------

































































































