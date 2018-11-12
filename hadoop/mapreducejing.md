# word count�Ľ����������д
## word count�Ĵ��뽲��:
	- ��������Ҫ������ͳ����ͬ�ֶ�.
	
### ��������:
		- map����:
	- public static class TokenizerMapper
		extends Mapper<Object, Text, Text, IntWritable>
		{
		private static final IntWritable one = new IntWritable(1);
		private Text word = new Text();
    
		public void map(Object key, Text value, Mapper<Object, Text, Text, IntWritable>.Context context)
			throws IOException, InterruptedException
			{
			StringTokenizer itr = new StringTokenizer(value.toString());
			while (itr.hasMoreTokens())
				{
        this.word.set(itr.nextToken());
        context.write(this.word, one);
				}
			}
		}
		
		- Reducer����:
	- public static class IntSumReducer
			extends Reducer<Text, IntWritable, Text, IntWritable>
		{
			private IntWritable result = new IntWritable();
    
			public void reduce(Text key, Iterable<IntWritable> values, Reducer<Text, IntWritable, Text, IntWritable>.Context context)
			throws IOException, InterruptedException
			{
			int sum = 0;
			for (IntWritable val : values) {
			sum += val.get();
				}
			this.result.set(sum);
			context.write(key, this.result);
			}
		}
		
		- ������:�Ե���jobΪ��. 
	- public static void main(String[] args)
		throws Exception
		{
		Configuration conf = new Configuration();
		String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
		if (otherArgs.length < 2)
		{
		System.err.println("Usage: wordcount <in> [<in>...] <out>");
		System.exit(2);
		}
		Job job = Job.getInstance(conf, "word count");
		job.setJarByClass(WordCount.class);
		job.setMapperClass(countmapper.class);
		job.setCombinerClass(IntSumReducer.class);
		job.setReducerClass(countReducer.class);
		job.setOutputKeyClass(Text.class);
		job.setOutputValueClass(IntWritable.class);
		for (int i = 0; i < otherArgs.length - 1; i++) {
		  	FileInputFormat.addInputPath(job, new Path(otherArgs[i]));
			}
		FileOutputFormat.setOutputPath(job, new Path(otherArgs[(otherArgs.length - 1)]));
    
		System.exit(job.waitForCompletion(true) ? 0 : 1);
		}
	