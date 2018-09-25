---
layout: post
title: C#打包
date: 2017-02-27
categories: blog
tags: [C#]
description: 一句话描述。

---

作者 ： 卿笃军
原文地址：http://blog.csdn.net/qingdujun/article/details/37563661

开发环境：VS2010+SQL Server 2008
操作系统：win7_32bit 旗舰版
开发语言：C#
项目名称：学生寄宿管理系统

下面开始介绍：如何给windows应用程序打包?

###### 第一步：

打开VS2010，打开你要打包的项目，然后右击"解决方案"，”添加“，"新建项目",弹出如下图所示界面：
点击”安装和部署“左边的三角形，选择下面的”Visual studio Installer“，再选择”安装项目“，同时将下面的命名改为”Setup“点击确定。
![][1]

###### 第二步：

点击解决方案里面生成的”Setup“，将属性中的ProtectName改为”学生寄宿系统 V1.0 “（你的项目名字）
![][2]

###### 第三步：

右击解决方案里面的”Setup“，然后再选择”属性“。弹出属性页界面如下第二张图：
再点击里面的系统必备。
重要一点：勾选"从与我的应用程序相同的位置下载系统必备组件(D)"，其实意思就是说你勾选后，生成安装项目时，在你安装项目的路径下，会有你在系统必备组件列表中勾选的组件.(系统自动完成，这一点还不错，不需要你自己去下载组件)
1）、Windows Installer 3.1（必选）

2）、.NET Framework 3.5 （可选）参考最后说明

3）、Crystal Report Basic for Visual Studio2008(x86,x64) (可选) 项目中用到了水晶报表就需要勾选此项
![][3]
![][4]

###### 第四步：

右击”应用程序文件夹“，点击”添加“，再点击文件夹，命名为”DB“（可随意）用于存放你的数据库文件。
然后再右击刚才添加好的"DB"文件夹，”添加“，”文件“，将你的数据库添加进来。
![][5]
![][6]

###### 第五步：

右击”应用程序文件夹“，点击”添加“，点击”文件“。将你的Release目录下面的文件全部添加进来。
![][7]

###### 第六步：

右击”应用程序文件夹“，点击”添加“，选择”项目输出...“，注意：在项目栏要选择你自己的项目（我的项目名：StudentJisu），然后选择”主输出“，点击确定。下面。
![][8]
![][9]

######第七步：

创建桌面快捷方式，右击刚才添加的”主输出“，然后选择第一个”创建快捷方式“，然后你可以将快捷方式重新命名（我重新命名为：学生寄宿管理系统）
最后，鼠标左键点住快捷方式，然后拖放到”用户桌面“文件夹下面。
![][10]

######第八步：

创建卸载程序。右击”应用程序文件夹“，点击”添加“，选择”文件“，然后将"C:\Windows\System32" 下面的”msiexec.exe“文件给添加进来，如果找不到，你可以直接搜。当然，你也可以再给msiexec.exe创建一个快捷方式命名为”UnInstall“。

![][11]
![][12]

命名了快捷方式之后，将Setup属性（点击解决方案里面的setup弹出属性）ProductCode拷贝到Uninstall属性的Arguments里面：
同时在前头加上 ”/X “，注意：x后面有一个空格。
![][13]
![][14]

######第九步：

改变桌面快捷方式的Logo。自带的logo实在是太挫了，你可以去网上下载一个图片，然后转换为.ico格式。
下图中”应用程序文件夹“是指logo存放的位置，一般存在在该处就行了。
![][15]

######第十步：

附加数据库。我们现在添加一个类，用于编写附加数据库代码。
右击”解决方案“，点击”添加“，选择”新建项目“，然后新建一个C#类库，并命名为”InstallDB“。
最后，将”class1.cs“删掉。
![][16]
![][17]
![][18]

######第十一步：

新建一个类，用于写数据库附加到 数据库管理系统中的 代码。右击刚新建的那个”InstallDB“，点击”添加“，选择”新建项“。
然后在弹出的界面中，选择”安装程序类“，并命名为”InstallDB.cs“。
![][19]
![][20]

######第十二步：

由于附加数据库需要用户输入本机数据库的一些信息，比如：服务器名称，数据库管理员名称和密码等等。这时候，我们可以在安装过程中弹出一个等待用户输入的框：
右击”Setup“，点击”视图“，选择”用户界面“。弹出如下第二个界面，再右击”启动“，点击“添加对话框”，选择”文本框（A）“，同时将其拖放到”欢迎使用“下面，如下第三张图。
最后，根据自己的需要填写”文本框（A）“的属性，可以参考第三张图。
注：里面定义的变量，主要是为了下面的附加代码而定义的。

![][21]
![][22]
![][23]

######第十三步：

添加 附加数据库的 主输出。右击”setup“，选择”视图“，”自定义操作“。
然后，右击”安装“，选择”应用程序文件夹“，选择安装程序类”InstallDB“，还是选择”主输出“，确定。
接着，在CostomActionData里面复制粘贴如下：

```sql
/dbname=[DBNAME] /server=[SERVER] /user=[USER] /pwd=[PWD] /targetdir="[TARGETDIR]\"  
```

![][24]
![][25]
![][26]

######第十四步：

在InstallDB.cs中编写附加数据库代码。先点击”单机此处切换到代码视图“。
然后添加 几个 命名空间。
当然，要使用MessageBox()函数，需要添加using System.Windows.Forms;之外，同时需要添加System.Windows.Forms引用（具体操作：右击InstallDB，选择添加引用，选择.NET）
![][28]
![][29]

当然，最后写好的代码，如下所示：

```Csharp
using System;  
using System.Collections;  
using System.Collections.Generic;  
using System.ComponentModel;  
using System.Configuration.Install;  
using System.Linq;  
using System.Data.SqlClient;  
using System.Windows.Forms;  
using System.IO;  
using System.Security.AccessControl;  
  
namespace InstallDB  
{  
    [RunInstaller(true)]  
    public partial class InstallDB : System.Configuration.Install.Installer  
    {  
        public InstallDB()  
        {  
            InitializeComponent();  
        }  
        //创建数据库  
        private void CreateDataBase(string strSql, string DataName, string strMdf, string strLdf, string path)  
        {  
            SqlConnection myConn = new SqlConnection(strSql);  
            String str = null;  
            try  
            {  
                str = @" EXEC sp_attach_db @dbname='" + DataName + "',@filename1='" + strMdf + "',@filename2='" + strLdf + "'";  
                SqlCommand myCommand = new SqlCommand(str, myConn);  
                myConn.Open();  
                myCommand.ExecuteNonQuery();  
                MessageBox.Show("数据库安装成功！点击确定继续");//需Using System.Windows.Forms  
            }  
            catch (Exception e)  
            {  
                MessageBox.Show("数据库安装失败！" + e.Message + "\n\n" + "您可以手动附加数据");  
                System.Diagnostics.Process.Start(path);//打开安装目录  
            }  
            finally  
            {  
                myConn.Close();  
            }  
        }  
        //权限管理  
        private static void SetFullControl(string path)  
        {  
            FileInfo info = new FileInfo(path);  
            FileSecurity fs = info.GetAccessControl();  
            fs.AddAccessRule(new FileSystemAccessRule("Everyone", FileSystemRights.FullControl, AccessControlType.Allow));  
            info.SetAccessControl(fs);  
        }  
        //重载的Install函数  
        public override void Install(System.Collections.IDictionary stateSaver)  
        {  
            string server = this.Context.Parameters["server"];//服务器名称  
            string uid = this.Context.Parameters["user"];//SQlServer用户名  
            string pwd = this.Context.Parameters["pwd"];//密码  
            string path = this.Context.Parameters["targetdir"];//安装目录  
            string ch = path.Substring(path.Length - 1, 1);  
            if (ch == @"\")   //对路径进行处理，判断末尾是否有'\'  
                path = path.Substring(0, path.Length - 1);//有则删掉  
            
            string strSql = "server=" + server + ";uid=" + uid + ";pwd=" + pwd + ";database=master";//连接数据库字符串  
            string DataName = @"StuBoardDB";//数据库名  
            string strMdf = path + @"XSJSGLXT.mdf";//MDF文件路径，这里需注意文件名要与刚添加的数据库文件名一样！  
            SetFullControl(strMdf);    //设置权限为EveryOne  
            string strLdf = path + @"XSJSGLXT_log.ldf";//LDF文件路径  
            SetFullControl(strLdf);    //设置权限为EveryOne  
            base.Install(stateSaver);  
            this.CreateDataBase(strSql, DataName, strMdf, strLdf, path);//开始创建数据库  
        }  
    }  
}  

```

可能你看到代码比较多，其实这些你都可以重用，你只需要改其中的一点点就行了。如下图（黑框里面的东西）：你懂得。

![][30]

######第十五步：
好了，最后，生成安装包。
![][27]



[1]: http://ocp77h2r6.bkt.clouddn.com/packed1.png
[2]: http://ocp77h2r6.bkt.clouddn.com/packed2.png
[3]: http://ocp77h2r6.bkt.clouddn.com/packed3.png
[4]: http://ocp77h2r6.bkt.clouddn.com/packed4.png
[5]: http://ocp77h2r6.bkt.clouddn.com/packed5.png
[6]: http://ocp77h2r6.bkt.clouddn.com/packed6.png
[7]: http://ocp77h2r6.bkt.clouddn.com/packed7.png
[8]: http://ocp77h2r6.bkt.clouddn.com/packed8.png
[9]: http://ocp77h2r6.bkt.clouddn.com/packed9.png
[10]: http://ocp77h2r6.bkt.clouddn.com/packed10.png
[11]: http://ocp77h2r6.bkt.clouddn.com/packed11.png
[12]: http://ocp77h2r6.bkt.clouddn.com/packed12.png
[13]: http://ocp77h2r6.bkt.clouddn.com/packed13.png
[14]: http://ocp77h2r6.bkt.clouddn.com/packed14.png
[15]: http://ocp77h2r6.bkt.clouddn.com/packed15.png
[16]: http://ocp77h2r6.bkt.clouddn.com/packed16.png
[17]: http://ocp77h2r6.bkt.clouddn.com/packed17.png
[18]: http://ocp77h2r6.bkt.clouddn.com/packed18.png
[19]: http://ocp77h2r6.bkt.clouddn.com/packed19.png
[20]: http://ocp77h2r6.bkt.clouddn.com/packed20.png
[21]: http://ocp77h2r6.bkt.clouddn.com/packed21.png
[22]: http://ocp77h2r6.bkt.clouddn.com/packed22.png
[23]: http://ocp77h2r6.bkt.clouddn.com/packed23.png
[24]: http://ocp77h2r6.bkt.clouddn.com/packed24.png
[25]: http://ocp77h2r6.bkt.clouddn.com/packed25.png
[26]: http://ocp77h2r6.bkt.clouddn.com/packed26.png
[27]: http://ocp77h2r6.bkt.clouddn.com/packed27.png
[28]: http://ocp77h2r6.bkt.clouddn.com/packed28.png
[29]: http://ocp77h2r6.bkt.clouddn.com/packed29.png
[30]: http://ocp77h2r6.bkt.clouddn.com/packed30.png
