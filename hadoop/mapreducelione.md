# mapreducee的实例2:
## 本实例主要是练习在编写mapreduce时输入与输出的类等实际应用:
### 计算上下行流量与流量总和:
	- 生成类:
		- 本方法中我还会逐步标注一写基础调试
			- public class stearmbear  implements WritableComparable {
		private int  upload;
		private int download;
		private int sum;
		public int getUpload() {
			return upload;
		}
		public void setUpload(int upload) {
			this.upload = upload;
		}
		public int getDownload() {
			return download;
		}
		public void setDownload(int download) {
			this.download = download;
		}
		public int getSum() {
			return sum;
		}
		public void setSum(int sum) {
			this.sum = sum;
		}
		- 这个是构造方法.注意:一定要生成一个无参构造
		public stearmbear() {
			
		}
		- 下面两个是继承(implements)Writable所生成的方法:
			- 主要用途是在写入时进行序列化与反序列化
		@Override
		public void readFields(DataInput arg0) throws IOException {
			// TODO Auto-generated method stub
			
			upload = arg0.readInt();
			download = arg0.readInt();
			sum = arg0.readInt();
		}
		@Override
		public void write(DataOutput arg0) throws IOException {
			// TODO Auto-generated method stub
		arg0.writeInt(this.upload);
		arg0.writeInt(this.download);
		arg0.writeInt(this.sum);
		}
		- 下面两个是继承(implements)Comparable所产生的方法:
			- 主要是用于对输入的数据进行排序: 
		@Override
		public int compareTo(Object o) {
			
			// TODO Auto-generated method stub
			stearmbear bin = (stearmbear)o;
			- 这条就是控制排序的升降
			return this.sum > bin.sum ?1:-1;
		}
		- 为tostring方法:一般会进行改造,改造成所适应的形式,
		@Override
		public String toString() {
			return "stearmbear [upload=" + upload + ", download=" + download + ", sum=" + sum + "]";
		}
	
### 进行map方法的编写:
	- public class bearmapper extends Mapper<Object, Text, Text, stearmbear> {
		- 此处的泛型注意:
			- 基本上输入的类型都是Object与Text,后两个为输出类型(也就是传到reduce的类型);所以写的时候一定要慎重,不然会出现格式错误
		private Text phonetext  = new Text();
		private stearmbear stearmbear = new stearmbear(); 
		@Override
		protected void map(Object key, Text value, Mapper<Object, Text, Text, stearmbear>.Context context)
				throws IOException, InterruptedException {
			// TODO Auto-generated method stub
			- 此处用来截取字段,来配合你的需求
			String[] vals = value.toString().split("\\s+");
			String phone = vals[1];
			String downsteam = vals[vals.length-2];
			String downsteam2 = vals[vals.length-3];
			int parseInt = Integer.parseInt(downsteam);
			int parseInt1 = Integer.parseInt(downsteam2);
			int sum = parseInt+parseInt1;
			//Text text = new Text();
			//text.set(phone);
			- 把所截取的数据存到相应的类中
			stearmbear.setUpload(parseInt1);
			stearmbear.setDownload(parseInt);
			stearmbear.setSum(sum);
			- 存入主建中
			phonetext.set(phone);
			- 传入reduce方法中
			context.write(phonetext , stearmbear);
		}
	}
	
### 进行reduce方法的编写:
	- public class bearreducer extends Reducer<Text, stearmbear, Text, stearmbear>  {
	private stearmbear stearmbear = new stearmbear();
	@Override
	protected void reduce(Text  key, Iterable<stearmbear>values,
			Reducer<Text, stearmbear, Text, stearmbear>.Context context) throws IOException, InterruptedException {
		// TODO Auto-generated method stub
		int sum = 0 ;
		int  up = 0;
		int dow = 0;
		for (stearmbear stearmbear : values) {
			up +=stearmbear.getUpload();
			dow += stearmbear.getDownload();
			sum += stearmbear.getSum();
		}
		stearmbear.setUpload(up);
		stearmbear.setDownload(dow);
		stearmbear.setSum(sum);
		context.write(key, stearmbear);
	}
	
### 主方法的编写
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
				- 此job里写的是主类名
			    job.setJarByClass(beartset.class);
				- 此job写的是map方法
			    job.setMapperClass(bearmapper.class);
				- 此job里写的combiner(reduce方法).在运行reduce之前运行,节约占用
			    job.setCombinerClass(bearreducer.class);
				- 此job中写的是reduce的类名
			    job.setReducerClass(bearreducer.class);
				-此job中写的是key的应用类
			    job.setOutputKeyClass(Text.class);
				- 此job中写的是value的应用类
			    job.setOutputValueClass(stearmbear.class);
			    for (int i = 0; i < otherArgs.length - 1; i++) {
			      FileInputFormat.addInputPath(job, new Path(otherArgs[i]));
			    }
			    FileOutputFormat.setOutputPath(job, new Path(otherArgs[(otherArgs.length - 1)]));
			    
			    System.exit(job.waitForCompletion(true) ? 0 : 1);
			  }
}