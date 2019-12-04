# 操作hdfs时常用的java类

- org.apache.hadoop.fs.FileSystem
  > 一个通用文件系统的抽象基类，可以被分布式文件系统继承。所有可能使用Hadoop文件系统的代码都要使用到这个类。Hadoop为FileSystem这个抽象类提供了多种具体的实现，如 LocalFileSystem、DistributedFileSystem、HftpFileSystem、HsftpFileSystem、HarFileSystem、KosmosFileSystem、FtpFileSystem, NativeS3FileSystem

- org.apache.hadoop.fs.FileStatus
  > 一个接口，用于向客户端展示系统中文件和目录的元数据，具体包括文件大小、块大小、副本信息、所有者、修改时间等，可通过FileSystem.listStatus()方法获得具体的实例对象。

- org.apache.hadoop.fs.FSDataInputStream
  > 文件输入流，用于读取Hadoop文件.

- org.apache.hadoop.fs.FSDataOutputStream
  > 文件输出流，用于写Hadoop文件

- org.apache.hadoop.conf.Configuration
  > 访问配置项。所有的配置项的值，如果在core-site.xml中有对应的配置，则以core-site.xml为准。

- org.apache.hadoop.fs.Path
  > 用于表示Hadoop文件系统中的一个文件或者一个目录的路径。

- org.apache.hadoop.fs.PathFilter
  > 一个接口，通过实现方法 PathFilter.accept(Path path)来判定是否接收路径path表示的文件或目录。

