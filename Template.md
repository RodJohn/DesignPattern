# 概述


    定义一个流程中的骨架，而将一些步骤延迟到子类中。模板方法使子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。


        设计者需要给出一个算法的固定步骤，并将某些步骤的具体实现留给子类来实现。
        需要对代码进行重构，将各个子类公共行为提取出来集中到一个共同的父类中以避免代码重复。




# 示例

JDBC基本流程

	对于create update delete
	1、载数据库驱动
	2、创建数据库连接
	3、创建语句对象 
	4、绑定参数
	5、执行语句
	6、释放资源

	对于select
	1、载数据库驱动
	2、创建数据库连接
	3、创建语句对象 
	4、绑定参数
	5、执行查询
	6、遍历结果集
	7、释放资源

可以使用模板方法的模式将基本流程定义在一个抽象类中，并实现相同的操作，把不同的实现放到子类中去实现


模板方法类

	public abstract class JdbcTemplate {

		private String SQL;
		private Object[] params;

		JdbcTemplate(String sql, Object... params) {
			this.SQL = sql;
			this.params = params;
		}

		public final void jdbc() {
			loadDBDriver();
			Connection connection = null;
			PreparedStatement ps = null;
			try {
				connection = getConnection();
				ps = createPreparedStatement(connection);
				Object rs = preparedStatementExecute(ps);
				processResult(rs);
			} catch (SQLException e) {
				e.printStackTrace();
			} finally {
				if (connection != null) {
					try {
						connection.close();
					} catch (SQLException e) {
						e.printStackTrace();
					}
				}
				if (ps != null) {
					try {
						ps.close();
					} catch (SQLException e) {
						e.printStackTrace();
					}
				}
			}
		}

		private static void loadDBDriver() {
			try {
				Class.forName("com.mysql.jdbc.Driver");
			} catch (ClassNotFoundException e) {
				e.printStackTrace();
			}
		}

		private static Connection getConnection() throws SQLException {
			return DriverManager.getConnection("url", "user", "password");
		}

		public PreparedStatement createPreparedStatement(Connection conn) throws SQLException {
			PreparedStatement ps = conn.prepareStatement(this.SQL);
			for (int i = 0; i < params.length; i++) {
				ps.setObject(i, params[i]);
			}
			return ps;
		}

		protected abstract Object preparedStatementExecute(PreparedStatement ps) throws SQLException;

		protected abstract Object processResult(Object rs);
	}


update子类


	public class JdbcUpdate extends JdbcTemplate {

		JdbcUpdate(String sql, Object... params) {
			super(sql, params);
		}

		@Override
		protected Object preparedStatementExecute(PreparedStatement ps) throws SQLException {
			return ps.executeUpdate();
		}

		@Override
		protected Object processResult(Object rs) {
			return (Integer) rs;
		}
	}

query子类

	public class JdbcQuery extends JdbcTemplate {

		JdbcQuery(String sql, Object... params) {
			super(sql, params);
		}

		@Override
		protected Object preparedStatementExecute(PreparedStatement ps) throws SQLException {
			return ps.executeQuery();
		}

		@Override
		protected Object processResult(Object rs) {
			return (ResultSet) rs;
		}
	}





# 参考

https://www.jianshu.com/p/fb56847a7010


