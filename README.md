# distributed_crawler with nutch

下载nutch
git clone https://github.com/apache/nutch

切换版本			
git checkout  release-1.12 (Our change is based on Nutch 1.12) 
这里的版本其实都已经存在的所以是不需要创建的。
可以打git checkout 打tab'看看有哪些版本
Apply patch
patch -p1 < ../week1/googleplaycrawler/googleplaycrawler.patch 
//注意这个是 1是数字1， 不是字母l


打包jar
ant job 
在nutch目录下操作

进入docker container， 打开hadoop。 

		 	 	 		
			
				
					
生成seed文件						
echo "https://play.google.com/store/apps/details?id=com.facebook.orca" > seed

local模式开始爬虫
hadoop jar build/apache-nutch-1.12.job org.apache.nutch.googleplay.GooglePlayCrawler -Dmapreduce.framework.name=local -Dfs.defaultFS=file:/// seed -numFetchers 10 -depth 2
在nutch文件夹下， 因为你要运行的jar在build下面，build在nutch文件夹下
-numFetchers的意思是并行度
depth是抓取多少层


查看输出的结果
输出的结果在nutc/nutchdb/segments/xxxxx/parse_data/part-0000x/data里面以二进制存储	
find . -name data -exec ls -l {} \;
# find the largest data file
一般情况下爬的东西很多都是乱七八糟的， 你要的东西最有价值的就是size最大的文件
export HADOOP_CLASSPATH=/home/hadoop/src/nutch/build/apache-nutch-1.12.job
修改环境变量
hadoop fs -text file:///home/hadoop/src/nutch/nutchdb/segments/20180401233018/parse_data/part-00003/data
注意修成你的  20180401233018 以及你所需要的part 
