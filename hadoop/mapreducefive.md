# outputformat与inputformat的自定义改写:
## 主方法:
### ......................
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
		    job.setMapperClass(SelfMap.class);
				- 下面是调用input与output方法:
		    job.setInputFormatClass(SelfInputFormat .class);
		    job.setOutputFormatClass(SelfOutputFormat.class);
		    job.setNumReduceTasks(0);
    	   // job.setCombinerClass(FileReduce.class);
		    job.setReducerClass(OneAndoneReduce.class);
		    job.setOutputKeyClass(Text.class);
		    job.setOutputValueClass(Text.class);
		    for (int i = 0; i < otherArgs.length - 1; i++) {
		      FileInputFormat.addInputPath(job, new Path(otherArgs[i]));
		    }
		    FileOutputFormat.setOutputPath(job, new Path(otherArgs[(otherArgs.length - 1)]));
		    
		    System.exit(job.waitForCompletion(true) ? 0 : 1);
		  }
		  
## 然后进行output的编写:
### ........................
	- 对outputformat进行编写;
		- public class SelfOutputFormat extends FileOutputFormat<Text, Text>{


	@Override
	public RecordWriter<Text, Text> getRecordWriter(TaskAttemptContext arg0) throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		
		Path outputPath = this.getOutputPath(arg0);
		FileSystem fileSystem = FileSystem.get(arg0.getConfiguration());
		String succ = outputPath.toString() +"/succ.log";
		String error = outputPath.toString() +"/error.log";
		Path  succc = new Path(succ);
		Path  errorr = new Path(error);
		FSDataOutputStream succteam ;
		FSDataOutputStream errorteam ;
		- 此处一定要注意;如果一味的用create的话就会一直覆盖文件,最后文件中只保留最后一条数据.所以此处要加上一个if判定:
		if (fileSystem.isFile(succc)) {
			succteam = fileSystem.append(new Path(succ));
		}else {
			succteam = fileSystem.create(new Path(succ));
		}
		if (fileSystem.isFile(errorr)) {
			errorteam = fileSystem.append(errorr);
		}else {
			 errorteam = fileSystem.create(errorr);
		}
			
		return new SelfRecordWriter(succteam,errorteam);
	}

}
	- public class SelfRecordReader extends RecordReader<NullWritable, Text> {
	
	private FileSplit files;
	private Configuration conf;
	private boolean res = true;
	private Text valueText = new Text();
	
	@Override
	public void initialize(InputSplit arg0, TaskAttemptContext arg1) throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		files = (FileSplit) arg0;
		conf = arg1.getConfiguration();
	}
	@Override
	public void close() throws IOException {
		// TODO Auto-generated method stub
		
	}

	@Override
	public NullWritable getCurrentKey() throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		return NullWritable.get();
	}

	@Override
	public Text getCurrentValue() throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		return valueText;
	}

	@Override
	public float getProgress() throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		return (float)(res?0.0:1.0);
	}

	

	@Override
	public boolean nextKeyValue() throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		// 先想好开关怎么设置
		//  因为 我们要一次性将整个文件读出;
		if (res) {
			FileSystem fileSystem = FileSystem.get(conf);
			FSDataInputStream open = fileSystem.open(files.getPath());
			byte[] buf = new byte[(int)files.getLength()];
			IOUtils.readFully(open,buf, 0, buf.length);
			// 将结果写入到我们对应的属性中,
			// 供对应的方法进行调用
			valueText.set(buf,0,buf.length);
			res = false;
			return true;
		}
		return false;
	}

}

## input方法的编写:
### .....................................
	- public class SelfInputFormat extends FileInputFormat<NullWritable, Text>{

	@Override
	protected boolean isSplitable(JobContext context, Path filename) {
		// TODO Auto-generated method stub
		return false;
	}
	@Override
	public RecordReader<NullWritable, Text> createRecordReader(InputSplit split, TaskAttemptContext context)
			throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		SelfRecordReader selfRecordReader = new SelfRecordReader();
		selfRecordReader.initialize(split, context);
		return selfRecordReader;
	}

}
	- public class SelfRecordWriter extends RecordWriter<Text,Text> {
	private FSDataOutputStream success;
	private FSDataOutputStream error;
	
	
	
	public SelfRecordWriter(FSDataOutputStream success, FSDataOutputStream error) {
		super();
		this.success = success;
		this.error = error;
	}

	@Override
	public void close(TaskAttemptContext arg0) throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		if (success != null) {
			success.close();
		}
		if (error != null) {
			error.close();
			
		}
	}

	@Override
	public void write(Text arg0, Text arg1) throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		
		
		String result = arg0.toString()+"\t"+arg1.toString();
		if (arg1.toString().contains("error")) {
			error.writeUTF(result);
		}else{
			success.writeUTF(result);
		}
	}

}

## map方法的编写:
### ...................................
	- public class SelfMap extends Mapper<NullWritable, Text, Text, Text> {
	private  Text fileText = new Text();
	private Text valueText = new Text();
	@Override
	protected void setup(Mapper<NullWritable, Text, Text, Text>.Context context)
			throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		FileSplit fileSplit = (FileSplit)context.getInputSplit();
		fileText.set(fileSplit.getPath().getName());
	}
	@Override
	protected void map(NullWritable key, Text value, Mapper<NullWritable, Text, Text, Text>.Context context)
			throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		System.out.println("map is run");
		System.out.println("value"+value);
		
		String[] vals = value.toString().split("\n");
		for (String string : vals) {
			System.out.println("\n result" +string);
			String[] values = string.split("\\s+");
			string = string.replace("\r", "");
			string = string.replace("\n", "");
			if (values.length != 3) {
				StringBuffer sBuffer = new StringBuffer(string);
				sBuffer.append("\t");
				sBuffer.append("error");
				valueText.set(sBuffer.toString());
			}else {
				valueText.set(string);
			} 
			context.write(fileText,valueText );
		}
		
		
	}
}
