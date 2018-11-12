# outputformat的自定义改写:
## 主方法:
### ...........................................
	- public static void main(String[] args)
		    throws Exception
		  {
		    Configuration conf = new Configuration();
		    System.setProperty("HADOOP_USER_NAME", "baitian");
	         conf.set("fs.defaultFS", "hdfs://172.18.24.160:9000");

		    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
		    if (otherArgs.length < 2)
		    {
		      System.err.println("Usage: wordcount <in> [<in>...] <out>");
		      System.exit(2);
		    }
		    Job job = Job.getInstance(conf, "word count");
		    job.setJarByClass(MyOutputText.class);
		    job.setMapperClass(MyOutputMap.class);
			- 此处是调用自定义的outputformat的方法 
		    job.setOutputFormatClass(MyOutputFormat.class);
		    job.setNumReduceTasks(0);
    	   // job.setCombinerClass(FileReduce.class);
		    job.setReducerClass(OneAndoneReduce.class);
		    job.setOutputKeyClass(Text.class);
		    job.setOutputValueClass(NullWritable.class);
		    for (int i = 0; i < otherArgs.length - 1; i++) {
		      FileInputFormat.addInputPath(job, new Path(otherArgs[i]));
		    }
		    FileOutputFormat.setOutputPath(job, new Path(otherArgs[(otherArgs.length - 1)]));
		    
		    System.exit(job.waitForCompletion(true) ? 0 : 1);
		  }
		  
## MyOutputFormat的方法代码:
###	.....................................
	- public class MyOutputFormat extends FileOutputFormat<Text, NullWritable>{

	@Override
	public RecordWriter<Text, NullWritable> getRecordWriter(TaskAttemptContext job)
			throws IOException, InterruptedException {
		
		Path outPath = this.getOutputPath(job);
		String succ = outPath.toString()+"/success.log";
		String rerr = outPath.toString()+"/rerro.log";	
		// TODO Auto-generated method stub
		Configuration configuration = job.getConfiguration();
		FileSystem fileSystem = FileSystem.get(configuration);
		FSDataOutputStream success = fileSystem.create(new Path(succ));
		FSDataOutputStream error = fileSystem.create(new Path(rerr));
		return new MyRecordWriter(success, error);
	}

}

## MyRecordWriter方法写代码:
### ..................................
	- public class MyRecordWriter extends RecordWriter<Text,NullWritable>{
	private FSDataOutputStream success;
	private FSDataOutputStream error;
	
	

	
	public MyRecordWriter(FSDataOutputStream success, FSDataOutputStream error) {
		super();
		this.success = success;
		this.error = error;
	}

	@Override
	public void close(TaskAttemptContext arg0) throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		if (success != null) {
			success.close();
		}else if (error != null) {
			error.close();
		}
	}

	@Override
	public void write(Text arg0, NullWritable arg1) throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		System.out.println(arg0);
		System.out.println(arg1);
		//success.write(arg0);
		if (arg0.toString().contains("error")) {
			error.writeUTF(arg0.toString());
		}else{
			success.writeUTF(arg0.toString());
		}
		
	}

}

## map方法的代码:
### ............................
	- public class MyOutputMap extends Mapper<Object, Text, Text,NullWritable 	>{  
	private  Text resText = new  Text();
	
	@Override
	protected void map(Object key, Text value, Mapper<Object, Text, Text, NullWritable>.Context context)
			throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		
		String[] vals = value.toString().split("\\t+");
		
		if (vals.length!=3) {
			StringBuffer sb = new StringBuffer(value.toString());
			sb.append("\t");
			sb.append("error");
			value.set(sb.toString());
		}
		context.write(value, NullWritable.get());
	}
}