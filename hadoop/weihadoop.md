# hadoop��α�ֲ�ʽ����
## α�ֲ�ʽ�������ǻ��ڵ���hadoop�����õ�.����Ҫ����õ���hadoop
### ׼������:
	- ���úõ���hadoop

### �޸������ļ�:
	- ���Ȼص���Ŀ¼��
	- vim hadoop-2.7.3/etc/hadoop/hadoop-env.sh �򿪴��ļ�,�޸�javahome:  ���Լ���java����·��д��ȥ
### vim hadoop-2.7.3/etc/hadoop/core_cite.xml �򿪴��ļ�
	- <configuration></configuration>�������������
		- <property>
			<name>hadoop.tmp.dir</name>
			<value>���Լ��ĸ�Ŀ¼�ļ�·��</value>
		</property>
		<property>
			<name>fs.defaultFS</name>
			<value>hdfs://�Լ���ip:9000</value>
		</property>
		
### vim hadoop-2.7.3/etc/hadoop/hdfs-site.xml �򿪴��ļ�
	- <configuration></configuration>�������������
		- <property>
			<name>dfs.namenode.name.dir</name>
			<value>file:/home/baitian/dfs/name</value>
		</property>
		<property>
			<name>dfs.datanode.data.dir</name>
			<value>file:/home/baitian/dfs/data</value>
		</property>

### vim hadoop-2.7.3/etc/hadoop/yarn-site.xml�򿪴��ļ�
	- <configuration></configuration>�������������
		- <property>
			<name>mapreduce.framework,name</name>
			<value>yarn</value>
		</property>
		<property>
			<name>yarn.nodemanager.aux-services</name>
			<value>mapreduce_shuffle</value>
		</property>

### vim hadoop-2.7.3/etc/hadoop/mapred-site.xml�򿪴��ļ�
	- <configuration></configuration>�������������
		- <property>
			<name>mapreduce.framework.name</name>
			<value>yarn</value>
		</property>

### ���ú��Ժ� :
	- hadoop namenode -format  ִ�д�����
	
![TIM��ͼ20181030200631.png](https://upload-images.jianshu.io/upload_images/14477271-2e7343c59cc13820.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

	- ���ִ������˵���ɹ�
	- start-all.sh ִ�д�����
	- ��  jps����֤�Ƿ�ɹ�
	
![TIM��ͼ20181030200847.png](https://upload-images.jianshu.io/upload_images/14477271-c15c645f24a7a0a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
	


