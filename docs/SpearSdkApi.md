## SpearSdkApi v5.1.0

`SpearSdkApi` class provides access to the SPEAR Speech Engine. Use `SpearSdkApi.getInstance()` to get the instance of this class. 
Please note that `subscribeSpearInitEvent(int, SpearInitListener)` must be called before 
dispatching any command to the created SpearSdkApi to receive notifications.


## public static SpearSdkApi getInstance()

Gets the instance of SpearSdkApi. Please note that `subscribeSpearInitEvent(int, SpearInitListener)` 
must be called before dispatching any command to the created SpearSdkApi to receive notifications.

 * **Returns:** 
    * SpearSdkApi: A `SpearSdkApi` instance.

## public void initialize(String SpearLmPackage)
 
 * **Parameters:** 
    * `SpearLmPackage` — Absolute path of provided SPEAR LM package on device.

Initializes SPEAR ASR Engine. This method will run in a separate thread and generally takes a
longer time to load the engine. `SpearInitListener#onSpearInitComplete(RegistrationMode)` will be called on successful 
initialization. `SpearInitListener#onSpearInitError(Throwable)` will be called when initialization fails.

## public SpeechRecognizer createSpeechRecognizer() throws UnInitializeException

Creates a new `SpeechRecognizer`. Please note that `SpeechRecognizer#subscribeRecognitionListener(int, RecognitionListener)` 
should be called before dispatching any command to the created `SpeechRecognizer`, otherwise no notifications will be received.

 * **Returns:** 
    * `SpeechRecognizer`: a new `SpeechRecognizer`.
 
 * **Exceptions:**
    * `UnInitializeException` — If called without initializing SPEAR SDK (`initialize(String)`)
                                an error in creating a new `SpeechRecognizer`.

## public SpearWakeUp createSpearWakeUp() throws UnInitializeException

Creates a new `SpearWakeUpAi`. Please note that
`SpearWakeUp#subscribeSpearWakeUpListener(int, SpearWakeUpListener)` should be called before
dispatching any command to the created `SpearWakeUp`, otherwise no notifications will be received.

 * **Returns:**
    * `SpearWakeUp`: a new `SpearWakeUp`.

 * **Exceptions:**
   * `UnInitializeException` — If called without initializing SPEAR SDK (`initialize(String)`)
                                an error in creating a new `SpearWakeUp`.

## public void subscribeSpearInitEvent(int uniqueId, SpearInitListener spearInitListener)

Subscribes `SpearInitListener` to receive SPEAR ASR Engine's initialization callback.

 * **Parameters:**
    * `uniqueId` — unique ID for the `SpearInitListener`.
    * `spearInitListener` — will receive SPEAR ASR Engine's initialization callbacks.

## public void unsubscribeSpearInitEvent(int uniqueId)

Unsubscribes previously subscribed `SpearInitListener` with its uniqueId.

 * **Parameters:**
    * `uniqueId` — unique ID of the subscribed `SpearInitListener`.

## public boolean isSpearInitialized()

Checks whether SPEAR ASR Engine is initialized or not.

 * **Returns:**
    * true: if SPEAR ASR Engine is successfully initialized with `initialize(String)`
    * false: otherwise.

## public void subscribeMicStateChangeListener(int uniqueId, AudioTask.OnMicStateChangeListener onMicStateChangeListener)

Subscribes `AudioTask.OnMicStateChangeListener` to receive microphone state changes.

 * **Parameters:**
    * `uniqueId` — unique ID for the `AudioTask.OnMicStateChangeListener`.
    * `onMicStateChangeListener` — will receive Mic state changes.

## public void unsubscribeMicStateChangeListener(int uniqueId)

Unsubscribes previously subscribed `AudioTask.OnMicStateChangeListener` with its uniqueId.

 * **Parameters:** 
    * `uniqueId` — unique ID of the subscribed `AudioTask.OnMicStateChangeListener`.

## public int getVolume()

Gets the volume level of the incoming audio.

 * **Returns:** 
    * 0 to 100: volume level.
    * -1: If fails to retrieve volume level.

## public void destroy()

Stops listening for incoming audio and destroys the current instance of SpearSdkApi. 
Call this method when re-initialization of SpearSdkApi is needed before calling 
`initialize(String)` method again.

## public void updateSpearConfig(final String[] commands, SpearConfigUpdateListener spearConfigUpdateListener)
Subscribes `SpearConfigUpdateListener` to receive status message.

Updates the config parameters. This method will run in a separate thread. 
`SpearConfigUpdateListener#updateSpearConfigStatus(String message)` will be called only if there is any warning message that user should know. 

 * **Parameters:**
    * `commands` — Strings array of the updated spear config parameters. e.g. new String[]{"--case-preference=lower", "--ignored-symbols=<UNK>,<NOISE>,SPEAR"}
    * `spearConfigUpdateListener` — To recieve warning status message.

# SpearInitListener

## void onSpearInitComplete(RegistrationMode registrationMode)

Called when SPEAR ASR Engine initialized successfully.
 * **Parameters:**
    * `registrationMode`  —
        * UNREGISTERED: when no license file is provided.
        * REGISTERED: when license file is provided and valid.
        * EXPIRED: when license file is provided but expired.

## void onSpearInitError(Throwable throwable)

Called when SPEAR ASR Engine failed on initialization.

 * **Parameters:** 
    * `throwable` — throws error
