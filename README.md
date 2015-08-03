# MultiAsynctask
相当于Asynctak，但是核心是不同的。系统的AsyncTask在不同版本的API的并发策略是不同的，在Android4.0以上只能运行一个Asynctask。Multiasynctask可以同时10个或更多的子线程做的事情。
Equivalent to Asynctak, but the difference is that. The Asynctask of the system is different in different versions of the API concurrency strategy. MultiAsynctask can simultaneously 10 or more of the sub thread to do things.   

使用方式如下，基本和系统API相同：
The use of the following ways, the same as the system API:
```
public class LongAsynctask extends MultiAsynctask<Paramer, Updater, Resulter> {

	@Override
	public Resulter onTask(Paramer... params) {
		Paramer paramer = params[0];
		Logger.i("onTask打印：" + "what:" + paramer.getWhat() + ";value:" + paramer.getValue());
		for (int i = 0; i < 10; i++) {
			paramer.setValue(paramer.getValue() + 1);
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			Updater updater = new Updater(paramer.getWhat(), paramer.getValue());
			postUpdate(updater);
		}
		return new Resulter(paramer.getWhat(), paramer.getValue());
	}

	@Override
	public void onUpdate(Updater update) {
		Logger.i("onUpdate打印：" + "what:" + update.getWhat() + ";value:" + update.getValue());
	}

	@Override
	public void onResult(Resulter result) {
		Logger.i("onResult打印：" + "what:" + result.getWhat() + ";value:" + result.getValue());
	}
}
```