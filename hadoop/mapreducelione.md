# mapreducee��ʵ��2:
## ��ʵ����Ҫ����ϰ�ڱ�дmapreduceʱ��������������ʵ��Ӧ��:
### ���������������������ܺ�:
	- ������:
		- ���������һ����𲽱�עһд��������
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
		- ����ǹ��췽��.ע��:һ��Ҫ����һ���޲ι���
		public stearmbear() {
			
		}
		- ���������Ǽ̳�(implements)Writable�����ɵķ���:
			- ��Ҫ��;����д��ʱ�������л��뷴���л�
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
		- ���������Ǽ̳�(implements)Comparable�������ķ���:
			- ��Ҫ�����ڶ���������ݽ�������: 
		@Override
		public int compareTo(Object o) {
			
			// TODO Auto-generated method stub
			stearmbear bin = (stearmbear)o;
			- �������ǿ������������
			return this.sum > bin.sum ?1:-1;
		}
		- Ϊtostring����:һ�����и���,���������Ӧ����ʽ,
		@Override
		public String toString() {
			return "stearmbear [upload=" + upload + ", download=" + download + ", sum=" + sum + "]";
		}
	
### ����map�����ı�д:
	- public class bearmapper extends Mapper<Object, Text, Text, stearmbear> {
		- �˴��ķ���ע��:
			- ��������������Ͷ���Object��Text,������Ϊ�������(Ҳ���Ǵ���reduce������);����д��ʱ��һ��Ҫ����,��Ȼ����ָ�ʽ����
		private Text phonetext  = new Text();
		private stearmbear stearmbear = new stearmbear(); 
		@Override
		protected void map(Object key, Text value, Mapper<Object, Text, Text, stearmbear>.Context context)
				throws IOException, InterruptedException {
			// TODO Auto-generated method stub
			- �˴�������ȡ�ֶ�,������������
			String[] vals = value.toString().split("\\s+");
			String phone = vals[1];
			String downsteam = vals[vals.length-2];
			String downsteam2 = vals[vals.length-3];
			int parseInt = Integer.parseInt(downsteam);
			int parseInt1 = Integer.parseInt(downsteam2);
			int sum = parseInt+parseInt1;
			//Text text = new Text();
			//text.set(phone);
			- ������ȡ�����ݴ浽��Ӧ������
			stearmbear.setUpload(parseInt1);
			stearmbear.setDownload(parseInt);
			stearmbear.setSum(sum);
			- ����������
			phonetext.set(phone);
			- ����reduce������
			context.write(phonetext , stearmbear);
		}
	}
	
### ����reduce�����ı�д:
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
	
### �������ı�д
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
				- ��job��д����������
			    job.setJarByClass(beartset.class);
				- ��jobд����map����
			    job.setMapperClass(bearmapper.class);
				- ��job��д��combiner(reduce����).������reduce֮ǰ����,��Լռ��
			    job.setCombinerClass(bearreducer.class);
				- ��job��д����reduce������
			    job.setReducerClass(bearreducer.class);
				-��job��д����key��Ӧ����
			    job.setOutputKeyClass(Text.class);
				- ��job��д����value��Ӧ����
			    job.setOutputValueClass(stearmbear.class);
			    for (int i = 0; i < otherArgs.length - 1; i++) {
			      FileInputFormat.addInputPath(job, new Path(otherArgs[i]));
			    }
			    FileOutputFormat.setOutputPath(job, new Path(otherArgs[(otherArgs.length - 1)]));
			    
			    System.exit(job.waitForCompletion(true) ? 0 : 1);
			  }
}