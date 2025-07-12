![React Native Google Sign In](img/header.png)

<p align="center">
  <a href="https://www.npmjs.com/package/@react-native-google-signin/google-signin"><img src="https://badge.fury.io/js/@react-native-google-signin%2Fgoogle-signin.svg" alt="NPM Version"></a>
</p>

❤️❤️ [New documentation site available!](https://react-native-google-signin.github.io/docs/install) ❤️❤️

[website sources live here](https://github.com/react-native-google-signin/docs/tree/main/docs)

## Licence

(MIT)

# react-native-google-signin

Google Sign-In for React Native

## Features

- Support for both iOS and Android platforms
- TypeScript support
- Flow type definitions
- **Nonce support for enhanced security** (iOS: v9.0.0+, Android: Limited support - see limitations)

## Nonce Support

This library supports nonce parameter to prevent replay attacks and enhance security:

### iOS

- **Full support** with Google Sign-In SDK v9.0.0+
- Nonce is automatically included in ID tokens when provided

### Android

- **Limited support** due to legacy Google Sign-In SDK limitations
- Nonce parameter is accepted for API compatibility but not used
- For full nonce support on Android, consider migrating to Google's Credential Manager API

### Usage

```javascript
import { GoogleSignin } from '@react-native-google-signin/google-signin';
import { generateSecureRandom } from 'react-native-securerandom';

// Generate a cryptographically secure nonce
const generateNonce = async () => {
  const randomBytes = await generateSecureRandom(32);
  return randomBytes
    .map(
      (byte) =>
        '0123456789ABCDEFGHIJKLMNOPQRSTUVXYZabcdefghijklmnopqrstuvwxyz-._'[
          byte % 64
        ],
    )
    .join('');
};

// Configure with nonce
await GoogleSignin.configure({
  webClientId: 'YOUR_WEB_CLIENT_ID',
  nonce: await generateNonce(), // Optional: for enhanced security
});

// Sign in with nonce
const nonce = await generateNonce();
const userInfo = await GoogleSignin.signIn({
  nonce: nonce, // Optional: for enhanced security
});
```

### Security Recommendations

1. Always generate a cryptographically secure random nonce
2. Use a different nonce for each sign-in request
3. Verify the nonce in the received ID token on your backend
4. Nonce should be at least 32 characters long

### Platform Limitations

- **iOS**: Full nonce support available with Google Sign-In SDK v9.0.0+
- **Android**: Legacy SDK does not support nonce. Consider using Credential Manager API for full support
