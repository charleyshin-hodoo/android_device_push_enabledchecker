# android_device_push_enabledchecker

## aar파일을 Unity의 Plugins/Android폴더로 copy
## 사용법
public class AndroidPushChecker : MonoBehaviour
{
 #if UNITY_ANDROID && !UNITY_EDITOR
    private void Start()
    {
        CallNativePlugin();
    }
    //method that calls our native plugin.
    public void CallNativePlugin()
    {
        // Retrieve the UnityPlayer class.
        AndroidJavaClass unityPlayerClass = new AndroidJavaClass("com.unity3d.player.UnityPlayer");

        // Retrieve the UnityPlayerActivity object ( a.k.a. the current context )
        AndroidJavaObject unityActivity = unityPlayerClass.GetStatic<AndroidJavaObject>("currentActivity");

        // Retrieve the "Bridge" from our native plugin.
        // ! Notice we define the complete package name.              
        AndroidJavaObject bridge = new AndroidJavaObject("com.hodoolabs.hodoomobile.devicepushchecker.DevicePushChecker");

        bridge.Call("setContext", unityActivity);

        var result = bridge.Call<bool>("areNotificationsEnabled");
        

        // Setup the parameters we want to send to our native plugin.              
        object[] parameters = new object[2];
        parameters[0] = unityActivity;
        parameters[1] = $"This is an call to native android! noti enabled =  {result}";

        // Call PrintString in bridge, with our parameters.
        bridge.Call("PrintString", parameters);
    }
#endif
}
