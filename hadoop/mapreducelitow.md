# join��mapreduce�е�Ӧ��:
## ��������:
### ������:
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
		- ���������Ǽ̳�(implements)Writable�����ɵķ���:
			- ��Ҫ��;����д��ʱ�������л��뷴���л�
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
		
		- ���췽��:,���ڸ�aa.txt��ֵ
		public void setaa(int oid,String goodsId,int count){
			this.oid = oid;
			this.goodsId = goodsId;
			this.count = count;
			this.flag = 0;
			this.goodsname = "";
			this.ck = -1;
			// û�л�ȡ��ֵҲҪ���丳ֵ,�������ֿ�ָ���쳣.
		}
		
		- ���췽��:���ڸ�bb.txt��ֵ
		public void setbb( String goodsId,String goodsname,int ck){
			this.goodsname = goodsname;
			this.ck = ck;
			this.goodsId= goodsId;
			this.flag = 1;
			this.oid = 0;
			this.count = 0;
		}
		
        - tostring����:
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

### map����