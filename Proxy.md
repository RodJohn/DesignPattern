

# 代理模式

代理

	有绝对控制权 把工作交给目标对象来做 单一职责原则
	

解耦

	将目标对象与使用的用户隔离开，
	代理模式可以屏蔽用户真正请求的对象，是用户程序和正在的对象解耦。
aop

	而且在调用目标对象的方法前后还能执行额外的操作



# 示例


接口

	public interface BuyHouse {
	  void buyHosue();
	}

实现类


	public class BuyHouseImpl implements BuyHouse {
		@Override
		public void buyHosue() {
			System.out.println("我要买房");
		}
	}
	
代理类1

	public class BuyHouseProxy implements BuyHouse {

		private BuyHouse buyHouse;

		public BuyHouseProxy(final BuyHouse buyHouse) {
			this.buyHouse = buyHouse;
		}

		@Override
		public void buyHosue() {
			System.out.println("买房前准备");
			buyHouse.buyHosue();
			System.out.println("买房后装修");

		}
	}

代理类2

	public class BuyHouseProxy implements BuyHouse {
	
		private BuyHouse buyHouse;

		public BuyHouseProxy(final BuyHouse buyHouse) {
			this.buyHouse = buyHouse;
		}

		@Override
		public void buyHosue() {
			System.out.println("把钱拿去了旅游");

		}
	}

客户端

	public class ProxyTest {
		public static void main(String[] args) {
			BuyHouse buyHouse = new BuyHouseImpl();
			buyHouse.buyHosue();
			BuyHouseProxy buyHouseProxy = new BuyHouseProxy(buyHouse);
			buyHouseProxy.buyHosue();
		}
	}



# 静态代理 动态代理 

参考JDK动态代理


代理  VS 委托

     proxy :译为代理， 被代理方（B）与代理方（A）的接口完全一致。 主要使用场景（语义）应该是：为简化编程（或无法操作B），不直接把请求交给被代理方（B），而把请求交给代码方（A），由代理方与被代理方进行通信，以完成请求。
     delegete : 译为委托，主要语义是：一件事情（或一个请求）对象本身不知道怎样处理，对象把请求交给其它对象来做。

https://blog.csdn.net/fu123123fu/article/details/80159551


能不用继承就不用继承，能使用委托实现的就不使用继承。两个类有明显示的层级关系时使用继承，没有明显的层级关系，仅仅是为了在一个类中使用另一个类的方法时应该使用委托。


