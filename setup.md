# Alto Test Environment Setup

Setup instructions for the Alto Testing environment.

### Requirements

1. Passenger app built to iPhone simulator (or) Passenger app Test build on an iPhone device to run the app
2. Driver app built to Android Emulator (or) Driver app Test build on an Android device to run the app
3. Testing database access (Sequel Pro is recommended)
4. Bestmile Dashboard access (for Virtual Vehicle Fleet)

#### Passenger App Setup

###### Option 1: Simulator Testing (best for quick debugging and builds)

1. Follow [passenger-app set-up instructions](https://github.com/ridealto/passenger-app#alto-passenger-app) if you haven't done so already
2. From the root directory, build the app to a simulator via the test environment:

   ###### For iOS:

   1. run `yarn launch -p ios --simulator='<IPHONE_MODEL>' --environment=testing --port=<PORT_NUMBER>`
      for example:
      ```sh
      $ yarn launch -p ios --simulator='iPhone 6' --environment=testing --port=8080
      ```

   ###### For Android:

   1. open an emulator with Android Studio's AVD manager
   2. run `yarn launch -p android --environment=testing --port=8080`

3. Open [http://localhost:8080/debugger-ui/](http://localhost:8080/debugger-ui/) and inspect to view the debugger console

###### Option 2: Physical Device Testing (it is recommended to test on the physical devices for any nuances)

1. Follow [passenger-app set-up instructions](https://github.com/ridealto/passenger-app#alto-passenger-app) if you haven't done so already
   ###### Extra step For iOS only:
2. Get your device id:

   1. iOS steps
      1. download [ios-deploy command line tool](https://github.com/ios-control/ios-deploy):
         ```
         npm install -g ios-deploy
         ```
      2. Get the device UUID
         ```
         ios-deploy -c detect
         ```
         reponse will look something like this: `Found aab0ec826c74ebd23d7650bf42ca134ffd2eca96`
      3. Make sure this UUID is added to Alto's provisioning profile (might not need this step - but let's double check)

3. From the root directory, build the plugged-in device via the test environment with the command:
   ###### for IOS
   ```sh
   $ yarn launch -p ios -e testing --udid=<YOUR_DEVICE_UUID>
   ```
   ###### for Android
   ```sh
   $ yarn launch -p android -e testing
   ```

   _Pro-Tip: you can "refresh" physical android devices with the following command:_
   ```sh 
   $ adb -t <TRANSPORT_ID> shell input text "RR"
   ```
   
4. If you run into issues, check out [react native's docs](https://facebook.github.io/react-native/docs/running-on-device). You might need to change some xcode settings, etc.

#### Driver App Setup

1. Follow [driver-app set-up instructions](https://github.com/ridealto/driver-app) if you haven't done so already
2. Log into the [Bestmile Dashboard](https://od2.partner.gce1.bestmile.io/sites/19d80f6a-b1fe-4455-aa9c-193186bec620/map) to view virtual vehicles (you may need access)
3. Log into the Test database with Sequel Pro, find a virtual vehicle that is not in use. Copy the VIN
4. Launch the Driver app:
   ###### Emulator
   1. Open an emulator
   2. launch the driver app by running this command in the root directory
      `yarn launch -p android --environment=testing --port=8089`
      note: I picked a port that is not running anything (since at this point you likely have the passenger app running on the default port)
   3. View the developer options and make sure your emulator is on the same IP and port (command "m" > dev settings > debug server host and port for device) - if you update the port, you may have to refresh, rebuild, or completely shut down the emulator and then open again and build.
   ###### Physical Device
   1. Plug in your android device to your computer, and make sure you allow debugging on the device (you will see a modal) run the same command:
      ```sh
      $ yarn launch -p android --environment=testing --port=8089
      ```
5. Once the app is built, start the login process by entering the VIRTUAL VEHICLE VIN, your driver login info, and go through the pre-shift checklist.
6. Now you are officially available for passengers!
