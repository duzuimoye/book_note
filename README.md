# book_note
# 函数
# 函数对象
	  js中的函数就是对象对象就是'名/值'对的集合并拥有一个连到原型对象的隐藏连接。
	  函数是对象，可以像任何其他的值一样被使用
	  因为函数是对象，所以函数可以拥有方法
# 函数字面量
	  函数对象通过函数字面量来创建。
	  函数字面量包括四个部分 - 保留字funciton - 函数名，可以被省略 - 包围在圆括号中的一组参数 - 包围在花括号中的一组参数
	函数字面量可以出现在任何允许表达式出现的地方
# 调用
	调用一个函数会暂停当前函数的执行，传递控制权和参数给新的函数，每个函数还接收两个附加的参数：this 和 arguments ,this很重要
1. 方法调用模式
	当一个函数被保存为对象的一个属性时，我们称它为一个方法
	方法可以使用this访问自己所属的对象，所以能从对象中取值或对对象进行修改。
2. 函数调用模式

	  给myobject增加一个double 方法
	  myobject.double=function(){
	      var that= this; // 解决方法
	      var helper=function(){
		  that.value=add(that.value,that.value);
	      };
	      helper();//以函数的形式调用helper
	  };
	  //以方法的形式调用doouble
	  myobject.double();
3. 构造器调用模式

	  如果在一个函数前面带上new 来调用，那么背地里将会创建一个连接到该函数的prototype成员的新对象，同时this会被绑定呆那个新对象上。
	  //创建一个名为Quo的构造器函数，它构造一个带有status属性的对象

	  var Quo=function(string){
	    this.stastus=string;
	  };
	  //给Quo 的所有实例提供一个名为get_status 的公共方法
	  Quo.prototype.get_status=function(){
	      return this..status;
	  };
	  //构造一个Quo实例
	   var myQuo =new Quo("confused");
	  document.writeln(myQuo.get_status());
	  一个函数，如果创建的目的就是希望结合new 前缀来调用，那么它就被称为构造器函数 
4. Apply 调用模式

	  apply 方法让我们构建一个参数数组传递给调用函数。它允许我们选择this的值。apply方法接收两个参数，第一个是要绑定给this 的值，第二个就是一个参数数组

	  //构造一个包含两个数字的数组，并将它们相加
	  var array =[3,4];
	  var sum=add.apply(null,array);//sum的值为7
	  //构造一个包含status成员的对象
	  var statusObject={
	      status:'A-OK'
	  };
	  //statusObject 并没有继承自Quo.prototype,但我们可以在statusObject上调用
	  //用get_status方法，尽管statusObject并没有一个名为get_status的方法
	  var status=Quo.prototype.get_status.apply(statusObject);
	  //status的值为 'A-OK'
#异常

	js提供了异常处理机制，异常是干扰程序的正常流程的不寻常事故，当发现这样的事故时，程序抛出一个异常
	递归

	递归函数就是会直接或间接的调用自身的一种函数。把一个问题分解为一组相似的子问题，每一个都用一个寻常解去解决。
	//  还需要多看看，不是很好的理解
#作用域

	在编程语言中，作用域控制着变量与参数的可见性及生命周期。减少了名称冲突，并且提供了自动内存管理。
	    var foo =function(){
		var a=3,b=5;
		var bar=function(){
		    var b=7,c=11;
		    //此时 a-3 b=7 c =11
		    a+=b+c;
		    //此时 a=21 b=7 c=11
		};
		//此时 a=3 b=5 c未定义
		bar();
		此时a=21 b=5
	    };
#闭包

	作用域的好处是内部函数可以访问定义他们的外部函数的参数和变量（除了this和argments）。
	和以对象字面量形式去初始化myObject不同，这里通过调用一个函数的形式去初始化myObject,该函数会返回一个对象字面量

	  var myObject=(function(){
	      var value =0 ;
	      return{
		  increment: function (inc){
		      value +=typeof inc ==='number' ? inc : 1;  //=== 全等 包括类型长度
		  },
		  getValue:function(){
		      return value;
		  }
	      };
	  }());
	这里没有把一个函数赋值给myObject。把调用该函数后返回的结果赋值给他注意最后一行的（）。

用Quo构造器

	//创建一个Quo的构造函数 它构造出带有get_sttus方法和status私有属性的一个对象

	  var quo=function(status){
	      return{
		  get_status:function(){
		      return status;
		  }
	      };
	  };
	  //构造一个quo实例
	  var myQuo = quo("zengkai");
	  console.log(myQuo.get_status());//zengkai 
#回调

	request=prepare_the_request();
	send_request_asynchronously(request,function(response){
	    display(response);
	});

	传第一个函数作为参数给send_request_asynchronously函数，一旦接收到响应它就会被调用
	模块

	我们可以使用函数和闭包来构造，模块是一个提供接口却隐藏状态与现实的函数或对象。通过使用函数产生模块，我们几乎可以完全摒弃全局变量的使用，从 而缓解这个js的最为糟糕的特性之一所带来的影响（全局变量）

	模块模式利用了函数作用域和闭包来创建被绑定对象与私有成员的关联

	模块模式的一般形式是：一个定义了私有变量和函数的函数； 利用闭包床架可以访问私有变量和函数的特权函数； 最后返回这个特权函数，或者把他们保存到一个可以访问到的地方

	使用模块可以摒弃全局变量的使用

