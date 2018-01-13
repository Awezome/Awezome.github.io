---
title: Qt 实现的拷贝文件夹的函数
id: 920
categories:
  - Other
tags:
---

#include <qdir>
02	#include <qfileInfoList>
03
04	/**
05	  qCopyDirectory -- 拷贝目录
06	  fromDir : 源目录
07	  toDir   : 目标目录
08	  bCoverIfFileExists : ture:同名时覆盖  false:同名时返回false,终止拷贝
09	  返回: ture拷贝成功 false:拷贝未完成
10	*/
11	bool qCopyDirectory(const QDir& fromDir, const QDir& toDir, bool bCoverIfFileExists)
12	{
13	    QDir formDir_ = fromDir;
14	    QDir toDir_ = toDir;
15
16	    if(!toDir_.exists())
17	    {
18	        if(!toDir_.mkdir(toDir.absolutePath()))
19	            return false;
20	    }
21
22	    QFileInfoList fileInfoList = formDir_.entryInfoList();
23	    foreach(QFileInfo fileInfo, fileInfoList)
24	    {
25	        if(fileInfo.fileName() == "." || fileInfo.fileName() == "..")
26	            continue;
27
28	        //拷贝子目录
29	        if(fileInfo.isDir())
30	        {
31	            //递归调用拷贝
32	            if(!qCopyDirectory(fileInfo.filePath(), toDir_.filePath(fileInfo.fileName())))
33	                return false;
34	        }
35	        //拷贝子文件
36	        else
37	        {
38	            if(bCoverIfFileExists && toDir_.exists(fileInfo.fileName()))
39	            {
40	                toDir_.remove(fileInfo.fileName());
41	            }
42	            if(!QFile::copy(fileInfo.filePath(), toDir_.filePath(fileInfo.fileName())))
43	            {
44	                return false;
45	            }
46	        }
47	    }
48	    return true;
49	}