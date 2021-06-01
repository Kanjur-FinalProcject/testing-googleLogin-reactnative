# testing-googleLogin-reactnative

how to implement google login

npm i firebase
npm i expo-google-app-auth
copy config folder to your own react-native-app folder

import pacakge in pages that want to implement googlelogin (aka. LoginScreen)
```
import * as firebase from 'firebase'
import * as Google from 'expo-google-app-auth';
import { firebaseConfig } from './config/config'
import { LogBox } from 'react-native';
LogBox.ignoreLogs(['Deprecated: Native Google Sign-In has been moved to Expo.GoogleSignIn (\'expo-google-sign-in\') Falling back to `web` behavior. `behavior` deprecated in SDK 34'])
```

NB. Logbox for hide depcrecated message because if you want it not to be depcrecated you must publish your app first

initialize firebase using code 
```
if (!firebase.apps.length) {
    firebase.initializeApp(firebaseConfig);
  }else {
      firebase.app()
  }
```
put it in highest possible after function screen name itself


now implement function for button
```
async function signInWithGoogleAsync() {
    try {
      const result = await Google.logInAsync({
        behavior: "web",
        androidClientId: "83160479476-t0ajeas3b2p403a7o4h5u1ver42cj06q.apps.googleusercontent.com",
        iosClientId: "83160479476-75021rn5andmc1atknqvdm81064omqkj.apps.googleusercontent.com",
        scopes: ['profile', 'email'],
      });
      if (result.type === 'success') {
        const idToken = result.idToken
        const name = result.user.name
        const email = result.user.email
        console.log(idToken, name, email)
        return result.accessToken;
      } else {
        return { cancelled: true };
      }
    } catch (e) {
      return { error: true };
    }
  }
```
implement those function to your custom google login button.
you can handle what response it gave, 
NB it also gave idToken but our kanjur server require name and email only. 
NBB you can modify anything in that function except await Google.logInAsync