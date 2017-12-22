# html2canvas-cross-domain-screenshots

版本：html2canvas 0.5.0-beta3
公司项目跨域截图问题描述：
    一张上传到阿里云OSS上的图片，使用html2canvas截取该元素内的内容，然后转成base64码流存储到服务器,接着操作下一步跳转页面在一块区域显示截图内容，
结果显示出的内容没有截取到图片。

解决方法：
    将html2canvas源码中line1218改成self.image.src = src+"?"+new Date().getTime();添加一个时间戳，调用部分代码：
    
    html2canvas(el,{
					useCORS:true,
					allowTaint:true,
					onrendered: function(canvas) { 
						var imageData = canvas.toDataURL("image/png");
						$.ajax({
							url: url,
							type: "POST",
							data:...,
							success: function(data){
								...
							}
						});
					} 
			})
      
      此时已将截图转换成base64码流存储到服务器，接下来按需要传图片就OK了。
