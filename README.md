# JFlieglCounterHDController
An Java Controller class with Listener Pattern for connecting, configuring and managing Fliegl Agrartechnik CounterHD iBeacon Hardware. And a simple example implementation of an Android App.

# Usage

Create an instance of JCounterHDController i.e. from within your Activity and add a propper Listener to it.

```
public class YOUR_ACTIVITY extends AppCompatActivity implements JCounterHDController.CounterHDControllerListener

// ...

JCounterHDController counterController;

//...

protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // ...

        counterController = new JCounterHDController(this);
        
        counterController.setCounterHDControllerListener(this);

        scanButton = (Button) findViewById(R.id.ScanButton);
        scanButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                scanButtonPushed();
            }
        });
        
        // ...

    }
```

<img src="Screenshot.png" alt="Drawing" style="width: 350px;"/>

Make sure you implement/override all listener calls, especially ```cc_didFindPeripherals``` wichs gives you the opportunity to connect to a Fliegl CounterHD beacon (Peripheral) as soon as one was found over BTLE.



```
public void cc_didFindPeripherals(final List<BluetoothDevice> peripheralList) {

        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                if (peripheralList.size() > 0)
                {
                    Log.i(TAG, "Did find peripherals: " + peripheralList.size());
                    textView.append("\nFound peripherals: " + peripheralList.size());

                    // For Simplicity - I only retrieve the first peripheral and automagically
                    // connect to it. In real life applications you may want to populate a list view
                    // and connect to a peripheral only if the user touches the list view entry.

                    //BluetoothDevice d = peripheralList.get(0);
                    //counterController.connectPeripheral(d);

                    // ** or **

                    // only use peripheral with given HardwareAddress: YOUR_BEACON_HARDWARE_ADDRESS
                    for (BluetoothDevice d :
                            peripheralList) {
                        //Log.i(TAG, d.getAddress().toString());
                        if (d.getAddress().toString().equalsIgnoreCase("YOUR_BEACON_HARDWARE_ADDRESS")){
                            counterController.connectPeripheral(d);
                        }
                    }
                }
            }
        });



    }
```

I use this class also in my configuration app for example:
<blockquote class="imgur-embed-pub" lang="en" data-id="a/t4MOu"><a href="//imgur.com/t4MOu"></a></blockquote><script async src="//s.imgur.com/min/embed.js" charset="utf-8"></script>
