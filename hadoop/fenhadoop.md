# hadoop����ȫ�ֲ�ʽ����
## ��Ҫ���ú�hadoop��ȫ�ֲ�ʽ,����Ҫȷ��һ̨namenode�����
### ȷ����namenode����� hadoop��α�ֲ�ʽ�����ļ��ı�.

### ���úú� ������������hadoop������� 
	- scp -r ��Ҫ����hadoop�����ļ� �û���@ip:��ŵ�·��
	- ���������ú�������datanode

### Ȼ���namedode���vim hadoop-2.7.3/etc/hadoop/slaves 
	- �ڴ��ļ���,����������datanode��ip��ַ

### ��һ��vim /etc/hosts 
	- �ڴ��ļ����������datanode��ip��ַ�����ǵ�����,���ڼ���
	
### ���ú��Ժ� :
	- hadoop namenode -format  ִ�д�����
	
![TIM��ͼ20181030200631.png](https://upload-images.jianshu.io/upload_images/14477271-2e7343c59cc13820.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

	- ���ִ������˵���ɹ�
	- start-all.sh ִ�д�����
	- ��  jps����֤�Ƿ�ɹ�
	
![TIM��ͼ20181030200847.png](https://upload-images.jianshu.io/upload_images/14477271-c15c645f24a7a0a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
### �ڲ鿴datanode�Ƿ�ɹ�:
	- ��  jps����֤�Ƿ�ɹ�
	
![TIM��ͼ20181030204237.png](https://upload-images.jianshu.io/upload_images/14477271-09521ee6ff4b4f60.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

