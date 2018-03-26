# Springboot
springboot配置类

使用内部类惊醒相关的配置

    //Configuration表示当前类为java的配置类
    //MapperScan表示此数据库配置文件的作用范围
    @Configuration
	  @MapperScan(basePackages = "com.fid.mapper.statistics", sqlSessionTemplateRef = "statisticsSqlSessionTemplate")
  	public class PointsSourceConfig {
		@Bean(name = "statisticsDataSource")
		@Primary
    //读取配置文件中的prefix开头的dataSource的相关配置
		@ConfigurationProperties(prefix = "spring.datasource.statistics")
		public DataSource dataSource() {
			return DataSourceBuilder.create().build();
		}
    //设置工厂
		@Bean(name = "statisticsSqlSessionFactory")
		@Primary
		public SqlSessionFactory statisticsSqlSessionFactory() throws Exception {
			SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
			sessionFactory.setDataSource(dataSource());
			return sessionFactory.getObject();
		}
    //配置事物管理器
		@Bean(name = "statisticsTransactionManager")
		@Primary
		public DataSourceTransactionManager statisticsTransactionManager(
				@Qualifier("statisticsDataSource") DataSource dataSource) {
			return new DataSourceTransactionManager(dataSource);
		}
    //配置模板
		@Bean(name = "statisticsSqlSessionTemplate")
		@Primary
		public SqlSessionTemplate statisticsSqlSessionTemplate(
				@Qualifier("statisticsSqlSessionFactory") SqlSessionFactory sqlSessionFactory) throws Exception {
			return new SqlSessionTemplate(sqlSessionFactory);
		}
	}
