> 前几天有同学被问到对于手机端的跑马灯抽奖用JS是怎么实现的，刚刚面完阿里想想是时候继续更新下github。下面是一个初版，没能封装成插件，下一个版本在弄成一个插件。

```HTML
<table>
	<tr>
		<td><img id="img1" src="img/1.jpg"></td>
		<td><img id="img2" src="img/2.jpg"></td>
		<td><img id="img3" src="img/3.jpg"></td>
	</tr>
	<tr>
		<td><img id="img8" src="img/4.jpg"></td>
		<td id="control" onclick="start()" style="text-align: center;font-size: 72px;color:red;background-color: yellow;opacity:1;">
		  START
		</td>
		<td><img id="img4" src="img/5.jpg"></td>
	</tr>
	<tr>
		<td><img id="img7" src="img/6.jpg"></td>
		<td><img id="img6" src="img/7.jpg"></td>
		<td><img id="img5" src="img/8.jpg"></td>
	</tr>
</table>
```

> 下面是JS代码

```JavaScript
	var seed = null;

	function start(){
		var imgCount = 1;
		var status = $("#control");

		if( status.html().trim() == "START" ){
			status.html("END");
			seed = setInterval(function(){
				if( imgCount > 8 ){
					imgCount = 1;
				}

				$("[id^=img]").each(function(index, value){
					var imgIndex = $(value).attr("id").substring(3,4);

					if(imgIndex == imgCount){
						$("#img" + imgIndex).css("opacity", 1);
					} else {
						$("#img" + imgIndex).css("opacity", 0.5);
					}
				});
				++imgCount;
			});
		} else {
			status.html("START");
			clearInterval( seed );
		}
	}
```

> 从代码中可以看到顺序是通过图片的id进行修改的，这就规定死了图片先从做到右的顺序进行的。这两天想好怎么封装成插件之后，给用户提供一个规定图片
播放的顺序参数。
