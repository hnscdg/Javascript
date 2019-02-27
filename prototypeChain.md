# 深入理解JS原型链与继承

## 原型写法和用法

```javascript
function Cat(){
    this.Color = "black";
    this.eat = function(){
        alert("吃老鼠");
    };
}
Cat.prototype.A = function(){
    alert("Cat A");
};
Cat.prototype.B = function(){
    alert("Cat B");
};
var cat1 = new Cat();
cat1.eat(); //吃老鼠
cat1.A(); //Cat A
```

上面这种写法是我们熟知的工厂模式，这种模式我觉得应该算是标准的原型写法。但是一般我不会使用这种写法，我会用直接量来写实现上面的方法，因为使用函数封装方法我个人感觉是很危险的，因为函数写法相当于是一个全局方法，他的执行顺序也是优先级最高的，这种方法不利于在大的项目中管理，所以一般用直接量，直接量最大的好处是只有当代码执行到这段代码后才会开始运行。接下来我们修改下上面的代码：

```javascript
var Cat = function(){
    this.Color = "black";
    this.eat = function(){
        alert("吃老鼠");
    };
}
Cat.prototype.A = function(){
    alert("Cat A");
};
Cat.prototype.B = function(){
    alert("Cat B");
};
var cat1 = new Cat();
cat1.eat(); //吃老鼠
cat1.A(); //Cat A
```

## 使用原型扩展提高性能

可能很多人会无法理解为什么我们要通过prototype来输入方法，也听过看过很多人说直接使用函数的效率是最低的，但是不知道原理。其实我们拿上面的案例做个简单的实验你或许就能懂了：

```javascript
var Cat = function(){
    this.Color = "black";
    this.Eat = function(){
        alert("吃老鼠");
    };
}
Cat.prototype.A = function(){
    alert("Cat A");
};
Cat.prototype.B =  function(){
    alert("Cat B");
}
var cat1 = new Cat();
var cat2 = new Cat();
alert(cat1.Eat == cat2.Eat) // false
alert(cat1.A == cat2.A) // true
```

看到结果是否很震惊，Eat和A方法都是Cat对象里面的，为什么两个实例一对比会产生不同的结果呢？在JS原型链中，通过prototype声明的方法会被存入内存中，不管我们实例化多少次Cat访问通过prototype扩展的A或者B方法，他们都是去读取同一个内存，但是Cat自身的属性和方法却不是这样，而是每次都会跟着实例化，如果该对象被频繁调用，那将会占用大量的内存，这就是为什么我们用prototype来扩展我们对象的属性和方法。

## 构造函数

上面这两种写法都是标准的原型写法，每个原型都有一个构造函数，每个原型的实例也都有一个构造函数。这个知识点非常的关键，这个构造函数你可以理解为和我们的身份证一样，每个原型构造函数都是唯一的，我们不能随意的去改变他们的身份证。我们来检测下上面的代码的构造函数。

```javascript
 var Cat = function(){
    this.Color = "black";
    this.Eat = function(){
        alert("吃老鼠");
    };
}
Cat.prototype.A = function(){
    alert("Cat A");
};
Cat.prototype.B = function(){
    alert("Cat B");
};
var cat1 = new Cat();
console.log(Cat.prototype.constructor == Cat); //true
console.log(cat1.constructor == Cat); //true
console.log(cat1.constructor == Cat.prototype.constructor); //true
```

可能看到上面的有些人会说，你不是说每个构造函数都是一个身份证吗？为啥cat1的构造函数和Cat构造函数一样呢，坑爹吧。别急，这就是JS这门有趣的原因之一，cat1我们专业名词称它是Cat的实例，他们的构造函数是共享的。你也可以把cat1理解为Cat的一个复制品，或者说克隆人。我们可以无限复制Cat出来。

```javascript
var cat1 = new Cat();
cat1.Eat(); //吃老鼠
var cat2 = new Cat();
cat2.Eat(); //吃老鼠
```

Cat的复制品的构造函数是都指向Cat本身的，记住这点。

## 原型继承

我觉得要正真理解原型链就需要先理解原型继承的原理，理解了如何继承，基本上你就对原型链掌握很深了。我们来实现一个简单的原型继承，我通过阮老师的文章中写的直接继承prototype来实现继承，修改上面的代码：

```javascript
var Cat = function(){
    this.Color = "black";
    this.Eat = function(){
        alert("吃老鼠");
    };
}
Cat.prototype.A = function(){
    alert("Cat A");
};
Cat.prototype.B = function(){
    alert("Cat B");
};
var Dog = function(){
    this.Weight = "30";
}
Dog.prototype = Cat.prototype;
console.log(Dog.prototype.constructor == Cat); // true
var dog1 = new Dog();
dog1.A(); // Cat A
dog1.B(); // Cat B
```

上面的代码，我设置了一个Dog来继承Cat,我们使用prototype来实现继承，实际继承成功了,Dog的实例dog1调用了Cat里面的prototype的A和B方法。但是这里出了一个小问题，通过prototype继承导致了Dog的构造函数发生了改变，导致它指向了Cat，这就是我们代码中console输出的原因。我们上面说过每个原型都有一个自己的独立的构造函数，我们却改变了它，这样会导致原型混乱，所以我们必须把Dog的构造函数指回Dog本身。所以修改下代码：

```javascript
var Cat = function(){
    this.Color = "black";
    this.Eat = function(){
        alert("吃老鼠");
    };
}
Cat.prototype.A = function(){
    alert("Cat A");
};
Cat.prototype.B = function(){
    alert("Cat B");
};
var Dog = function(){
    this.Weight = "30";
}
Dog.prototype = Cat.prototype;
Dog.prototype.constructor = Dog;
console.log(Dog.prototype.constructor == Cat); // false
console.log(Cat.prototype.constructor == Dog); // true
var dog1 = new Dog();
dog1.A(); // Cat A
dog1.B(); // Cat B
```

上面我通过Dog.prototype.constructor = Dog;这句话把Dog构造函数指回自己了，但是坑爹的是这样做之后，原先的Cat的构造函数也被改变成了Dog，唉，这是要闹哪样，完全坑爹，所以这种继承方式也是失败的，但是我们已经接近成功了，阮老师后面提出了利用空对象作为中介来继承。好的的直接上代码：

```javascript
var Cat = function(){
    this.Color = "black";
    this.Eat = function(){
        alert("吃老鼠");
    };
}
Cat.prototype.A = function(){
    alert("Cat A");
};
Cat.prototype.B = function(){
    alert("Cat B");
};
var Dog = function(){
    this.Weight = "30";
}

var Fn = function(){};
Fn.prototype = Cat.prototype;
Dog.prototype = new Fn();
Dog.prototype.constructor = Dog;
console.log(Dog.prototype.constructor == Dog); // true
console.log(Cat.prototype.constructor == Cat); // true
var dog1 = new Dog();
dog1.A(); // Cat A
dog1.B(); // Cat B
```

这下实现完美的继承了，上面是我根据阮老师的提供的方式实现的一个继承，Dog不止继承了Cat里面的prototype的方法，而且构造函数还是指回自己，Cat的构造函数也没被篡改。貌似非常完美的继承

## prototype继承缺陷

上面的通过原型继承看起来很完美，但是还是有缺陷，并不是说@阮老师的方法有问题，他的继承方法是没问题的，但是只能针对空对象继承。实际上通过prototype继承，他只能继承对象通过prototype的属性和方法，他无法继承对象本身的属性，举个例子：

```javascript
var Cat = function(){
    this.Color = "black";
    this.Eat = function(){
        alert("吃老鼠");
    };
}
Cat.prototype.A = function(){
    alert("Cat A");
};
Cat.prototype.B = function(){
    alert("Cat B");
};
var Dog = function(){
    this.Weight = "30";
}
var Fn = function(){};
Fn.prototype = Cat.prototype;
Dog.prototype = new Fn();
Dog.prototype.constructor = Dog;
console.log(Dog.prototype.constructor == Dog); // true
console.log(Cat.prototype.constructor == Cat); // true
var dog1 = new Dog();
dog1.A(); // Cat A
dog1.B(); // Cat B
console.log(dog1.Color);//undefined
dog1.Eat();//has no method 'Eat'
```

我们在之前的代码里面调用了我们继承Cat的方法和属性，但是只有Cat里面的A和B方法被调用成功了，但是Cat的自身属性里面的Color和Eat方法都没调用成功，说明咱们根本没有继承到他自身的属性，只继承了通过prototype扩展的方法，这就是JS原型链的奇特现象之一，这种原型继承的缺陷貌似阮一峰老师也没发现。

所以我针对上面的方法做了些修改

```javascript
var Cat = function(){
    this.Color = "black";
    this.Eat = function(){
        alert("吃老鼠");
    };
}
Cat.prototype.A = function(){
    alert("Cat A");
};
Cat.prototype.B = function(){
    alert("Cat B");
};
var Dog = function(){
    this.Weight = "30";
}
Dog.prototype = new Cat();
Dog.prototype.constructor = Dog;
console.log(Dog.prototype.constructor == Dog); // true
console.log(Cat.prototype.constructor == Cat); // true
var dog1 = new Dog();
alert(Dog.Weight);// 30
dog1.A(); // Cat A
dog1.B(); // Cat B
alert(dog1.Color);//black
dog1.Eat();//吃老鼠
```

上面的我将继承者Dog的原型直接指向了Cat的实例，然后再将Dog的构造函数指回本身，这样就可以实现完整的继承了，Dog不仅仅继承了Cat的prototype而已还继承了Cat本身自带的属性和方法。在上面中我们知道Cat的实例cat1其实就是包含了Cat所有的属性和方法，他不会区分你是不是在原型中的方法还是在自身中的方法，都会完全被复制到实例中，所以我们直接去继承实例，这样子就可以直接获取到Cat中所有的方法和属性。而且直接继承实例我感觉也更加安全且高效，因为不去直接操作原型本身，只是操作原型实例。

## 通过深拷贝实现完美继承

其实上面的方法离完美的继承方式还是存在着一个缺陷的。我们的的继承者Dog如果他现在里面存在着原型方法的时候，我们又想让她保留现在的原型方法情况下，还可以去继承Cat里面的所有方法怎么办，用上面的方法是无法实现的，请看代码：

```javascript
var Cat = function(){
    this.Color = "black";
    this.Eat = function(){
        alert("吃老鼠");
    };
}
Cat.prototype ={
    A:function(){
        alert("Cat A");
    },
    B:function(){
        alert("Cat B");
    }
}
var Dog = function(){
    this.Weight = "30";
}
Dog.prototype.testDog = function(){
    alert("test Dog");
}

Dog.prototype = new Cat();
Dog.prototype.constructor = Dog;

var dog1 = new Dog();
alert(dog1.Weight);//30
dog1.testDog();//has no method 'testDog'
```

看上面的代码，我只是在继承者Dog的原型里面添加了一个testDog的方法，然后Dog用我们上面的方法去继承Cat后，Dog自身的属性Weight在继承Cat的过程中也被保留下来了，但是Dog存在原型链中的testDog却在继承过程中被干掉了，无言，心碎。这个时候我想到了阮一峰老师的拷贝继承，他的拷贝继承依然是存在的缺陷，但是我直接改进了他的方法，那样实现了完美的继承：

```javascript
var Cat = function(){
    this.Color = "black";
    this.Eat = function(){
        alert("吃老鼠");
    };
}
Cat.prototype.A = function(){
    alert("Cat A");
};
Cat.prototype.B =  function(){
    alert("Cat B");
}
var Dog = function(){
    this.Weight = "30";
}
Dog.prototype.testDog = function(){
    alert("test Dog");
}

var extend = function(Child,Parent){
    var p = new Parent();
    var c = Child.prototype;
    for (var i in p) {
        c[i] = p[i];
    }
    c.uber = p;
}
//用我们写好的继承方法执行继承
extend(Dog,Cat);
var dog1 = new Dog();
dog1.A(); // Cat A
dog1.Eat(); // 吃老鼠
dog1.testDog(); // test Dog
alert(dog1.Weight); // 30
```

其实上面我们extend方法中我只是通过了for..in去遍历Cat生成的实例中的所有属性和方法，然后将这些值复制到我们的Dog中，这样子就可以实现保留本身属性又继承，这种方法是应该算是最优的解决方法。

[原文链接](https://www.520ued.com/article/538838d7b992a7c43f5c205a)