###前端为什么使用JSON作为数据传输
1 这个问题其实是在面试的过程中被问到的，回答的时候基本都答到点子上去了。现在做一个总结，前端使用JSON作为数据传输肯定是因为JSON数据格式本身优于其他数据格式的特点。
> 1 JSON数据格式在前后端都能够被很好的支持。(面试的时候面试官追问，很好的支持是什么意思呢？当时没有能很好的回答出来，只是说了前后端分离采用的架构中也都是使用JSON作为数据传输)。现在想想其实应该是后台有提供相应的jar包处理JSON格式，而前端JSON则是被原声JavaScript支持，不需要任何其他的辅助就可以解析。  
2 JSON数据格式的体量相对小，其实这个很好理解。对比其他的数据格式，比如说xml或者html，都需要其他额外的标签，而JSON不需要，直接就是数据本身，所以说使用JSON作为传输体量相对小。  
3 第三点理由其实在第一点有提及到一点，主要是因为JavaScript对于JSON的支持，如果使用xml格式，还需要额外进行解析。  

#####使用JavaScript对xml数据格式进行解析
> 其实对于xml自己并没有用过JavaScript在前台处理过，后台倒是有处理过。所以面试的时候能答出第三点也是挺幸运的，现在将前端使用JavaScript对于xml数据格式的解析做了个demo，记录下。

```XML
	//xml原始数据
	var dataSource = "<?xml version='1.0' encoding='UTF-8'?>
	<bookstore>
		<book id='NO1'>
			<title>An Introduction to XML</title>
			<author>Chunbin</author>
			<year>2010</year>
			<price>98.0</price>
		</book>
		<book id='NO2'>
			<title>The Preformance of DataBase</title>
			<author>John</author>
			<year>1996</year>
			<price discount='7' data='8'>56.0</price>
		</book>
	</bookstore>";
```

```JavaScript
	//创建xmlDoc对象
	function createXML(xmlString){
		var xmlDoc = null;
		var domParser = null;
		if(window.DOMParser){	//FF, webkit, IE9+
			try{
				domParser = new DOMParser();
				xmlDoc = domParser.parseFromString(xmlString, "text/xml");
			} catch (e) {
			}
		} else if(!window.DOMParser && window.ActiveXObject){
			var xmlDomVersions = ['MSXML2.DOMDocument', 'Microsoft.XMLDOM'];
			for(var i = 0; i < xmlDomVersions.length; ++i){
				try{
					xmlDoc = new ActiveXObject(xmlDomVersions[i]);
					xmlDoc.async = false;
					xmlDoc.load(xmlString);
				} catch(e) {
					continue;
				}
			}
		} else {
			return null;
		}
		return xmlDoc;
	}
```

```JavaScript
	//解析过程
	function loadXML(dataSource){
		var xmlDoc = createXML(dataSource);
		if(xmlDoc){
			var books = xmlDoc.getElementsByTagName("book");
			var book = xmlDoc.getElementById("NO2");
			if(books){
				for(var i = 0; i < books.length; ++i){
					var title = books[i].getElementsByTagName("title")[0].firstChild.nodeValue;
					var author = books[i].getElementsByTagName("author")[0].innerHTML;
					var year = books[i].getElementsByTagName("year")[0].innerHTML;
					var price = Number(books[i].getElementsByTagName("price")[0].innerHTML);
					console.log(title + " : " + author + " : " + year + " : " + price);
				}
			}
			if(book){
				var attrs = book.attributes;
				var attrs2 = book.getElementsByTagName("price")[0].attributes;
				for(var i = 0; i < attrs.length; ++i){
					var attr = attrs[i];
					var attr_name = attr.name;
					var attr_value = attr.value;
					console.log(attr_name + " : " + attr_value);
				}
				for(var i = 0; i < attrs2.length; ++i){
					var attr = attrs2[i];
					var attr_name = attr.name;
					var attr_value = attr.value;
					console.log(attr_name + " : " + attr_value);
				}
			}
		}
	}
```
