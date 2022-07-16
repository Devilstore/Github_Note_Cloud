# Typora Craker 使用说明



1. 解压压缩包进入进入typoraCracker-master目录下

2. 在文件夹内执行 `pip install -r requirements.txt -i http://pypi.doubanio.com/simple/ --trusted-host pypi.doubanio.com`

3. 执行完依赖之后，解密js文件。 执行 `python typora.py "typora安装路径" .`

   ![image-20220716181012761](https://devil-picture-bed.oss-cn-shenzhen.aliyuncs.com/image/202207161810808.png)

4. 然后吧typoraCracker-master目录下的dec_app文件夹改为app文件夹，复制到typora安装目录下的resources的文件夹内。

5. 查找app文件夹内的的 License.js 文件， 搜索 `getInstallDate` 找到 js 记录的安装日期并修改。（下图的红框框的是修改后的效果，照着改进行，改完之后保存，重新打开typora即可。）



![image-20220716181405457](https://devil-picture-bed.oss-cn-shenzhen.aliyuncs.com/image/202207161814521.png)
