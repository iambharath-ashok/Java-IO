# Java Zipping and Unzipping Files and Directories

------------------------------------------------	
## Java Zip File Folder Example

-	java.util.zip.ZipOutputStream can be used to compress a file into ZIP format
-	Since a zip file can contain multiple entries, 
-	ZipOutputStream uses java.util.zip.ZipEntry to represent a zip file entry
	

	Code Snippet:
		
		List<String> filesListInDir = new ArrayList<String>();
		
		public static void main(String[] args) {
			File file = new File("/Users/Iambharath/sitemap.xml");
			String zipFileName = "/Users/Iambharath/sitemap.zip";
			
			File dir = new File("/Users/Iambharath/tmp");
			String zipDirName = "/Users/Iambharath/tmp.zip";
			
			zipSingleFile(file, zipFileName);
			
			ZipFiles zipFiles = new ZipFiles();
			zipFiles.zipDirectory(dir, zipDirName);
		}
	
	
### Zipping Single File

-	Creating a zip archive for a single file is very easy, 
-	we need to create a ZipOutputStream object from the FileOutputStream object of destination file
-	Then we add a new ZipEntry to the ZipOutputStream and use FileInputStream to read the source file to ZipOutputStream object
-	Once we are done writing, we need to close ZipEntry and release all the resources
	
	
	Code Snippet of Zipping Single File:
	
		**
		 * This method compresses the single file to zip format
		 * @param file
		 * @param zipFileName
		 ***/
		private static void zipSingleFile(File file, String zipFileName) {
			try {
				//create ZipOutputStream to write to the zip file
				FileOutputStream fos = new FileOutputStream(zipFileName);
				ZipOutputStream zos = new ZipOutputStream(fos);
				
				//add a new Zip Entry to the ZipOutputStream
				ZipEntry ze = new ZipEntry(file.getName());
				zos.putNextEntry(ze);
				
				//read the file and write to ZipOutputStream
				FileInputStream fis = new FileInputStream(file);
				byte[] buffer = new byte[1024];
				int len;
				while ((len = fis.read(buffer)) > 0) {
					zos.write(buffer, 0, len);
				}
				
				//Close the zip entry to write to zip file
				zos.closeEntry();
				//Close resources
				zos.close();
				fis.close();
				fos.close();
				System.out.println(file.getCanonicalPath()+" is zipped to "+zipFileName);
				
			} catch (IOException e) {
				e.printStackTrace();
			}

		}

### Java Zip Folder

-	Zipping a directory is little tricky, 
-	First we need to get the files list as absolute path
-	Then process each one of them separately
-	We need to add a ZipEntry for each file 
-	And use FileInputStream to read the content of the source file to the ZipEntry corresponding to that file
	
	
	Code Snippet:
		
		/**
		 * This method zips the directory
		 * @param dir
		 * @param zipDirName
		 ***/
		private void zipDirectory(File dir, String zipDirName) {
			try {
				populateFilesList(dir);
				
				//now zip files one by one
				//create ZipOutputStream to write to the zip file
				FileOutputStream fos = new FileOutputStream(zipDirName);
				ZipOutputStream zos = new ZipOutputStream(fos);
				
				for(String filePath : filesListInDir){
					System.out.println("Zipping "+filePath);
					
					//for ZipEntry we need to keep only relative file path,
					//so we used substring on absolute path
					ZipEntry ze = new ZipEntry(filePath.substring(dir.getAbsolutePath().length()+1, filePath.length()));
					zos.putNextEntry(ze);
				
					//read the file and write to ZipOutputStream
					FileInputStream fis = new FileInputStream(filePath);
					byte[] buffer = new byte[1024];
					int len;
					while ((len = fis.read(buffer)) > 0) {
						zos.write(buffer, 0, len);
					}
					zos.closeEntry();
					fis.close();
				}
				zos.close();
				fos.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
------------------------------------------------		

	
-	To unzip a zip file,
-	We need to read the zip file with ZipInputStream 
-	And then read all the ZipEntry one by one
-	Then use FileOutputStream to write them to file system

-	We also need to create the output directory if it doesn’t exists 
-	And any nested directories present in the zip file

	Code Snippet:
	
		public static void main(String[] args) {
        String zipFilePath = "/Users/Iambharath/tmp.zip";
        
        String destDir = "/Users/Iambharath/output";
        
        unzip(zipFilePath, destDir);
    }

    private static void unzip(String zipFilePath, String destDir) {
        File dir = new File(destDir);
        
		// create output directory if it doesn't exist
        if(!dir.exists()) dir.mkdirs();
        FileInputStream fis;
        
		//buffer for read and write data to file
        byte[] buffer = new byte[1024];
        try {
            fis = new FileInputStream(zipFilePath);
            ZipInputStream zis = new ZipInputStream(fis);
            ZipEntry ze = zis.getNextEntry();
           
			while(ze != null){
				String fileName = ze.getName();
				File newFile = new File(destDir + File.separator + fileName);
				System.out.println("Unzipping to "+newFile.getAbsolutePath());
			   
			   //create directories for sub directories in zip
				new File(newFile.getParent()).mkdirs();
				FileOutputStream fos = new FileOutputStream(newFile);
				int len;
				
				while ((len = zis.read(buffer)) > 0) {
					fos.write(buffer, 0, len);
				}
				
				fos.close();
				//close this ZipEntry
				zis.closeEntry();
				ze = zis.getNextEntry();
            }
            //close last ZipEntry
            zis.closeEntry();
            zis.close();
            fis.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        
    }
---------------------------------------------------------------	
	
## Java GZIP Example – Compress and Decompress File	
	
-	We can compress a single file in GZIP format 
-	But we can’t compress and archive a directory using GZIP like ZIP files


	Code Snippet:
	
		public class GZIPExample {

			public static void main(String[] args) {
				String file = "/Users/Iambharath/sitemap.xml";
				String gzipFile = "/Users/Iambharath/sitemap.xml.gz";
				String newFile = "/Users/Iambharath/new_sitemap.xml";
				
				compressGzipFile(file, gzipFile);
				
				decompressGzipFile(gzipFile, newFile);
					   
			}

			private static void decompressGzipFile(String gzipFile, String newFile) {
				try {
					FileInputStream fis = new FileInputStream(gzipFile);
					GZIPInputStream gis = new GZIPInputStream(fis);
					FileOutputStream fos = new FileOutputStream(newFile);
					byte[] buffer = new byte[1024];
					int len;
					while((len = gis.read(buffer)) != -1){
						fos.write(buffer, 0, len);
					}
					//close resources
					fos.close();
					gis.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
				
			}

			private static void compressGzipFile(String file, String gzipFile) {
				try {
					FileInputStream fis = new FileInputStream(file);
					FileOutputStream fos = new FileOutputStream(gzipFile);
					GZIPOutputStream gzipOS = new GZIPOutputStream(fos);
					byte[] buffer = new byte[1024];
					int len;
					while((len=fis.read(buffer)) != -1){
						gzipOS.write(buffer, 0, len);
					}
					//close resources
					gzipOS.close();
					fos.close();
					fis.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
				
			}
		}
-	While decompressing a GZIP file, if it’s not in GZIP format
-	Following exception will be thrown:
	
	
	Exception:
		
		java.util.zip.ZipException: Not in GZIP format
			at java.util.zip.GZIPInputStream.readHeader(GZIPInputStream.java:164)
			at java.util.zip.GZIPInputStream.<init>(GZIPInputStream.java:78)
			at java.util.zip.GZIPInputStream.<init>(GZIPInputStream.java:90)
			at com.journaldev.files.GZIPExample.decompressGzipFile(GZIPExample.java:25)
			at com.journaldev.files.GZIPExample.main(GZIPExample.java:18)

----------------------------------------------------------	
## Java Temp File	
	
	
-	java.io.File class can be used to create temp file in Java
-	There are two methods in File class that be used to create temp file in Java

-	There are two methods in File class that can be used to create temp file in java:

	-	createTempFile(String prefix, String suffix, File directory):
		
		-	This method creates a temp file with given suffix and prefix in the directory argument
		-	The directory should already be existing and should be a directory, 
		-	Else it throws exception

		-	The file name is created with random long number,
		-	So the file name becomes prefix+random_long_no+suffix

-	This is done to make the application safe as it will be impossible to guess the file name 
-	Since application has instance of temp file, we can use it in Application
-	The prefix String should be minimum three characters long. 
-	If suffix is null, “.tmp” suffix is used

-	If directory is null, then temp file is created in operating system temp directory

	-	createTempFile(String prefix, String suffix): 
		
		-	This method is used to create temp file OS temp directory
		-	It’s easy way to create temp file in operating system temp directory
	
	
	Code Snippet:
	
		public class JavaTempFile {

			public static void main(String[] args) {
				try {
					File tmpFile = File.createTempFile("data", null);
					File newFile = File.createTempFile("text", ".temp", new File("/Users/Iambharath/temp"));
					System.out.println(tmpFile.getCanonicalPath());
					System.out.println(newFile.getCanonicalPath());
					// write,read data to temporary file like any normal file

					// delete when application terminates
					tmpFile.deleteOnExit();
					newFile.deleteOnExit();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}

		}
	
------------------------------------------------------------	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
