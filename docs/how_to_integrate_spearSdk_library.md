## How to integrate SpearSdk library v5.1.0

- Add SpearSdk-<version>.jar to lib folder under your project in Eclipse.
- Add the following line to `.classpath` file

```
    <classpathentry kind="lib" path="lib/SpearSdk-5.1.0.jar" sourcepath="/SpearSdk/src"/>
```
  Or right click the project in Project Explorer -> choose Properties -> choose Java Build Path from left panel -> select Libraries tab -> Add SpearSdk-<version>.jar from lib folder by clicking on Add Jars button.

- Your class should implement the following interfaces:

    - `SpearInitListener`: To get the callback result when SPEAR ASR Engine initialization completes and when Engine or SPEAR WakeUp 
    initialization error occurs.
    - `RecognitionListener`: To get the callback results when SPEAR ASR Engine returns intermediate or final results, 
        when the engine starts or stops listening, and when SPEAR ASR engine return errors.
    - `SpearWakeUpListener`: To get the callback results when SPEAR WakeUp Engine returns final results and when the engine starts or stops
    listening.
    - `AudioTask.OnMicStateChangeListener`: To get the callback results when incoming audio mic status has changed.

    Check `SpearSdkApi.md` documentation for more details on each interface methods.

```java
public class MyClass implements SpearInitListener, RecognitionListener, AudioTask.OnMicStateChangeListener {
   // Initialize SPEAR ASR Engine
   // Implements interface methods    
}
```   

- SPEAR ASR Engine can be initialized by calling `SpearSdkApi.getInstance().initialize()`.
- Get the instance of `SpeechRecognizer` by calling `SpearSdkApi.getInstance().createSpeechRecognizer()` to access SPEAR Recognizer's functionality.
Check `SpearSdkApi.md` for more details.
- Your application must include FST, if shared, to `com.think-a-move.spear/models/` and
 call `setFstGrammar("example.fst", false)` on created `SpeechRecognizer` object.

#### Example:

```java
public class MyClass implements SpearInitListener, RecognitionListener, VadListener, AudioTask.OnMicStateChangeListener {
    private SpearSdkApi spearSdkApi;
    private SpeechRecognizer speechRecognizer;
    private SpearWakeUp spearWakeUp;
    private static String LM_Package = "/storage/self/primary/SPEAR-DATA-EN";

    // Call this method to load native SPEAR libraries.
    public void initializeSpear() {
        spearSdkApi = SpearSdkApi.getInstance();
        spearSdkApi.subscribeSpearInitEvent(this.hashCode(), this);
        spearSdkApi.initialize(LM_Package);
    }

    // Call this method when SPEAR ASR is initialized successfully. See `onSpearInitComplete()` implementation.
    public void initializeSpearRecognizer() {
        try {
            speechRecognizer = spearSdkApi.createSpeechRecognizer();
            speechRecognizer.subscribeRecognitionListener(this.hashCode(), this);
            speechRecognizer.subscribeVadListener(this.hashCode(), this);
            speechRecognizer.setFstGrammar("example.fst", false);
            speechRecognizer.saveRecognizerInputAudio(true);
            speechRecognizer.saveRecognizerResult(true);
        } catch (UnInitializeException e) {
             LOGGER.log(LEVEL.SEVERE, "UnInitialized SpearSdkApi error", e);
        }
    }

    // Call this method when permissions are granted and SPEAR WakeUp is initialized successfully. See `onSpearInitComplete()` implementation.
    public void initializeSpearWakeUp() {
        try {
            spearWakeUp = spearSdkApi.createSpearWakeUp();
            spearWakeUp.subscribeSpearWakeUpListener(this.hashCode(), this);
            spearWakeUp.initWithFst(WAKEUP_FST, false);
        } catch (UnInitializeException e) {
             LOGGER.log(LEVEL.SEVERE, "UnInitialized SpearSdkApi error", e);
        }
    }

    @Override
    public void onSpearInitComplete() {
       // This callback method does not run on main UI thread. Make sure to run UI changes on UI thread.
       initializeSpearWakeUp();
       initializeSpearRecognizer();
    }

    @Override
    public void onSpearInitError(Throwable throwable) {
       // This callback method does not run on the main UI thread. Make sure to run UI changes on UI thread.
       LOGGER.log(LEVEL.SEVERE, "Error in initializing SPEAR", throwable);
    }

    @Override
    public void onStartListening() {
        // This callback method does not run on the main UI thread. Make sure to run UI changes on UI thread.
        // Do something when SPEAR starts listening for incoming audio after calling
        // speechRecognizer.startListening()
    }
    
    @Override
    public void onStopListening() {
        // This callback method does not run on the main UI thread. Make sure to run UI changes on UI thread.
        // Do something when SPEAR stops listening for incoming audio after calling
        // speechRecognizer.stopListening() or speechRecognizer.forceStopListening()
    }
    
    @Override
    public void onCommitResult(final TamTranscriptionPair[] commitResult) {
        // This callback method does not run on the main UI thread. Make sure to run UI changes on UI thread.
        // Do something with the decoded N-best "commitResults"
    }
    
    @Override
    public void onIntermediateResult(final TamTranscriptionPair[] intermediateResult) {
        // This callback method does not run on the main UI thread. Make sure to run UI changes on UI thread.
        // Do something with the decoded N-best "intermediateResults"
    }

    @Override
    public void onRecognitionError(Throwable throwable) {
            // This callback method does not run on the main UI thread. Make sure to run UI changes on UI thread.
            Log.e(TAG, "Error in Recognizing audio", throwable);
    }

    @Override
    public void onStartWakeUpListening() {
        // This callback method does not run on the main UI thread. Make sure to run UI changes on UI thread.
        // Do something when SPEAR WakeUp starts listening for incoming audio after calling
        // spearWakeUp.startListening()
    }
    
    @Override
    public void onStopWakeUpListening() {
        // This callback method does not run on the main UI thread. Make sure to run UI changes on UI thread.
        // Do something when SPEAR stops listening for incoming audio after calling
        // spearWakeUp.stopListening() or spearWakeUp.forceStopListening()
    }

    @Override
    public void onCommitResult(final SpearWakeUpResult[] spearWakeUpResult) {
        // This callback method does not run on the main UI thread. Make sure to run UI changes on UI thread.
        // Do something with returned "spearWakeUpResult"
    }

    @Override
    public void onVadResult(final TamVadPair vadResult) {
        // This callback method does not run on the main UI thread. Make sure to run UI changes on UI thread.
        // Do something with the vadResult.
    }

    @Override
    public void onStateChange(AudioTask.MicState micState) {
        // This callback method does not run on the main UI thread. Make sure to run UI changes on UI thread.
        // Do something with the current mic status
    }
    @Override
    public void updateSpearConfigStatus(final String status) {
        // This callback method does not run on the main UI thread. Make sure to run UI changes on UI thread.
        // Do something with the config status if there is any
    }


}
```
