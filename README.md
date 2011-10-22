# pubsub-android


## Install
* TODO bullet list for installing the android library in Eclipse

## Getting started
*How to make use of Pubsub in your Android Activity*

First, create a ServiceConnection in your activity. This is required because pubsub-android is a Service.

	private ServiceConnection serviceConnection = new ServiceConnection() {
		@Override
		public void onServiceConnected(ComponentName className, IBinder service) {
			mPubsub = ((Pubsub.LocalBinder) service).getService();
			mPubsub.setHandler(mHandler);
			mPubsub.connect();
		}

		@Override
		public void onServiceDisconnected(ComponentName className) {
			mPubsub = null;
			mPubsub.setHandler(null);
		}
	};
	
Then, create the Handler. This will act as a message callback between the Servive and your Activity.

	private Handler mHandler = new Handler() {
		@Override
		public void handleMessage(Message msg) {
			switch (msg.what) {
			case Pubsub.RAW_TEXT:
				byte[] readBuf = (byte[]) msg.obj;
				String readMessage = new String(readBuf, 0, msg.arg1);
				// TODO Do something with the message here...
				break;
			case Pubsub.TERMINATED:
				// TODO React when the connection fails...
				break;
			case Pubsub.ERROR:
				// TODO Read error messages here...
				break;
			}
		}
	};