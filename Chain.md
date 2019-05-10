
# 责任链

流程

	客户将请求发送到链头处理器，
	处理器决定本次的操作和是否传递给下一个处理器

好处

	职责链上的处理者负责处理请求，客户只需要将请求发送到职责链上即可，无须关心请求的处理细节和请求的传递，
	所以职责链将请求的发送者和请求的处理者解耦了。
  

# 结构

结构

    抽象处理者（Handler）
      定义一个处理请求的接口，包含抽象处理方法和一个后继连接。
    具体处理者（Concrete Handler）
      实现抽象处理者的处理方法，判断能否处理本次请求，如果可以处理请求则处理，否则将该请求转给它的后继者。
    客户类（Client）
      创建处理链，并向链头的具体处理者对象提交请求，它不关心处理细节和请求的传递过程。




# 示例

abstracthandle


	public abstract  class AbstractRequstHandle {

		protected AbstractRequstHandle next;

		public void setNext(AbstractRequstHandle next) {
			this.next = next;
		}

		public abstract void handleRequest(Request request);
	}
	
ConcreteHandler

	public class RequstHandleFirst extends AbstractRequstHandle {

		@Override
		public void handleRequest(Request request) {
			System.out.println("RequstHandleFirst handle "+request.getName());
			if (this.next != null){
				next.handleRequest(request);
			}
		}
	}
	
	public class RequstHandleSecond extends AbstractRequstHandle {
		@Override
		public void handleRequest(Request request) {
			System.out.println("RequstHandleSecond handle "+request.getName());
		}
	}

Request

	public class Request {
		private String name;

		public Request(String name) {
			this.name = name;
		}

		public String getName() {
			return name;
		}

		public void setName(String name) {
			this.name = name;
		}
	}
	
TestChain

	public class TestChain {
		public static void main(String[] args) {
			AbstractRequstHandle chain = new RequstHandleFirst();
			chain.setNext(new RequstHandleSecond());
			chain.handleRequest(new Request("請求1"));
		}
	}
	

