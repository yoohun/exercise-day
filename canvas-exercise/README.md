##该文档会记录学习canvas过程包括笔记以及练习的思路  

###基本笔记  
####canvas与svg之间的区别  
**1.**canvas可以看做是一个画布，可以使用js在上面进行绘制，并且绘制出来的是标量图，但是放大了之后会失真，可以用canvas来画饼状图或柱状图等；而svg是xml技术描述二维图形的语言,是通过标签来来实现，所绘制出来的图片是矢量图， 放大之后并不会失真。   

**2.**canvas可以引入jpg或png这类格式的图片,在实际开发中，大型的网络游戏都是用canvas画布做出来的;而svg因为绘制的是矢量图，所以不能引入图片，但是可以用svg来做动态的图标，实际开发中，svg用来做地图，  
 
**3.**canvas绘制的图形不能被引擎抓取,如我们要让canvas里面的一个图片跟随鼠标事件：canvas.οnmοuseοver=function(){}。而svg里面的图形可以被引擎抓取，支持事件的绑定。  

**4.**canvas绘制的图形不能修改，但是svg绘制的图形可以，同一图形的一个canvas标记中移除元素，需要擦掉重新绘制；而svg很容易编辑，只要从其描述中移除元素即可。  


####canvas的基本知识  
#####canvas的基本语法    

	<canvas id="canvasMain" width="800" height="600" >
    	您的浏览器不支持canvas
	</canvas>  

当没有设置canvas的宽高的时候，canvas会初始化宽度为300px和高度为150px；当浏览器不支持canvas标签的时候，会显示其中的文字。  

在canvas坐标体系中，以左上角为坐标原点，向右为x轴正方向，向下为y轴正方向，如下图： 
![](https://images2015.cnblogs.com/blog/1164364/201707/1164364-20170706150320862-1695036730.png)  

绘制canvas需要在js中绘制，而且需要先获取到canvas才能继续绘制    

	var canvas = document.getElementById('canvasMain')  
	var ctx = canvas.getContext("2d");	  

进行绘制需要获取canvas的上下文环境context，之后调用API进行图像绘制  


#####描边和填充  
在canvas中描边和填充主要是用这两个方法： fill() 和 stroke() 一般使用canvas的API绘制完矩形或者图形后，要记得填充或者描边，否则绘制的图案无法显示出来！！！！ fillStyle，strokeStyle属性用于设置或者返回填充绘画或笔触的颜色、渐变或模式（记住！！！不是方法是属性！！）

####绘制矩形 
在canvas绘制矩形的方法有：rect() fillRect()  strokeRect()   clearRect()  参数依次为：矩形x坐标、y坐标、宽度、高度  
**注意！！！fillRect()  strokeRect()都是只能绘制带边框的矩形或者填充颜色的矩形，如果想要有边框加填充的矩形的话，fillRect()  strokeRect()内的参数需要一致**  

	var canvasDiv = document.getElementById('canvasDiv')
	var ctx = canvasDiv.getContext("2d")
	ctx.fillStyle='red'
	ctx.fillRect(0,0,100,100) //绘制一个填充了红色的矩形
	
	ctx.fillStyle='green'  
	ctx.strokeStyle='pink' 
	ctx.lineWidth=10  //设置边框的粗细
	ctx.strokeRect(100,100,50,50)  //绘制了一个边框为粉色，且边框粗10px的矩形
	ctx.fillRect(200,200,50,50)   //绘制了一个填充了绿色的矩形
	ctx.clearRect(200,200,50,50)  //清除了x方向上200px，y方向上200px的大小为50px的矩形  


####绘制路径 
在canvas中，绘制路径的方法如下：  

fill()	填充当前绘图（路径）  
stroke()	绘制已定义的路径  

beginPath()	起始一条路径，或重置当前路径  
moveTo()	把路径移动到画布中的指定点，不创建线条  
closePath()	创建从当前点回到起始点的路径 即把最后一个点和第一个点相连  
lineTo()	添加一个新点，然后在画布中创建从该点到最后指定点的线条   

	  ctx.beginPath()  //开始绘制路径
	  ctx.lineWidth=5; //设置路径的粗细
	  ctx.moveTo(400,200)  //指定初始点
	  ctx.lineTo(400,500)  //指定第二个点
	  ctx.lineTo(450,200)  //指定第三个点
	  ctx.strokeStyle='yellow'   //指定线段的颜色
	  ctx.lineCap='round'   //指定线段的末端的样式
	  ctx.lineJoin='miter'  //设置线段拐角的类型
	  ctx.miterLimit=100  //设置交汇处内角和外角之间的距离   
	  //ctx.closePath()   //如果设置了closePath的话，会将最后一个点和第一个点连接起来
	  ctx.stroke()   

  

**注意！！在绘制线段的时候，首先要先开始绘制beginPath（如果新画的线段不先begin的话，会导致新的线段和原来的线段重合），moveTo方法是指定起始点的位置，lineTo是指定线段的结束点的位置，如果需要对线段进行一些修饰（线段的粗细[lineWidth]，线段的颜色[strokeStyle]，线段的结束端点的样式[lineCap],两线段相交时，所创建的拐角的类型[lineJoin],线段相交时，交汇处内角和外角之间的距离[miterLimit]）**      


clip()	从原始画布剪切任意形状和尺寸的区域,并只有被剪切区域内的绘制部分是可见的，超过该区域的内容不可见。*使用 clip() 方法前通过使用 save() 方法对当前画布区域进行保存，并在以后的任意时间对其进行恢复（通过 restore() 方法）。*    

	  ctx.strokeStyle='purple'
	  ctx.rect(50, 20, 200, 120)
	  ctx.stroke()    //绘制紫色边框的矩形
	  ctx.clip()	//紫色边框处即剪切的矩形
	  ctx.fillStyle='black'
	  ctx.fillRect(0, 0, 150, 100) //绘制矩形


quadraticCurveTo()	创建二次贝塞尔曲线  
bezierCurveTo()	创建三次方贝塞尔曲线         

	ctx.beginPath()   
	ctx.moveTo(20, 20)   //设置起始点	
	ctx.quadraticCurveTo(100,90,400,20)   指定控制点和结束点
	ctx.stroke();   //绘制二次贝塞尔曲线    

**注意！！！二次贝塞尔曲线需要两个点。第一个点是用于二次贝塞尔计算中的控制点，第二个点是曲线的结束点。曲线的开始点是当前路径中最后一个点。如果路径不存在，那么请使用 beginPath() 和 moveTo() 方法来定义开始点。  如图：  
![](https://www.w3school.com.cn/i/quadraticcurve.gif)**  

**- 开始点：moveTo(20,20)  
- 控制点：quadraticCurveTo(20,100,200,20)  (前面的两个坐标) 
- 结束点：quadraticCurveTo(20,100,200,20) （后面的两个坐标）**  

	ctx.beginPath()
	ctx.moveTo(100,100)   //设置起始点
	ctx.bezierCurveTo(120,200,180,200,240,100)    //设置控制点1，控制点2，以及结束点
	// ctx.closePath()  //如果closePath了即把起始点和结束点相连
	ctx.stroke()    //绘制三次贝塞尔曲线   

**三次贝塞尔曲线需要三个点。前两个点是用于三次贝塞尔计算中的控制点，第三个点是曲线的结束点。曲线的开始点是当前路径中最后一个点。如果路径不存在，那么请使用 beginPath() 和 moveTo() 方法来定义开始点。
如图：![](https://www.w3school.com.cn/i/beziercurve.gif)**  

**- 开始点：moveTo(20,20)  
- 控制点 1：bezierCurveTo(20,100,200,100,200,20)  （前两个坐标）  
- 控制点 2：bezierCurveTo(20,100,200,100,200,20)	 （中间两个坐标）  
- 结束点：bezierCurveTo(20,100,200,100,200,20)    （最后两个坐标）**


arc()	创建弧/曲线（用于创建圆形或部分圆）  
arcTo()	创建两切线之间的弧/曲线

	ctx.beginPath()
	// ctx.arc(200,200,30,0,2*Math.PI)   //创建一个坐标（200,200），半径30，一整圈的圆
	// ctx.arc(300,300,50,0,0.5*Math.PI)  //创建一个坐标(300,300),半径50，四分之一个圆
	// ctx.arc(600,600,100,0,1*Math.PI)  //创建一个坐标（600,600），半径100，半个圆
	ctx.arc(300,300,50,0,1.5*Math.PI)  //创建一个坐标（300,300），半径50，一个半圆
	ctx.stroke()

**context.arc(x,y,r,sAngle,eAngle,counterclockwise);  
x是圆的x坐标，y是圆的y坐标。  
sAngle是起始角度，以弧度计，eAngle是结束角度，以弧度计。  
counterclockwise，可选。规定应该逆时针还是顺时针绘图。False = 顺时针，true = 逆时针**   

	ctx.beginPath()
	ctx.moveTo(100,100)  
	ctx.lineTo(200,100)
	ctx.arcTo(250,100,250,150,50)  //绘制弧
	ctx.lineTo(250,200)
	ctx.stroke()

**context.arcTo(x1,y1,x2,y2,r);  
x1是弧的起点的 x 坐标， y1是弧的起点的 y 坐标；x2是弧的终点的 x 坐标，y2是弧的终点的 y 坐标；r是弧的半径**


  
isPointInPath()	如果指定的点位于当前路径中，则返回 true，否则返回 false     
*context.isPointInPath(x,y);  
x是测试的点的x坐标，y是测试点的y坐标*



####绘制线条
线条的样式如下：（属性）   
lineCap	设置或返回线条的结束端点样式   round square butt  
lineJoin	设置或返回两条线相交时，所创建的拐角类型  bevel  round   miter  
lineWidth	设置或返回当前的线条宽度  (数字)
miterLimit	设置或返回最大斜接长度    (数字) **只有当lineJoin为miter的时候才有效果**



####绘制文本  
font	设置或返回文本内容的当前字体属性
textAlign	设置或返回文本内容的当前对齐方式
textBaseline	设置或返回在绘制文本时使用的当前文本基线
fillText()	在画布上绘制“被填充的”文本
strokeText()	在画布上绘制文本（无填充）
measureText()	返回包含指定文本宽度的对象  

**textAlign**  
start	默认。文本在指定的位置开始。  
end	文本在指定的位置结束。  
center	文本的中心被放置在指定的位置。   
left	文本左对齐。  
right	文本右对齐。  

	ctx.beginPath()
	ctx.moveTo(300,500)
	ctx.lineTo(300,660)
	ctx.stroke()         //绘制一条线作为参照
	ctx.font='40px 微软雅黑'   //设置文本的字体大小和字体样式
	ctx.textAlign='left'       //设置字体的对齐方式
	ctx.fillText('textalign=left',300,510)  //设置字体的内容以及字体的坐标
	ctx.textAlign='end'
	ctx.fillText('textalign=end',300,540)
	ctx.textAlign='start'
	ctx.fillText('textalign=start',300,580)
	ctx.textAlign='right'
	ctx.fillText('textalign=right',300,620)
	ctx.textAlign = 'center'
	ctx.fillText('textalign=center', 300, 660)

**textBaseline  当前文本基线**  
alphabetic	默认。文本基线是普通的字母基线。
top	文本基线是 em 方框的顶端。。
hanging	文本基线是悬挂基线。
middle	文本基线是 em 方框的正中。
ideographic	文本基线是表意基线。
bottom	文本基线是 em 方框的底端。

	  ctx.beginPath()
	  ctx.moveTo(100,500)
	  ctx.lineTo(800,500)
	  ctx.strokeStyle='red'
	  ctx.stroke()          //设置参照线
	  ctx.font='20px 微软雅黑'    //设置文字的样式
	  ctx.textBaseline='alphabetic'       //设置文字的文本基线
	  ctx.fillText('textBaseline=alphabetic',20,500)
	  ctx.textBaseline='top'
	  ctx.fillText('textBaseline=top',100,500)
	  ctx.textBaseline='hanging'
	  ctx.fillText('textBaseline=hanging',300,500)
	  ctx.textBaseline='middle'
	  ctx.fillText('textBaseline=middle',400,500)
	  ctx.textBaseline='ideographic'
	  ctx.fillText('textBaseline=ideographic',650,500)
	  ctx.textBaseline='bottom'
	  ctx.fillText('textBaseline=bottom',800,500)      


**context.strokeText(text,x,y,maxWidth);   
context.fillText(text,x,y,maxWidth);**  
text	规定在画布上输出的文本。  
x	开始绘制文本的 x 坐标位置（相对于画布）。  
y	开始绘制文本的 y 坐标位置（相对于画布）。  
maxWidth	可选。允许的最大文本宽度，以像素计。  

**context.measureText(text).width;   检查字体的宽度**   
text	要测量的文本。

	ctx.font='40px 微软雅黑'
	ctx.strokeStyle='blue'
	ctx.fillStyle='red'
	ctx.strokeText('吴世勋',100,400)  //以描边的形式写文本
	ctx.fillText('吴世勋',100,400)    //以填充的形式写文本
	console.log(ctx.measureText('吴世勋asasdas').width);  //返回文本的宽度   


####颜色、样式和阴影  
fillStyle	设置或返回用于填充绘画的颜色、渐变或模式  
strokeStyle	设置或返回用于笔触的颜色、渐变或模式  
shadowColor	设置或返回用于阴影的颜色  
shadowBlur	设置或返回用于阴影的模糊级别  
shadowOffsetX	设置或返回阴影距形状的水平距离  
shadowOffsetY	设置或返回阴影距形状的垂直距离  

createLinearGradient()	创建线性渐变（用在画布内容上）  
createPattern()	在指定的方向上重复指定的元素   
createRadialGradient()	创建放射状/环形的渐变（用在画布内容上）   
addColorStop()	规定渐变对象中的颜色和停止位置  

	  ctx.fillStyle='red'         //设置填充矩形的颜色
	  ctx.shadowBlur=50			  //设置阴影的模糊级别
	  ctx.shadowColor='black'	  //设置阴影的颜色
	  ctx.shadowOffsetX=100		  //设置阴影距离矩形的水平距离
	  ctx.shadowOffsetY=10		  //设置阴影距离矩形的垂直距离
	  ctx.fillRect(100,100,200,200)	 //绘制矩形 


**gradient.addColorStop(stop,color);规定 gradient 对象中的颜色和位置。**   
stop	介于 0.0 与 1.0 之间的值，表示渐变中开始与结束之间的位置。  
color	在结束位置显示的 CSS 颜色值  
*addColorStop() 方法与 createLinearGradient() 或 createRadialGradient() 一起使用。*


**context.createLinearGradient(x0,y0,x1,y1);  创建线性的渐变对象**
x0	渐变开始点的 x 坐标   
y0	渐变开始点的 y 坐标  
x1	渐变结束点的 x 坐标  
y1	渐变结束点的 y 坐标   
*可以配合使用addColorStop()方法指定渐变的颜色*
   
	var grd=ctx.createLinearGradient(0,0,170,0)   //创建线性的渐变对象
	grd.addColorStop(0, "red");					  //指定渐变对象的位置颜色
	grd.addColorStop(0.4, "yellow");			  
	grd.addColorStop(0.7, "pink");				
	grd.addColorStop(1, "white");
	ctx.fillStyle=grd							 //设置形状的填充效果
	ctx.fillRect(0,0,170,100)					 //创建有填充效果的矩形



**context.createRadialGradient(x0,y0,r0,x1,y1,r1);  创建放射状/圆形渐变对象**    
x0	渐变的开始圆的 x 坐标   
y0	渐变的开始圆的 y 坐标   
r0	开始圆的半径   
x1	渐变的结束圆的 x 坐标   
y1	渐变的结束圆的 y 坐标   
r1	结束圆的半径      

	var grd=ctx.createRadialGradient(100, 100, 10, 100, 100, 100)   //创建圆形渐变对象
	grd.addColorStop(0, "red");           //指定渐变对象的位置颜色
	grd.addColorStop(0.4, "yellow");
	grd.addColorStop(1, "pink");
	ctx.fillStyle=grd					  //把填充效果赋给形状
	ctx.fillRect(0,0,200,200)			  //创建有以上填充效果的矩形  



**context.createPattern(image,"repeat|repeat-x|repeat-y|no-repeat"); 方法在指定的方向内重复指定的元素**    
image	规定要使用的图片、画布或视频元素。  
repeat	默认。该模式在水平和垂直方向重复。   
repeat-x	该模式只在水平方向重复。   
repeat-y	该模式只在垂直方向重复。  
no-repeat	该模式只显示一次（不重复）。  



####转换    
scale()	缩放当前绘图至更大或更小  
rotate()	旋转当前绘图  
translate()	重新映射画布上的 (0,0) 位置   
transform()	替换绘图的当前转换矩阵   
setTransform()	将当前转换重置为单位矩阵。然后运行 transform()
   

**context.scale(scalewidth,scaleheight);缩放当前绘图，更大或更小。**  
scalewidth	缩放当前绘图的宽度 (1=100%, 0.5=50%, 2=200%, 依次类推)  
scaleheight	缩放当前绘图的高度 (1=100%, 0.5=50%, 2=200%, etc.)

*注释：如果您对绘图进行缩放，所有之后的绘图也会被缩放(包括形状之间的距离)。定位也会被缩放。如果您 scale(2,2)，那么绘图将定位于距离画布左上角两倍远的位置。而且！！！！缩放之后如果还需要缩放，那么该操作是基于上一个缩放操作的，即会在上一个操作的基础上进行缩放*        

	ctx.strokeRect(0,0,100,100)       	//第一个矩形
	ctx.strokeRect(10,0,100,100)		//第二个矩形
	ctx.scale(2, 4)                   //将画布放大
	ctx.strokeRect(10,0,100,100)      //放大之后再进行绘制，会发现，距离第一个矩形的距离是20，即意味着画布的放大缩小，伴随着图形之间的距离也有放大缩小相同的倍数  


**context.rotate(angle);旋转当前的绘图。**    
angle	         旋转角度，以弧度计。  
如需将角度转换为弧度，请使用 degrees*Math.PI/180 公式进行计算。   
举例：如需旋转 5 度，可规定下面的公式：5*Math.PI/180。  
   

	ctx.strokeRect(0, 0,100,100)
	ctx.strokeRect(100,100,100,100)
	ctx.strokeRect(200,200,100,100)
	ctx.strokeRect(300,300,100,100)
	ctx.rotate(20 * Math.PI / 180)   //设置旋转的度数
	ctx.strokeRect(0, 0,100,100)
	ctx.strokeRect(100,100,100,100)
	ctx.strokeRect(200,200,100,100)
	ctx.strokeRect(300,300,100,100)  //旋转的是整个画布  


**context.translate(x,y);方法重新映射画布上的 (0,0) 位置。**      
x	添加到水平坐标（x）上的值   
y	添加到垂直坐标（y）上的值
   
	ctx.strokeRect(0,0,100,100)
	ctx.translate(100,100)     //设置画布新的原点 该点就是画布的原点
	ctx.strokeRect(0,0,100,100)  //以（100，,100）为原点进行绘制
	ctx.strokeRect(10,10,100,100)
  

**context.transform(a,b,c,d,e,f);缩放、旋转、移动并倾斜当前的环境。**      
a	水平缩放绘图   在水平方向上缩放绘图
b	水平倾斜绘图   绘图在水平方向上的倾斜程度
c	垂直倾斜绘图   绘图在垂直方向上的倾斜程度
d	垂直缩放绘图   在垂直方向上缩放绘图
e	水平移动绘图   绘图在水平方向上的移动
f	垂直移动绘图   绘图在垂直方向上的移动

*注释：该变换只会影响 transform() 方法调用之后的绘图。而且transform() 方法的行为相对于由 rotate(), scale(), translate(), or transform() 完成的其他变换。例如：如果您已经将绘图设置为放到两倍，则 transform() 方法会把绘图放大两倍，您的绘图最终将放大四倍。*
   
	  ctx.fillStyle = "yellow";
	  ctx.fillRect(0, 0, 250, 100)             //绘制对比的形状
	  ctx.transform(1, 0.5, -0.5, 1,100, 100);  //对绘图进行水平，垂直上不放大不缩小，水平上顺时针旋转0.5，垂直方向上逆时针旋转0.5，水平，垂直方向上各移动100px的距离
	  ctx.fillStyle = "red";
	  ctx.fillRect(0, 0, 250, 100);
	  ctx.transform(1,0.5, -0.5, 1,100, 100); //在前一个变换矩阵上重新变换
	  ctx.fillStyle = "pink";
	  ctx.fillRect(0, 0, 250, 100);     


**context.setTransform(a,b,c,d,e,f);画布上的每个对象都拥有一个当前的变换矩阵。**    
*在使用了transform对对象进行旋转等操作之后，使用setTransform方法会重置并创建新的变换矩阵，再次绘制矩形，重置并创建新的变换矩阵*    
 
a	水平缩放绘图   在水平方向上缩放绘图
b	水平倾斜绘图   绘图在水平方向上的倾斜程度
c	垂直倾斜绘图   绘图在垂直方向上的倾斜程度
d	垂直缩放绘图   在垂直方向上缩放绘图
e	水平移动绘图   绘图在水平方向上的移动
f	垂直移动绘图   绘图在垂直方向上的移动
     
	  ctx.fillStyle = "yellow";                 
	  ctx.fillRect(0, 0, 250, 100)				//绘制一个矩形作为参照
	  ctx.transform(1, 0.5, -0.5, 1,100, 100);  //对画布进行旋转等转换
	  ctx.fillStyle = "red";
	  ctx.fillRect(0, 0, 250, 100);				//在进行转换之后的画布上绘制新的矩形
	  ctx.setTransform(1, 0.5, -0.5, 1, 100, 100)  //重置并且对画布进行新的转换操作
	  ctx.fillStyle = "pink";
	  ctx.fillRect(0, 0, 250, 100);         	 //因为setTransform，粉色红色的矩形合并在一起  



####图像绘制  
drawImage()	    向画布上绘制图像、画布或视频    

**context.drawImage(img,x,y,width,height);在画布上定位图像，并且指定其宽高**    
img	规定要使用的图像、画布或视频。  
x	在画布上放置图像的 x 坐标位置。  
y	在画布上放置图像的 y 坐标位置。  
width	可选。要使用的图像的宽度。（伸展或缩小图像）  
height	可选。要使用的图像的高度。（伸展或缩小图像）



**context.drawImage(img,sx,sy,swidth,sheight,x,y,width,height); 剪切图像，并在画布上定位被剪切的部分：**    
img	规定要使用的图像、画布或视频。  
sx	可选。开始剪切的 x 坐标位置。  
sy	可选。开始剪切的 y 坐标位置。  
swidth	可选。被剪切图像的宽度。  
sheight	可选。被剪切图像的高度。  
x	在画布上放置图像的 x 坐标位置。  
y	在画布上放置图像的 y 坐标位置。  
width	可选。要使用的图像的宽度。（伸展或缩小图像）  
height	可选。要使用的图像的高度。（伸展或缩小图像）  



####像素操作     
createImageData()	创建新的、空白的 ImageData 对象  
getImageData()	返回 ImageData 对象，该对象为画布上指定的矩形复制像素数据    
putImageData()	把图像数据（从指定的 ImageData 对象）放回画布上  

width	返回 ImageData 对象的宽度  
height	返回 ImageData 对象的高度   
data	返回一个对象，其包含指定的 ImageData 对象的图像数据  






  






































####


  