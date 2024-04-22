# KFI-File2Str

There are some differences to be noted: class File which, to some extend, plays
the rôle of the class Path from the java.nio.file package. Notice also, that Buffere-
dReader must be created in two steps: first we create ‘raw’ byte stream of type, e.g.,
FileInputStream, then we pass it to the constructor of InputStreamReader with, as
the second argument, the required encoding, and then this to the constructor of the
BufferedReader. In the example above there are only two steps: to the constructor
of BufferedReader we pass directly an object of type FileReader, but then there is
no way to specify the encoding — default system encoding will be assumed. Note how
try-with-resources simplifies operations on IO streams; in the program above, we don’t
use it and therefore the somewhat complicated finally clause is needed (as close may
itself also throw an exception).  
  
It is sometimes useful to a read text file into a single string (for example, to process it with regular exceptions). This is a very common task, and it can be accomplished, for example, as shown belo
