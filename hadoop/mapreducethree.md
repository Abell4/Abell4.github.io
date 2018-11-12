# inputformat�ĸ�д:
## text������:
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
			- ���������Զ���inputformat�ĵ������
		    job.setInputFormatClass(MyInputFormat.class);
		   // job.setOutputFormatClass(MyOutputFormat.class);
		   -  ��Ϊ�ǲ��Ժ���ϰ,û����reduce�ı�Ҫ,����д�����������
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
		  
## inputformat�ķ����Ĵ���:
### .................................
	- ���Ҫ�Զ���input�½������һ��Ҫ�̳�FileInputFormat,�������ܵ���
	- public class MyInputFormat extends FileInputFormat<Text, NullWritable>{
		- ��д�Ĵ˷�����ȷ���Ƿ��Ƭ,�Լ���ز���
	@Override
	protected boolean isSplitable(JobContext context, Path filename) {
		// TODO Auto-generated method stub
		return false;
	}
		- ����д�����Ƕ����ݼ��Դ���ķ���;���Ҫ����MyRecordReard��;
	@Override
	public RecordReader<Text, NullWritable> createRecordReader(InputSplit arg0, TaskAttemptContext arg1)
			throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		
		MyRecordReard myRecordReard = new MyRecordReard();
		myRecordReard.initialize(arg0, arg1);
		return myRecordReard;
	}

		}

## 	MyRecordReard�ķ�������:
### ...................................
	- public class MyRecordReard extends RecordReader<Text, NullWritable>{
		- ��������,�Ա�������ʹ��
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
			- �����ݴ�������ж�
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
			- ����������д���ǰ�return�Ĵ�������ĳɶ�Ӧ��key��vakue�ͺ�
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
			// ����
			public float getProgress() throws IOException, InterruptedException {
				// TODO Auto-generated method stub
				return (float)(result?1.0:0.0);
			}

			
			

		}

## map����
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

### �ܽ�:
	- �˷������Ƕ�input���и�д,���������������Լ���Ը��ʹ��,�����Լ�������,���ҾͲ�����Ҫ�̶�������������