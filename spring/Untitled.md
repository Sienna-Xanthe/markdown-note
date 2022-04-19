1. 通过bean的id来获取 xml中配置的 bean的class类的对象
2. 由于Spring反射new出来的对象是 Object类型的，所以要强制转换
3. 这里还用了多态，因为bean里面的类是 UserDaoImpl (UserDao接口的实现类)

