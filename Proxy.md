

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

