# Java IO Interview Questions


What is I/O in java?.

-	IO Contains all the files and packages that are used to perform IO Operations
-	IO Package contains class that are used to work with Streams from Input Source and Output Destination
-	Stream in java.io supports many data formats like primitives, Object, characters
-	Stream can represent many kind of sources and destinations including files, devices, n/w, db


Tell something about BufferedWriter ? What are flush() and close() used for ?

-	A Buffer is a temporary storage area for data. 
-	It is an abstract class that creates a buffered character-output stream.
-	Flush() is used to clear all the data characters stored in the buffer and clear the buffer.
-	Close() is used to closes the character output stream.


What is Scanner class used for ? when was it introduced in Java ?

-	Scanner class introduced in Java 1.5 for reading Data Stream from the input device, File, Path, String


What is a stream and what are the types of Streams and classes of the Streams?

-	A Stream is an abstraction that either produces or consumes information
-	There are two types of Streams :

-	Byte Streams: Provide a convenient means for handling input and output of bytes
-	Character  Streams:  Provide a convenient means for handling input & output of characters

What is the difference between the Reader/Writer class hierarchy and the InputStream/OutputStream class hierarchy?

-	The Reader/Writer class hierarchy is character-oriented, and the InputStream/OutputStream class hierarchy is byte-oriented	


Which is the abstract parent class of FileWriter ?

-	OutputStreamWriter

Which class is used to read streams of raw bytes from a file?

FileInputStream

Which is the Parent class of FileInputStream ?

-	InputStream

Which exceptions should be handled with the following code ?
FileOutputStream fileOutputStream = new FileOutputStream(new File("newFile.txt"));

Ans. FileNotFoundException

Will this code compile fine ?
ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(new File("newFile.txt")));

-	Yes


Which is the Parent Class of  ByteArrayInputStream and AudioInputStream ?

InputStream

Which interfaces are implemented by  InputStream?

Ans.[Closeable,  AutoCloseable]

What is the package name for  ObjectInputStream class?

java.io

Which is the Parent Class of  StringBufferInputStream class?

Ans.InputStream

Which is the Parent Class of  SequenceInputStream class?

Ans.InputStream

Which is the Parent Class of  FilterInputStream class?

Ans.InputStream

What is the package name for  FilterInputStream class?

Ans.java.io

Which interfaces are implemented by  FileInputStream?

Ans.[Closeable,  AutoCloseable]

Which is the Parent Class of InputStream class?

Ans.Object

Which is the Parent Class of FileInputStream class?

Ans.InputStream

What is the difference between System.out ,System.err and System.in?

-	System.out and System.err both represent the monitor by default 
-	And hence can be used to send data or results to the monitor
-	But System.out is used to display normal messages 
-	And results whereas System.err is used to display error messages
-	System.in represents InputStream object, which by default represents standard input device, i.e., keyboard


What is the purpose of the System class?

-	The purpose of the System class is to provide access to system resources


What will the following code print when executed on Windows ?

public static void main(String[] args){
  String parent = null;
  File file = new File("/file.txt");
  System.out.println(file.getPath());
  System.out.println(file.getAbsolutePath());
  try {
   System.out.println(file.getCanonicalPath());
  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
 }

Ans. 

\file.txt
C:\file.txt
C:\file.txt


What will be the output of following code ?

public static void main(String[] args){
  String name = null;
  File file = new File("/folder", name);
  System.out.print(file.exists());
 }

Ans. NullPointerException at line: 

"File file = new File("/folder", name);"


What will be the output of following code ?

public static void main(String[] args){
  String parent = null;
  File file = new File(parent, "myfile.txt");
  try {
   file.createNewFile();
  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
 }
 
 Ans. It will create the file myfile.txt in the current directory.
 
 
What will be the output of following code ?

public static void main(String[] args){
  String child = null;
  File file = new File("/folder", child);
  try {
   file.createNewFile();
  } catch (IOException e) {
   e.printStackTrace();
  }
 }
Ans. NullPointerException at line: 
 File file = new File("/folder", child);

Q7.  What will be the output of following code, assuming that currently we are in c:\Project ? 

public static void main(String[] args){
  String child = null;
  File file = new File("../file.txt");
  System.out.println(file.getPath());
  System.out.println(file.getAbsolutePath());
  try {
   System.out.println(file.getCanonicalPath());
  } catch (IOException e) {
   e.printStackTrace();
  }
 }

Ans. 

..\file.txt
C:\Workspace\Project\..\file.txt
C:\Workspace\file.txt


Q8.  Which is the abstract parent class of FileWriter ?

Ans. OutputStreamWriter

Q9.  Which class is used to read streams of characters from a file?

Ans. FileReader

Q10.  Which class is used to read streams of raw bytes from a file?

Ans. FileInputStream

Q11.  Which is the Parent class of FileInputStream ?

Ans. InputStream

Q12.  Which of the following code is correct ?

a. 


FileWriter fileWriter = new FileWriter("../file.txt");
File file = new File(fileWriter );
BufferedWriter bufferedOutputWriter = new BufferedWriter(fileWriter);

b. 

BufferedWriter bufferedOutputWriter = new BufferedWriter("../file.txt");
File file = new File(bufferedOutputWriter );
FileWriter fileWriter = new FileWriter(file);

c. 

File file = new File("../file.txt");
FileWriter fileWriter = new FileWriter(file);
BufferedWriter bufferedOutputWriter = new BufferedWriter(fileWriter);

d.

File file = new File("../file.txt");
BufferedWriter bufferedOutputWriter = new BufferedWriter(file); 
FileWriter fileWriter = new FileWriter(bufferedOutputWriter );

Ans. c. 

File file = new File("../file.txt");
FileWriter fileWriter = new FileWriter(file);
BufferedWriter bufferedOutputWriter = new BufferedWriter(fileWriter);

Q13.  Which exception should be handled in the following code ?

File file = new File("../file.txt");
FileWriter fileWriter = new FileWriter(file);

Ans. IOException

Q14.  Which exceptions should be handled with the following code ?

FileOutputStream fileOutputStream = new FileOutputStream(new File("newFile.txt"));

Ans. FileNotFoundException

Q15.  Will this code compile fine ?

ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(new File("newFile.txt")));

Ans. Yes.

Q16.  What is the problem with this code ?

class BuggyBread1 {
 
 private BuggyBread2 buggybread2;
 
 public static void main(String[] args){
  try {
   BuggyBread1 buggybread1 = new BuggyBread1();
      ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(new File("newFile.txt")));
      objectOutputStream.writeObject(buggybread1);
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  
 }
}

Ans. Though we are trying to serialize BuggyBread1 object but we haven't declared the class to implement Serializable.

This will throw java.io.NotSerializableException upon execution.

Q17.  Will this code run fine if BuggyBread2 doesn't implement Serializable interface ?

class BuggyBread1 implements Serializable{
 
 private BuggyBread2 buggybread2 = new BuggyBread2();
 
 public static void main(String[] args){
  try {
   BuggyBread1 buggybread1 = new BuggyBread1();
      ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(new File("newFile.txt")));
      objectOutputStream.writeObject(buggybread1);
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  
 }
}

Ans. No, It will throw java.io.NotSerializableException.

Q18.  Will this code work fine if BuggyBread2 doesn't implement Serializable ?

class BuggyBread1 extends BuggyBread2 implements Serializable{
 
 private int x = 5;
 
 public static void main(String[] args){
  try {
   BuggyBread1 buggybread1 = new BuggyBread1();
      ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(new File("newFile.txt")));
      objectOutputStream.writeObject(buggybread1);
  } catch (Exception e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  
 }
}

Ans. Yes.


