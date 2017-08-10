## 关于HTML中对象的理解 ##

参考网址：
	http://www.w3school.com.cn/jsref/dom_obj_event.asp

Browser对象包括：
	
	Window:表示浏览器中打开的窗口。
		如果文档包含框架（frame和iframe标签），浏览器会为HTML文档创建一个window对象，并为每个框架创建一个额外的windon对象。
	Navigator
	Screen
	History
	Location

HTML DOM对象：

	1、DOM Document:
		
		每个载入浏览器HTML文档都会成为Document对象。
		
		Document对象使我们可以从脚本中对HTML页面中的所有元素进行访问。
		
		提示：Document 对象是 Window 对象的一部分，可通过 window.document 属性对其进行访问
	
	2、DOM Event：
		
		Event对象代表事件的状态。
		
	