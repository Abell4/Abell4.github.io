# inputformat的改写:
## text主方法:
### .......................
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
		    job.setJarByClass(MyInputTest.class);
		    job.setMapperClass(MyIntoutMap.class);
			- 此语句就是自定义inputformat的调用语句
		    job.setInputFormatClass(MyInputFormat.class);
		   // job.setOutputFormatClass(MyOutputFormat.class);
		   -  因为是测试和练习,没有用reduce的必要,所以写下下面的命令
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
		  
## inputformat的方法的代码:
### .................................
	- 如果要自定义input新建的类就一定要继承FileInputFormat,这样才能调用
	- public class MyInputFormat extends FileInputFormat<Text, NullWritable>{
		- 重写的此方法是确定是否分片,以及相关操作
	@Override
	protected boolean isSplitable(JobContext context, Path filename) {
		// TODO Auto-generated method stub
		return false;
	}
		- 此重写方法是对数据记性传输的方法;因此要引用MyRecordReard类;
	@Override
	public RecordReader<Text, NullWritable> createRecordReader(InputSplit arg0, TaskAttemptContext arg1)
			throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		
		MyRecordReard myRecordReard = new MyRecordReard();
		myRecordReard.initialize(arg0, arg1);
		return myRecordReard;
	}

		}

## 	MyRecordReard的方法代码:
### ...................................
	- public class MyRecordReard extends RecordReader<Text, NullWritable>{
		- 设置属性,以便于下面使用
		private FileSplit fileSplit ;
		private Configuration conf;
		private boolean result = false;
		private Text resultText = new Text();
		- 
		@Override
			public void initialize(InputSplit arg0, TaskAttemptContext arg1) throws IOException, InterruptedException {
				// TODO Auto-generated method stub
				fileSplit = (FileSplit) arg0; 
				conf =arg1.getConfiguration();
			}
			- 对数据处理进行判断
		@Override
		public boolean nextKeyValue() throws IOException, InterruptedException {
			// TODO Auto-generated method stub
			if (!result) {
				
				FileSystem fSystem = FileSystem.get(conf);
				FSDataInputStream open = fSystem.open(fileSplit.getPath());
				byte[] buf = new byte[(int) fileSplit.getLength()];
				IOUtils.readFully(open,buf,0,buf.length);
				resultText.set(buf,0,buf.length);
				result = true;
				return true;
			}
			
			return false;
		}
			@Override
			public void close() throws IOException {
				// TODO Auto-generated method stub
				
			}
			- 下面连个重写就是把return的传出对象改成对应的key与vakue就好
			@Override
			public Text getCurrentKey() throws IOException, InterruptedException {
				// TODO Auto-generated method stub
				return resultText;
			}

			@Override
			public NullWritable getCurrentValue() throws IOException, InterruptedException {
				// TODO Auto-generated method stub
				return NullWritable.get();
			}


			@Override
			// 进度
			public float getProgress() throws IOException, InterruptedException {
				// TODO Auto-generated method stub
				return (float)(result?1.0:0.0);
			}

			
			

		}

## map方法
### .........................
	- public class MyIntoutMap extends Mapper<Text, NullWritable, Text, NullWritable> {
	@Override
	protected void map(Text key, NullWritable value, Mapper<Text, NullWritable, Text, NullWritable>.Context context)
			throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		System.out.println("map is  run");
		context.write(key, value);
	}
}

### 总结:
	- 此方法就是对input进行改写,这样才能做到随自己意愿的使用,方便自己的输入,而且就不在需要固定的类型输入了