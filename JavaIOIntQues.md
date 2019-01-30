# Java IO Interview Questions

-------------------------------------------------------------
## What is I/O in Java?
-	Java I/O (Input and Output) is used to process the input and produce the output
-	Java makes use of the stream concepts to make I/O operation fast
-	IO Contains all the files and packages that are used to perform Input Output Operations
-	IO Package contains classes that are used to work with Streams from Input Source and Output Destination
-	Stream in java.io supports many data formats like primitives, Object, characters
-	Stream can represent many kind of sources and destinations including files, devices, n/w, db
-------------------------------------------------------------
## What is a IO stream?

-	Stream of data that flows from source to destination
-	Ex File Copying -	Two streams are involved – input stream and output stream
-	An input stream reads from the file and stores the data in the process (generally in a temporary variable)
-	The output stream reads from the process and writes to the destination file
-------------------------------------------------------------
## What is the necessity of two types of streams – byte streams and character streams?
-	Byte Stream is used to deal with Bytes or Binary Data
-	Character Streams are used to deal with Textual Data
-	Byte streams were introduced with JDK 1.0 
	-	And operate on the files containing ASCII characters

-	Java Supports other language characters also known as Unicode Characters
-	To read files containing Unicode Characters, Character Streams were introduced in JDK 1.1
-	ASCII is subset of Unicode 
-	For files with English Characters we can go with either Byte Streams or Character Streams
-------------------------------------------------------------

## What are the FileReader and BufferedReader?
-	We have 2 ways to read data from the file
	-	FileReader:-
		-	We can use FileReader to read Character data from the file
		-	FileReader contains
		-	read() and read(char[ ] ch) methods to read the data char by char from the file
	-	BufferedReader:-
		-	BufferedReader read the file line by line 
		-	And it can’t communicate directly with the file
		-	It can communicate with some reader object
		-	It is most enhance reader
		-	BufferedReader contains read(), read(char[] ch), close() and readLine() methods
-------------------------------------------------------------

## What are the FileWriter, BufferedWriter and PrintWriter?
-	We have 3 ways to write character data into a file:	
	-	1. FileWriter:-
		-	Note: - if the specified file is not already available then this constructor will create that file.
		-	Methods:-
				-	write(int ch):- to write single character
				-	write(char[] ch):- to write characters
				-	write(String s):- to write string
				-	flush():- to  guaranty that total data should be written properly to the file including last character
				-	close():- to close the string
				-	delete():- to delete file or directory
	-	2. BufferedWriter:-
		-		To overcome FileWriter problem we use BufferedWriter to write text data in to the file
		-		In this we have one extra method “newline()” to insert a newline
		-		BufferedWriter can’t communicate directly with the file
		-		In this approach we should take newline() method for every line so length of the code will be increased
	-	3. PrintWriter:-
		-	To overcome the BufferedWriter Problem we use PrintWriter to write any type of data into the file
		-	printWriter is most enhance writer.
		-	If file not available then I will create the file and write data into file
		-	Methods:-
		-	Here we have some more extra methods print(char ch), print(int i), print(double d), print(String s), 
		print(Boolean b).
-------------------------------------------------------------
## What are the most enhanced Writer and Reader?
-	The most enhanced writer is PrintWriter and the most reader is BufferedReader.
-------------------------------------------------------------
## What are the super most classes of all streams?

-	Byte Streams - Input Stream and Output Stream
-	Character Streams - Reader and Writer	
-------------------------------------------------------------

## What are FileInputStream and FileOutputStream?

-  	FileInputStream and FileOutputStream is used to copy files from source to destination
-	These classes works with Bytes
-	These classes work well with files containing less data of a few thousand bytes 
-	As by performance these are very poor
-	For larger data, it is preferred to use BufferedInputStream (or BufferedReader) and BufferedOutputStream (or BufferedWriter)
-------------------------------------------------------------
## Which you feel better to use – byte streams or character streams?

-	I feel personally to go with character streams as they are the latest
-	Many features exist in character streams that do not in byte streams like:
	-	a) using BufferedReader in place of BufferedInputStreams and DataInputStream (one stream for two)
	-	b) using newLine() method to go for next line and for this effect we must go for extra coding in byte streams etc
-------------------------------------------------------------
	
## What System.out.println()?

-	"println()" is a method of PrintStream class. 
-	"out" is a static object in "System" class of type PrintStream class
-	System is a class from java.lang package 
-	Used to interact with the underlying operating system by the programmer
-------------------------------------------------------------

## What are filter streams?

-	Filter streams are a category of IO streams 
-	Whose responsibility is to add extra functionality (advantage) to the existing streams 
-	Like giving line numbers in the destination file that do not exist in the source file
-	Or increasing performance of copying etc
-------------------------------------------------------------
## Name the filter streams available?

-	There are four filter streams in java.io package:
	-	Two in byte streams side and two in character streams side
	-	They are FilterInputStream, FilterOutputStream, FilterReader and FilterWriter
	-	These classes are abstract classes and you cannot create of objects of these classes
-------------------------------------------------------------	
## Name the filter stream classes on reading side of byte stream?

-	There are four classes 
	–   LineNumberInputStream 
		-	(the extra functionality is it adds line numbers in the destination file)
	-	DataInputStream 
		-	(contains special methods like readInt(), readDouble() and readLine() etc that can read an int, a double and a string at a time)
	-	BufferedInputStream 
		-	(gives buffering effect that increases the performance to the peak)
	-	PushbackInputStream 
		-	(pushes the required character back to the system)
	
	Code Snippet:
		
		public class LISDemo
		{
		  public static void main(String args[]) throws IOException
		  {                                                                                   // reading-side
			String str1 = "The\nonly\ntrue\nwisdom\nis\nin\nknowing\nyou\nknow\nnothing\n";
			StringBufferInputStream sbstream = new StringBufferInputStream(str1);             // low-level
			LineNumberInputStream listream = new LineNumberInputStream(sbstream);             // high-level
			DataInputStream distrea m = new DataInputStream(listream);                        // high-level
																		  // writing-side
			FileOutputStream fostream = new FileOutputStream("Quotes.txt");                   // low-level
			BufferedOutputStream bostream = new BufferedOutputStream(fostream);               // high-level
			PrintStream pstream = new PrintStream(bostream);                                  // high-level
			String str; 
			while((str = distream.readLine()) != null)
			{
			  String s1 = listream.getLineNumber() + ".  " + str;
			  pstream.println(s1);
			  System.out.println(s1);
			}
			pstream.close();  bostream.close(); fostream.close();
			distream.close(); listream.close(); sbstream.close();
		  }
		}
	
	Data InputStream and DataOutput Stream:
		
		public class DISandDOS1
		{
		  public static void main(String args[]) throws IOException
		  {
																   // reading side
			FileInputStream fistream = new FileInputStream("pqr.txt"); // a low-level byte stream
			DataInputStream distream = new DataInputStream(fistream);    //   a high-level byte stream
														  // writing side
			FileOutputStream fostream = new FileOutputStream("def.txt"); // a low-level byte stream
			DataOutputStream dostream = new DataOutputStream(fostream); // a high-level byte stream

			int numLines = 0;
			String str;
			String lineEnd = System.getProperty("line.separator");
			while( ( str = distream.readLine() ) != null )
			{
			  dostream.writeBytes(str);    // writes in the destination file, def.txt
			  dostream.writeBytes(lineEnd); // gives a new line in def.txt
			  System.out.println(str);      // prints at DOS prompt
			  numLines++;
			}
			System.out.println("No. of lines: " + numLines);
			dostream.close();   fostream.close();
			distream.close();    fistream.close();
		  }
		}
-----------------------------------------------------

## What is the functionality of SequenceInputStream?

-	It is very useful to copy multiple source files into one destination file with very less code

	Code Snippet:	
	
			public class TwoFiles
			{
			  public static void main(String args[]) throws IOException
			  {
				FileInputStream fistream1 = new FileInputStream("Sayings.txt");  // first source file
				FileInputStream fistream2 = new FileInputStream("Morals.txt");  //second source file
			 
				SequenceInputStream sistream = new SequenceInputStream(fistream1, fistream2);  
				FileOutputStream fostream = new FileOutputStream("Result.txt");        // destination file
			 
				int temp;
				while( ( temp = sistream.read() ) != -1)
				{
				  System.out.print( (char) temp ); // to print at DOS prompt
				  fostream.write(temp);	// to write to file
				}
				fostream.close();
				sistream.close();
				fistream1.close();
				fistream2.close();
			   }
			}

-----------------------------------------------------
## BufferedInputStream and BufferedOutputStream
	
	Code Snippet:
	
		public class InputAndOutputBuffering
		{
		  public static void main(String args[]) throws IOException
		  {						// reading-side
			FileInputStream fistream = new FileInputStream("pqr.txt"); // a low-level stream
			BufferedInputStream bistream = new BufferedInputStream(fistream); // a high-level stream
								// wriring-side
			FileOutputStream fostream = new FileOutputStream("xyz.txt"); // a low-level stream
			BufferedOutputStream bostream = new BufferedOutputStream(fostream);  // a high-level stream
		  
			int temp;
			while( ( temp = bistream.read() ) != -1 )
			{
			  bostream.write(temp);   	
			  System.out.print((char) temp);   	
			}
			bostream.close();  fostream.close();
			bistream.close();   fistream.close();
		  }
		}
-----------------------------------------------------

## System Class 

		Code Snippet:

			public class TimeAndProperties
			{
			  public static void main(String args[])
			  {
													  // time for executing the while loop
				long startLoop = System.currentTimeMillis();          
				int x = 1;
				while(x <= 10000000)
				{ x++; }
				long endLoop = System.currentTimeMillis();      
				System.out.println("Starting time of loop in milliseconds: " + startLoop);
				System.out.println("Ending time of loop in milliseconds: " + endLoop);
				System.out.println("while loop took " + (endLoop - startLoop) + " milliseconds for " + x + " iterations");

											 // to get the OS properties
				System.out.println("\n\t\tSystem properties are:\n");  
				Properties props = System.getProperties();
				Enumeration e1 = props.propertyNames();
				while(e1.hasMoreElements())
				{
				  String k1 = (String) e1.nextElement();
				  String v1 = props.getProperty(k1);
				  System.out.println(k1 + " : " + v1);
				}
						  // to get environment variables
				System.out.println("\n\t\tFollowing are system environment variables:\n");  
				Map map1 = System.getenv();            
				Set set1 = map1.keySet();
				Iterator it = set1.iterator();
				while(it.hasNext())
				{
				  String key = (String) it.next();
				  String value = (String) map1.get(key);
				  System.out.println(key + " : " + value);
				}
			  }
			}
-----------------------------------------------------

## BufferedReader and BufferedWriter File Copy
	
		Code Snippet:
			
			public class SpeedCopying
			{
			  public static void main(String args[]) throws IOException
			  {	                                       // reading-side
				FileReader fr = new FileReader("Morals.txt");     
				BufferedReader br = new BufferedReader(fr);   
														   // writing-side
				FileWriter fw = new FileWriter("xxx.txt");      
				BufferedWriter bw = new BufferedWriter(fw);   

				String s1;
				while((s1 = br.readLine() ) != null )
				{
				  bw.write(s1); 
				  bw.newLine();  
				  System.out.println(s1); 
				}

				bw.close(); fw.close();
				br.close();  fr.close();
			  }
			}

-----------------------------------------------------

## 	What is PrintStream and PrintWriter?

-	Functionally both are same but belong to two different categories 
	– Byte streams and Character streams. 
	-	println() method exists in both classes
	
-	PrintStream

	-	The PrintStream is designed to print data at DOS prompt with its print() and println() methods
	-	This stream comes with the ability of automatic flushing of data.
	-	PrintStream methods are deprecated in favor of PrintWriter, but not System.out. 
	-	There exists a similar class on character streams side, PrintWriter with println() method
	-	The println() and print() methods are overloaded many times in the PrintStream class
	-	to accept different data types and objects as parameters
	-	println() method is overloaded 10 times and print() method is overloaded 9 times
	
		
			public class PSDemo
			{
			  public static void main(String args[]) throws IOException
			  {
				PrintStream pstream = new PrintStream(System.out);
				pstream.println(100);	
				pstream.println("World");
				pstream.print(false);  
				pstream.println(new Double(10.5));
					
				System.out.println("Java is simple but ocean");
				System.err.println("Practice it carefully");   
						
				pstream.close();
			  }
			}
	
-	PrintWriter
	
	-	PrintWriter class comes with methods to format the data
	-	And write to the output stream (not only DOS prompt) as text
	- 	This class also contains the methods of PrintStream like print() and println()
	-	This class cannot read raw bytes as in images and for this PrintStream is only preferable
	-	The data is automatically flushed with print(), println() and printf() (introduced with JDK 1.5) methods
		
		
		public class PWDemo
		{
		  public static void main(String args[]) throws IOException
		  {
			PrintWriter pwriter = new PrintWriter(System.out);
				   
			pwriter.println(100);	
			pwriter.println("Hai");
			pwriter.println(new Double(10.5));
			pwriter.print(false);

			pwriter.close();
		  }
		}
-----------------------------------------------------
		
## What are piped streams?

-	There are four piped streams 
	– 	PipedInputStream
	-	PipedOutputStream
	-	PipedReader
	-	PipedWriter

-	These streams are very useful to pass data between two running threads (say, processes)
-----------------------------------------------------

##  What is difference between Scanner and BufferedReader?
-	Scanner is used for parsing tokens from the contents of the stream
-	Scanner uses regular expression to split the contents 
-	has nextLine(), nextInt() methods ... other primitive type methods
-	BufferedReader doesn't do any special processsing
-	Usually we pass a BufferedReader to a scanner as the source of characters to parse
-----------------------------------------------------
##  How to process java.io.InputStream object and produce a String?

	Code Snippet:
	
		static String convertStreamToString(java.io.InputStream iStream)
		 {
			java.util.Scanner scanInput = new java.util.Scanner(iStream).useDelimiter("\\A");
			return scanInput.hasNext() ? scanInput.next() : "";
		 }

-----------------------------------------------------		 
## How to create a Java String from the contents of a file??

	Code Snippet:
	
		static String readFile(String path, Charset encoding)  throws IOException 
		{
		  byte[] encoded = Files.readAllBytes(Paths.get(path));
		  return new String(encoded, encoding);
		}
-----------------------------------------------------		
## What are the uses of FileInputStream and FileOutputStream in java?

-	java.io.FileInputStream and java.io.FileOutputStream were introduced in JDK 1.0
-	These APIs are used to read and write stream input and output
-	They can also be used to read and write images
-----------------------------------------------------

## Tell something about BufferedWriter ? What are flush() and close() used for ?

-	A Buffer is a temporary storage area for data
-	Its used to increase the speed of the I/O Operations
-	It is an abstract class that creates a buffered character-output stream
-	Flush() is used to clear all the data characters stored in the buffer and clear the buffer
-	Close() is used to closes the character output stream
-----------------------------------------------------
## What is File class?

-	It is a non-stream (not used for file operations) 
-	File class used to know the properties of a file 
	-	like when it was created (or modified)
	-	has read and write permissions, size etc
-----------------------------------------------------	
	
## What is RandomAccessClass?

-	It is a special class from java.io package 
-	Which is neither a input stream nor a output stream (because it can do both)
-	It is directly a subclass of Object class
-	Generally, a stream does only one purpose of either reading or writing;
-	But RandomAccessFile can do both reading from a file and writing to a file
-	All the methods of DataInputStream and DataOutStream exist in RandomAccessFile
-----------------------------------------------------	
	
## What is Scanner class used for ? when was it introduced in Java ?

-	Scanner class introduced in Java 1.5 
-	For reading Data Stream from the input device, File, Path, String

-----------------------------------------------------

## What is a stream and what are the types of Streams and classes of the Streams?

-	A Stream is an abstraction that either produces or consumes information
-	There are two types of Streams :
	-	Byte Streams: Provide a convenient means for handling input and output of bytes
	-	Character  Streams:  Provide a convenient means for handling input & output of characters
-----------------------------------------------------

## What is the difference between the Reader/Writer class hierarchy and the InputStream/OutputStream class hierarchy?

-	The Reader/Writer class hierarchy is character-oriented
-	And the InputStream/OutputStream class hierarchy is byte-oriented	
-----------------------------------------------------

## Which is the abstract parent class of FileWriter ?

-	OutputStreamWriter
-----------------------------------------------------
## Which class is used to read streams of raw bytes from a file?

-	FileInputStream

-----------------------------------------------------

## Which is the Parent class of FileInputStream ?

-	InputStream
-----------------------------------------------------

## Which exceptions should be handled with the following code ?

	Code Snippet:	
		FileOutputStream fileOutputStream = new FileOutputStream(new File("newFile.txt"));

	Ans. FileNotFoundException
-----------------------------------------------------
## Will this code compile fine ?
ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(new File("newFile.txt")));

-	Yes

-----------------------------------------------------
## Which is the Parent Class of  ByteArrayInputStream and AudioInputStream ?

-	InputStream
-----------------------------------------------------
## Which interfaces are implemented by  InputStream?

Ans.[Closeable,  AutoCloseable]
-----------------------------------------------------
## What is the package name for  ObjectInputStream class?

java.io
-----------------------------------------------------
## Which is the Parent Class of  StringBufferInputStream class?

Ans.InputStream
-----------------------------------------------------
## Which is the Parent Class of  SequenceInputStream class?

Ans.InputStream
-----------------------------------------------------
## Which is the Parent Class of  FilterInputStream class?

Ans.InputStream
-----------------------------------------------------
## What is the package name for  FilterInputStream class?

Ans.java.io
-----------------------------------------------------
## Which interfaces are implemented by  FileInputStream?

Ans.[Closeable,  AutoCloseable]
-----------------------------------------------------
## Which is the Parent Class of InputStream class?

Ans.Object
-----------------------------------------------------
## What is the difference between System.out ,System.err and System.in?

-	System.out and System.err both represent the monitor by default 
-	And hence can be used to send data or results to the monitor
-	But System.out is used to display normal messages 
-	And results whereas System.err is used to display error messages
-	System.in represents InputStream object, which by default represents standard input device, i.e., keyboard
-----------------------------------------------------
## What is the purpose of the System class?

-	The purpose of the System class is to provide access to system resources
-----------------------------------------------------
## What will the following code print when executed on Windows ?

	Code Snippet:
		
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

	Output:

		\file.txt
		C:\file.txt
		C:\file.txt
----------------------------------------------
## What will be the output of following code ?

public static void main(String[] args){
  String name = null;
  File file = new File("/folder", name);
  System.out.print(file.exists());
 }

Ans. NullPointerException at line: 

"File file = new File("/folder", name);"
----------------------------------------------
## What will be the output of following code ?
	
	Code Snippet:
				
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
	 
-	Ans. It will create the file myfile.txt in the current directory.
---------------------------------------------- 
## What will be the output of following code ?
	
	Code Snippet:
		public static void main(String[] args){
		  String child = null;
		  File file = new File("/folder", child);
		  try {
		   file.createNewFile();
		  } catch (IOException e) {
		   e.printStackTrace();
		  }
		 }
-	Ans. NullPointerException at line:  File file = new File("/folder", child);
----------------------------------------------
## What will be the output of following code, assuming that currently we are in c:\Project ? 
	
	Code Snippet:
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

	Ans: 

		..\file.txt
		C:\Workspace\Project\..\file.txt
		C:\Workspace\file.txt

----------------------------------------------
## Which is the abstract parent class of FileWriter ?

Ans. OutputStreamWriter
----------------------------------------------
## Which class is used to read streams of characters from a file?

Ans. FileReader
----------------------------------------------
## Which class is used to read streams of raw bytes from a file?

Ans. FileInputStream
----------------------------------------------
## Which is the Parent class of FileInputStream ?

Ans. InputStream
----------------------------------------------
## Which of the following code is correct ?
	
	Code Snippet:
	
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

	Ans:

		c
		File file = new File("../file.txt");
		FileWriter fileWriter = new FileWriter(file);
		BufferedWriter bufferedOutputWriter = new BufferedWriter(fileWriter);
----------------------------------------------
## Which exception should be handled in the following code ?

File file = new File("../file.txt");
FileWriter fileWriter = new FileWriter(file);

Ans. IOException
----------------------------------------------
## Which exceptions should be handled with the following code ?
	
	Code Snippet:
	
		FileOutputStream fileOutputStream = new FileOutputStream(new File("newFile.txt"));

Ans. FileNotFoundException
----------------------------------------------
## Will this code compile fine ?
	Code Snippet:
	
		ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(new File("newFile.txt")));

Ans. Yes.
----------------------------------------------
## What is the problem with this code ?

		Code Snippet:
		
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

-	Ans:
-	Though we are trying to serialize BuggyBread1 object 
-	but we haven't declared the class to implement Serializable
-	This will throw java.io.NotSerializableException upon exception
----------------------------------------------
## Will this code run fine if BuggyBread2 doesn't implement Serializable interface ?

	Code Snippet:
	
		class BuggyBread1 implements Serializable{
		 
		 private BuggyBread2 buggybread2 = new BuggyBread2();
		 
		 public static void main(String[] args){
		  try {
		   BuggyBread1 buggybread1 = new BuggyBread1();
			  ObjectOutputStream objectOutputStream =
			  new ObjectOutputStream(new FileOutputStream(new File("newFile.txt")));
			  objectOutputStream.writeObject(buggybread1);
		  } catch (Exception e) {
		   // TODO Auto-generated catch block
		   e.printStackTrace();
		  }
		 }
		}

-	Ans. No, It will throw java.io.NotSerializableException.
----------------------------------------------
## Will this code work fine if BuggyBread2 doesn't implement Serializable ?
	
	Code Snippet:
	
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

Ans. Yes
----------------------------------------------

