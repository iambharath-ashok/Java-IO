# Reading and Writing Files Using Streams and Readers


------------------------------------------------------
## Java Read Text File

-	There are four ways to read a Text File:

	-	Read text file using Files class
	-	Read text file in java using FileReader
	-	Read text file using BufferedReader
	-	Using scanner to read text file in java	
	
### Java read text file using java.nio.file.Files


	Code Snippet:
	
		String fileName = "/Users/Iambharath/source.txt";
		Path path = Paths.get(fileName);
		byte[] bytes = Files.readAllBytes(path);
		List<String> allLines = Files.readAllLines(path, StandardCharsets.UTF_8);
		Stream<String> lines = Files.lines(Paths.get(), StandardCharsets.UTF_8);
		
		
### Java read text file using FileReader

	
	Code Snippet:
	
		String fileName = "/Users/Iambharath/source.txt";
		File file = new File(fileName);
		FileReader fr = new FileReader(file);
		BufferedReader br = new BufferedReader(fr);
		String line;
		while((line = br.readLine()) != null){
			//process the line
			System.out.println(line);
		}
		br.close();
		
### Java Read text file using java.io.BufferedReader	

	Code Snippet:
	
		String fileName = "/Users/Iambharath/source.txt";
		File file = new File(fileName);
		FileInputStream fis = new FileInputStream(file);
		InputStreamReader isr = new InputStreamReader(fis, cs);
		BufferedReader br = new BufferedReader(isr);

		String line;
		while((line = br.readLine()) != null){
			 //process the line
			 System.out.println(line);
		}
		br.close();
	
	
### Using scanner to read text file in java

-	If we want to read file, line by line or based on some java regular expression
	-	Scanner is the class to use
-	Scanner breaks its input into tokens using a delimiter pattern, which by default matches whitespace
-	The resulting tokens may then be converted into values of different types using the various next methods
-	Scanner is not synchronized and hence not thread safe

	Code Snippet:
		
		Path path = Paths.get(fileName);
		Scanner scanner = new Scanner(path);
		System.out.println("Read text file using Scanner");
		//read line by line
		while(scanner.hasNextLine()){
			//process each line
			String line = scanner.nextLine();
			System.out.println(line);
		}
		scanner.close();
		
		
###	Reading using Random Access File

RandomAccessFile file = new RandomAccessFile("/Users/Iambharath/Downloads/myfile.txt", "r");
String str;

while ((str = file.readLine()) != null) {
	System.out.println(str);
}
file.close();	
			
--------------------------------------------------
## Read CSV File in Java using Scanner

-	We can use Java Scanner class to read CSV file and convert to collection of java bean.
-	For example, we might have a CSV file like below.
	
	Employee.csv:
		
		1,Iambharath Kumar,Developer,5000 USD
		2,Mani,Programmer,4000 USD
		3,Avinash,Developer,5000 USD
		4,David,QA Lead,4000 USD
		
	
	Code Snippet of Reading a CSV file and Parsing it to Java Object:
	
		public class CsvToPersonUsingScanner {

			public static void createPersonFromCsv(File file) throws IOException {

				if (file != null && file.exists()) {

					BufferedReader br = new BufferedReader(new FileReader(file));
					String line = null;
					List<Person> persons = new ArrayList<Person>();
					while ((line = br.readLine()) != null) {
						Scanner scan = new Scanner(line);
						scan.useDelimiter(",");
						Person p = new Person();
						int index = 0;
						while (scan.hasNext()) {
							switch (index) {
							case 0:
								p.setId(scan.nextInt());
								break;
							case 1:
								p.setName(scan.next());
								break;
							case 2:
								p.setRole(scan.next());
								break;
							case 3:
								p.setSalary(scan.next());
								break;
							}
							index++;
						}
						persons.add(p);
					}
					persons.stream().forEach(System.out::println);
				}

			}

			public static void main(String[] args) throws IOException {
				createPersonFromCsv(new File("data/input/emp.txt"));
			}

		}

		class Person {
			private int id;
			private String name;
			private String role;
			private String salary;

			public int getId() {
				return id;
			}

			public void setId(int id) {
				this.id = id;
			}

			public String getName() {
				return name;
			}

			public void setName(String name) {
				this.name = name;
			}

			public String getRole() {
				return role;
			}

			public void setRole(String role) {
				this.role = role;
			}

			public String getSalary() {
				return salary;
			}

			public void setSalary(String salary) {
				this.salary = salary;
			}

			@Override
			public String toString() {
				return "Person [id=" + id + ", name=" + name + ", role=" + role + ", salary=" + salary + "]";
			}

		}
	Output:
	
		Person [id=1, name=Iambharath, role=Developer, salary=5000 USD]
		Person [id=2, name=Mani, role=Programmer, salary=4000 USD]
		Person [id=3, name=Avinash, role=Developer, salary=5000 USD]
		Person [id=4, name=David, role=QA Lead, salary=4000 USD]

	Code Snippet of Using Scanner as Delimiter:
	
		public static void main(String[] args) {
		
			try {
				Scanner scan = new Scanner(Paths.get("file1.txt"));
				//scan.useDelimiter(Pattern.compile("\\s\\r"));
				//scan.useDelimiter(Pattern.compile("\\s?\\r"));
				//scan.useDelimiter(Pattern.compile("\\n"));
				scan.useDelimiter(",");
				while(scan.hasNextLine()) {
					System.out.println(scan.next());
				}
			} catch (IOException e) {
				e.printStackTrace();
			}

		}
		
	Output of scan.useDelimiter(",") :

		1
		Iambharath
		Developer
		5000 USD
		2
		Mani
		Programmer
		4000 USD
		3
		Avinash
		Developer
		5000 USD
		4
		David
		QA Lead
		4000 USD
		
	scan.useDelimiter(Pattern.compile("\\s\\r")):
	
		public static void createPersonFromCsv(File file) throws IOException {

			BufferedReader br = new BufferedReader(new FileReader(file));
			char[] buffer = new char[(int)file.length()];
			br.read(buffer);
			Scanner scan  = new Scanner(new String(buffer));
			scan.useDelimiter("\\s\\r");
			while(scan.hasNext()) {
				String next = scan.next();
				System.out.println(next);
			}


		}
		
	Output:
		1,Iambharath,Developer,5000 USD

		2,Mani,Programmer,4000 USD

		3,Avinash,Developer,5000 USD

		4,David,QA Lead,4000 USD
		
	
---------------------------------------
## Java read file to String

-	There are many ways to read file to String in java

	-	Java read file to String using BufferedReader
	-   Read file to String in java using FileInputStream
	-   Java read file to string using Files class
	-   Read file to String using Scanner class
	-   Java read file to string using Apache Commons IO FileUtils class
	


### Java read file to String using BufferedReader


	Code Snippet:
	
		BufferedReader reader = new BufferedReader(new FileReader(fileName));
		StringBuilder stringBuilder = new StringBuilder();
		String line = null;
		String ls = System.getProperty("line.separator");
		while ((line = reader.readLine()) != null) {
			stringBuilder.append(line);
			stringBuilder.append(ls);
		}
		// delete the last new line separator
		stringBuilder.deleteCharAt(stringBuilder.length() - 1);
		reader.close();

		String content = stringBuilder.toString();

		
	
	Code Snippet of file to String using buffer:
	
		BufferedReader reader = new BufferedReader(new FileReader(fileName));
		StringBuilder stringBuilder = new StringBuilder();
		char[] buffer = new char[10];
		while (reader.read(buffer) != -1) {
			stringBuilder.append(new String(buffer));
			buffer = new char[10];
		}
		reader.close();

		String content = stringBuilder.toString();
		
### Read file to String in java using FileInputStream


	Code Snippet:
	
		FileInputStream fis = new FileInputStream(fileName);
		byte[] buffer = new byte[10];
		StringBuilder sb = new StringBuilder();
		while (fis.read(buffer) != -1) {
			sb.append(new String(buffer));
			buffer = new byte[10];
		}
		fis.close();

		String content = sb.toString();


### Java read file to string using Files class

	Code Snippet:

		String content = new String(Files.readAllBytes(Paths.get(fileName)));
	
### Read file to String using Scanner class	
	
	Code Snippet:

		Scanner scanner = new Scanner(Paths.get(fileName), StandardCharsets.UTF_8.name());
		String content = scanner.useDelimiter("\\A").next();
		scanner.close();
	
### Java read file to string using Apache Commons IO FileUtils class

	
	Code Snippet:
	
		String content = FileUtils.readFileToString(new File(fileName), StandardCharsets.UTF_8);
	
----------------------------------------------------------

## Java read file line by line


### Java Read File line by line using BufferedReader

	Code Snippet:

		public static void main(String[] args) {
		BufferedReader reader;
		try {
			reader = new BufferedReader(new FileReader(	"/Users/Iambharath/Downloads/myfile.txt"));
			String line = reader.readLine();
			while (line != null) {
				System.out.println(line);
				// read next line
				line = reader.readLine();
			}
			reader.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	
### Java Read File line by line using Scanner

		
	Code Snippet:
	
		public static void main(String[] args) {
			try {
				Scanner scanner = new Scanner(new File("/Users/Iambharath/Downloads/myfile.txt"));
				while (scanner.hasNextLine()) {
					System.out.println(scanner.nextLine());
				}
				scanner.close();
			} catch (FileNotFoundException e) {
				e.printStackTrace();
			}
		}
	
### Java Read File line by line using Files

	
	Code Snippet:
	
		Public static void main(String[] args) {
			try {
				List<String> allLines = Files.readAllLines(Paths.get("/Users/Iambharath/Downloads/myfile.txt"));
				for (String line : allLines) {
					System.out.println(line);
				}
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	
### Java Read File line by line using RandomAccessFile

	Code Snippet:

		
		RandomAccessFile file = new RandomAccessFile("/filepath");
		
		String line;
		while((line = file.readLine())!=null) {
			sop(line);
		}
--------------------------------------------

## Java Write to File – 4 Ways to Write File in Java

-	FileWriter
-	BufferedWriter
-	Files
-	FileOutputStream


-  FileWriter: 
	
	-	FileWriter is the simplest way to write a file in java
	-	It provides overloaded write method to write int, byte array and String to the File
	-	We can also write part of the String or byte array using FileWriter
	-	FileWriter writes directly into Files
	-	FileWriter should be used only when number of writes are less
	
	Code Snippet:
		
		private static void writeUsingFileWriter(String data) {
			File file = new File("/Users/Iambharath/FileWriter.txt");
			FileWriter fr = null;
			try {
				fr = new FileWriter(file);
				fr.write(data);
			} catch (IOException e) {
				e.printStackTrace();
			}finally{
				//close resources
				try {
					fr.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
	
	
-	BufferedWriter: 
	
	-	BufferedWriter is almost similar to FileWriter but it uses internal buffer to write data into File
	-   If the number of write operations are more:
		-	The actual IO operations are less and performance is better
		
	-  BufferedWriter should be used when number of write operations are more
	
	Code snippet:
	
		/**
		 * Use BufferedWriter when number of write operations are more
		 * It uses internal buffer to reduce real IO operations and saves time
		 * @param data
		 * @param noOfLines
		 **/
		private static void writeUsingBufferedWriter(String data, int noOfLines) {
			File file = new File("/Users/Iambharath/BufferedWriter.txt");
			FileWriter fr = null;
			BufferedWriter br = null;
			String dataWithNewLine=data+System.getProperty("line.separator");
			try{
				fr = new FileWriter(file);
				br = new BufferedWriter(fr);
				
				br.write(dataWithNewLine);
	
			} catch (IOException e) {
				e.printStackTrace();
			}finally{
				try {
					br.close();
					fr.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}

	
-	FileOutputStream: 
	
	-	FileWriter and BufferedWriter are meant to write text to the file 
	-	But when we need raw stream data(binary data) to be written into file, we should use FileOutputStream to write file
	
	
	Code snippet:
		/**
		 * Use Streams when we are dealing with raw data
		 * @param data
		 **/
		private static void writeUsingOutputStream(String data) {
			OutputStream os = null;
			try {
				os = new FileOutputStream(new File("/Users/Iambharath/os.txt"));
				os.write(data.getBytes(), 0, data.length());
			} catch (IOException e) {
				e.printStackTrace();
			}finally{
				try {
					os.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}

-	Files: 
	
	-	Java 7 introduced Files utility class and we can write a file using it’s write function
	-	Internally it uses OutputStream to write byte array into file
	
	Code snippet:
	
		/**
		 * Use Files class from Java 1.7 to write files, internally uses OutputStream
		 * @param data
		 **/
		private static void writeUsingFiles(String data) {
			try {
				Files.write(Paths.get("/Users/Iambharath/files.txt"), data.getBytes());
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	
------------------------------------------------------------	
## Java append to file

-	If we are working on text data and number of write operations are less
	-	Use FileWriter and use it’s constructor with append flag value as true
-	If the number of write operations are huge, we should use the BufferedWriter
-	To append binary or raw stream data to an existing file, we should use FileOutputStream

### Java append to file using FileWriter

	Code Snippet:
		
		File file = new File("append.txt");
		FileWriter fr = new FileWriter(file, true); <==================
		fr.write("data");
		fr.close();

	
### Java append content to existing file using BufferedWriter

	Code Snippet:
	
		File file = new File("append.txt");
		FileWriter fr = new FileWriter(file, true);  <==================
		BufferedWriter br = new BufferedWriter(fr);
		br.write("data");

		br.close();
		fr.close();	
	
### Append text to file in java using PrintWriter	
	
	
	Code Snippet:
	
		File file = new File("append.txt");
		FileWriter fr = new FileWriter(file, true); <==================
		BufferedWriter br = new BufferedWriter(fr);
		PrintWriter pr = new PrintWriter(br);
		pr.println("data");
		pr.close();
		br.close();
		fr.close();
	
### Append to file in java using FileOutputStream

	Code Snippet:

		OutputStream os = new FileOutputStream(new File("append.txt"), true); <==================
		os.write("data".getBytes(), 0, "data".length());
		os.close();
	
--------------------------------------------------------

## Java InputStream to File Example

-	Files can be read using Stream or Reader
-	Reader is good for Textual Data
-	Stream is good for Binary Data


	Code Snippet:
	
		public class InputStreamToFile {
		
			public static void main(String[] args) {
				try {
					InputStream is = new FileInputStream("/Users/bharath/source.txt");
					
					OutputStream os = new FileOutputStream("/Users/bharath/new_source.txt");
					
					byte[] buffer = new byte[1024];
					int bytesRead;
					//read from is to buffer
					while((bytesRead = is.read(buffer)) !=-1){
						os.write(buffer, 0, bytesRead);
					}
					is.close();
					//flush OutputStream to write any buffered data to file
					os.flush();
					os.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
	
--------------------------------------------------------
## Java RandomAccessFile Example

-	Java RandomAccessFile provides facility to both read and write data to a file
-	RandomAccessFile treats file as byte array 
-	RandomAccessFile works with:
	-	File as large array of bytes stored in the file system
	-   And a cursor using which we can move the file pointer position
	
-	RandomAccessFile class is part of Java IO
-	While creating the instance of RandomAccessFile in java: 
	-	We need to provide the mode to open the file
	-	For example, to open the file for read only mode we have to use “r” 
	-   And for read-write operations we have to use “rw”
	
-	Using file pointer, we can read or write data from random access file at any position
-	To get the current file pointer, we need to call getFilePointer() method	
-	and to set the file pointer index, we can call seek(int i) method.

-	If we write data at any index where data is already present, it will override existing data	
-	RandomAccessFile treats file as byte array


### Java RandomAccessFile read example

-	We can read byte array from a file using RandomAccessFile in java

	Code Snippet:
	
		RandomAccessFile raf = new RandomAccessFile("file.txt", "r");
		raf.seek(1);
		byte[] bytes = new byte[5];
		raf.read(bytes);
		raf.close();
		System.out.println(new String(bytes));


-	In first line, we are creating RandomAccessFile instance for file in read-only mode
-	Then in second line, we are moving the file pointer to index 1
-	We have created byte array of length 5, so when we are calling read(bytes) method then 5 bytes are read from file to the byte array
-	Finally we are closing the RandomAccessFile resource and printing the byte array to console

	
### Java RandomAccessFile write example

	Code Snippet:
	
		RandomAccessFile raf = new RandomAccessFile("file.txt", "rw");
		raf.seek(5);
		raf.write("Data".getBytes());
		raf.close();
	
-	Since RandomAccessFile treats file as byte array, 
	-	Write operation can override the data as well as it can append to a file
	
-	It all depends on the file pointer position
-	If the pointer is moved beyond the file length and then a write operation is called
	-	Then there will be junk data written in the file
	-	So we should take care of this while using write operation	
	
	
### RandomAccessFile append example

-	While appending data we need to make sure file pointer is at the end of file


	Code snippet:
		
		RandomAccessFile raf = new RandomAccessFile("file.txt", "rw");
		raf.seek(raf.length());
		raf.write("Data".getBytes());
		raf.close();
			
--------------------------------------------------------	
	
### Java Download File from URL
	
-	 To download file from URL in java, wee can use java.net.URL openStream() method
-	 We can use Java NIO Channels or Java IO InputStream to read data from the URL openStream() method and then save it to file
	
	Code Snippet of downloading file using Stream:
	
		private static void downloadUsingStream(String urlStr, String file) throws IOException{
			URL url = new URL(urlStr);
			BufferedInputStream bis = new BufferedInputStream(url.openStream());
			FileOutputStream fis = new FileOutputStream(file);
			byte[] buffer = new byte[1024];
			int count=0;
			while((count = bis.read(buffer,0,1024)) != -1)
			{
				fis.write(buffer, 0, count);
			}
			fis.close();
			bis.close();
		}
		
-	downloadUsingStream: In this method, we are using URL openStream method to create the input stream
	-	Then we are using file output stream to read data from input stream and write data to the file	
	
	Code Snippet of downloading file using NIO:
	
		private static void downloadUsingNIO(String urlStr, String file) throws IOException {
			URL url = new URL(urlStr);
			ReadableByteChannel rbc = Channels.newChannel(url.openStream());
			FileOutputStream fos = new FileOutputStream(file);
			fos.getChannel().transferFrom(rbc, 0, Long.MAX_VALUE);
			fos.close();
			rbc.close();
		}

-	downloadUsingNIO: In this method, we are creating byte channel from URL stream data
	-	Then use file output stream to write it to file		
	
	
-------------------------------------------------------	
	
## Java Properties File – java.util.Properties	
	
	
-	Java properties file are used to store key-value pair configuration
-	java.util.Properties class is used to work with properties file in java

	Code Snippet of Main Method:
	
		public class PropertyFilesUtil {

			public static void main(String[] args) throws IOException {
				String propertyFileName = "DB.properties";
				String xmlFileName = "DB.xml";
				writePropertyFile(propertyFileName, xmlFileName);
				readPropertyFile(propertyFileName, xmlFileName);
				readAllKeys(propertyFileName, xmlFileName);
				readPropertyFileFromClasspath(propertyFileName);
			}
		}
		
### readPropertyFileFromClasspath

#### prop.load(PropertyFilesUtil.class.getClassLoader().getResourceAsStream(propertyFileName));

-	When we just give the file name, it looks for the FILE IN PROJECT ROOT DIRECTORY, same place where it stores the property files
-	When we try to load property file from classpath, it throws NullPointerException 
-	Because it tries to load file from Classpath that IS SRC DIRECTORY OF THE PROJECT
-	If we copy the properties file in the classes src directory, it’s able to load them and works fine

	Code Snippet:

			/**
			 * read property file from classpath
			 * @param propertyFileName
			 * @throws IOException
			 ***/
			private static void readPropertyFileFromClasspath(String propertyFileName) throws IOException {
				Properties prop = new Properties();
				prop.load(PropertyFilesUtil.class.getClassLoader().getResourceAsStream(propertyFileName));
				System.out.println(propertyFileName +" loaded from Classpath::db.host = "+prop.getProperty("db.host"));
				System.out.println(propertyFileName +" loaded from Classpath::db.user = "+prop.getProperty("db.user"));
				System.out.println(propertyFileName +" loaded from Classpath::db.pwd = "+prop.getProperty("db.pwd"));
				System.out.println(propertyFileName +" loaded from Classpath::XYZ = "+prop.getProperty("XYZ"));
				
			}

### readAllKeys

-	prop.load(reader);
	-	Set<Object> keys= prop.keySet();
	
-	prop.loadFromXML(is);
	-	keys= prop.keySet();

			/**
			 * read all the keys from the given property files
			 * @param propertyFileName
			 * @param xmlFileName
			 * @throws IOException 
			 **/
			private static void readAllKeys(String propertyFileName, String xmlFileName) throws IOException {
				System.out.println("Start of readAllKeys");
				Properties prop = new Properties();
				FileReader reader = new FileReader(propertyFileName);
				prop.load(reader);
				Set<Object> keys= prop.keySet();
				for(Object obj : keys){
					System.out.println(propertyFileName + ":: Key="+obj.toString()+"::value="+prop.getProperty(obj.toString()));
				}
				//loading xml file now, first clear existing properties
				prop.clear();
				InputStream is = new FileInputStream(xmlFileName);
				prop.loadFromXML(is);
				keys= prop.keySet();
				for(Object obj : keys){
					System.out.println(xmlFileName + ":: Key="+obj.toString()+"::value="+prop.getProperty(obj.toString()));
				}
				//Now free all the resources
				is.close();
				reader.close();
				System.out.println("End of readAllKeys");
			}
			
### readPropertyFile

-	prop.load(reader);
-	prop.loadFromXML(is);

			/**
			 * This method reads property files from file system
			 * @param propertyFileName
			 * @param xmlFileName
			 * @throws IOException 
			 * @throws FileNotFoundException 
			 ***/
			private static void readPropertyFile(String propertyFileName, String xmlFileName) throws FileNotFoundException, IOException {
				System.out.println("Start of readPropertyFile");
				Properties prop = new Properties();
				FileReader reader = new FileReader(propertyFileName);
				prop.load(reader);
				System.out.println(propertyFileName +"::db.host = "+prop.getProperty("db.host"));
				System.out.println(propertyFileName +"::db.user = "+prop.getProperty("db.user"));
				System.out.println(propertyFileName +"::db.pwd = "+prop.getProperty("db.pwd"));
				System.out.println(propertyFileName +"::XYZ = "+prop.getProperty("XYZ"));
				//loading xml file now, first clear existing properties
				prop.clear();
				InputStream is = new FileInputStream(xmlFileName);
				prop.loadFromXML(is);
				System.out.println(xmlFileName +"::db.host = "+prop.getProperty("db.host"));
				System.out.println(xmlFileName +"::db.user = "+prop.getProperty("db.user"));
				System.out.println(xmlFileName +"::db.pwd = "+prop.getProperty("db.pwd"));
				System.out.println(xmlFileName +"::XYZ = "+prop.getProperty("XYZ"));
				//Now free all the resources
				is.close();
				reader.close();
				System.out.println("End of readPropertyFile");
			}
			

### writePropertyFile

-	prop.store(new FileWriter(propertyFileName), "DB Config file");
-	prop.storeToXML(new FileOutputStream(xmlFileName), "DB Config XML file");

			
			/**
			 * This method writes Property files into file system in property file
			 * and xml format
			 * @param fileName
			 * @throws IOException
			 **/
			private static void writePropertyFile(String propertyFileName, String xmlFileName) throws IOException {
				System.out.println("Start of writePropertyFile");
				Properties prop = new Properties();
				prop.setProperty("db.host", "localhost");
				prop.setProperty("db.user", "user");
				prop.setProperty("db.pwd", "password");
				prop.store(new FileWriter(propertyFileName), "DB Config file");
				System.out.println(propertyFileName + " written successfully");
				prop.storeToXML(new FileOutputStream(xmlFileName), "DB Config XML file");
				System.out.println(xmlFileName + " written successfully");
				System.out.println("End of writePropertyFile");
			}

		}
	
	
	Output:
	
		#DB Config file
		#Fri Nov 16 11:16:37 PST 2012
		db.user=user
		db.host=localhost
		db.pwd=password
		
		<?xml version="1.0" encoding="UTF-8" standalone="no"?>
		<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
		<properties>
		<comment>DB Config XML file</comment>
		<entry key="db.user">user</entry>
		<entry key="db.host">localhost</entry>
		<entry key="db.pwd">password</entry>
		</properties>

-----------------------------------------		
	
## How to Compile and Run Java Program from another Java Program

-	We can use Runtime.exec(String cmd) to issue commands to the underlying operating system
-	
		
	Command to Pass to Runtime.exec():
		Process pro = Runtime.getRuntime().exec(command);
		"javac -cp src src/com/bharath/files/Test.java"
		"java -cp src com/bharath/files/Test"
		 pro.waitFor();
		 pro.exitValue();
		
			System.out.println(Runtime.getRuntime().freeMemory()/(1024 */* 1024));
			System.out.println(Runtime.getRuntime().totalMemory()/(1024 */* 1024));
			System.out.println(Runtime.getRuntime().maxMemory()/(1024 */* 1024));	
			
			
			
----------------------------------------------			
##	Data Output and Input Stream Example:


- 	Data Output and Input Stream is used to read and write Primitive data type values in a machine independent way
-	DataInputStream is not thread safe classes
-	Data streams are used to read and write primitive types and String values in binary format
-	DataInput and DataOutput are the base interfaces for all data streams 
-	DataInputStreamand DataOutputStreamare the two widely-used implementations


	Code Snippet:
	
		public class DataInputOutputStreamExample {
			public static void main(String[] args) {
				File file = new File("file.txt");
				
				/*Write primitive data type to file*/
				FileOutputStream fileOutputStream = null;
				DataOutputStream dataOutputStream = null;
				try {
					fileOutputStream=new FileOutputStream(file);
					dataOutputStream=new DataOutputStream(fileOutputStream);
					dataOutputStream.writeInt(50);
					dataOutputStream.writeDouble(400.25);
					dataOutputStream.writeChar('A');
					dataOutputStream.flush();
				} catch (IOException e) {
					e.printStackTrace();
				}finally {
					try {
						if(fileOutputStream!=null){
							fileOutputStream.close();
						}
						if(dataOutputStream!=null){
							dataOutputStream.close();
						}
					} catch (Exception e) {
						e.printStackTrace();
					}
				}
				
				
				/*Read primitive data type from file*/
				FileInputStream fileInputStream = null;
				DataInputStream dataInputStream = null;
				try {
					fileInputStream = new FileInputStream(file);
					dataInputStream = new DataInputStream(fileInputStream);
					System.out.println(dataInputStream.readInt());
					System.out.println(dataInputStream.readDouble());
					System.out.println(dataInputStream.readChar());
				} catch (IOException e) {
					e.printStackTrace();
				}finally {
					try {
						if(fileInputStream!=null){
							fileInputStream.close();
						}
						if(dataInputStream!=null){
							dataInputStream.close();
						}
					} catch (Exception e) {
						e.printStackTrace();
					}
				}
			}
		}



	Output:
	
		50
		400.25
		A
---------------------------------------------------------------------------
## How to Compare two text files 

	Code Snippet:
	
		public static void main(final String[] args) throws IOException {
			final Path firstFile = Paths.get("/home/src/main/resources/a.txt");
			final Path secondFile = Paths.get("/home/src/main/resources/b.txt");
			final List<String> firstFileContent = Files.readAllLines(firstFile,
			    Charset.defaultCharset());
			final List<String> secondFileContent = Files.readAllLines(secondFile,
			    Charset.defaultCharset());

			System.out.println(diffFiles(firstFileContent, secondFileContent));
			System.out.println(diffFiles(secondFileContent, firstFileContent));
		    }

		    private static List<String> diffFiles(final List<String> firstFileContent,
			final List<String> secondFileContent) {
			final List<String> diff = new ArrayList<String>();
			for (final String line : firstFileContent) {
			    if (!secondFileContent.contains(line)) {
				diff.add(line);
			    }
			}
			return diff;
		    }

---------------------------------------------------------------------------
How To Read And Write Images In Java?

-	To read and write image files either from local disk or from URL, javax.imageio.ImageIO class is used
-	javax.imageio.ImageIO is a final class with some static methods which are very useful for image processing	

	Code Snippet:
	
		main {
			File file = new File("I:/input.jpg");
			BufferedImage image = null;
			try
			{
				image = ImageIO.read(file);
				ImageIO.write(image, "jpg", new File("I:/output.jpg"));
				ImageIO.write(image, "png", new File("I:/output.png"));
				ImageIO.write(image, "gif", new File("I:/output.gif"));
				ImageIO.write(image, "bmp", new File("I:/output.bmp"));
			} 
			catch (IOException e) 
			{
				e.printStackTrace();
			}
		}
		
		
		main {
			URL url = null;
			BufferedImage image = null;
			try
			{
				url = new URL("https://...................../NumberPatternPrograms.png");
			} 
			catch (MalformedURLException e1) 
			{
				e1.printStackTrace();
			}
			try
			{
				image = ImageIO.read(url);
				ImageIO.write(image, "jpg", new File("I:/output.jpg"));
				ImageIO.write(image, "png", new File("I:/output.png"));
				ImageIO.write(image, "gif", new File("I:/output.gif"));
				ImageIO.write(image, "bmp", new File("I:/output.bmp"));
			} 
			catch (IOException e) 
			{
				e.printStackTrace();
			}
			 
			System.out.println("done");
		}
		
------------------------------------------------------------------------------
## How to sort the text file?

	Code Snippet:
	
		public static void main(String[] args)
		{   
			BufferedReader reader = null; 
			BufferedWriter writer = null;
			//Create an ArrayList object to hold the lines of input file
			ArrayList<String> lines = new ArrayList<String>();
			try
			{
				//Creating BufferedReader object to read the input file
				reader = new BufferedReader(new FileReader("C:\\input.txt"));
				//Reading all the lines of input file one by one and adding them into ArrayList
				String currentLine = reader.readLine();
				while (currentLine != null) 
				{
					lines.add(currentLine);
					currentLine = reader.readLine();
				}
				//Sorting the ArrayList
				Collections.sort(lines);
				//Creating BufferedWriter object to write into output file
				writer = new BufferedWriter(new FileWriter("C:\\output.txt"));
				//Writing sorted lines into output file
				for (String line : lines)
				{
					writer.write(line);
					writer.newLine();
				}
			} 
		
		public static void main(String[] args)throws IOException
		{
			//Creating BufferedReader object to read the input text file
			BufferedReader reader = new BufferedReader(new FileReader("C:\\input.txt"));
			//Creating ArrayList to hold Student objects
			ArrayList<Student> studentRecords = new ArrayList<Student>();
			//Reading Student records one by one
			String currentLine = reader.readLine();
			while (currentLine != null)
			{
				String[] studentDetail = currentLine.split(" ");
				String name = studentDetail[0];
				int marks = Integer.valueOf(studentDetail[1]);
				//Creating Student object for every student record and adding it to ArrayList
				studentRecords.add(new Student(name, marks));
				currentLine = reader.readLine();
			}
			//Sorting ArrayList studentRecords based on marks
			Collections.sort(studentRecords, new marksCompare());
			//Creating BufferedWriter object to write into output text file
			BufferedWriter writer = new BufferedWriter(new FileWriter("C:\\output.txt"));
			//Writing every studentRecords into output text file
			for (Student student : studentRecords) 
			{
				writer.write(student.name);
				writer.write(" "+student.marks);
				writer.newLine();
			}
			//Closing the resources
			reader.close();
			writer.close();
		}
		
		
		public class Sort {

			public static void main(String[] args) throws Exception {
				BufferedReader reader = new BufferedReader(new FileReader("fileToRead"));
				Map<String, String> map=new TreeMap<String, String>();
				String line="";
				while((line=reader.readLine())!=null){
					map.put(getField(line),line);
				}
				reader.close();
				FileWriter writer = new FileWriter("fileToWrite");
				for(String val : map.values()){
					writer.write(val);  
					writer.write('\n');
				}
				writer.close();
			}

			private static String getField(String line) {
				return line.split(" ")[0];//extract value you want to sort on
			}
		}
------------------------------------------------------------------------------























		
			
			
