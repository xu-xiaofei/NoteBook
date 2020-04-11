
```java
/**
 * 本质上声明一个服务，就是开启一个线程，监听一个端口。在端口接收数据（调用的方法名，方法的签名类型，方法的参数）
 * 然后利用声明时候传入的对象实例，进行方法的调用。方法返回的对象也通过socket写回去。
 *
 * 本质上调用一个远程服务就是开启一个代理对象，而代理对象调用的实际上是网络通信，
 * 将想要调用的方法和参数通过socket传递给正在监听的线程，并接收返回的对象。
 *
 * @Author fei
 */
public class RpcDemo {
    public static void export(Object service,int port)throws  Exception{
        //TODO check
        System.out.println("server start...");
        ServerSocket server = new ServerSocket(port);
        while(true){
        Socket socket = server.accept();
        ObjectInputStream inputStream = new ObjectInputStream(socket.getInputStream());
        String methodName = inputStream.readUTF();
        Class<?>[] methodTypes = (Class<?>[]) inputStream.readObject();
        Object[] args = (Object[]) inputStream.readObject();

        Method method= service.getClass().getMethod(methodName,methodTypes);
        Object result= method.invoke(service,args);
        ObjectOutputStream outputStream = new ObjectOutputStream(socket.getOutputStream());
        outputStream.writeObject(result);
        outputStream.close();
        socket.close();
        }

    }

    public static  <T> T refer(Class<T> interfaceClass,String host,int port) throws Exception{
        return (T) Proxy.newProxyInstance(interfaceClass.getClassLoader(), new Class[]{interfaceClass}, new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                Socket socket = new Socket(host,port);
                ObjectOutputStream objectOutputStream = new ObjectOutputStream(socket.getOutputStream());
                objectOutputStream.writeUTF(method.getName());
                objectOutputStream.writeObject(method.getParameterTypes());
                objectOutputStream.writeObject(args);

                ObjectInputStream objectInputStream = new ObjectInputStream(socket.getInputStream());
                Object result = objectInputStream.readObject();
                return result;
            }
        });
    }

}

```