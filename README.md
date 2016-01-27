# EmotionLibrary [![GitHub license](https://img.shields.io/badge/license-Apache%202-blue.svg)](https://github.com/alecrespo/EmotionLibrary/blob/master/LICENSE)
Android Wrapper for the Microsoft Project Oxford Emotion API.

#Download

In your gradle file 
```groovy
compile 'com.alternativecoding:EmotionLibrary:v1.2'
```

#Usage

##Initialisation

First, add the subscription_key to the string.xml  EmotionAnalysis in your application
```xml
  <string name="subscription_key">Please give a valid API Key</string>
```
You can get a free subscription key from https://www.projectoxford.ai/emotion

Second, create a new instance of the service
```java
  EmotionServiceRestClient sFaceServiceClient = new EmotionServiceRestClient(getString(R.string.subscription_key));
```

##Sample

Full reference at https://github.com/alecrespo/EmotionLibrarySample

```java
private class DetectionTask extends AsyncTask<InputStream,String,EmotionFace[]> {
  protected EmotionFace[] doInBackground(InputStream... params) {
      // Get an instance of face service client to detect faces in image.
      EmotionServiceRestClient faceServiceClient = SampleApp.getKey();
      try {
          publishProgress("Detecting...");

          // Start detection.
          return faceServiceClient.detect(params[0]);
      } catch (Exception e) {
          mSucceed = false;
          publishProgress(e.getMessage());
          addLog(e.getMessage());
          return null;
      }
  }
  @Override
  protected void onPostExecute(EmotionFace[] result) {
      if (mSucceed) {
          // Show the result on screen when detection is done.
           addLog("Response: Success. Detected " + (result == null ? 0 : result.length) + " face(s) in " + mImageUri);
          Toast.makeText(getApplicationContext(),"Success",Toast.LENGTH_SHORT).show();
      }
      setUiAfterDetection(result, mSucceed);
  }  
}
//Called by user
  public void detect(View view) {
      // Put the image into an input stream for detection.
      ByteArrayOutputStream output = new ByteArrayOutputStream();
      mBitmap.compress(Bitmap.CompressFormat.JPEG, 100, output);
      ByteArrayInputStream inputStream = new ByteArrayInputStream(output.toByteArray());
    
      // Start a background task to detect faces in the image.
      new DetectionTask().execute(inputStream);
    
      // Prevent button click during detecting.
     // setAllButtonsEnabledStatus(false);
      detect.setEnabled(false);
  }
```


#Community

Feel free to fork !

#Developed By

Author: Alejandro Crespo www.alternativecoding.com/

#License

    Copyright 2015 Alejandro Crespo

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
