---
title: java 用Socket获取网页内容
id: 1168
categories:
  - Java
date: 2013-09-19 10:56:43
tags:
---

TestSocket.java

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.HttpURLConnection;
import java.net.InetAddress;
import java.net.Socket;
import java.net.URL;

/**
*
* @author Dao
*/
public class TestSocket
{

    public TestSocket()
    {
    }

    public static void main(String args[])
    {
        //你想获取代码的网站
        String strServer = www.baidu.com;
        //起始页面，/为根页
        String strPage = "/";

        try
        {
            //设置端口，通常http端口不就是80罗，你在地址栏上没输就是这个值
            int port = 80;
            //用域名反向获得IP地址
            InetAddress addr = InetAddress.getByName(strServer);

            //建立一个Socket
            Socket socket = new Socket(addr, port);

            //发送命令,无非就是在Socket发送流的基础上加多一些握手信息，详情请了解HTTP协议
            BufferedWriter wr = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream(), "UTF-8"));
            wr.write("GET " + strPage + " HTTP/1.0rn");
            wr.write("HOST:" + strServer + "rn");
            wr.write("Accept:*/*rn");
            wr.write("rn");
            wr.flush();

            //接收Socket返回的结果,并打印出来
            BufferedReader rd = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            String line;
            while ((line = rd.readLine()) != null)
            {
                System.out.println(line);
            }
            wr.close();
            rd.close();
        }
        catch (Exception e)
        {
            e.printStackTrace();
        }
    }
}

/**from http://gavin-chen.javaeye.com/blog/240422*/