# join在mapreduce中的应用:
## 具体事例:
### 生成类:
	- 
		- public class JoinBean implements Writable{
	 
		private int oid;
		private String goodsId;
		private int count;
		private String goodsname;
		private int ck;
		private int flag; //0 -aa   1-bb
		
		@Override
		public void write(DataOutput paramDataOutput) throws IOException {
			// TODO Auto-generated method stub
			paramDataOutput.writeInt(oid);
			paramDataOutput.writeUTF(goodsId);
			paramDataOutput.writeInt(count);
			paramDataOutput.writeUTF(goodsname);
			paramDataOutput.writeInt(ck);
			paramDataOutput.writeInt(flag);
		}
		- 下面两个是继承(implements)Writable所生成的方法:
			- 主要用途是在写入时进行序列化与反序列化
		@Override
		public void readFields(DataInput in) throws IOException {
			// TODO Auto-generated method stub
			this.oid = in.readInt();
			this.goodsId = in.readUTF();
			this.count = in.readInt();
			this.goodsname = in.readUTF();
			this.ck = in.readInt();
			this.flag = in.readInt();
		}
		
		- 构造方法:,用于给aa.txt赋值
		public void setaa(int oid,String goodsId,int count){
			this.oid = oid;
			this.goodsId = goodsId;
			this.count = count;
			this.flag = 0;
			this.goodsname = "";
			this.ck = -1;
			// 没有获取的值也要给其赋值,否则会出现空指针异常.
		}
		
		- 构造方法:用于给bb.txt赋值
		public void setbb( String goodsId,String goodsname,int ck){
			this.goodsname = goodsname;
			this.ck = ck;
			this.goodsId= goodsId;
			this.flag = 1;
			this.oid = 0;
			this.count = 0;
		}
		
        - tostring方法:
		@Override
		public String toString() {
			return  oid + "\t" + goodsId + "\t" + count + "\t" + goodsname
					+ "\t" + ck + "\t" + flag ;
		}
		public int getFlag() {
			return flag;
		}
		public void setFlag(int flag) {
			this.flag = flag;
		}
		public int getId() {
			return oid;
		}

		public void setId(int id) {
			this.oid = id;
		}

		public String getGoodsId() {
			return goodsId;
		}

		public void setGoodsId(String goodsId) {
			this.goodsId = goodsId;
		}

		public int getCount() {
			return count;
		}

		public void setCount(int count) {
			this.count = count;
		}

		public String getGoodsname() {
			return goodsname;
		}

		public void setGoodsname(String goodsname) {
			this.goodsname = goodsname;
		}

		public int getCk() {
			return ck;
		}

		public void setCk(int ck) {
			this.ck = ck;
		}

		public JoinBean() {
			
			// TODO Auto-generated constructor stub
		}

		- 
		public void setbbByaa(JoinBean bb) {
			// TODO Auto-generated method stub
			this.goodsname = bb.getGoodsname();
			this.ck = bb.getCk();
		}

	}

### map方法