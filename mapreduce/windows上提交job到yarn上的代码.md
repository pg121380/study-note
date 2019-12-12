~~~java
package pub.liyf.mapreducedemo.wordcount;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

import java.io.IOException;

public class JobSubmitter {
    public static void main(String[] args) {
        Configuration conf = new Configuration();
        //1.设置job运行时访问的文件系统
        conf.set("fs.defaultFS", "hdfs://master:9000");
        //2.设置job提交到哪去运行(yarn/本地模拟)
        conf.set("mapreduce.framework.name", "yarn");       //在本地模拟 则填写local
        //3.设置yarn地址(resource manager)
        conf.set("yarn.resourcemanager.hostname", "master");

        conf.set("mapreduce.app-submission.cross-platform","true" );      //要在windows上提交job需要加上这个跨平台参数

        conf.set("yarn.resourcemanager.scheduler.address", "master:8030");      //设置调度器的位置,必须是resourcemanager的位置 不然运行结果无法返回客户端
        try {
            Job word_count = Job.getInstance(conf, "word count");
            //1. 封装jar包所在的位置
//            word_count.setJarByClass(JobSubmitter.class);
            word_count.setJar("H:\\jar包\\mapreducedemo-1.0-SNAPSHOT.jar");
            //2. 封装参数: 本次job所要调用的mapper reducer实现类
            word_count.setMapperClass(WordCountMapper.class);
            word_count.setReducerClass(WordCountReducer.class);
            //3. 封装参数： 本次job的Mapper实现类产生的结果数据的key/value类型
            word_count.setMapOutputKeyClass(Text.class);
            word_count.setMapOutputValueClass(IntWritable.class);
            //4. 指定reducer实现类产生的数据的结果类型
            word_count.setOutputKeyClass(Text.class);
            word_count.setOutputValueClass(IntWritable.class);
            //5. 封装本次job要处理的输入数据集所在的数据
            FileInputFormat.setInputPaths(word_count,new Path("/wordcount/input"));
            //6. 封装本次job要处理的最终结果的输出路径 必须是不存在的路径
            FileOutputFormat.setOutputPath(word_count,new Path("/wordcount/mapreduce_output1"));

            //7. 封装想要启动的reduce task的数量
            word_count.setNumReduceTasks(2);

            //8. 提交job给yarn
            boolean completion = word_count.waitForCompletion(true);//是否等待job完成
            if (completion){
                System.out.println("job successfully finished!");
            }

        } catch (IOException | InterruptedException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

~~~