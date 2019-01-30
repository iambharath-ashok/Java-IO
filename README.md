# Java-IO

##	Java create new file

-	java.io.File class can be used to create a new File in Java
-	We can call createNewFile() method to create new file in Java

-	File createNewFile() method returns true if new file is created and false if file already exists
-	This method also throws java.io.IOException when it’s not able to create the file
-	The files created is empty and of zero bytes

-	When we create the File object by passing file name:	
	
	-	It can be with absolute path
	-	Or we can only provide the file name or we can provide relative path

-	For non-absolute path, File object tries to locate files in the project root directory
-	If we run the program from command line, for non-absolute path, File object tries to locate files from the current directory
-	While creating the file path, we should use System property "file.separator" to make our program platform independent
	
	
	Code Snippet:
	
		 public static void main(String[] args) throws IOException {
			String fileSeparator = System.getProperty("file.separator");
			
			//absolute file name with path
			String absoluteFilePath = fileSeparator+"Users"+fileSeparator+"Iambharath"+fileSeparator+"file.txt";
			File file = new File(absoluteFilePath);
			if(file.createNewFile()){
				System.out.println(absoluteFilePath+" File Created");
			}else System.out.println("File "+absoluteFilePath+" already exists");
			
			//file name only
			file = new File("file.txt");
			if(file.createNewFile()){
				System.out.println("file.txt File Created in Project root directory");
			}else System.out.println("File file.txt already exists in project root directory");
			
			//relative path
			String relativePath = "tmp"+fileSeparator+"file.txt";
			file = new File(relativePath);
			if(file.createNewFile()){
				System.out.println(relativePath+" File Created in Project root directory");
			}else System.out.println("File "+relativePath+" already exists in project root directory");
		}
		
	
	Output:
	
		/Users/Iambharath/file.txt File Created
		file.txt File Created in Project root directory
		Exception in thread "main" 
		java.io.IOException: No such file or directory
			at java.io.UnixFileSystem.createFileExclusively(Native Method)
			at java.io.File.createNewFile(File.java:947)
			at com.bharath.files.CreateNewFile.main(CreateNewFile.java:32)
	
	
	
	
	
	
### FileOutputStream.write(byte[] b)

-	If we want to create a new file and at the same time write some data into it, 
-	We can use FileOutputStream write method	
	
	
	
	Code Snippet:
	
		String filname = "Iambharathfile1";
		System.out.println(Arrays.toString(filname.getBytes()));
		
		FileOutputStream fos = new FileOutputStream("name.txt");
		fos.write(filname.getBytes());
		fos.flush();
		fos.close();
	
	
	Output:
		
		[98, 104, 97, 114, 97, 116, 104, 102, 105, 108, 101, 49, 46, 116, 120, 116]

	
### Java NIO Files.write()	
	
-	We can use Java NIO Files class to create a new file and write some data into it	
	
	
	Code Snippet:
	
		String fileData = "Bahrfath";
		Files.write(Paths.get("name.txt"), fileData.getBytes());
	
------------------------------------------------

## Java Delete File, directory	
	
-	Java File delete() method can be used to delete files or empty directory/folder in java.
-	Java file delete method returns true if file gets deleted and returns false if file doesn’t exist.
-	If we are trying to delete a directory, it checks java File delete() method check if it’s empty or not.
	-	If directory is empty, it gets deleted 
	-	else delete() method doesn’t do anything and return false. 
-	So in this case, we have to recursively delete all the files and then the empty directory.
-	Another way to delete a non-empty directory is by using Files.walkFileTree() method. 
-	In this method, we can process all the files one by one, and call delete method on single files.


	Code snippet:
	
		public class DeleteFileJava {

			/**
			 * This class shows how to delete a File in Java
			 * @param args
			 **/
			public static void main(String[] args) {
				//absolute file name with path
				File file = new File("/Users/Iambharath/file.txt");
				if(file.delete()){
					System.out.println("/Users/Iambharath/file.txt File deleted");
				}else System.out.println("File /Users/Iambharath/file.txt doesn't exists");
				
				//file name only
				file = new File("file.txt");
				if(file.delete()){
					System.out.println("file.txt File deleted from Project root directory");
				}else System.out.println("File file.txt doesn't exists in project root directory");
				
				//relative path
				file = new File("tmp/file.txt");
				if(file.delete()){
					System.out.println("tmp/file.txt File deleted from Project root directory");
				}else System.out.println("File tmp/file.txt doesn't exists in project root directory");
				
				//delete empty directory
				file = new File("tmp");
				if(file.delete()){
					System.out.println("tmp directory deleted from Project root directory");
				}else System.out.println("tmp directory doesn't exists or not empty in project root directory");
				
				//try to delete directory with files
				file = new File("/Users/Iambharath/project");
				if(file.delete()){
					System.out.println("/Users/Iambharath/project directory deleted from Project root directory");
				}else System.out.println("/Users/Iambharath/project directory doesn't exists or not empty");
			}

		}
	
### Java delete directory

-	Deleting Non-Empty directory in the Java 7

	Code Snippet:
	
		public class JavaDeleteDirectory {

			public static void main(String[] args) {
				File dir = new File("/Users/Iambharath/log");
				
				if(dir.isDirectory() == false) {
					System.out.println("Not a directory. Do nothing");
					return;
				}
				File[] listFiles = dir.listFiles();
				for(File file : listFiles){
					System.out.println("Deleting "+file.getName());
					file.delete();
				}
				//now directory is empty, so we can delete it
				System.out.println("Deleting Directory. Success = "+dir.delete());
				
			}

		}
	
	
###	Java delete directory recursively

-	Earlier we had to write recursion based code to delete a directory with nested directories
-	But with Java 7, we can do this using Files class
-	Another way to delete a non-empty directory is by using Files.walkFileTree() method. 
-	In this method, we can process all the files one by one, and call delete method on single files.
	Code Snippet:
	
		public class JavaDeleteDirectoryRecursively {

			public static void main(String[] args) throws IOException {
				
				Path directory = Paths.get("/Users/Iambharath/log");
				Files.walkFileTree(directory, new SimpleFileVisitor<Path>() {
				   @Override
				   public FileVisitResult visitFile(Path file, BasicFileAttributes attributes) throws IOException {
					   Files.delete(file); // this will work because it's always a File
					   return FileVisitResult.CONTINUE;
				   }

				   @Override
				   public FileVisitResult postVisitDirectory(Path dir, IOException exc) throws IOException {
					   Files.delete(dir); //this will work because Files in the directory are already deleted
					   return FileVisitResult.CONTINUE;
				   }
				});
			}
		}
				
---------------------------------------------

## 	Java File separator, separatorChar, pathSeparator, pathSeparatorChar

	
-	java.io.File class contains four static separator variables

	-	File.separator: Platform dependent default name-separator character as String. 
	-	For windows, it’s ‘\’ and for unix it’s ‘/’.
	-	File.separatorChar: Same as separator but it’s char.
	-	File.pathSeparator: Platform dependent variable for path-separator.
	-	For example PATH or CLASSPATH variable list of paths separated by ‘:’ in Unix systems and ‘;’ in Windows system.
	-	File.pathSeparatorChar: Same as pathSeparator but it’s char.	
	
	
	
	Code snippet:
	
		public static void main(String[] args) {
			System.out.println("File.separator = "+File.separator);
			System.out.println("File.separatorChar = "+File.separatorChar);
			System.out.println("File.pathSeparator = "+File.pathSeparator);
			System.out.println("File.pathSeparatorChar = "+File.pathSeparatorChar);
			
		}
	
	
	Output linux:
	
		File.separator = /
		File.separatorChar = /
		File.pathSeparator = :
		File.pathSeparatorChar = :
		
	Output widows:

		File.separator = \
		File.separatorChar = \
		File.pathSeparator = ;
		File.pathSeparatorChar = ;
	
-----------------------------------------------------------------

## How to Delete a Directory/Folder in Java using Recursion

Code Snippet:

		
		public class DeleteFolderRecursively {

			public static void main(String[] args) {
				String folder = "/Users/Iambharath/tmp";
				//delete folder recursively
				recursiveDelete(new File(folder));
			}
			
			public static void recursiveDelete(File file) {
				//to end the recursive loop
				if (!file.exists())
					return;
				
				//if directory, go inside and call recursively
				if (file.isDirectory()) {
					for (File f : file.listFiles()) {
						//call recursively
						recursiveDelete(f);
					}
				}
				//call delete to delete files and empty directory
				file.delete();
				System.out.println("Deleted file/folder: "+file.getAbsolutePath());
			}
		}
-----------------------------------------------------------------
## Java Rename File – Jave Move File
	
	
-	We can use File.renameTo(File dest) method for java rename file and java move file operations.
-	File renameTo method returns true if file rename is successful, else it returns false.
-	Some of the rename operation are platform dependent. 
	-	For example it might fail if we move a file from one filesystem to another or if a file already exists with the same name at destination directory
-	In Mac OS, if destination file already exists renameTo override the existing file with source file
-	We should always check the renameTo return value to make sure rename file is successful 
-	Because it’s platform dependent and it doesn’t throw IO exception if rename fails


	public static void main(String[] args) {
        //absolute path rename file
        File file = new File("/Users/Iambharath/java.txt");
        File newFile = new File("/Users/Iambharath/java1.txt");
        if(file.renameTo(newFile)){
            System.out.println("File rename success");;
        }else{
            System.out.println("File rename failed");
        }
		
		//java move file from one directory to another
        file = new File("/Users/Iambharath/DB.properties");
        newFile = new File("DB_Move.properties");
        if(file.renameTo(newFile)){
            System.out.println("File move success");;
        }else{
            System.out.println("File move failed");
        }
	}
	
	
-----------------------------------------------------------------	
## Java get file size
	
### Java get file size using File class


-	Java File length() method returns the file size in bytes.
-	Will only work for file and not for directory. 
-	So before calling this method to get file size in java
-	Make sure file exists and it’s not a directory.


	Code Snippet:
	
		public class JavaGetFileSize {
			static final String FILE_NAME = "/Users/Iambharath/Downloads/file.pdf";
			public static void main(String[] args) {
				File file = new File(FILE_NAME);
				if (!file.exists() || !file.isFile()) return;

				System.out.println(getFileSizeBytes(file));
				System.out.println(getFileSizeKiloBytes(file));
				System.out.println(getFileSizeMegaBytes(file));
			}

			private static String getFileSizeMegaBytes(File file) {
				return (double) file.length() / (1024 (Mul) 1024) + " mb";
			}
			
			private static String getFileSizeKiloBytes(File file) {
				return (double) file.length() / 1024 + "  kb";
			}

			private static String getFileSizeBytes(File file) {
				return file.length() + " bytes";
			}
		}
		
	Output;
	
		34353544343 bytes
		343.345323 kb
		3.35343 mb
---------------------------------------------		
		
## How to Get File Extension in Java		

	Code Snippet:
		
		private static String getFileExtension(File file) {
			String fileName = file.getName();
			if(fileName.lastIndexOf(".") != -1 && fileName.lastIndexOf(".") != 0)
			return fileName.substring(fileName.lastIndexOf(".")+1);
			else return "";
		}
		
---------------------------------------------		
## How to check if File exists in Java


	Code Snippet:
		
		public class FileExists {

			public static void main(String[] args) {
				File file = new File("/Users/Iambharath/source.txt");
				File notExist = new File("xyz.txt");
				
				try {
					System.out.println(file.getCanonicalPath() + " exists? "+file.exists());
					System.out.println(notExist.getCanonicalPath() + " exists? "+notExist.exists());
				} catch (IOException e) {
					e.printStackTrace();
				}
				
			}

		}
		
	Output:
		
		/Users/Iambharath/source.txt exists? true
		/Users/Iambharath/JavaPrograms/xyz.txt exists? false
---------------------------------------------		
## How to check File is Directory or File in java

-	java.io.File class contains two methods using which we can find out if the file is a directory or a regular file in java.
-	isFile(): This method returns true if file exists and is a regular file
	-	Note that if file doesn’t exist then it returns false.

-	isDirectory(): This method returns true if file is actually a directory
	-	If path doesn’t exist then it returns false.
	
-	While checking if a file is directory or regular file, we should first check if file exists or not. 
-	If it exists then only we should check if it’s a directory or file.
	
	Code Snippet:
	
		public class CheckDirectoryOrFile {

			public static void main(String[] args) {
				File file = new File("/Users/Iambharath/source.txt");
				File dir = new File("/Users/Iambharath");
				File notExists = new File("/Users/Iambharath/notafile");
				
				System.out.println("/Users/Iambharath/source.txt is file?"+file.isFile());
				System.out.println("/Users/Iambharath/source.txt is directory?"+file.isDirectory());
				
				System.out.println("/Users/Iambharath is file?"+dir.isFile());
				System.out.println("/Users/Iambharath is directory?"+dir.isDirectory());
				
				System.out.println("/Users/Iambharath/notafile is file?"+notExists.isFile());
				System.out.println("/Users/Iambharath/notafile is directory?"+notExists.isDirectory());
			}

		}
	
	
	Output:
		
		/Users/Iambharath/source.txt is file?true
		/Users/Iambharath/source.txt is directory?false
		/Users/Iambharath is file?false
		/Users/Iambharath is directory?true
		/Users/Iambharath/notafile is file?false
		/Users/Iambharath/notafile is directory?false
			
---------------------------------------------			
## How to get File last modified date in Java

-	java.io.File class lastModified() returns last modified date in long
-	we can construct date object in human readable format with this time.
-	If file doesn’t exists, lastModified() returns 0L, 


	Code Snippet:
	
		public class FileDate {

			public static void main(String[] args) {
				File file = new File("employee.xml");
				
				long timestamp = file.lastModified();
				System.out.println("employee.xml last modified date = "+new Date(timestamp));
			}

		}
		
	Output:

		employee.xml last modified date = Fri Dec 07 14:19:10 PST 2012
		
-	After deleting employee.xml then the output is:		
	
	
	Output:
		
		employee.xml last modified date = Wed Dec 31 16:00:00 PST 1969
---------------------------------------------	
## Java File Path, Absolute Path and Canonical Path

### Getting file paths in java 

-	getPath()
-	getAbsolutePath()
-	getCanonicalPath()

-	These methods returns String value of file Path based on how its created
-	They just work on the pathname of the file used while creating the File object

	Code Snippet:
	
		public static void main(String[] args) throws IOException, URISyntaxException {
			File file = new File("/Users/Iambharath/test.txt");
			printPaths(file);
			// relative path
			file = new File("test.xsd");
			printPaths(file);
			// complex relative paths
			file = new File("/Users/Iambharath/../Iambharath/test.txt");
			printPaths(file);
			// URI paths
			file = new File(new URI("file:///Users/Iambharath/test.txt"));
			printPaths(file);
		}
		
		private static void printPaths(File file) throws IOException {
			System.out.println("Absolute Path: " + file.getAbsolutePath());
			System.out.println("Canonical Path: " + file.getCanonicalPath());
			System.out.println("Path: " + file.getPath());
		}
	
-	Using canonical path is best suitable to avoid any issues because of relative paths
-	Java file path methods doesn’t check if file exists or not. 
-	They just work on the pathname of the file used while creating the File object
	
---------------------------------------------	
## Java File Permissions

-	Java File class contains methods to check file permissions for application user
-	They also have some methods to set file permissions for the user and everybody else
-	These File set permission methods return false if they are not able to set the file permissions
-	This can happen due to user privilege
	
	-	For example, if I change the owner of my sample file to root
	-	Then all the set file permission method calls return false

	
	Code Snippet:
	
		public class JavaFilePermissions {

			public static void main(String[] args) {
				File file = new File("/Users/Iambharath/run.sh");
				//check file permissions for application user
				System.out.println("File is readable? "+file.canRead());
				System.out.println("File is writable? "+file.canWrite());
				System.out.println("File is executable? "+file.canExecute());
				//change file permissions for application user only
				file.setReadable(false);
				file.setWritable(false);
				file.setExecutable(false);
				//change file permissions for other users
				file.setReadable(true, false);
				file.setWritable(true, false);
				file.setExecutable(true, true);
			}

		}
	
--------------------------------------------------

## How to set File Permissions in Java Easily using Java 7 PosixFilePermission
	
-	Java File class has the ability to set the file permissions but it’s not versatile
-	The biggest drawback is that:
	-	we can divide file permissions into two set of users – owner and everybody else only
- 	We can’t set different file permissions for group and other users

-	Java 7 has introduced PosixFilePermission Enum and java.nio.file.Files 
-	java.nio.file.Files  includes a method setPosixFilePermissions(Path path, Set<PosixFilePermission> perms) that can be used to set file permissions easily


	Code Snippet:
	
		public class FilePermissions {

			/**
			 * File Permissions Java Example using File and PosixFilePermission
			 * @param args
			 * @throws IOException 
			 ***/
			public static void main(String[] args) throws IOException {
				File file = new File("/Users/Iambharath/temp.txt");
				
				//set application user permissions to 455
				file.setExecutable(false);
				file.setReadable(false);
				file.setWritable(true);
				
				//change permission to 777 for all the users
				//no option for group and others
				file.setExecutable(true, false);
				file.setReadable(true, false);
				file.setWritable(true, false);
				
				//using PosixFilePermission to set file permissions 777
				Set<PosixFilePermission> perms = new HashSet<PosixFilePermission>();
				//add owners permission
				perms.add(PosixFilePermission.OWNER_READ);
				perms.add(PosixFilePermission.OWNER_WRITE);
				perms.add(PosixFilePermission.OWNER_EXECUTE);
				//add group permissions
				perms.add(PosixFilePermission.GROUP_READ);
				perms.add(PosixFilePermission.GROUP_WRITE);
				perms.add(PosixFilePermission.GROUP_EXECUTE);
				//add others permissions
				perms.add(PosixFilePermission.OTHERS_READ);
				perms.add(PosixFilePermission.OTHERS_WRITE);
				perms.add(PosixFilePermission.OTHERS_EXECUTE);
				
				Files.setPosixFilePermissions(Paths.get("/Users/Iambharath/run.sh"), perms);
			}
	}
--------------------------------------------------

## Java File  Copy 

- 	No Direct Methods on java.io.File class
-	4 Other ways are 

	-	Using Input Stream and Output Stream
	-	Java 4 NIO FileChannels
	-	Apache Common FileUtils
	-	Java 7 Files.copy
	
	
### Using Input and Output Stream

	Code Snippet:
	
		private static void copyFileUsingStream(File source, File dest) throws IOException {
			InputStream is = null;
			OutputStream os = null;
			try {
				is = new FileInputStream(source);
				os = new FileOutputStream(dest);
				byte[] buffer = new byte[1024];
				int length;
				while ((length = is.read(buffer)) > 0) {
					os.write(buffer, 0, length);
				}
			} finally {
				is.close();
				os.close();
			}
		}
### Using NIO FileChannels

-	Java NIO classes were introduced in Java 1.4 and FileChannel can be used to copy file in java
-	According to transferFrom() method javadoc, FileChannel is supposed to be faster than using Streams for java copy files

	
	Code snippet:
	
		private static void copyFileUsingChannel(File source, File dest) throws IOException {
			FileChannel sourceChannel = null;
			FileChannel destChannel = null;
			try {
				sourceChannel = new FileInputStream(source).getChannel();
				destChannel = new FileOutputStream(dest).getChannel();
				destChannel.transferFrom(sourceChannel, 0, sourceChannel.size());
			   }finally{
				   sourceChannel.close();
				   destChannel.close();
		   }
		}

### Apache Common FileUtils

	
	Code Snippet:
	
		private static void copyFileUsingApacheCommonsIO(File source, File dest) throws IOException {
			FileUtils.copyFile(source, dest);
		}
	
### Java 7 Files.copy()

		
	Code Snippet:
	
		private static void copyFileUsingJava7Files(File source, File dest) throws IOException {
			Files.copy(source.toPath(), dest.toPath());
		}
-------------------------------------	

createNewFile()
delete()
isDirectory()
lists()
exists()
renameTo() - move and rename both
isFile()
length()
mkdirs()

getCanonicalPath()
getPath()
getAbsolutePath()

// Permission
canRead()
canExecute()
canWrite()

setReadable(true)
setWriteable(false)
setExecutable(false)


Files.setPosixFilePermissions(Path.get(), Set<PosixFilePermission>)

inputStream to OutStream
destChannel.transferFrom(srcChannel,0, srcChannel.size())
FileUtils.copyFile(File, File)
Files.copy(File, File)

readAllByets(Path)
readAllLines(Path)
lines(Path, StCharSets.UTF_16)

FileReader
	

	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
