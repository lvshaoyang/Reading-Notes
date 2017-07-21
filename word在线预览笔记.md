2017/7/21 11:24:22 

	根据公司需求要做一个word在线预览，但是word在线预览的插件是没有的，以前到是做过一个pdf预览，使用的是pdf.js插件。
	然后思路就是在doc所在目录下，根据openoffice将doc转为pdf,只改一下后缀，路径和文件名不变，前台将原先的下载链接改为pdf.js所要求改的链接即可。

## 步骤： ##
	
	1、在win7系统上安装openoffice软件，下载地址：
	官网即可：http://www.openoffice.org/版本为4.1.3，直接下一步式的安装。服务是在C:\Program Files (x86)\OpenOffice 4\program这个路径下，在DOS窗口中，
	
	开启服务，命令：
	soffice -headless -accept="socket,host=127.0.0.1,port=8100;urp;" -nofirststartwizard
	
	杀死服务，命令：
	taskkill /f /t /im soffice.bin
	
	2、使用JODConvert包，可以从官网上下载：https://sourceforge.net/projects/jodconverter/推荐使用的是2.2.2版本
	
	或者https://code.google.com/archive/p/jodconverter/
	
	github上下载的源码：
	https://github.com/acmersch/jodconverter4.0这个是使用了2.2但是他把3.0版本上的代码加了上去，可以使用代码来启动和关闭服务。
	
	可以使用3.0-Beta-4版，因为2.2版本中没有开启和关闭服务的代码。
	
	然后就是核心代码了：
	`package com.eplugger.system.util;
	import java.io.File;
	import java.io.FileInputStream;
	import java.io.FileNotFoundException;
	import java.io.IOException;
	import java.util.Properties;
	import java.util.Scanner;
	
	import javax.servlet.ServletException;
	import javax.servlet.http.HttpServlet;
	import javax.servlet.http.HttpServletRequest;
	import javax.servlet.http.HttpServletResponse;
	
	import org.artofsolving.jodconverter.OfficeDocumentConverter;
	import org.artofsolving.jodconverter.office.DefaultOfficeManagerConfiguration;
	import org.artofsolving.jodconverter.office.OfficeManager;
	
	public class OfficeWord2Pdf  extends HttpServlet{
	/**
	 * 
	 */
	private static final long serialVersionUID = 1L;

	private static final String RUL_PATH = Thread.currentThread()
			.getContextClassLoader().getResource("").getPath()
			.replace("%20", " ")
			+ "url.properties";
     
    private static OfficeManager officeManager;
     
    static String OFFICE_HOME = "";
    private static int[] port = { 8100 }; //端口号
    public static void startService() {
    	Properties prop = new Properties();
		FileInputStream fis = null;
		try {
			fis = new FileInputStream(RUL_PATH);
			prop.load(fis);
			fis.close();
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
		

		String OFFICE_HOME = prop.getProperty("OFFICE_HOME");
		if (OFFICE_HOME == null){
			return;
		}
		if (OFFICE_HOME.charAt(OFFICE_HOME.length() - 1) != '\\') {
			OFFICE_HOME += "\\";
		}
    	
    	
        DefaultOfficeManagerConfiguration configuration = new DefaultOfficeManagerConfiguration();
        try {
            // 准备启动服务
            configuration.setOfficeHome(OFFICE_HOME);// 设置OpenOffice.org安装目录
            // 设置转换端口，默认为8100
            configuration.setPortNumbers(port);
            // 设置任务执行超时为30秒
            configuration.setTaskExecutionTimeout(1000 * 30L);
            // 设置任务队列超时为24小时
            configuration.setTaskQueueTimeout(1000 * 60 * 60 * 24L);
 
            officeManager = configuration.buildOfficeManager();
            System.out.println(officeManager);
             
            officeManager.start(); // 启动服务
            System.out.println("office转换服务启动成功!");
        } catch (Exception ce) {
            System.out.println("office转换服务启动失败!详细信息:" + ce);
        }
    }
     
    public static void stopService() {
        System.out.println("关闭office转换服务....");
        if (officeManager != null) {
            officeManager.stop();
        }
        System.out.println("关闭office转换成功!");
    }
     
    public static void convert2PDF(String inputFile, String pdfFile, String extend) {
        startService();
        System.out.println("进行文档转换转换:" + inputFile + " --> " + pdfFile);
        OfficeDocumentConverter converter = new OfficeDocumentConverter(
                officeManager);
        try {
        	File input = new File(inputFile);
        	File pdf = new File(pdfFile);
        	
            converter.convert(input, pdf);
        } catch (Exception e) {
            System.out.println(e.getMessage());
            shutdownProcess();//防止服务器如果8100已打开导致的阻塞
        }
        
        stopService();
        System.out.println("进行文档转换转换---- 结束----");
    }
    
    public static void main(String[] args){
        convert2PDF("D:/poi/4.doc","D:/poi/doc/5.pdf","doc");
    }
    
   @Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		doPost(request, response);
	}
   
   @Override
	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		String rootpath = request.getParameter("rootpath");
		if(rootpath!=null && !"".equals(rootpath.trim())){
			convertFile(rootpath.trim());
			request.getSession().setAttribute("mess", "1");
		 }else{
			 request.getSession().setAttribute("mess", "0");
		 }
		
		
		response.sendRedirect("jsp/convert.jsp");
	}
   
   public static boolean convertFile(String rootpath) throws FileNotFoundException, IOException {
	   File file = new File(rootpath);
       if (!file.isDirectory()) {
               System.out.println("文件");
               System.out.println("path=" + file.getPath());
               System.out.println("absolutepath=" + file.getAbsolutePath());
               System.out.println("name=" + file.getName());

       } else if (file.isDirectory()) {
               System.out.println("文件夹");
               String directoryName = file.getAbsolutePath();//文件夹绝对路径
               System.out.println("文件夹绝对路径："+directoryName);
               String[] filelist = file.list();
               
               for (int i = 0; i < filelist.length; i++) {
               	String tempFileName = filelist[i];
               	
               	File tempFile= new File(directoryName+File.separator+tempFileName);
               	
               	String suffix = "";
               	String docName =  "";
               	
               	if(!tempFile.isDirectory()){
               		suffix = tempFileName.substring(tempFileName.lastIndexOf(".")+1,tempFileName.length());//获取后缀名
               		docName =tempFileName.substring(0,tempFileName.lastIndexOf("."));//获取文件名
               		
               		if("doc".equals(suffix)){
               			File pdf = new File(directoryName+File.separator+docName+".pdf");
               			System.out.println(docName+".pdf是否存在："+pdf.exists());
               			if(!pdf.exists()){
               				convert2PDF(directoryName+File.separator+tempFileName, directoryName+File.separator+docName+".pdf", "doc");
               			}
               		}
               	}else{
               		convertFile(directoryName+File.separator+tempFileName);
               	}		
               }

       }
	   return true;
   }
   
   @Override
	public void destroy() {
		   stopService();
		super.destroy();
	}
   
   public static void shutdownProcess(){
	   try {
			Process process = Runtime.getRuntime().exec("tasklist");
			Scanner in = new Scanner(process.getInputStream());
			
			 int count = 0;
	         while (in.hasNextLine()) {
	             count++;
	             String temp = in.nextLine();
	
	             if (temp.contains("soffice.bin")) {
	            	// Process p = Runtime.getRuntime().exec("tskill " + pid);
					Runtime.getRuntime().exec("taskkill /F /IM soffice.bin");
				} 
              }
         }catch (IOException e) {
				e.printStackTrace();
		} 
   }
}

`
其中shutdownProcess()方法可以调用dos命令强制关闭服务或者进程。
	
# 使用pdf.js插件预览 #
	
	注意：IE8不兼容，IE9及以上可以。基于HTML5的插件。
	参考网址：
	https://github.com/mozilla/pdf.js
	
	主要代码：
	`	/*  通过此方法来为附件添加链接 ，通过pdf.js插件来展示Pdf文件。*/
	$(function(){
		$(".showpdf").each(function(i){
			var id = $(this).attr("myAttr");
			$(this).attr("href","javascript:void(0)");
			//此处为调用Pdf.js插件中的viewr.html页面，需要将文档以流的形式传来
			var url = "../../pdfJs/generic/web/viewer.html?file=" + encodeURIComponent("download.download?id=" + id);
			$(this).click(function(){
				window.showModalDialog(url,"1","dialogWidth:1024px;dialogHeight:800px;");
				return false;
			});
		})
	})
	
	document.onmousedown = function () {
        if (window.event) {
            if (event.button == 2) {
          	  event.returnValue=false;
                alert('请不要点击鼠标右键！'); 
                return;
            }
        }
    }`
	file参数的请求为向action或者servlet请求了一个输入流。
	将Pdfjs放到根目录下即可，这个还要用到node.js等什么的来构建，比较麻烦可以云github上下载别人弄好的。
	
	另外：file传参时，可以解析第一个问号（html后的），但是我请求时要传id这个时候的问号就解析不了了，用pdfjs中的system.js中的encodeURIComponent()方法来对字符串进行编译。对？相当于转义了。