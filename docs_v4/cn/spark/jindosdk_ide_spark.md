# Spark 使用 JindoSDK 在 IDE 开发调试 

在maven中添加本地sdk jar包的依赖
```xml
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-common</artifactId>
            <version>2.8.5</version>
        </dependency>
        <dependency>
            <groupId>com.aliyun.jindodata</groupId>
            <artifactId>jindofs-hadoop-sdk</artifactId>
            <version>4.0.0</version>
            <scope>system</scope>
            <systemPath>/Users/xx/xx/jindosdk-dls-4.0.0.jar</systemPath>
        </dependency>
```
然后您可以编写Java程序使用SDK
```java
import org.apache.spark.sql.SparkSession;
public class TestJindoSDK {
  public static void main(String[] args) throws Exception {
    SparkSession spark = SparkSession
        .builder()
        .config("spark.hadoop.fs.AbstractFileSystem.oss.impl", "com.aliyun.jindodata.dls.JindoDlsFileSystem")
        .config("spark.hadoop.fs.oss.impl", "com.aliyun.jindodata.dls.DLS")
        .config("spark.hadoop.fs.dls.accessKeyId", "xxx")
        .config("spark.hadoop.fs.dls.accessKeySecret", "xxx")
        .appName("TestJindoSDK")
        .getOrCreate();
    spark.read().parquet("oss://xxx").count();
    spark.stop();
  }
}
```
<br />