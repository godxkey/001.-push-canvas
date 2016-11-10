# 	绘图的canvas的版本设计(不兼容移动端)	
##	时间：2016-6-22 20:34:03
+++	作者：肖平
+ 1.功能：
 - 实现：	
   - 1.1填充
   - 1.2绘图
   - 1.3选择画笔颜色
+ 2.设计原理：
 - 主要是：
   - 2.1canvas的画图
   - 2.2javascript里面的事件设计的
   - 2.3核心是stroke(),for in的使用，
+ 3.后续发展：
 - 3.1移动端的需要处理clientX获取的问题
 - 补充并总结canvas的常用方法
	var c=document.getElementById('canvas');
	var ctx=c.getContext('2d');
	ctx.save();
	ctx.beginPath();
		/*画线*/
		ctx.moveTo(0,0);
		ctx.lineTo(23,330);
		ctx.stroke();

		/*绘制填充*/
		ctx.fillStyle="#FF0000";
		ctx.fillRect(0,0,150,75);

		/*画圆*/
		ctx.beginPath();
		ctx.arc(95,50,40,0,2*Math.PI);
		ctx.stroke();

		/*绘制文字*/
		ctx.font="30px Arial";
		ctx.fillText("Hello World",10,50);
		ctx.font="30px Arial";
		ctx.strokeText("Hello World",10,50);

		/*绘制渐变*/
		// Create gradient
		var grd=ctx.createLinearGradient(0,0,200,0);
		grd.addColorStop(0,"red");
		grd.addColorStop(1,"white");
		// Fill with gradient
		ctx.fillStyle=grd;
		ctx.fillRect(10,10,150,80);	
		// Create gradient
		var grd=ctx.createRadialGradient(75,50,5,90,60,100);
		grd.addColorStop(0,"red");
		grd.addColorStop(1,"white");
		// Fill with gradient
		ctx.fillStyle=grd;
		ctx.fillRect(10,10,150,80);

		/*画图片*/
		var img=document.getElementById("scream");
		ctx.drawImage(img,10,10);
	ctx.closePath();

## 总体代码预览-肖平
```

	/*1.0选取画笔的颜色-肖平*/
	var color1=['red','green','blue','purple','black','orange','lightpink'];
	var licol=document.getElementsByTagName('li');	
	var test=document.getElementById('test');		 
	var clearRect1=document.getElementById('clearRect');			
	for(k in color1){
		licol[k].style.backgroundColor=color1[k];
		console.log(color1[k]+'\n');
	}

	/*2.0开始写作canvas的绘图-肖平*/
	/*2.1获取上下文2d，绘制stroke是关键-肖平*/
	var canvas2=document.getElementById('canvas');
	canvas2.width=360*3;
	canvas2.height=150*3;
	canvas2.style.border="3px solid rgba(0,133,0,.8)";
	var ctx=canvas2.getContext('2d'); 
	
	/*2.2.1决定画笔的颜色*/
	var colorxp;
	for(var i=0;i<licol.length;i++){
		licol[i].index=i;
		licol[i].innerHTML="";
		licol[i].onmousedown=function(){ 
			colorxp=color1[this.index];
			ctx.strokeStyle=colorxp;
			console.log('colorxp-'+this.index+'---'+colorxp);
		}
	}
	/*2.2.2核心画图代码-肖平*/
	/*2.2.3画笔绘画*/
	ctx.save();
	canvas.onmousedown=function(event){
		ctx.beginPath();
			var event=event||window.event;
			ctx.moveTo(event.clientX,event.clientY);
			var flagTr=true;
			canvas.onmousemove=function(event){
				canvas.onmouseup=function(){flagTr=false;}
				if(flagTr){
					var event=event||window.event;
					ctx.lineTo(event.clientX,event.clientY);
					ctx.lineWidth=10;

					// console.log('++colorxp-'+this.index+'---'+colorxp);
					ctx.stroke();
				}
			}
		ctx.closePath();
	}
	/*2.3有填充色和文字的随机变化-肖平*/
	var num1=-1;
	test.onmousedown=function(){
		var flagF;num1++;
		if(num1%2==0){flagF=true;}
		else if(num1%2!=0){flagF=false;};
		var startxp=true,xpp=true;
		canvas.onmousedown=function(event){
			ctx.beginPath();xpp=true;
			canvas.onmousemove=function(event){ 
				canvas.onmouseup=function(event){ 
					xpp=false;
					canvas.onmouseup=function(event){ 
						xpp=true;startxp=true;console.log('you are ok');
					}
				}
				if(xpp&&flagF){ 
					var event=event||window.event;
					var clientxp=event.clientX;
					var clientyp=event.clientY;
					var txtxp="我是肖平";
					var colorfillx=parseInt(Math.floor(Math.random()*1000000)); 
					console.log('event.clientX--'+event.clientX+'\n++\n'+'this.clientY--'+event.clientY); 
					if(startxp){
						ctx.moveTo(clientxp,clientyp);startxp=false;
					}else{
						ctx.lineTo(clientxp,clientyp);							
					} 
					ctx.fillText(txtxp,clientxp+20,clientyp+20);
					ctx.font="300 normal 22px microsoft yahei";

					var col="#"+colorfillx;
					ctx.lineWidth=3;
					ctx.strokeStyle=col;
					ctx.fillStyle=col;
					ctx.fill();
					ctx.stroke();
				};
			};
			ctx.closePath(); 
		};
	}

	/*3.0清空画布--肖平*/
	clearRect1.onmousedown=function(){
		ctx.restore();
		ctx.beginPath();
			ctx.clearRect(0,0,canvas.width,canvas.height);
			// console.dir(canvas.width+"---"+canvas.height);
		ctx.closePath();
	}

``` 
###版本开源，欢迎各位大大完善它哦~\ (≧▽≦) /~
+ 作者：
 - 肖平
   - 2016-6-26 19:54:02
